.. _component_attribute_translatedtabletext:

|svg_attr_translatedtabletext_22| |img_translatedtabletext| Translated Table Text
=================================================================================

The "Translated Table Text" attribute is the multilingual variant of the
:ref:`Table Text <component_attribute_tabletext>` attribute. It enables the input of
text data in a tabular structure with configured columns — per language with its own
column labels and data values. The data is stored in a dedicated translation table,
so **no own column** is created in the MetaModel table for this attribute.

Typical use cases:

* Multilingual specification tables (e.g. "property + value" in DE and EN)
* Translated opening hours with language-dependent weekday labels
* Language-specific price lists or feature tables

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_tabletext`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedtabletext


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings, the attribute offers the following
specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Number of columns
     - Defines the number of table columns. This value determines how many columns
       can be configured in the multilingual column wizard.
   * - Column settings (translated)
     - Multi-column wizard for defining column labels per language.
       For each language and each column, the following can be specified:

       * **Language** — Language code (e.g. ``de``, ``en``)
       * **Label** — Name of the column in this language
       * **Width** — Width of the column in the input form (e.g. ``200px``)

       The column labels are output language-specifically in the backend and in the
       frontend template.
   * - Minimum number of rows
     - Minimum number of data rows displayed in the input form (0 = no minimum).
   * - Maximum number of rows
     - Maximum number of data rows that can be entered (0 = no limit).
   * - Disable sorting
     - Hides the up/down buttons for manual row sorting in the input form.


Settings in Render Settings
-----------------------------

The attribute has its own render setting:

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
   * - Template for frontend
     - Selection of a custom widget template for frontend editing
       (only available if the "Frontend Editing" extension is installed).


Filter Rules
------------

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Levenshtein-based search
     - Similarity search with typo tolerance across all cell values of the active
       language; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search across all cell values; requires the package
       ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Database storage**

The table values are stored per language in ``tl_metamodel_translatedtabletext``
(fields: ``att_id``, ``item_id``, ``langcode``, ``row`` (row index), ``col``
(column index), ``value``). No column is created in the MetaModel table.

**Language-dependent column labels**

Unlike the monolingual table text attribute, the column headings can be defined
differently per language. In the frontend template, the column labels of the active
language are output.

**Fallback language**

If a value is missing for a language, MetaModels falls back to the fallback language.

**Column structure in the template**

In the frontend template, the values are available as a nested array:
``$arrData['raw']`` contains rows (row) with columns (col_0, col_1, …), where the
column names are automatically generated from the number of columns. The translated
column labels are available as ``$arrData['cols']``.


.. |svg_attr_translatedtabletext_22| image:: /_img/icons_svg/translatedtabletext.svg
   :width: 22px
.. |img_translatedtabletext| image:: /_img/icons/translatedtabletext.png
.. |br| raw:: html

   <br />
