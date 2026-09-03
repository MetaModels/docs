.. _component_attribute_timestamp:

|svg_attr_timestamp_22| |img_timestamp| Timestamp
=================================================

The "Timestamp" attribute stores date, time, or both as a Unix timestamp (``bigint``).
In the backend, a date picker with the configured Contao date format is displayed.
Typical use cases:

* Publication date, event date, expiry date
* Opening hours (time only)
* Booking period with start and end date (two timestamp attributes)
* Timestamps for events

.. note:: Date values are stored as Unix timestamps. For custom SQL filters or queries,
   conversions may be necessary (e.g. ``FROM_UNIXTIME()`` in MySQL).

.. seealso:: For a modern date picker in the frontend:
   :ref:`rst_cookbook_templates_flatpickr-integration`


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_timestamp


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the timestamp attribute offers the following specific option:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Schema
     - Defines which input type is used in the backend:

       * **Date** — Date only (without time)
       * **Date and time** — Date together with time
       * **Time** — Time only (without date)


Settings in Render Settings
-----------------------------

The timestamp attribute has its own render setting:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Format
     - Custom date format for the output, formatted with the PHP function ``date()``
       (e.g. ``d.m.Y``, ``H:i``, ``d.m.Y H:i``). If the field is left empty, the
       default format configured in the Contao system is used.
   * - Template
     - Selection of a custom template for the output of the date value.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the timestamp attribute is added to an input form, the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form (e.g.
       ``w50`` for half width).
   * - Template for backend
     - Selection of a custom widget template for the backend form.
   * - Template for frontend
     - Selection of a custom widget template for frontend editing
       (only available if the "Frontend Editing" extension is installed).

**Functions**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Required field
     - Makes the field a required field.
   * - Date and time handling
     - Defines which part of the timestamp is set to 0 when saving. Important for
       correct filtering:

       * **Store date only without time** — The time is reset to ``00:00:00``.
         Useful for pure date filters.
       * **Store time only without date** — The date is reset to the Unix epoch
         start day (01.01.1970).

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

The timestamp attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Value from/to for one date field
     - From/to range filter for a single timestamp attribute; e.g. all events within a
       time period. Custom template: ``mm_filteritem_datepicker.html5`` for an HTML5
       date picker (format ``YYYY-MM-DD``).
   * - Value from/to for two date fields
     - Range filter across two timestamp attributes; e.g. when start and end date are
       stored as separate attributes.


Special Functions
-----------------

**Database storage**

Date and time values are stored as Unix timestamps in a ``bigint(10) NULL`` field.
An empty value is stored as ``NULL``.

**Formatting**

The output is controlled via the Contao event ``ParseDateEvent``. The format from the
render settings takes precedence over the system-wide Contao format. In templates, the
formatted value is directly available as ``$arrData['html5']`` or ``$arrData['text']``.


.. |svg_attr_timestamp_22| image:: /_img/icons_svg/timestamp.svg
   :width: 22px
.. |img_timestamp| image:: /_img/icons/timestamp.png
.. |br| raw:: html

   <br />
