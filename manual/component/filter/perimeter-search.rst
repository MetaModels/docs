.. _component_filter_perimeter-search:

|img_filter_perimetersearch| Perimeter Search
==========================================

The "Perimeter Search" filter rule (package ``filter_perimetersearch``) filters items
based on their geographic position. Visitors enter an address or coordinates and choose
a search radius; the filter rule finds all items whose geocoordinates (latitude/longitude)
lie within the specified perimeter.

A prerequisite is that items have geocoordinates stored in two separate decimal attributes
(latitude and longitude). External lookup services are used for geocoding addresses to
coordinates (e.g. OpenStreetMap/Nominatim, Google Maps API).

.. seealso:: Detailed documentation on perimeter search:
   :ref:`extended_perimetersearch`


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

       * **Single attribute** — The coordinates are stored in a single combined
         attribute (e.g. "lat,long" as text). Additional option:
         **Attribute (single)** — Selection of the attribute.
       * **Two attributes** — Latitude and longitude are stored in two separate
         attributes. Additional options:
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
   * - Lookup service
     - Configuration of the geocoding service (MCW table):

       * **Service** — Selection of the geocoding service (e.g. Nominatim, Google Maps).
       * **API token** — Optional API key for paid services.


Matching Attributes
------------------

The "Perimeter Search" filter rule requires geocoordinates in decimal form:

* :ref:`Decimal <component_attribute_decimal>` (for latitude and longitude separately)

Additionally, the :ref:`Geo distance <component_attribute_geodistance>` attribute can
be used for display and sorting by distance.


.. |img_filter_perimetersearch| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
