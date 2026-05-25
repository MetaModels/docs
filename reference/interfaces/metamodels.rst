.. _ref_api_interf_mm:

MetaModels Interfaces
=====================

The MetaModels interfaces form the foundation of the API and provide access to a
MetaModel down to the individual item.

Much of the work when using the interfaces focuses on querying existing data from a
MetaModel. The structure follows the same pattern as a query or listing via a content
element or frontend module with:

* Connection to MetaModels — e.g. to establish a connection outside a MetaModel template —
  see :ref:`ref_api_interf_mm_metamodelsservicecontainer`
* Connection to the MetaModel — see :ref:`ref_api_interf_mm_factory`
* Querying a MetaModel with filter rules applied
  — see :ref:`ref_api_interf_mm_metamodel`
* Querying and setting the active language for translated MetaModels — see
  :ref:`ref_api_interf_mm_translatedmetamodel`
* Access to all items; optionally parsing items with a specified output format
  (Text, HTML5) and render setting — see :ref:`ref_api_inteface_items`
* Access to an individual item and its output (Raw, Text, HTML5) — see :ref:`ref_api_interf_mm_item`

Via the MetaModel interfaces, various objects (MetaModel, attribute, item) can also be
created, their values modified, or properties such as count or language queried.


.. _ref_api_interf_mm_metamodelsservicecontainer:

MetaModelsServiceContainer Interface:
.....................................

The MetaModelsServiceContainer interface allows a connection to MetaModels to be
established. This is necessary, for example, when accessing a MetaModel outside a
MetaModel template.

For access you need a "service container", which can be obtained e.g. in global scope:

``$container = $this->getContainer();``

Then an interface can be accessed via it — e.g.:

``$factory = $container->getFactory();`` |br|
or |br|
``$factory = $this->getContainer()->get('metamodels.factory');``

The service container can easily be accessed in custom templates and programming.
Other options include events such as "\MetaModelsEvents::SUBSYSTEM_BOOT".

Current information at: `IMetaModelsServiceContainer <https://github.com/MetaModels/core/blob/master/src/IMetaModelsServiceContainer.php>`_

**Interfaces:**

``getFactory()`` |br|
returns access to MetaModels

``getAttributeFactory()`` |br|
returns access to the attributes

``getFilterFactory()`` |br|
returns access to the filters

``getRenderSettingFactory()`` |br|
returns access to the render settings

``getEventDispatcher()`` |br|
returns access to the event dispatcher

``getDatabase()`` |br|
returns access to the database

``getCache()`` |br|
returns access to the cache

``setService($service, $serviceName = null)`` |br|
adds a custom service to the container

``getService($serviceName)`` |br|
returns access to a service with the given name


.. _ref_api_interf_mm_servicecontaineraware:

ServiceContainerAware Interface:
................................

The ServiceContainerAware interface provides access to the service container or
allows a new service container to be assigned.

Current information at: `IServiceContainerAware <https://github.com/MetaModels/core/blob/master/src/IServiceContainerAware.php>`_

**Interfaces:**

``setServiceContainer(IMetaModelsServiceContainer $serviceContainer)`` |br|
sets the service container to be used

``getServiceContainer()`` |br|
returns the service container


.. _ref_api_interf_mm_factory:

Factory Interface:
..................

The Factory interface allows instances of a MetaModel to be created and certain
properties to be queried.

Creating a new MetaModel is not intended — though possible — as very complex parameters
would need to be passed and creation is oriented around backend work.

Current information at: `IFactory <https://github.com/MetaModels/core/blob/master/src/IFactory.php>`_

**Interfaces:**

``getMetaModel($modelName);`` |br|
creates a MetaModel instance with the given name

``translateIdToMetaModelName($modelId);`` |br|
returns the name for a given MetaModel ID

``collectNames();`` |br|
returns all MetaModel names as an array

``getServiceContainer();`` |br|
returns the service container

.. warning:: The methods `byTableName`, `byId` and `getAllTables` were removed in version 2.0

``byTableName($strTableName);`` |br|
use method ``getMetaModel($modelName);`` instead

``byId($intMetaModelId);`` |br|
use method ``getMetaModel($modelName);`` with
``translateIdToMetaModelName($modelId);`` instead

``getAllTables();`` |br|
use method ``collectNames();`` instead



.. _ref_api_interf_mm_metamodel:

MetaModel Interface:
....................

The MetaModel interface allows properties of a MetaModel instance to be queried or
modified.

First, a MetaModels instance must be created via the ID or name of a MetaModel
(see :ref:`ref_api_interf_mm_factory`)

``$model = \MetaModels\IFactory::getMetaModel($modelName);``

or including the service container:

.. code-block:: php
   :linenos:

   <?php
   $modelId = 42;

   /** @var $container */
   $factory   = $this->getContainer()->get('metamodels.factory');
   $modelName = $factory->translateIdToMetaModelName($modelId);
   $model     = $factory->getMetaModel($modelName);


Then a property can be queried or set — e.g. querying all available attributes:

``$attributes = $metaModel->getAttributes();``

Current information at: `IMetaModel <https://github.com/MetaModels/core/blob/master/src/IMetaModel.php>`_

**Interfaces:**

.. warning:: The method `getServiceContainer` is deprecated — please use as a service

``getServiceContainer()`` |br|
returns the service container

``get($strKey)``  |br|
returns the configuration settings

``getTableName()``  |br|
returns the table name of the instantiated MetaModel

``getName()``  |br|
returns the name of the instantiated MetaModel

.. warning:: The method `isTranslated` is deprecated — please use ITranslatedMetaModel

``isTranslated()``  |br|
checks whether the instantiated MetaModel can create translations

``hasVariants()``  |br|
checks whether the instantiated MetaModel can create variants

.. warning:: The method `getAvailableLanguages` is deprecated — please use ITranslatedMetaModel

``getAvailableLanguages()``  |br|
returns all language codes of the instantiated MetaModel as an array

.. warning:: The method `getFallbackLanguage` is deprecated — please use ITranslatedMetaModel

``getFallbackLanguage()``  |br|
returns the language code of the fallback language of the instantiated MetaModel

.. warning:: The method `getActiveLanguage` is deprecated — please use ITranslatedMetaModel

``getActiveLanguage()``  |br|
returns the language code of the active language of the instantiated MetaModel

``addAttribute(IAttribute $attribute)``  |br|
adds an attribute to the internal attribute list

``hasAttribute($attributeName)``  |br|
checks whether an attribute with the given name exists in the internal attribute list

``getAttributes()``  |br|
returns an array of all attributes of the instantiated MetaModel

``getInVariantAttributes()``  |br|
returns an array of the attributes of the instantiated MetaModel that are not
defined as variants

``getAttribute($attributeName)``  |br|
returns the instance of the attribute with the given attribute name

``getAttributeById($id)``  |br|
returns the instance of the attribute with the given attribute ID

``findById($id, $attrOnly = [])``  |br|
returns the item with the given ID; optionally an array of attribute names can be
specified whose values should be returned

``getEmptyFilter()``  |br|
creates an "empty" filter object without filter rules

.. warning:: The method `prepareFilter` is deprecated — please use Filter-Setting-Factory

``prepareFilter($filterSettings, $filterUrl)``  |br|
creates a filter object from a given filter ID and an optional array of filter
parameters, e.g. for taking GET values from a URL

``findByFilter(
$filter,
$sortBy = '',
$offset = 0,
$limit = 0,
$sortOrder = 'ASC',
$attrOnly = []
)``  |br|
returns the items found by a given filter in the instantiated MetaModel — in addition
to the sorting, offset, limit, and sort direction parameters, an array of attribute
names can be specified whose values should be returned

``getIdsFromFilter(
$filter,
$sortBy = '',
$offset = 0,
$limit = 0,
$sortOrder = 'ASC'
)``  |br|
returns the IDs of items found by a given filter in the instantiated MetaModel —
sorting, offset, limit, and sort direction parameters can be specified

``getCount($filter)``  |br|
returns the number of items found by a given filter

``findVariantBase($filter)``  |br|
returns all items of a variant base found by a given filter

``findVariants($ids, $filter)``  |br|
returns all variant items for an array of IDs and a given filter

``findVariantsWithBase($ids, $filter)``  |br|
returns all variant items for an array of IDs and a given filter; the query does not
distinguish between variant base items and variant items

``getAttributeOptions($attribute, $filter = null)``  |br|
returns all options of a given attribute; optionally a filter can be specified

``saveItem($item, $timestamp = null)``  |br|
saves a given item, or creates a new item if no ID was passed

``delete($item)``  |br|
deletes a given item

.. warning:: The method `getView` is deprecated — please use Render-Setting-Factory

``getView($viewId = 0)``  |br|
returns the render settings instance of the instantiated MetaModel


.. _ref_api_interf_mm_translatedmetamodel:

Translated MetaModel Interface:
....................

.. note:: This feature is available from MM 2.2.

The Translated MetaModel interface allows the language settings of a translated
MetaModel to be queried or set.

Up to version MM 2.1, the active language of a translated MetaModel could only be set
via the (temporary) assignment of ``$GLOBALS['TL_LANGUAGE']``. With this interface, the
language of the MetaModel can be set independently of Contao's backend language.

For example, to save an item in a specific language for a translated MetaModel, the
language can be set via the language code (de, en, fr, ...) as follows:

``$model->selectLanguage('de');``

A type check can be implemented as follows:

.. code-block:: php
   :linenos:

   <?php

   use MetaModels\ITranslatedMetaModel;

   if ($model instanceof ITranslatedMetaModel) {
       // make anything...
   }

From MetaModels 2.2, the following interfaces must be used:

**Interfaces:**

``getLanguages()``  |br|
determines all language codes marked as available for translation in this MetaModel

``getMainLanguage()``  |br|
determines the language code marked as the fallback language in this MetaModel

``getLanguage()``  |br|
determines the current language code

``selectLanguage($activeLanguage)``  |br|
sets the new active language and returns the previous language code


.. _ref_api_inteface_items:

Items Interface:
................

The Items interface allows properties of items to be queried.

First, a MetaModels instance must be created via the ID or name of a MetaModel, and
then a list of items retrieved, e.g. via a filter.

``$items = $model->findByFilter($filter);``

Then a property can be queried — e.g. the total count of all items:

``$amountItems = $items->getCount();``

Current information at: `IItems <https://github.com/MetaModels/core/blob/master/src/IItems.php>`_

**Interfaces:**

``getItem()``  |br|
returns the current item

``getCount()``  |br|
returns the number of items

``first()``  |br|
sets the pointer to the first element of the items

``prev()``  |br|
sets the pointer to the previous element of the items

``last()``  |br|
sets the pointer to the last element of the items

``reset()``  |br|
resets the current result

``getClass()``  |br|
returns the CSS class of the current item (first, last, even, odd)

``parseValue($outputFormat = 'text', $settings = null)``  |br|
parses the current item and returns the result as an array of attributes;
for HTML5 output the render settings must be passed as
$objSettings, e.g. $metaModel->getView(3)

``parseAll($outputFormat = 'text', $settings = null)``  |br|
parses all items and returns the result as an array of items with their attributes;
for HTML5 output the render settings must be passed as
$objSettings, e.g. $metaModel->getView(3)


.. _ref_api_interf_mm_item:

Item Interface:
...............

The Item interface allows properties of an item to be queried.

First, a MetaModels instance must be created via the ID or name of a MetaModel, and
then a list of items retrieved via a filter (optionally also an empty filter).

``$items = $model->findByFilter($filter);``  |br|

Then a property can be queried — e.g. the value of an attribute:

``$attribute = $items->getItem()->get($attributeName);``  |br|

A new item is created as follows:

``$item = new \MetaModels\Item($model, []);``

Key-value pairs can be passed in the array — but this is only useful for simple item
types such as Text.

Current information at: `IItem <https://github.com/MetaModels/core/blob/master/src/IItem.php>`_

**Interfaces:**

``get($attributeName)``  |br|
returns the value of an attribute for the given attribute name

``set($attributeName, $value)``  |br|
sets the value of an attribute for the given attribute name

``getMetaModel()``  |br|
returns the MetaModel instance of the item

``getAttribute($attributeName)``  |br|
returns the instance of an attribute for the given attribute name

``isVariant()``  |br|
determines whether the item is a variant of another item

``isVariantBase()``  |br|
determines whether the item is a variant base

``getVariants($filter)``  |br|
returns an array of the item's variants, or null if the item does not support variants

``getVariantBase()``  |br|
returns the variant base item; for an item without variants, the variant base is
the item itself

``parseValue($outputFormat = 'text', $settings = null)``  |br|
renders the item in the specified format; raw data is always included in output,
including attributes of referenced MetaModels

``parseAttribute($attributeName, $outputFormat = 'text', $settings = null)``  |br|
renders a single attribute of the item in the specified format; raw data is always
included in output, including attributes of referenced MetaModels

``copy()``  |br|
creates a new item as a copy of an existing item

``varCopy()``  |br|
creates a new item as a copy of an existing item as a variant

``save()``  |br|
saves the current value(s) for the item


Example:
........

The following example provides a brief introduction to working with the interfaces.
For inspiration when testing the API, see the
`talk by Ingolf Steinhardt at CK23 <https://www.e-spin.de/contao-metamodels/metamodels-vortrag-contao-konferenz-2023.html>`_.

Examples for using filters can be found here: :ref:`ref_api_interf_filter`

The example builds on ":ref:`mm_first_index`".

.. code-block:: php
   :linenos:

   <?php
   // Example for implementation in a template file for testing.
   // In a live environment, use a "helper class" and inject services there.


   // Name of the MetaModel table (see "The First MetaModel")
   $modelName = 'mm_employeelist';
   // ID of the render setting "FE List"
   $renderId = 2;
   // ID of the filter
   $filterId = 1;

   // MM factories
   $factory       = \Contao\System::getContainer()->get('metamodels.factory');
   $renderFactory = \Contao\System::getContainer()->get('metamodels.render_setting_factory');

   // Create MetaModel when table/MetaModel name is known.
   $model = $factory->getMetaModel($modelName);
   // Create MetaModel when only id is known ($metaModelId == tl_metamodel.id of the MetaModel).
   //$model = $factory->getMetaModel($factory->translateIdToMetaModelName($metaModelId));

   // empty filter - see also "Filter interfaces"
   $filter = $model->getEmptyFilter();
   // predefined filter via filter ID; an array of values can be passed as second parameter
   //$filter = $model->prepareFilter($filterId, []);

   // fetch all items with filter
   $items = $model->findByFilter($filter);

   // number of items
   echo 'Count: '.$items->getCount()."<br>\n";
   // or check
   if (!$items->getCount()) {
       return;
   }

   // Output: variant 1 - items object
   /*
   foreach ($items as $item)
   {
       echo $item->get('name')."<br>\n";
   }
   */

   // Output: variant 2 - items array
   // all items parsed to array with HTML5 nodes
   $arrItems = $items->parseAll('html5', $renderFactory->createCollection($model, $renderId));
   // alternatively only raw and text nodes
   //$arrItems = $items->parseAll('text');
   foreach ($arrItems as $arrItem)
   {
       echo $arrItem['html5']['name']."<br>\n";
   }

   // Output: variant 3 - process only current item
   $item = $items->getItem()->parseValue('text', $renderFactory->createCollection($model, $renderId));
   echo $item['text']['name']."<br>\n";


.. |br| raw:: html

   <br />
