.. _rst_extended_attribute_mcw:

Multi-Column Wizard Attribute
==============================

The Multi-Column Wizard (MCW) allows defining a variable input table with different
input types such as text, checkboxes, and select in the columns — more about the MCW
possibilities on `Github <https://github.com/MetaModels/attribute_tablemulti>`_ or in
the `Contao Wiki <http://de.contaowiki.org/MultiColumnWizard>`_.

With the extension `attribute_tablemulti <https://github.com/MetaModels/attribute_tablemulti>`_,
the MCW can be used as an attribute in MetaModels. Note, however, that the MCW cannot
be fully configured via the backend but requires a corresponding DCA configuration
file. Additionally, it is not possible to search or filter by the stored values of the
MCW attribute. The MCW values are stored in the database as a serialized array.

In addition to the mentioned version, there is also the multilingual counterpart as
the extension `attribute_translatedtablemulti <https://github.com/MetaModels/attribute_translatedtablemulti>`_.

The MCW attribute can be used, for example, to allow a variable number of inputs with
different input types in an input mask. A simple example would be specifying multiple
links with a text field for the URL, a text field for the link text, and a checkbox for
the link target.

Installation and use consists of:

* Installing the attribute via Composer from Github or via the Contao Manager
* Customizing the DCA configuration file


Customizing the DCA Configuration File
---------------------------------------

The DCA configuration file ``<mm-table-name>.php`` or ``config.php`` must be placed at
an appropriate location in the Contao installation or an existing file must be
supplemented with the information. This can be done, for example, as:

* contao/dca/<mm-table-name>.php
* contao/config/config.php

or in a custom bundle in:

* src/AppBundle/Resources/contao/dca/<mm-table-name>.php
* src/AppBundle/Resources/contao/config/config.php

This file must be customized with an editor according to your own MetaModel parameters
and desired fields — see `Contao Wiki <http://de.contaowiki.org/MultiColumnWizard>`_.

A configuration for the MetaModel "mm_my_table" with the MCW attribute "my_mcw" could
look as follows:

.. code-block:: php
   :linenos:

   <?php
   // /contao/dca/mm_my_table.php

   $GLOBALS['TL_CONFIG']['metamodelsattribute_multi']['mm_my_table']['my_mcw'] = array(
      'minCount'     => 2,
      'maxCount'     => 4,
      'tl_class'     => 'clr w50',
      'columnFields' => array(
         'ts_client_os'     => array(
            'label'     => 'My Options',
            'exclude'   => true,
            'inputType' => 'select',
            'options'   => array(
               'option1' => 'Option 1',
               'option2' => 'Option 2',
            ),
            'eval'      => array('style' => 'width:250px', 'includeBlankOption' => true, 'chosen' => true)
         ),
         'ts_client_mobile' => array(
            'label'     => 'My Checkbox',
            'exclude'   => true,
            'inputType' => 'checkbox',
            'eval'      => array('style' => 'width:40px')

         ),
         'ts_extension'     => array(
            'label'     => 'The Text Field',
            'inputType' => 'text',
            'eval'      => array('mandatory' => true, 'style' => 'width:115px')
         ),
      ),

   );

Note: The labels in "label" can also be included as a language array.

Clear the cache after making configuration changes!

View in the input mask:

|img_input_mask|

.. note:: From MM 2.4, the ``fileTree`` input type including multiple selection,
   gallery display, and sortability is also supported in both "MM MCW attributes".

The configuration for image selection including custom sorting looks as follows:

.. code-block:: php
   :linenos:

   <?php
   // /contao/dca/mm_my_table.php

   $GLOBALS['TL_CONFIG']['metamodelsattribute_multi']['mm_my_table']['my_mcw'] = [
       'tl_class'     => 'clr',
       'minCount'     => 0,
       'columnFields' => [
           'col_title'     => [
               'label'     => 'Title',
               'exclude'   => true,
               'inputType' => 'text',
               'eval'      => [
                   'style'         => 'width:100%',
                   'tl_class'      => 'my_class',
                   'wrapper_style' => 'width:50%',
               ]
           ],
           'col_images' => [
               'label'     => 'File selection',
               'exclude'   => true,
               'inputType' => 'fileTree',
               'eval'      => [
                   'filesOnly'     => true,
                   'multiple'      => true,
                   'fieldType'     => 'checkbox',
                   'isGallery'     => true,
                   'orderField'    => 'col_images',
                   'extensions'    => \Contao\Config::get('validImageTypes'),
                   'isSortable'    => true,
                   'style'         => 'width:100%',
                   'tl_class'      => 'my_class',
                   'wrapper_style' => 'width:50%',
               ]
           ],
       ],
   ];

.. |img_input_mask| image:: /_img/screenshots/extended/attribute_mcw/input_mask.jpg
