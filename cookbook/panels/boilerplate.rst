.. _rst_cookbook_panels_default-values:

Input screens: pre-defined values
=================================

You can fill the input fields automatically with pre-defined default values. This makes it easier for users to fill in the fields, when they have to add a new data record. 

You can treat the MetaModels input fields in (almost) the same way as the input fields of the Contao core or standard extensions, which have been created with a DCA array. Differences may be partly caused by the dynamic generation of the fields in MetaModels by the DC-general.

You can create your pre-defined values by adding them to the DC array with the key "default". You can amend the array either by an addition in the file "dcaconfig.php" in "/system/config/" or, if there is a custom module folder, in "config.php".

The respective entries are already preconfigured in "config.php" in the module `"Metamodels-Boilerplate" <https://github.com/MetaModels/boilerplate>`_ . 

In order to create a pre-defined value you need to know the (internal) name of the MetaModel and also the name of the column. Then you can add this details in an array in the following format:

.. code-block:: php
   :linenos:
   
   <?php
   $GLOBALS['TL_DCA']['<MM-Table-Name>']['fields']['<Field-Column-Name>']['default'] = <Value>;

For an email field ([text]) from :ref:`mm_first_index` a pre-defined value could look as follows:

.. code-block:: php
   :linenos:
   
   <?php
   $GLOBALS['TL_DCA']['mm_employeelist']['fields']['email']['default'] = '@mmtest.com';

There are specific requirements how the values are expected for each type of attribute :

* **Text**: Text in quotation marks e.g. '@mmtest.com' |br|
  ``...['default'] = '@mmtest.com';``
* **Timestamp**: Integer for the timestamp e.g. 1463657005 or PHP function time() |br|
  ``...['default'] = 1463657005;`` or |br|
  ``...['default'] = time();``
* **Select**: Integer of the ID of the value in quotation marks |br|
  ``...['default'] = '2';``
* **Multiple selection**: Array with alias values from pre-defined alias column |br|
  ``...['default'] = array('sales', 'marketing');``
* **Checkbox (Checkbox)**: true |br|
  ``...['default'] = true;``


As you can see with the attribute "Timestamp", dynamic defaults are feasible. It would be possible to fall back on existng MetaModels values and to output them as default  - with computation if necessary. The API methods (:ref:`ref_api_interf_mm`) are available to get access to MetaModels.


.. |br| raw:: html

   <br />
