.. _component_filter_fromto-date:

|img_filter_fromto| Value from/to for one date field
=====================================================

The "Value from/to for one date field" filter rule (package ``filter_fromto``) filters
items based on a date range for a single date attribute. Visitors can enter a "from" date,
a "to" date, or both in the frontend. The filter rule compares the date values stored as
UNIX timestamps with the specified range.

Typical use cases: Event filters (events from date X to date Y), validity periods, or
general date range filters.

A dedicated template ``mm_filteritem_datepicker.html5`` is available for browser-based
date input. For the HTML5 ``date`` input field, the date must be passed in the format
``YYYY-MM-DD`` — `more information <https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/date>`_.

.. seealso:: For comparisons across two separate date attributes, the filter rule
   :ref:`component_filter_range-date` is available. |br|
   For numeric values, the filter rule :ref:`component_filter_fromto` is available.

.. seealso:: For a modern date picker in the frontend:
   :ref:`rst_cookbook_templates_flatpickr-integration`


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
     - Selection of the filter rule type — here: "Value from/to for one date field".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The date attribute by whose value items should be filtered.


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
     - The format in which the date is expected in the frontend input field (e.g.
       ``d.m.Y`` or ``Y-m-d``). Default: Contao date format from system settings.
   * - Time type
     - Defines whether only the date (``date``), date and time (``datim``), or only
       the time (``time``) is compared.
   * - Label
     - Label of the filter widget.
   * - Hide label at filter widget
     - Suppresses the output of the label.
   * - Template
     - Template for the widget output. Default: ``mm_filteritem_default``;
       for date picker input: ``mm_filteritem_datepicker``.
   * - Placeholder
     - Placeholder text in the input fields.
   * - Greater-than-or-equal (≥)
     - If this option is active, the "from" date is treated as inclusive (``>=``).
   * - Less-than-or-equal (≤)
     - If this option is active, the "to" date is treated as inclusive (``<=``).
   * - Show from field
     - Enables the display of the "from" date field in the widget.
   * - Show to field
     - Enables the display of the "to" date field in the widget.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Value from/to for one date field" filter rule is suitable for the
:ref:`Timestamp <component_attribute_timestamp>` attribute. If the filtering should be
for date values only, the "Date and time handling" option in the attribute should be set
to "Store date only without time".


.. |img_filter_fromto| image:: /_img/icons/filter_fromto.png

.. |br| raw:: html

   <br />
