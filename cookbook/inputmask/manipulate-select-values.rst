.. _rst_cookbook_inputmask_manipulate-select-values:

Input Mask: Adding More Values to Single Select
================================================

By default, the value column for the single select attribute is restricted to
selecting one attribute from any Contao table. If you want to display one or
more attributes/values from the referenced table in the input mask's single
select attribute, this can be done in several ways:

**1. "Combined values" attribute**

Create an additional attribute in the referenced model that combines the values
for display.

**2. Event "GetPropertyOptionsEvent"** (recommended)

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/GetPropertyOptionsListener.php

   namespace App\EventListener;

   use Contao\MemberModel;
   use ContaoCommunityAlliance\DcGeneral\Contao\View\Contao2BackendView\Event\GetPropertyOptionsEvent;
   use MetaModels\AttributeSelectBundle\Attribute\AbstractSelect;
   use MetaModels\DcGeneral\Data\Model;
   use Terminal42\ServiceAnnotationBundle\Annotation\ServiceTag;

   /**
    * @ServiceTag("kernel.event_listener", event="dc-general.view.contao2backend.get-property-options", priority="100")
    */
   class GetPropertyOptionsListener
   {
       public function __invoke(GetPropertyOptionsEvent $event)
       {
           // Check if options set.
           if ($event->getOptions() !== null) {
               return;
           }

           // Check if right model table and type.
           if ('mm_my_model' !== $event->getEnvironment()->getDataDefinition()->getName()) {
               return;
           }

           $model = $event->getModel();
           if (!($model instanceof Model)) {
               return;
           }

           // Check if right attribute and type.
           if ('member' !== $event->getPropertyName()) {
               return;
           }

           $attribute = $model->getItem()->getAttribute($event->getPropertyName());
           if (!($attribute instanceof AbstractSelect)) {
               return;
           }

           // Generate own options list.
           $members     = MemberModel::findAll(['order' => 'lastname ASC']); // add e.g. filter for not disabled...
           $aliasColumn = $attribute->get('select_alias');

           $options = [];

           foreach ($members as $member) {
               $options[$member->{$aliasColumn}] =
                   \sprintf('%s, %s [%s]', $member->lastname, $member->firstname, $member->email);
           }

           $event->setOptions($options);
       }
   }

Result: |br|
|img_manipulate-select-values_01|

Reference: |br|
`GetPropertyOptionsListener <https://github.com/MetaModels/attribute_select/blob/master/src/EventListener/GetPropertyOptionsListener.php>`_

**3. DCA callback "options_callback"**

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/<MM-Table-Name>.php
   $GLOBALS['TL_DCA']['<MM-Table-Name>']['fields']['<MM-Column-Name-Select>'] = [
    'options_callback' => function () {
        $modelName = '<MM-Table-Name-Select>';
        $factory   = $this->getContainer()->get('metamodels.factory');
        $model     = $factory->getMetaModel($modelName);
        $filter    = $model->getEmptyFilter();
        $items     = $model->findByFilter($filter);
        $arrItems  = $items->parseAll('text');

        $options = [];
        foreach ($arrItems as $arrItem) {
            $options[$arrItem['text']['<MM-Select-Column-Name-Alias>']] = \sprintf(
            '%s [%s]',
            $arrItem['text']['<MM-Select-Column-Name-1>'],
            $arrItem['text']['<MM-Select-Column-Name-2>']
            );
        }

        return $options;
       },
   ];

The keys of the ``$options`` array must match the "Alias" setting from the
attribute configuration.

Filters configured in the "Select" attribute for the backend are bypassed with
this approach.


.. |img_manipulate-select-values_01| image:: /_img/screenshots/cookbook/inputmask/manipulate-select-values_01.jpg

.. |br| raw:: html

   <br />
