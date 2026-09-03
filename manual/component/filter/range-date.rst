.. _component_filter_range-date:

|svg_filt_range_date_22| |img_filter_range| Value from/to for two date fields
=============================================================================

The "Value from/to for two date fields" filter rule (package ``filter_range``) filters
items based on a date range defined by two separate date attributes. The first date
attribute contains the start date ("from"), the second the end date ("to"). An item is
included in the result when a given search date lies within the period defined by both
attribute values.

Typical use cases: Event periods, booking periods, validity periods with separate date
attributes.

A dedicated template ``mm_filteritem_datepicker.html5`` is available for browser-based
date input. For the HTML5 ``date`` input field, the date must be passed in the format
``YYYY-MM-DD`` —
`more information <https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/date>`_.

.. seealso:: For numeric two-field comparisons, the filter rule
   :ref:`component_filter_range` is available.


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
     - Selection of the filter rule type — here: "Value from/to for two date fields".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute (from)
     - The first date attribute that contains the start date.
   * - Attribute 2 (to)
     - The second date attribute that contains the end date.


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
   * - Date format
     - The format in which the date is expected (e.g. ``d.m.Y`` or ``Y-m-d``).
   * - Time type
     - Defines whether only the date (``date``), date and time (``datim``), or only
       the time (``time``) is compared.
   * - Label
     - Label of the filter widget.
   * - Hide label at filter widget
     - Suppresses the output of the label.
   * - Template
     - Template for the widget output. For date picker input: ``mm_filteritem_datepicker``.
   * - Placeholder
     - Placeholder text in the input fields.
   * - Filter range type
     - Defines how the search date range is compared with the attribute values
       (s1–s5, analogous to the filter rule
       :ref:`component_filter_range`).
   * - Greater-than-or-equal (≥)
     - The "from" search date is compared inclusively (``>=``).
   * - Less-than-or-equal (≤)
     - The "to" search date is compared inclusively (``<=``).
   * - Show from field
     - Enables the display of the "from" date field in the widget.
   * - Show to field
     - Enables the display of the "to" date field in the widget.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Value from/to for two date fields" filter rule is suitable for the
:ref:`Timestamp <component_attribute_timestamp>` attribute. If the filtering should be
for date values only, the "Date and time handling" option in the attribute should be set
to "Store date only without time".


.. |svg_filt_range_date_22| image:: /_img/icons_svg/filter_rangedate.svg
   :width: 22px
.. |img_filter_range| image:: /_img/icons/filter_range.png

.. |br| raw:: html

   <br />
