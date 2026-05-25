.. _rst_cookbook_specials_add_items_at_navigation:

Displaying Detail Pages in the Contao Navigation
=================================================

The Contao navigation frontend module only outputs pages that exist in the page
tree as navigation items, e.g. ``/products/overview``. If you also want to show
detail pages such as ``/product/detail/item-number/1364`` in addition to regular
Contao pages — even though only the page ``/product/detail`` exists in the page
tree — there are several approaches.


Custom Pages in the Page Tree with Alias for Redirect
------------------------------------------------------

If the page ``/product/detail`` exists as a Contao page where the desired article
is shown via the slug parameters ``item-number/1364``, you can add a new page in
the page tree at the desired position for the navigation, e.g. with the title
"Article 1364". However, the alias of that page is set manually to the alias of
the detail view ``/product/detail/item-number/1364``. For the "Article 1364"
navigation link to also display the desired content, the page ``/product/detail``
must have a higher route priority (10) than the page ``item-number/1364`` (0).

:ref:`More tips on route priority <rst_cookbook_tips_set-route-priority>`.


ParseTemplateListener for Customising the Navigation
-----------------------------------------------------

With the `ParseTemplateListener <https://docs.contao.org/5.x/dev/reference/hooks/parseTemplate/>`_,
the template ``nav_default`` (or custom variants of it) can still be manipulated
before it is "delivered". This makes it possible to add custom navigation links
at any desired position (see ``getSublinks()``). The following is example code:

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/ParseTemplateListener.php

   namespace App\EventListener;

   use Contao\CoreBundle\DependencyInjection\Attribute\AsHook;
   use Contao\Template;
   use MetaModels\Filter\Setting\IFilterSettingFactory;
   use MetaModels\IFactory;
   use MetaModels\IMetaModel;
   use MetaModels\Render\Setting\IRenderSettingFactory;
   use Symfony\Component\HttpFoundation\RequestStack;

   use function sprintf;
   use function str_replace;
   use function trim;

   #[AsHook('parseTemplate')]
   class ParseTemplateListener
   {
       public function __construct(
           private readonly IFactory $factory,
           private readonly IFilterSettingFactory $filterFactory,
           private readonly IRenderSettingFactory $renderFactory,
           private readonly RequestStack $requestStack,
       ) {
       }

       public function __invoke(Template $template): void
       {
           if ('nav_default' === $template->getName()) {
               $levelData = $template->getData();

               // Check only level 1.
               if ('level_1' !== $levelData['level']) {
                   return;
               }

               $items = $levelData['items'];
               foreach ($items as &$item) {
                   // Check only page id 7.
                   if (7 !== ($item['id'] ?? null)) {
                       continue;
                   }

                   // Add subitems as level 2 at page id 7 and mark parent as trail.
                   if ([] !== ($subLinks = $this->getSublinks())) {
                       $item['subitems'] = $subLinks['subitems'];
                       $item['class']    =
                           trim(
                               'submenu ' . ($subLinks['trail'] ? str_replace('sibling', 'trail', $item['class']) : '')
                           );
                   }
               }
               unset($item);
               $levelData['items'] = $items;

               $template->setData($levelData);
           }
       }

       private function getSublinks(): array
       {
           // Begin configuration.
           $modelName = 'mm_employees';
           $renderId  = 4;
           $filterId  = 3;
           // End configuration.

           if (!(($model = $this->factory->getMetaModel($modelName)) instanceof IMetaModel)) {
               return [];
           }

           $filter           = $model->getEmptyFilter();
           $filterCollection = $this->filterFactory->createCollection($filterId);
           $filterCollection->addRules($filter, []);
           $items = $model->findByFilter($filter);

           if (!$items->getCount()) {
               return [];
           }

           $parsed = $items->parseAll('text', $this->renderFactory->createCollection($model, $renderId));
           unset($items, $filterCollection, $filter, $model);

           $request = $this->requestStack->getCurrentRequest();
           if (null === $request) {
               return [];
           }
           $path        = $request->getRequestUri();
           $isTrail     = false;
           $subLinkList = '<ul class="level_2">';
           foreach ($parsed as $item) {
               $href = $item['actions']['jumpTo']['href'];
               // Possibly clean up the path+href from GET parameters or anchor links.
               if ($path !== $href) {
                   $subLinkList .= sprintf(
                       '<li><a href="%1$s" title="%2$s">%2$s</a></li>',
                       $href,
                       $item['text']['name']
                   );
               } else {
                   $isTrail     = true;
                   $subLinkList .= sprintf(
                       '<li class="active"><strong class="active" aria-current="page">%s</strong></li>',
                       $item['text']['name']
                   );
               }
           }
           $subLinkList .= '</ul>';

           return ['trail' => $isTrail, 'subitems' => $subLinkList];
       }
   }

More on registering services in the :ref:`linked article <rst_cookbook_specials_register-services>`.


Extension "hofff/contao-navigation" and "TreeEvent"
----------------------------------------------------

The extension "`Contao-Navigation <https://github.com/hofff/contao-navigation>`_"
provides its own frontend module for navigation. It also has various events that
allow more elegant output manipulation compared to the ``ParseTemplateListener``.
The following is example code for appending an additional link in the navigation
— this can also be adapted for outputting MM detail pages.

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/NavigationMenuListener.php

   namespace App\EventListener;

   use Hofff\Contao\Navigation\Event\TreeEvent;
   use Symfony\Component\EventDispatcher\Attribute\AsEventListener;

   use function array_keys;

   #[AsEventListener('Hofff\Contao\Navigation\Event\TreeEvent')]
   class NavigationMenuListener
   {
       public function __invoke(TreeEvent $treeEvent): void
       {
           $moduleId  = $treeEvent->moduleModel()->id; // Module id for checking if it's the correct module.
           $pageId    = $treeEvent->items()->currentPage->id; // Page id for checking if it's the correct page.
           $pageItems = $treeEvent->items(); // Get the page items for the navigation tree.
           $rootIds   = $pageItems->roots;

           // Add a new item to the first root as the last one.
           $pageItems->subItems[array_keys($rootIds)[0]][] = 9999;

           // Item data.
           $pageItems->items[9999] = [
               'class'     => 'mm-page',
               'isInTrail' => false,
               'isActive'  => false,
               'pageTitle' => 'MetaModels',
               'accesskey' => '',
               'target'    => 'target="_blank"',
               'link'      => 'MetaModels',
               'href'      => 'https://now.metamodel.me',
           ];
       }
   }

More on registering services in the :ref:`linked article <rst_cookbook_specials_register-services>`.
