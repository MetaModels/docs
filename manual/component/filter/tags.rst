.. _component_filter_tags:

|img_filter_tags| Multi-Select
==================================

The "Multi-Select" filter rule (package ``filter_tags``) outputs a frontend widget
through which visitors can select multiple values simultaneously from a list. Items
are filtered by the selected values of an attribute. This filter rule is typically
combined with the :ref:`Multi-Select (tags) <component_attribute_tags>` attribute type
to make m:n relations filterable in the frontend.

Alternatively, the template ``mm_filteritem_linklist.html5`` (link list) is available.
The "OR condition" option can be used to configure whether items must match all or
only one of the selected values.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_tags


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Multi-Select".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The tags attribute by whose values items should be filtered.
   * - Label attribute
     - Optional attribute whose value is used as the display text for the options
       in the widget.


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
   * - Label
     - Label of the filter widget.
   * - Hide label at filter widget
     - Suppresses the output of the label.
   * - Template
     - Template for the widget output. Default: ``mm_filteritem_default``;
       alternatively: ``mm_filteritem_linklist``.
   * - Sorting
     - Sorting of the selection options in the widget (ascending or descending).
   * - Allow empty selection
     - Adds an empty option ("All").
   * - Show "Select all" button
     - Adds a button that allows all options to be selected at once.
   * - OR condition
     - If this option is active, an item is displayed if it contains at least one of
       the selected values (OR). If the option is inactive, all selected values must
       apply (AND).
   * - Only assigned values
     - Shows only values in the widget that are actually assigned in at least one item.
   * - Only remaining values
     - Shows only values for which results still exist after applying the other active
       filters.
   * - Ignore this filter for remaining values
     - This filter does not return its own options as a restriction when calculating
       remaining values.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Multi-Select" filter rule is particularly suitable for:

* :ref:`Multi-Select [tags] <component_attribute_tags>`
* :ref:`Translated Multi-Select [tags] <component_attribute_translatedtags>`


Special Functions
----------------

**Link lists**

The template ``mm_filteritem_linklist`` outputs the filter options as links. Each click
on a link passes the value as a URL parameter without requiring a form. This is
particularly suitable for SEO-friendly filter navigation.

**AND vs. OR condition**

With the OR option disabled (default), all selected tags must be present in an item
(AND filtering). With the OR option enabled, one matching tag is sufficient (OR filtering).
This significantly affects the result set with multiple selections.


.. |img_filter_tags| image:: /_img/icons/filter_tags.png

.. |br| raw:: html

   <br />
