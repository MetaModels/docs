.. _rst_cookbook_specials_ce_element_for_editors:

Predefined Content Element for Editors
=======================================

The MM List is available as a content element or frontend module for displaying
records from a MetaModel. Here various selections such as the MetaModel, render
setting, filter, etc. need to be made — this may not be desirable for editors.

If editors should simply be able to select one or more records from a fixed
MetaModel and have them displayed, this can be done using the following methods.


.. _rst_cookbook_specials_ce_element_for_editors_rstce:

Selection and Display with the RockSolid Custom Elements Extension
------------------------------------------------------------------

RockSolid Custom Elements (RST-CE) allows you to make all input fields available
in the Contao backend freely available as content elements and/or modules. More
on this on the `RockSolid <https://rocksolidthemes.com/de/contao/plugins/custom-content-elements>`_
website or in the `CK24 talk by Marcus Lelle <https://github.com/marcuslelle/contao-rsce>`_.

In the following example, an editor should be able to select a single
Point-of-Interest (POI) and have it displayed in the frontend. The name and
location should appear in the selection.

Configuration in RST-CE
.......................

As is typical with RST-CE, a configuration file for the backend display and a
template for the frontend output must be created. The source code is intended
to illustrate the approach only, and as noted it is recommended to extract the
API queries into separate files. More on the queries at :ref:`ref_api` or in the
`CK23 talk by Ingolf Steinhardt <https://www.e-spin.de/contao-metamodels/metamodels-vortrag-contao-konferenz-2023.html>`_.

.. code-block:: php
   :linenos:

   <?php
   // rsce_mm_poi_single_config.php
   /**
    * POI selection in RST-CE.
    *
    * Note: Option retrieval should be extracted to a helper class — see CK23 talk — or
    * fetched via options_callback (https://docs.contao.org/dev/reference/widgets/select/).
    */

   use Contao\System;

   // POI list.
   $options = [];

   // MetaModel table name.
   $modelName = 'mm_poi';
   // ID of the filter "BE POI single view: Published".
   $filterId = 12;

   // Retrieve items.
   $container        = System::getContainer();
   $factory          = $container->get('metamodels.factory');
   $model            = $factory->getMetaModel($modelName);
   $filter           = $model->getEmptyFilter();
   $filterFactory    = $container->get('metamodels.filter_setting_factory');
   $filterCollection = $filterFactory->createCollection($filterId);
   $items = $model->findByFilter($filter);

   if ($items->getCount()) {
       foreach ($items as $item) {
           $options[$item->get('id')] = \sprintf('%s - %s', $item->get('name'), $item->get('city'));
       }
   }

   return [
       'label'           => ['POI single view', 'POI single view'],
       'types'           => ['content'],
       'fields'          => [
           'poi' => [
               'label'     => ['POI selection', 'Select a POI to display.'],
               'inputType' => 'select',
               'options'   => $options,
               'eval'      => [
                   'chosen'             => true,
                   'mandatory'          => true,
                   'includeBlankOption' => true,
                   'tl_class'           => 'w50',
               ],
           ],
       ],
   ];

Filter in MM
............

To filter by the selected POI ID, a "Custom SQL" filter rule can be created and
filtering can be applied via the passed parameter from `$filterUrl`.

.. code-block:: SQL
   :linenos:

   -- Filter rule in filter 11
   SELECT id FROM {{table}}
   WHERE id = {{param::filter?name=poi}}

Additionally, you could also filter by publication status in the SQL or in a
separate filter rule.

Output template in HTML5
........................

An appropriate template still needs to be created for the output — the RST-CE
naming convention must be observed here.

.. code-block:: php
   :linenos:

   <?php
   // rsce_mm_poi_single.html5

   /**
    * Output of a POI — selection with RST-CE.
    *
    * Note: Item retrieval should be extracted to a helper class — see CK23 talk.
    */

   // Check POI ID.
   if (!$this->poi) {
       return;
   }

   // MetaModel table name.
   $modelName = 'mm_poi';
   // ID of the filter "FE POI single view: POI selection + Published".
   $filterId = 11;
   // Filter value POI ID.
   $filterUrl = ['poi' => (int) $this->poi];
   // ID of the render settings "FE detail view - POI single view".
   $renderId = 20;

   // Retrieve item.
   $factory          = $this->getContainer()->get('metamodels.factory');
   $model            = $factory->getMetaModel($modelName);
   $filter           = $model->getEmptyFilter();
   $filterFactory    = $this->getContainer()->get('metamodels.filter_setting_factory');
   $filterCollection = $filterFactory->createCollection($filterId);
   $filterCollection->addRules($filter, $filterUrl);
   $items = $model->findByFilter($filter);

   // Render item.
   $renderFactory = $this->getContainer()->get('metamodels.render_setting_factory');
   $arrItems      = $items->parseAll('html5', $renderFactory->createCollection($model, $renderId));
   ?>
   <?php if (count($arrItems)): ?>
       <div class="layout_full">
           <?php foreach ($arrItems as $arrItem): ?>
               <div class="poi_item">
                   <h2><?= $arrItem['text']['name'] ?></h2>
                   <p><?= $arrItem['text']['city'] ?></p>
                   <?= $arrItem['html5']['image'] ?>
                   <?php if($arrItem['actions']['jumpTo']['href']): ?>
                       <p><a href="<?= $arrItem['actions']['jumpTo']['href'] ?>" title="Details">Details</a></p>
                   <?php endif; ?>
               </div>
           <?php endforeach; ?>
       </div>
   <?php else : ?>
       <p class="info">No POI selected!</p>
   <?php endif; ?>

Output in Twig
..............

For output in Twig, the data to be rendered must be passed to the template —
querying data inside the template as with HTML5 is not possible in Twig.

The data for Twig is fetched and provided via a `TwigFunction`:

.. code-block:: php
   :linenos:

   <?php
   // src/Twig/AppExtension.php
   namespace App\Twig;

   use MetaModels\Filter\Setting\FilterSettingFactory;
   use MetaModels\IFactory;
   use MetaModels\IMetaModel;
   use MetaModels\Render\Setting\RenderSettingFactory;
   use Twig\Extension\AbstractExtension;
   use Twig\TwigFunction;

   class AppExtension extends AbstractExtension
   {
       public function __construct(
           private readonly IFactory $factory,
           private readonly FilterSettingFactory $filterFactory,
           private readonly RenderSettingFactory $renderFactory,
       ) {
       }

       public function getFunctions(): array
       {
           return [
               new TwigFunction('getPoiById', [$this, 'getPoiById']),
           ];
       }

       public function getPoiById(int $id): array
       {
           // MetaModel table name.
           $modelName = 'mm_poi';
           // ID of the filter "FE POI single view: POI selection + Published".
           $filterId = 11;
           // Filter value POI ID.
           $filterUrl = ['poi' => $id];
           // ID of the render settings "FE detail view - POI single view".
           $renderId = 20;

           // Retrieve item.
           $model            = $this->factory->getMetaModel($modelName);
           $filter           = $model->getEmptyFilter();
           $filterCollection = $this->filterFactory->createCollection($filterId);
           $filterCollection->addRules($filter, $filterUrl);

           // Render items.
           return $model->findByFilter($filter)->parseAll(
               'html5',
               $this->renderFactory->createCollection($model, $renderId)
           );
       }
   }

Registration in ``service.yml``:

.. code-block:: yaml
   :linenos:

   # config/services.yml
   services:
     _defaults:
       autoconfigure: true

     App\Twig\AppExtension:
       arguments:
         $factory: '@metamodels.factory'
         $filterFactory: '@metamodels.filter_setting_factory'
         $renderFactory: '@metamodels.render_setting_factory'

More information on ":ref:`rst_cookbook_specials_register-services`".

Output in the Twig template:

.. code-block:: twig
   :linenos:

   {{ rsce_mm_poi_single.html.twig }}
   {% set pois = getPoiById(poi) %}
   <div{% if id %} id={{ id }}{% endif %}{% if class %} class="{{ class }}"{% endif %}>
       {% if pois|length > 0 %}
           <div class="layout_full">
               {% for poi in pois %}
                   <div class="poi_item">
                       <h2>{{ poi.html5.name|raw }}</h2>
                       <p>{{ poi.html5.city|raw }}</p>
                       {{ ... }}
                   </div>
               {% endfor %}
           </div>
       {% else %}
           <p class="info">No POI selected!</p>
       {% endif %}
   </div>


.. _rst_cookbook_specials_ce_element_for_editors_ce:

Selection and Display with a Custom Content Element
---------------------------------------------------

If you want to implement the functionality using Contao's built-in tools instead
of an extension, you can create a custom content element.

In the example, a list of MM records should be selectable as products and
displayed on the website. The output order should be individually configurable.


Content Element and Callback
............................

First, a DCA configuration and translations are created. For the custom order,
``inputType`` is defined as ``checkboxWizard``. After creating the DCA definition,
a database migration must be performed.

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/tl_content.php
   use Doctrine\DBAL\Platforms\MySQLPlatform;

   $GLOBALS['TL_DCA']['tl_content']['palettes']['mm_products'] = '
       {type_legend},type,headline;
       {mm_products_legend},mm_products;
       {protected_legend:hide},protected;
       {expert_legend:hide},guests,cssID;
       {invisible_legend:hide},invisible,start,stop;';

   $GLOBALS['TL_DCA']['tl_content']['fields']['mm_products'] = [
       'label'            => &$GLOBALS['TL_LANG']['tl_content']['mm_products'],
       'inputType'        => 'checkboxWizard',
       //'options_callback' => See attribute config in MmProductsCallbackListener
       'eval'             => [
           'mandatory' => true,
           'multiple'  => true,
           'tl_class'  => 'w50',
       ],
       'sql'              => [
           'type'    => 'blob',
           'length'  => MySQLPlatform::LENGTH_LIMIT_BLOB,
           'notnull' => false,
       ],
   ];

.. code-block:: php
   :linenos:

   <?php
   // contao/languages/en/tl_content.php

   // CTE
   $GLOBALS['TL_LANG']['CTE']['mm_products'] = ['CE Product selection', 'CE Product selection for MM products'];
   // Legends
   $GLOBALS['TL_LANG']['tl_content']['mm_products_legend'] = 'Product selection';
   // Fields
   $GLOBALS['TL_LANG']['tl_content']['mm_products'] = ['Product selection', 'Select several products.'];


To generate the selection list for the new content element, the records from MM
must be read.

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/DataContainer/MmProductsCallbackListener.php
   namespace App\EventListener\DataContainer;

   use Contao\CoreBundle\DependencyInjection\Attribute\AsCallback;
   use Contao\DataContainer;
   use MetaModels\Filter\Setting\FilterSettingFactory;
   use MetaModels\IFactory;
   use MetaModels\IMetaModel;

   use function sprintf;

   #[AsCallback(table: 'tl_content', target: 'fields.mm_products.options')]
   class MmProductsCallbackListener
   {
       public function __construct(
           private readonly IFactory $factory,
           private readonly FilterSettingFactory $filterFactory,
       ) {
       }

       public function __invoke(DataContainer|null $dc = null): array
       {
           // Product list.
           $options = [];

           // MetaModel table name.
           $modelName = 'mm_products';
           // ID of the filter "List published".
           $filterId = 4;

           // Retrieve items - sorted by name.
           $model = $this->factory->getMetaModel($modelName);
           assert($model instanceof IMetaModel);
           $filter           = $model->getEmptyFilter();
           $filterCollection = $this->filterFactory->createCollection($filterId);
           $filterCollection->addRules($filter, []);
           $items = $model->findByFilter($filter, 'name');

           if ($items->getCount()) {
               foreach ($items as $item) {
                   $options[$item->get('id')] =
                       \sprintf('%s - %s [%s]', $item->get('name'), $item->get('measures'), $item->get('articleno'));
               }
           }

           return $options;
       }
   }

Output in Twig with a Custom Controller
.......................................

The next step is to create the product output. A controller and a Twig output
template are needed.

.. code-block:: php
   :linenos:

   <?php
   // src/Controller/ContentElement/MmProductsElement.php
   namespace App\Controller\ContentElement;

   use Contao\BackendTemplate;
   use Contao\ContentModel;
   use Contao\CoreBundle\Controller\ContentElement\AbstractContentElementController;
   use Contao\CoreBundle\Routing\ScopeMatcher;
   use Contao\CoreBundle\ServiceAnnotation\ContentElement;
   use Contao\CoreBundle\Twig\FragmentTemplate;
   use Contao\PageModel;
   use Contao\StringUtil;
   use MetaModels\Filter\Setting\FilterSettingFactory;
   use MetaModels\IFactory;
   use MetaModels\IMetaModel;
   use MetaModels\Render\Setting\RenderSettingFactory;
   use Symfony\Component\HttpFoundation\Request;
   use Symfony\Component\HttpFoundation\RequestStack;
   use Symfony\Component\HttpFoundation\Response;

   use function implode;
   use function is_array;

   /**
    * @ContentElement("mm_products",
    *   category="texts",
    *   template="ce_mm_products",
    * )
    */
   class MmProductsElement extends AbstractContentElementController
   {
       public function __construct(
           private readonly IFactory $factory,
           private readonly FilterSettingFactory $filterFactory,
           private readonly RenderSettingFactory $renderFactory,
           private readonly ScopeMatcher $scopeMatcher,
           private readonly RequestStack $requestStack,
       ) {
       }

       protected function getResponse(FragmentTemplate $template, ContentModel $model, Request $request): Response
       {
           $arrHeadline = StringUtil::deserialize($model->headline, true);
           $headline    = is_array($arrHeadline) ? $arrHeadline['value'] ?? '' : $arrHeadline;
           $template->set('headline', $headline);
           $template->set('hl', $arrHeadline['unit'] ?? 'h2');

           $productsList = StringUtil::deserialize($model->mm_products, true);

           if ($this->isBackend()) {
               $template = new BackendTemplate('be_wildcard');
               $template->title    = $headline;
               $template->wildcard = 'Products: ' . implode(', ', $productsList);

               return $template->getResponse();
           }

           $arrCssId = StringUtil::deserialize($model->cssID, true);
           $template->set('id', $arrCssId[0] ?? '');
           $template->set('class', $arrCssId[1] ?? '');

           $template->set('products', $this->getProductsByIds($productsList));

           // ID of the inquiry form page.
           $template->set('pageAlias', PageModel::findById(5)->alias);

           return $template->getResponse();
       }

       protected function getProductsByIds(array $ids): array
       {
           // MetaModel table name.
           $modelName = 'mm_products';
           // ID of the filter "List published + product IDs".
           $filterId = 5;
           // Filter value products.
           $filterUrl = ['products' => $ids];
           // ID of the render settings "Product list".
           $renderId = 4;

           // Retrieve items.
           $model = $this->factory->getMetaModel($modelName);
           assert($model instanceof IMetaModel);
           $filter           = $model->getEmptyFilter();
           $filterCollection = $this->filterFactory->createCollection($filterId);
           $filterCollection->addRules($filter, $filterUrl);

           // Render items.
           return $model->findByFilter($filter)->parseAll(
               'html5',
               $this->renderFactory->createCollection($model, $renderId)
           );
       }

       public function isBackend(): bool
       {
           if ($request = $this->requestStack->getCurrentRequest()) {
               return $this->scopeMatcher->isBackendRequest($request);
           }

           return false;
       }
   }


To filter by the selected product IDs, a "Custom SQL" filter rule can be created
and filtering applied via the passed parameter from `$filterUrl` — here a series
of IDs is passed and their order should remain unchanged.

.. code-block:: SQL
   :linenos:

   -- Filter rule in filter 11
   SELECT id FROM {{table}}
   WHERE id IN({{param::filter?name=products&aggregate=set&default=0}})
   ORDER BY FIELD(id, {{param::filter?name=products&aggregate=set&default=0}})

Additionally, you could filter by publication status in the SQL or in a
separate filter rule.


.. code-block:: twig
   :linenos:

   {# templates/ce_mm_products.html.twig #}
   {% if headline %}
       <{{ hl }}>{{ headline }}</{{ hl }}>
   {% endif %}
   <div{% if id %} id={{ id }}{% endif %}{% if class %} class="{{ class }}"{% endif %}>
       {% if products|length > 0 %}
       <div class="product__list">
           {% for product in products %}
               <div class="product">
                   <div class="product__image">
                       {{ product.html5.list_image|raw }}
                   </div>
                   <div class="product__features">
                       <div class="product__name"><a href="{{ product.actions.jumpTo.href }}">{{ product.text.name }}</a></div>
                       {% if product.text.sub_headline %}
                           <div class="product__subheadline">({{ product.text.sub_headline }})</div>
                       {% endif %}
                       {% if product.text.measures %}
                           <div class="product__measures">{{ product.text.measures }}</div>
                       {% endif %}
                   </div>
                   {% if product.text.inquiry %}
                       <div class="inquiry">
                           <a href="{{ pageAlias }}?articlno={{ product.text.articleno }}&name={{ product.text.name }}" class="inquiry__button">Enquire</a>
                       </div>
                   {% endif %}
               </div>
           {% endfor %}
       </div>
       {% else %}
           <p class="info">No product selected!</p>
       {% endif %}
   </div>


Loading Services
................

There are several ways to load all classes — see ":ref:`rst_cookbook_specials_register-services`".
With a custom ``services.yml``, it looks like this:


.. code-block:: yaml
   :linenos:

   # config/services.yml
   services:
     _defaults:
       autoconfigure: true

     App\Controller\ContentElement\MmProductsElement:
       arguments:
         $factory: '@metamodels.factory'
         $filterFactory: '@metamodels.filter_setting_factory'
         $renderFactory: '@metamodels.render_setting_factory'
         $scopeMatcher: '@contao.routing.scope_matcher'
         $requestStack: '@request_stack'

     App\EventListener\DataContainer\MmProductsCallbackListener:
       arguments:
         $factory: '@metamodels.factory'
         $filterFactory: '@metamodels.filter_setting_factory'

Whether everything is loaded can be tested via a console call — clear the cache
and run "composer install" if necessary.
