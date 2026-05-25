.. _rst_cookbook_inputmask_default-values:

Input Mask: Automatic Default Values
=====================================

The input fields of input masks can be pre-filled with default values
automatically. This can make filling in input masks easier when a new record is
created.

The default value can be set via the `BuildDataDefinitionEvent <https://github.com/contao-community-alliance/dc-general/blob/efe5e2de934946e1d51df56797b18d74b1683d12/src/Factory/Event/BuildDataDefinitionEvent.php>`_
of the DCG — the following is an example event listener to pre-fill the ``name``
attribute of the model ``mm_employees`` with "Moin".

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/SetDefaultValueListener.php
   namespace App\EventListener;

   use ContaoCommunityAlliance\DcGeneral\DataDefinition\Palette\PaletteInterface;
   use ContaoCommunityAlliance\DcGeneral\Factory\Event\BuildDataDefinitionEvent;

   class SetDefaultValueListener
   {
       public function __invoke(BuildDataDefinitionEvent $event): void
       {
           // Get container.
           $container = $event->getContainer();
           // Check right table present.
           if ('mm_employees' !== $container->getName()) {
               return;
           }
           // Set default value.
           $container->getPropertiesDefinition()->getProperty('name')->setDefaultValue('Moin');
       }
   }

.. code-block:: yml
   :linenos:

   services:
   # src/Resources/config/services.yml
     App\EventListener\SetDefaultValueListener:
       public: true
       tags:
         - { name: kernel.event_listener, event: dc-general.factory.build-data-definition }


Defaults with Legacy Code
--------------------------

.. note:: Defaults using legacy code should no longer be used. From MM 2.3, for
          correct label output for the field, an additional entry with an empty
          string must be created — e.g. |br|
          ``$GLOBALS['TL_DCA']['<MM-Table-Name>']['fields']['<Field-Column-Name>']['label'] = '';``  |br|
          otherwise "LABEL NOT SET: <column>" will be shown instead of the label.

MetaModels input fields are (almost) identical to fields from Contao core or
standard extensions created with a DCA array. Differences arise in part from
the dynamic generation of fields in MetaModels via DC-General.

Defaults for fields can be achieved by extending the DCA array with the "default"
key — `see the Contao documentation <https://docs.contao.org/dev/reference/dca/fields/>`_.

To add a default, the internal name of the MetaModel and the column name of the
attribute must be known. These can be added as an array entry using the general
form:

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/<MM-Table-Name>.php
   $GLOBALS['TL_DCA']['<MM-Table-Name>']['fields']['<Field-Column-Name>']['default'] = <Value>;

For the e-mail field ([text]) from :ref:`mm_first_index`, the default could look
like this:

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/mm_employeelist.php
   $GLOBALS['TL_DCA']['mm_employeelist']['fields']['email']['default'] = '@mmtest.com';

For the individual attribute types there are specific expectations about the
format of the values:

* **Text**: text in quotes, e.g. '@mmtest.com' |br|
  ``...['default'] = '@mmtest.com';``
* **Timestamp**: integer for the timestamp, e.g. 1463657005 or PHP function time() |br|
  ``...['default'] = 1463657005;`` or |br|
  ``...['default'] = time();``
* **Single select [Select]**: integer of the value ID in quotes |br|
  ``...['default'] = '2';``
* **Multi-select [Tags]**: array with the alias values from the configured alias column |br|
  ``...['default'] = ['purchasing', 'marketing'];``
* **Checkbox**: true |br|
  ``...['default'] = true;``

As can be seen with the "Timestamp" attribute, dynamic defaults are also
possible. It would also be possible to access existing values from MetaModels
and output them — optionally with a calculation — as the default. The API
methods (:ref:`ref_api_interf_mm`) are available for accessing MetaModels.

.. |br| raw:: html

   <br />
