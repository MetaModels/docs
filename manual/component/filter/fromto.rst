.. _component_filter_fromto:

|svg_filt_fromto_22| |img_filter_fromto| Value from/to for one field
====================================================================

The "Value from/to for one field" filter rule (package ``filter_fromto``) filters items
based on a value range for a single numeric or text-based attribute. Visitors can enter a
"from" value, a "to" value, or both in the frontend. The filter rule returns all items
whose attribute value falls within the specified range.

Typical use cases: Price filters (price from X to Y), size or age specifications, or
general numeric range filters.

.. seealso:: For comparisons across two separate attributes (e.g. "Valid from" and
   "Valid until"), the filter rule :ref:`component_filter_range` is available. |br|
   For date values, the filter rule :ref:`component_filter_fromto-date` is available.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_fromto


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Value from/to for one field".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The attribute by whose value items should be filtered (e.g. price, age).


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
     - Template for the widget output. Default: ``mm_filteritem_default``.
   * - Placeholder
     - Placeholder text displayed in the input fields while they are empty.
   * - Greater-than-or-equal (≥)
     - If this option is active, the "from" value is treated as inclusive (``>=``).
       Otherwise exclusive (``>``).
   * - Less-than-or-equal (≤)
     - If this option is active, the "to" value is treated as inclusive (``<=``).
       Otherwise exclusive (``<``).
   * - Show from field
     - Enables the display of the "from" input field in the widget.
   * - Show to field
     - Enables the display of the "to" input field in the widget.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Value from/to for one field" filter rule is suitable for attributes with numeric
or comparable values:

* :ref:`Numeric <component_attribute_numeric>`
* :ref:`Decimal <component_attribute_decimal>`


.. |svg_filt_fromto_22| image:: /_img/icons_svg/filter_fromto.svg
   :width: 22px
.. |img_filter_fromto| image:: /_img/icons/filter_fromto.png

.. |br| raw:: html

   <br />
