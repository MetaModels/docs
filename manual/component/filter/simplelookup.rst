.. _component_filter_simplelookup:

|svg_filt_simplelookup_22| |img_filter_default| Simple Lookup
=============================================================

The "Simple Lookup" filter rule filters items based on a single attribute value.
The filter value can either be passed dynamically via a URL parameter (GET/slug) or
configured as a fixed value in the content element or module using the "Static parameter"
option. This filter rule is suitable for simple selections such as "Show all items in
category X" or "Filter by a specific tag".

This filter rule is typically used to :ref:`display a record as a detail page
<mm_first_contentelements_detailpage>`.

Optionally, a frontend widget can be output that allows visitors to select a value
themselves — in this case, the filter rule works like :ref:`component_filter_select`.

With the "Static parameter" option, an overridable selection can be set as a filter
setting in the MetaModels list and filter content element/module — see
:ref:`rst_cookbook_filter_filter-with-static-parameter`.


Installation
------------

This filter rule is part of ``metamodels/core`` and is available without additional
packages after the basic MetaModels installation.


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Simple Lookup".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The attribute by whose value items should be filtered.
   * - Attribute for label text
     - Optional second attribute whose value is used as the display text in the frontend
       widget (e.g. a title attribute instead of the internal alias) — from MM 2.4.12.

       This setting only appears if the filtered attribute does not itself provide the
       display text — i.e. for attributes without a relation. It is omitted for Single
       Select (MetaModel), Tags and Translated Tags, since for those the display text is
       already determined via the value column of the attribute; the alias column
       provides the key for the URL.
   * - Search all languages
     - For multilingual MetaModels, this setting controls whether all languages or
       only the active language should be used for the comparison.


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
     - If this option is active, the filter value can be preset from a selection list
       in the content element/module and can be overridden.
   * - Provide frontend widget
     - Outputs a filter widget in the frontend through which visitors can select a value.
   * - Allow empty value
     - If the option is active and the URL parameter is empty, the filter rule behaves
       as if it were not defined (no filter active).
   * - Label
     - Label of the filter widget in the frontend. If no label is specified, the
       attribute name is used.
   * - Hide label at filter widget
     - Suppresses the output of the label in the frontend.
   * - Use the label as empty option
     - The label is displayed as the first empty option in the selection list.
   * - Template
     - Template for the output of the filter widget. Default: ``mm_filteritem_default``.
   * - Default
     - Pre-selected value in the frontend widget.
   * - Sorting
     - Sorting of the selection values in the widget (ascending or descending).
   * - Allow empty selection
     - Adds an empty option ("All") to the selection list.
   * - Only assigned values
     - Shows only values in the widget that are actually assigned in at least one
       item of the MetaModel.
   * - Only remaining values
     - Shows only values for which results still exist after applying the other active
       filters (dynamic filter options).
   * - Ignore this filter for remaining values
     - This filter does not return its own options as a restriction when calculating
       remaining values.
   * - CSS ID/class
     - Sets a CSS ID or class on the filter widget element.


Matching Attributes
------------------

The "Simple Lookup" filter rule supports attributes that store a single comparable value:

* Text, Alias, Combined Values, Token
* Numeric, Decimal
* Checkbox
* Single select (select)
* Multi-select (tags)


Special Functions
----------------

**Multilingual attributes**

For translated attributes (e.g. "Translated Text"), the "Search all languages" option
can be used to configure whether only the active language or all language variants
should be used for the comparison.

**Static parameter in the content element**

If "Static parameter" is enabled, a selection list appears in the content element/module
where a fixed filter value can be set. This setting is suitable for pages that should
always display a specific category without requiring a URL parameter — see
:ref:`rst_cookbook_filter_filter-with-static-parameter`.


.. |svg_filt_simplelookup_22| image:: /_img/icons_svg/filter_simplelookup.svg
   :width: 22px
.. |img_filter_default| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
