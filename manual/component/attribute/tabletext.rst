.. _component_attribute_tabletext:

|svg_attr_tabletext_22| |img_tabletext| Table Text
==================================================

The "Table text" attribute enables the input of text data in a tabular structure with
configured columns and any number of rows. The data is stored in a dedicated value table,
so **no own column** is created in the MetaModel table for this attribute.
Typical use cases:

* Multiple URLs or phone numbers per record (e.g. "URL + label")
* Technical specifications with multiple rows (e.g. "property + value")
* Opening hours (e.g. "weekday + from + to")
* Price lists with multiple columns

The number and names of the columns are defined when creating the attribute. Any number
of rows can then be added in the input form.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedtabletext` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_tabletext


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the table text attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Column settings
     - Definition of the table columns. For each column the following are specified:

       * **Label** — Name of the column (appears as column heading in the input form
         and in the frontend template)
       * **Width** — Width of the column in the input form (e.g. ``200px``, ``50%``);
         has no effect on the frontend output.

       The number of rows in the column settings determines the number of columns in the table.
   * - Minimum number of rows
     - Minimum number of data rows displayed in the input form (0 = no minimum).
   * - Maximum number of rows
     - Maximum number of data rows that can be entered (0 = no limit).
   * - Disable sorting
     - Hides the up/down buttons for manual row sorting in the input form.


Settings in Render Settings
-----------------------------

The table text attribute has its own render setting:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Hide table header
     - Hides the column headings (labels) in the frontend output.
   * - Template
     - Selection of a custom template for the output. If no template is specified,
       the output is as an HTML table.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the table text attribute is added to an input form, the following options are available:

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
   * - Template for frontend
     - Selection of a custom widget template for frontend editing
       (only available if the "Frontend Editing" extension is installed).


Filter Rules
------------

The table text attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Levenshtein-based search
     - Similarity search with typo tolerance across all cell values; requires the package
       ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search across all cell values; requires the package ``filter_loupe``
       (from MM 2.4).


Special Functions
-----------------

**Database storage**

The table values are **not** stored in the MetaModel table, but in the dedicated value
table ``tl_metamodel_tabletext`` with the columns ``item_id``, ``att_id``, ``row`` (row
index), ``col`` (column index), and ``value``. This means no column is created in the MM
table and no database migration is needed.

**Column structure in the template**

In the frontend template, the values are available as a nested array:
``$arrData['raw']`` contains rows (row) with columns (col_0, col_1, …), where the column
names are automatically generated from the column settings.


.. |svg_attr_tabletext_22| image:: /_img/icons_svg/tabletext.svg
   :width: 22px
.. |img_tabletext| image:: /_img/icons/tabletext.png
.. |br| raw:: html

   <br />
