.. _component_attribute_translatedtags:

Translated Multi-Select [tags]
==================================

The "Translated Multi-Select" attribute is an extension of the
:ref:`Multi-Select <component_attribute_tags>` attribute. It is used when the
referenced source table has its own language column — for example an external table
with a ``language`` field. The relations are stored in the relation table
``tl_metamodel_tag_relation`` just like the monolingual attribute, so no own column
is created in the MetaModel table.

Typical use cases:

* Multiple selection from an external language-dependent table (e.g. tags from a
  table with a language column)
* Linking with Contao's own multilingual tables
* Scenarios where the source table itself provides language-dependent entries and
  MetaModels should filter based on the active language

.. note:: For relations between two multilingual MetaModels tables, the monolingual
   multi-select attribute is usually sufficient — MetaModels automatically detects
   the language and switches accordingly.

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_tags`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedtags


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings and the options of the monolingual
multi-select attribute, the translated attribute offers the following additional options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Database table
     - The table from which the selection values are obtained.
   * - Table column for label/name
     - The column of the source table whose content is displayed as the label.
   * - Multi-select ID
     - The column that serves as the unique identifier (default: ``id``).
   * - Multi-select alias
     - The column used as a readable identifier in filter widgets.
   * - Multi-select sorting
     - Column by which the selection list is sorted.
   * - Sort direction
     - Ascending (A → Z) or descending (Z → A).
   * - SQL (WHERE condition)
     - Optional SQL WHERE condition to restrict the selection list. The alias ``t``
       represents the source table.
   * - Filter
     - Selection of a MetaModels filter set for dynamic restriction of options.
   * - Filter parameters
     - Default values for the parameters of the selected filter set.
   * - Language column
     - Column in the source table that contains the language code of the entry
       (e.g. ``language``). MetaModels automatically filters the selection list to
       the active language.
   * - Source table (translation)
     - Alternative source table for language-independent master data, from which
       fallback values are obtained when a translation is missing.
   * - Sorting (source table)
     - Sort column in the source table for the translated view.


Settings in Render Settings
-----------------------------

The attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the linked values.
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
   * - Select display type
     - Display type of the multiple selection:

       * **Checkbox menu** — Classic checkbox list
       * **Checkbox wizard** — Checkbox list with up/down sorting
       * **Popup picker** — Selection via a tree picker (for hierarchical source tables
         such as ``tl_page`` or ``tl_files``)
       * **Tag list** — Searchable dropdown with chosen.js
   * - Lowest level (tree)
     - For tree picker: records below this level are not selectable.
   * - Highest level (tree)
     - For tree picker: records above this level are not selectable.

**Functions**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Required field
     - Makes the field a required field.

**Overview (backend filter and search)**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Filterable
     - The attribute is available in the backend as a filter criterion.
   * - Searchable
     - The attribute is available in the backend as a search field.


Filter Rules
------------

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Multi-select
     - Filters by one or more selected values from the source table; the filter list
       is automatically restricted to the active language.
   * - Filter on attribute of model with a relation
     - Filters MetaModel items based on an attribute value of the linked MetaModel.


Special Functions
-----------------

**Database storage**

The relations are stored in the junction table ``tl_metamodel_tag_relation``
(columns: ``att_id``, ``item_id``, ``value_id``, ``value_sorting``).
No own column in the MetaModel table. The translation only affects the display values
of the selection list, not the stored relations.

**Language-dependent selection list**

MetaModels automatically filters the selection list in the backend based on the
configured language column to the active backend language.

**SQL WHERE condition**

In the WHERE condition, the alias ``t`` represents the source table. Example for
filtering on published entries:

.. code-block:: sql

   t.published = '1'


.. |br| raw:: html

   <br />
