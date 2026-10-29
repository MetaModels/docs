.. _component_attribute_tags:

|svg_attr_tags_22| |img_tags| Multi-Select [tags]
=================================================

The "Multi-select [tags]" attribute creates an :ref:`m:n relation <component_relations_standard-relation-mton>` to
another table — either a MetaModels table or any Contao table
(e.g. ``tl_member``, ``tl_page``). The link is stored in a dedicated relation table
(``tl_metamodel_tag_relation``), so **no own column** is created in the MetaModel table
for this attribute. Typical use cases:

* Assigning multiple tags to a product
* Linking an article with multiple categories or authors
* Selecting multiple regions, languages, or target audiences

The selection in the backend can be displayed as a checkbox list, checkbox wizard,
tree picker, or searchable dropdown.

.. note:: For relations between two multilingual MetaModels, the monolingual variant
   "Multi-select" should be used — MetaModels automatically recognizes the language
   and switches accordingly.

.. seealso:: For special cases with their own language column in the referenced table,
   :ref:`component_attribute_translatedtags` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_tags


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Database table
     - The table from which the selection values are obtained. MetaModels tables as well
       as all Contao database tables are available.
   * - Table column for label/name
     - The column of the source table whose content is displayed as the label in the
       selection list.
   * - Multi-select ID
     - The column that serves as the unique identifier (stored value). Default: ``id``.
   * - Multi-select alias
     - The column used as a readable identifier in filter widgets.
   * - Multi-select sorting
     - Column by which the selection list is sorted.
   * - Sort direction
     - Ascending (A → Z) or descending (Z → A).
   * - SQL (WHERE condition)
     - Optional SQL WHERE condition to restrict the selection list. The alias ``t``
       represents the source table (e.g. ``t.published = '1'``). The condition does not
       filter when the widget type "Popup picker" is selected.
   * - Filter
     - Selection of a MetaModels filter set for dynamic restriction of options.
   * - Filter parameters
     - Default values for the parameters of the selected filter set.


Settings in Render Settings
-----------------------------

The following options are available in the attribute list of a render setting:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the linked values.
   * - CSS class
     - Optional CSS class added to the output element.
   * - Additional attributes (since MM 2.5)
     - Only for a MetaModel as source table: attributes of the target MetaModel that are loaded in addition to the
       base attributes (value, alias, ID and selection sorting) and provided in the values of the tags
       (nodes ``raw`` and ``text``). See below.

**Additional attributes (since MM 2.5)**

From the selected records of the target MetaModel, the **base attributes** are always loaded - the columns
selected in the attribute settings as value column, alias column, ID column and selection sorting. All other
attributes of the target MetaModel can be ticked per render setting so that they are available as well, e.g.
for output in the detail view. Because the selection is made in the render setting, list and detail page can
load different attributes.

* If **no selection** has been saved yet (for existing render settings and for those created in bulk),
  **all** attributes are loaded as before; in the backend, all are then ticked.
* If **no** attributes are ticked and this is saved, only the base attributes are loaded. This is the
  fastest option; a template may then only access these.
* With a Contao table as source table, the option does not exist; the complete row is always loaded.


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

The multi-select attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Multi-select
     - Filters by one or more selected values from the source table; e.g. all products
       with a specific tag.
   * - Filter on attribute of model with a relation
     - Filters MetaModel items based on an attribute value of the linked MetaModel;
       e.g. all products whose tags have a specific characteristic.


Special Functions
-----------------

**Database storage**

The relations are **not** stored in the MetaModel table, but in the dedicated junction
table ``tl_metamodel_tag_relation`` with the columns ``att_id`` (attribute ID), ``item_id``
(item ID), ``value_id`` (ID of the linked record), and ``value_sorting`` (sort order).
This eliminates a column in the MM table and no database migration is needed.

**SQL WHERE condition**

In the WHERE condition, the alias ``t`` represents the source table. Example for filtering
on published entries:

.. code-block:: sql

   t.published = '1'


.. |svg_attr_tags_22| image:: /_img/icons_svg/tags.svg
   :width: 22px
.. |img_tags| image:: /_img/icons/tags.png
.. |br| raw:: html

   <br />
