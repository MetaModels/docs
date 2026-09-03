.. _component_filter_parent:

|svg_filt_parent_22| |img_filter_default| Parent Filter
=======================================================

.. note:: This filter rule is no longer being developed — its successor is the filter rule :ref:`component_filter_by-related`,
   which covers this functionality as well.

The "Parent Filter" filter rule (package ``filter_parent``, from MM 2.3) allows filtering
items based on a parent-child relationship to another MetaModel. An item of the target
MetaModel is linked to an item of the "parent" MetaModel via an attribute. The filter
rule then filters for items that are connected to a specific parent element.

Optionally, a frontend widget can be rendered that allows visitors to select the parent
element themselves.

With the "Static parameter" option, a pre-selectable value can be set in the MetaModels
list and filter CE/FE module as an overridable filter setting — see
:ref:`rst_cookbook_filter_filter-with-static-parameter`.

Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_parent


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Parent Filter".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Parent MetaModel
     - The MetaModel that serves as the parent level (the "parent" MetaModel).
   * - Parent attribute
     - The attribute in the current MetaModel that establishes the relation to the
       parent element (e.g. a single-select attribute pointing to the parent MetaModel).


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
   * - Template
     - Template for the widget output.
   * - Default
     - Pre-selected parent record in the frontend widget.
   * - Allow empty selection
     - Adds an empty option ("All").
   * - Only assigned values
     - Shows only parent elements in the widget that are actually linked to at least
       one item.
   * - Only remaining values
     - Shows only parent elements for which results still exist after applying other
       filters.
   * - Ignore this filter for remaining values
     - This filter does not return its own options as a restriction when calculating
       remaining values.


Matching Attributes
------------------

The "Parent Filter" filter rule works with an attribute of the current MetaModel that
establishes the relation to the parent MetaModel:

* :ref:`Single select [select] <component_attribute_select>`


.. |svg_filt_parent_22| image:: /_img/icons_svg/filter_default.svg
   :width: 22px
.. |img_filter_default| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
