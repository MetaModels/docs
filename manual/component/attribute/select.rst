.. _component_attribute_select:

Single Select [select]
======================

The "Single select [select]" attribute creates a :ref:`1:n relation <component_relations_standard-relation-1ton>` to
another table — either a MetaModels table or any Contao table
(e.g. ``tl_member``, ``tl_page``). The ID of the selected record is stored in the database.
Typical use cases:

* Assigning a product to a category
* Linking an article with an author
* Selecting a Contao page as the target page

The selection in the backend can be displayed as a dropdown, radio button list, or tree picker.

.. note:: For relations between two multilingual MetaModels, the monolingual variant "Single select"
   should be used — MetaModels automatically recognizes the language and switches accordingly.

.. seealso:: For special cases with their own language column in the referenced table,
   :ref:`component_attribute_translatedselect` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_select


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Source table
     - The table from which the selection values are obtained. MetaModels tables as well
       as all Contao database tables are available.
   * - Value column
     - The column of the source table whose content is displayed as the label in the
       selection list.
   * - ID column
     - The column that serves as the unique identifier (stored value). Default: ``id``.
   * - Alias column
     - The column used as a readable identifier in filter widgets. If unsure, select the
       same column as the ID column.
   * - Selection sorting
     - Column by which the selection list is sorted.
   * - Sort direction
     - Ascending (A → Z) or descending (Z → A).
   * - SQL (WHERE condition)
     - Optional SQL WHERE condition to restrict the selection list. The alias ``sourceTable``
       represents the source table (e.g. ``sourceTable.published = '1'``). The condition
       does not filter when the widget type "Popup picker" is selected.
   * - Filter
     - Selection of a MetaModels filter set for dynamic restriction of options.
   * - Filter parameters
     - Default values for the parameters of the selected filter set.


Settings in Render Settings
-----------------------------

The single select attribute has no specific render settings. In the attribute list of a
render setting, the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the linked value.
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
   * - Display type
     - Display type of the selection:

       * **Select menu** — Classic dropdown menu
       * **Radio button list** — All options as radio buttons
       * **Popup picker** — Selection via a tree picker (for hierarchical source tables
         such as ``tl_page`` or ``tl_files``)
   * - Lowest level (tree)
     - For tree picker: records below this level are not selectable (0 = no restriction).
   * - Highest level (tree)
     - For tree picker: records above this level are not selectable (0 = no restriction).

**Functions**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Required field
     - Makes the field a required field.
   * - Show empty option
     - Displays an empty selection option so that no pre-selection is made.

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

The single select attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Single select
     - Filters by a selected value from the source table; e.g. all products of a
       specific category.
   * - Filter on attribute of model with a relation
     - Filters MetaModel items based on an attribute value of the linked MetaModel;
       e.g. all products whose category has a specific characteristic.


Special Functions
-----------------

**Database storage**

The ID of the selected record is stored as ``int(11) NULL`` in the MetaModel table.

**Source table types**

The following can be selected as the source table:

* **MetaModels tables** — with full access to all attributes
* **Non-translated Contao tables** — any table from the Contao database
* **SQL tables** — direct table selection via SQL alias

**SQL WHERE condition**

In the WHERE condition, the alias ``sourceTable`` represents the source table.
Example for filtering on published entries:

.. code-block:: sql

   sourceTable.published = '1'


.. |br| raw:: html

   <br />
