.. _component_filter_range:

|img_filter_range| Value from/to for two fields
================================================

The "Value from/to for two fields" filter rule (package ``filter_range``) filters items
based on a value range defined by two separate attributes. The first attribute represents
the start value ("from"), the second attribute the end value ("to"). An item is included
in the result when a given search value lies within the range defined by both attribute
values.

Typical use cases: Validity periods ("Valid from" / "Valid until"), price ranges with
separate fields for minimum and maximum price, or event periods.

.. seealso:: For comparing a single attribute against a range entered by the visitor,
   the filter rule :ref:`component_filter_fromto` is available. |br|
   For two date fields, the filter rule :ref:`component_filter_range-date` is available.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_range


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Value from/to for two fields".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute (from)
     - The first attribute that contains the start value of the range.
   * - Attribute 2 (to)
     - The second attribute that contains the end value of the range.


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
     - Placeholder text in the input fields.
   * - Filter range type
     - Defines how the search range is compared with the attribute values:

       * **s1** — Search value lies completely within the attribute range
       * **s2** — Search value overlaps with the attribute range
       * **s3** — Search value starts within the attribute range
       * **s4** — Search value ends within the attribute range (default)
       * **s5** — Search value completely contains the attribute range
   * - Greater-than-or-equal (≥)
     - The "from" search value is compared inclusively (``>=``).
   * - Less-than-or-equal (≤)
     - The "to" search value is compared inclusively (``<=``).
   * - Show from field
     - Enables the display of the "from" input field in the widget.
   * - Show to field
     - Enables the display of the "to" input field in the widget.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Value from/to for two fields" filter rule is suitable for attributes with numeric
values:

* :ref:`Numeric <component_attribute_numeric>`
* :ref:`Decimal <component_attribute_decimal>`


.. |img_filter_range| image:: /_img/icons/filter_range.png

.. |br| raw:: html

   <br />
