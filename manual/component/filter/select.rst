.. _component_filter_select:

|svg_filt_select_22| |img_filter_select| Single Select
======================================================

The "Single Select" filter rule (package ``filter_select``) outputs a frontend widget
through which visitors can select a single value from a selection list. Items are filtered
by the selected value of an attribute. This filter rule is typically combined with the
:ref:`Single Select (select) <component_attribute_select>` attribute type to make a
1:n relation filterable in the frontend.

Alternatively, the templates ``mm_filteritem_radiobutton.html5`` (radio buttons) and
``mm_filteritem_linklist.html5`` (link list) are available.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_select


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Single Select".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The attribute by whose value items should be filtered.
   * - Attribute for label text
     - Optional second attribute whose value is used as the display text for the options
       in the widget (e.g. a name or title attribute) — from MM 2.4.12.

       This setting only appears if the filtered attribute does not itself provide the
       display text — i.e. for attributes without a relation. It is omitted for Single
       Select (MetaModel), Tags and Translated Tags, since for those the display text is
       already determined via the value column of the attribute; the alias column
       provides the key for the URL.


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
   * - Use the label as empty option
     - The label is used as the first empty option (instead of an empty row).
   * - Template
     - Template for the widget output. Default: ``mm_filteritem_default``;
       alternatively: ``mm_filteritem_radiobutton`` or ``mm_filteritem_linklist``.
   * - Sorting
     - Sorting of the selection options in the widget (ascending or descending).
   * - Default
     - Pre-selected value in the frontend widget.
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
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Single Select" filter rule is particularly suitable for:

* :ref:`Single Select [select] <component_attribute_select>`
* :ref:`Translated Single Select [select] <component_attribute_translatedselect>`
* :ref:`Text <component_attribute_text>`
* :ref:`Alias <component_attribute_alias>`
* :ref:`Combined Values <component_attribute_combinedvalues>`
* :ref:`Token <component_attribute_token>`


Special Functions
----------------

**Radio buttons and link lists**

Via the template selection, the widget can be output as a radio button list
(``mm_filteritem_radiobutton``) or as a link list (``mm_filteritem_linklist``).
Link lists are particularly suitable for SEO-optimized navigation without form
submission.


.. |svg_filt_select_22| image:: /_img/icons_svg/filter_select.svg
   :width: 22px
.. |img_filter_select| image:: /_img/icons/filter_select.png

.. |br| raw:: html

   <br />
