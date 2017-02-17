.. _rst_cookbook_panels_default-values:

Input mask: populate fields with pre-defined values
===================================================

The input fields in the input masks can be already filled in with pre-defined standard values. This can greatly facilitate data entry for the user, when creating new data records. 

The metamodels input fields can be (almost) used in the same way as the input fields of the Contao core or common Contao extensions which have been created with a DCA array.
There are some differences because MetaModels generates fields dynamically by the dc-general.

You can create pre-defined values with the key "default" added to the dc-array.
The dc-array can be amended either by an entry in the file "dcaconfig.php" in the folder "/system/config/" or if there is an own module folder in the file "config.php". 

Appropriate entries are already set up in the module `"Metamodels-Boilerplate" <https://github.com/MetaModels/boilerplate>`_
in the file "config.php".

To enter a pre-defined value, you need to know the (internal) name of the MetaModel and the column name of the attribute. This informations may be given with an array entry in the following general form:

.. code-block:: php
   :linenos:
   
   <?php
   $GLOBALS['TL_DCA']['<MM-Table-Name>']['fields']['<Field-Column-Name>']['default'] = <Value>;

E.g. for an email field ([text]) from :ref:`mm_first_index` the default value could be set up like this:

.. code-block:: php
   :linenos:
   
   <?php
   $GLOBALS['TL_DCA']['mm_employeelist']['fields']['email']['default'] = '@mmtest.com';


There are specifications for individual attribute types. Here is in which form the values are expected:


* **Text**: Text in inverted commas e.g. '@mmtest.com' |br|
  ``...['default'] = '@mmtest.com';``
* **Timestamp**: Integer for the timestamp e.g. 1463657005 or PHP function time() |br|
  ``...['default'] = 1463657005;`` or |br|
  ``...['default'] = time();``
* **Select**: Integer of the ID of the value in inverted commas |br|
  ``...['default'] = '2';``
* **Multiple selection**: Array with alias values from the selected alias column |br|
  ``...['default'] = array('purchase', 'marketing');``
* **Checkbox**: true |br|
  ``...['default'] = true;``


As you can see from the attribute "Timestamp", dynamic specifications are feasible. It would be possible to use existing values from MetaModels and to output them  - if necessary with a calculation - as default.
The methods of the API (:ref:`ref_api_interf_mm`) are available to you in order to access MetaModels.


.. |br| raw:: html

   <br />
