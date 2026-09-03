.. _component_filter_perimeter-search:

|svg_filt_perimeter_search_22| |img_filter_perimetersearch| Perimeter Search
============================================================================

The "Perimeter Search" filter rule (package ``filter_perimetersearch``) filters items
based on their geographic position. Visitors enter an address or coordinates and choose
a search radius; the filter rule finds all items whose geocoordinates (latitude/longitude)
lie within the specified perimeter.

A prerequisite is that items have their geocoordinates stored either in a single
:ref:`LatLong attribute <component_attribute_latlong>` or in two separate decimal or text
attributes (latitude and longitude). External lookup services are used for geocoding
addresses to coordinates (e.g. OpenStreetMap/Nominatim, Google Maps API).

.. seealso:: Detailed documentation on perimeter search:
   :ref:`extended_perimetersearch`

.. note:: **From MM 2.5:** When the address field is cleared, the previously selected
   perimeter selection also disappears from the widget — before, it remained visible even
   though it no longer had any effect without an address (`Issue #31
   <https://github.com/MetaModels/filter_perimetersearch/issues/31>`_). In MM 2.4 the
   previous behavior remains unchanged.

.. note:: **From MM 2.5:** If a :ref:`LatLong attribute <component_attribute_latlong>` with
   spatial index enabled is used (single attribute), the perimeter search automatically uses
   an index-backed bounding-box pre-filter — depending on the amount of data, several times
   faster than without an index. Details and benchmark figures: :ref:`Special functions of the
   LatLong attribute <component_attribute_latlong_special>`.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_perimetersearch


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Perimeter Search".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Data mode
     - Defines how the geocoordinates of items are stored:

       * **Single attribute** — The coordinates are stored in a single
         :ref:`LatLong attribute <component_attribute_latlong>`. Additional option:
         **Attribute (single)** — Selection of the attribute (only LatLong attributes
         are selectable).
       * **Two attributes** — Latitude and longitude are stored in two separate
         attributes (:ref:`Decimal <component_attribute_decimal>` or
         :ref:`Text <component_attribute_text>`). Additional options:
         **First attribute (lat)** and **Second attribute (long)**.
   * - URL parameter
     - The key of the URL parameter for passing the address/coordinate input.


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
     - Label of the address input field.
   * - Placeholder
     - Placeholder text in the address input field.
   * - Template (address field)
     - Template for the output of the address input field.
   * - Label (perimeter)
     - Label of the perimeter selection widget.
   * - Template (perimeter)
     - Template for the output of the perimeter widget. Default: ``mm_filteritem_default``.
   * - Perimeter mode
     - Defines how the search radius is determined:

       * **Free** — The visitor enters the radius as a numeric value. Additional
         option: **Placeholder (perimeter)**.
       * **Preset** — A fixed radius is used. Additional option:
         **Radius preset** (kilometer value).
       * **Selection** — The visitor selects from a predefined list of radii.
         Additional option: **Radius selection** (MCW table with value and default flag).
   * - Country mode
     - Defines whether and how a country is specified for geocoding:

       * **No country** — No country filter.
       * **Preset** — Fixed country (ISO code). Additional option: **Country preset**.
       * **GET parameter** — The country is passed via a URL parameter.
         Additional option: **Country GET parameter**.
   * - Lookup services
     - Multi-column assistant for configuring the services that convert an
       address into geocoordinates. Available services (depending on
       installation):

       * **Coordinates** — Direct coordinate input
       * **Google Maps** — Address resolution via the Google Maps API
       * **OpenStreetMap** — Address resolution via the Nominatim API

       For services that require an API token, it can be entered in the
       "API Token" field.

       The lookup services are processed in order from top to bottom and stop at the first
       match. If coordinates should also be allowed alongside address input in the frontend,
       this service must be placed first. On mobile devices, the lat/long values for the
       input can be read from the device via JavaScript.


Matching Attributes
------------------

Depending on the selected data mode, the "Perimeter Search" filter rule requires one of the
following attributes:

* :ref:`LatLong <component_attribute_latlong>` (single attribute — recommended, supports a
  spatial index for a significantly faster perimeter search)
* :ref:`Decimal <component_attribute_decimal>` or :ref:`Text <component_attribute_text>` (two
  attributes — for latitude and longitude separately)

Additionally, the :ref:`Geo distance <component_attribute_geodistance>` attribute can
be used for display and sorting by distance.


.. |svg_filt_perimeter_search_22| image:: /_img/icons_svg/filter_perimetersearch.svg
   :width: 22px
.. |img_filter_perimetersearch| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
