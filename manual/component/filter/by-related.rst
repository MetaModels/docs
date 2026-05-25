.. _component_filter_by-related:

|img_filter_default| Filter-by-related
=======================================

The "Filter-by-related" filter rule (package ``filter_by_related``, from MM 2.4) allows
items to be filtered based on properties of a related (relational) MetaModel. The
relation between the main MetaModel and the related MetaModel can be built either via a
child table (pid relation) or via a single-select attribute (select relation).

For example, in a "Manufacturer → Products" structure, items can be filtered by
properties of the manufacturer (e.g. "Show all products from manufacturers in Germany").

Optionally, a frontend widget can be rendered that allows visitors to select a value
themselves.

With the "Static parameter" option, a pre-selectable value can be set in the MetaModels
list and filter CE/FE module as an overridable filter setting — see
:ref:`rst_cookbook_filter_filter-with-static-parameter`.

.. seealso:: Detailed documentation on Filter-by-related:
   :ref:`rst_extended_filter_by_related`


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_by_related


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Filter-by-related".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Related MetaModel
     - The MetaModel through which the relation is built (the "parent MetaModel").
   * - Relation column
     - Defines how the relation to the main MetaModel is built:

       * **PID** — Via the child table relation (pid field)
       * **Meta attribute** — Via a meta attribute
       * **Attribute** — Via a single-select attribute in the main MetaModel
   * - Related attribute
     - The attribute in the related MetaModel by whose value items are filtered.
   * - Label attribute
     - The attribute in the related MetaModel whose value is used as display text in
       the frontend widget.


Settings for the Frontend Widget
--------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - URL parameter
     - The key of the URL parameter for passing the filter value. Without input,
       the column name of the attribute is used. With ``auto_item``, only the value —
       without key — is embedded in the URL.
   * - URL type for the parameter
     - Defines whether the parameter is passed as a slug (friendly URL) or as a
       GET parameter (from MM 2.4) — :ref:`see SEO <rst_cookbook_tips_seo_filter-url>`
   * - Static parameter
     - If this option is active, the filter value can be pre-set with an overridable
       selection from a dropdown in the content element/module.
   * - Provide frontend widget
     - Renders a filter widget in the frontend.
   * - Widget type
     - Display type of the frontend widget:

       * **Select** — Dropdown list (default)
       * **Text** — Text input field
       * **Radio** — Radio buttons
       * **Checkbox** — Checkboxes
   * - Allow empty value
     - If this option is active and the URL parameter is empty, no filter is applied.
   * - Label
     - Label of the filter widget.
   * - Hide label at filter widget
     - Suppresses the output of the label.
   * - Template
     - Template for the widget output.
   * - Default
     - Pre-selected value in the frontend widget.
   * - Allow empty selection
     - Adds an empty option ("All").
   * - Only assigned values
     - Shows only values that actually exist in a relation.
   * - Only remaining values
     - Shows only values for which results still exist after applying other filters.
   * - Ignore this filter for remaining values
     - This filter does not return its own options as a restriction when calculating
       remaining values.


Matching Attributes
------------------

The "Filter-by-related" filter rule does not work with an attribute of the main
MetaModel, but with attributes of the related MetaModel. Any attribute types can be
filtered there.

The relation to the main MetaModel can be built via the following attribute types:

* :ref:`Single select [select] <component_attribute_select>`
* Child table relation (pid/ptable)


.. |img_filter_default| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
