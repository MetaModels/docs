.. _component_attribute_tablemulti:

Table Multi (MCW)
=================

The "Table multi (MCW)" attribute is an extended variant of the
:ref:`table text attribute <component_attribute_tabletext>`. Instead of plain text inputs,
a custom widget type can be configured for each table cell — e.g. select, radio buttons,
checkboxes, date pickers, or text fields. The data is stored in a dedicated value table,
so **no own column** is created in the MetaModel table for this attribute.

Typical use cases:

* Technical specifications with mixed input types (text + selection + date)
* Opening hours with weekday selection and time fields
* Configurable feature lists with validated input

.. warning:: The column structure (number and type of columns) is **not** configured in
   the MetaModels backend, but in a PHP configuration file (``contao/config/config.php``).
   This requires developer knowledge.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedtablemulti` is available.

.. seealso:: This attribute is supported by the :ref:`File-Usage integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_tablemulti


Settings when Creating the Attribute
--------------------------------------

The attribute has no specific settings in the MetaModels backend when creating.
Only the general attribute settings are used:

* Name, column name, description
* Override variants

The actual configuration of the table structure is done in the PHP configuration file
via ``$GLOBALS['TL_CONFIG']['metamodelsattribute_multi']``:

.. code-block:: php

   $GLOBALS['TL_CONFIG']['metamodelsattribute_multi']['mm_example']['my_field'] = [
       'minCount'     => 0,
       'maxCount'     => 0,
       'columnFields' => [
           'col_name' => [
               'label'     => 'Name',
               'inputType' => 'text',
               'eval'      => ['style' => 'width:200px'],
           ],
           'col_type' => [
               'label'     => 'Type',
               'inputType' => 'select',
               'options'   => ['a' => 'Option A', 'b' => 'Option B'],
               'eval'      => ['style' => 'width:150px'],
           ],
       ],
   ];

Here ``mm_example`` represents the table name of the MetaModel and ``my_field`` the
column name of the attribute.


Settings in Render Settings
-----------------------------

The attribute has its own render setting:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Hide table header
     - Hides the column headings in the frontend output.
   * - Template
     - Selection of a custom template for the output.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the attribute is added to an input form, the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form.
   * - Template for backend
     - Selection of a custom widget template for the backend form.


Filter Rules
------------

The table multi attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Single select / Multi-select
     - Filters by existing cell values (distinct values from the value table).


Special Functions
-----------------

**Database storage**

The table values are **not** stored in the MetaModel table, but in the dedicated value
table ``tl_metamodel_tablemulti`` with the columns ``att_id``, ``item_id``, ``row`` (row
index), ``col`` (column identifier), and ``value``. This means no column is created in
the MM table and no database migration is needed.

**Widget types per column**

In contrast to the table text attribute, the individual columns can be assigned any Contao
widget type: ``text``, ``select``, ``checkbox``, ``radio``, ``datePicker``, ``colorPicker``,
etc.

**Storage format**

Binary UUIDs are converted to readable UUIDs before storage. Array values are stored
serialized and automatically deserialized when reading.


.. |br| raw:: html

   <br />
