.. _component_attribute_geodistance:

Geo Distance
============

The "Geo distance" attribute calculates the geographic distance between a stored coordinate
pair of an item and a search point during a perimeter search. The result enables the sorting
of lists by distance. Typical use cases:

* Dealer or branch search ("Find all dealers within a radius of 50 km")
* Event search by geographic proximity
* Sorting results by distance to the user's location

The attribute itself does not store any data values, but reads coordinates from other
attributes of the MetaModel (latitude and longitude) and calculates the distance on demand.

.. seealso:: In the cookbook:

   * :ref:`rst_cookbook_specials_generate-geocoordinates`
   * :ref:`rst_cookbook_specials_marker-for-gmap`


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_geodistance


Settings when Creating the Attribute
--------------------------------------

The attribute offers the following specific settings, divided into two groups:

**Parameters**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - GET parameter for address
     - Name of the URL GET parameter through which the search address is passed
       (e.g. ``address``). Required field.
   * - Country default
     - Defines how the country for address resolution is determined:

       * **Nothing** — No country is specified
       * **Default** — A fixed country is set as default
         (enter the country code in the sub-field "Default for country")
       * **Use GET parameter** — The country is passed via a URL parameter
         (enter the parameter name in the sub-field "GET parameter for country")

**Data**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Data mode
     - Defines how the geo coordinates are stored in the MetaModels attributes:

       * **Single mode** — Latitude and longitude are stored combined in a single
         attribute (e.g. of type "Geolocation"). Sub-field: select the attribute.
       * **Multi mode** — Latitude and longitude are stored in two separate attributes.
         Sub-fields: attribute for latitude (Lat) and attribute for longitude (Lng).
   * - Lookup services
     - Multi-column wizard for configuring the services that convert an address to
       geo coordinates. Available services (depending on installation):

       * **Coordinates** — Direct coordinate input
       * **Google Maps** — Address resolution via the Google Maps API
       * **OpenStreetMap** — Address resolution via the Nominatim API

       For services that require an API token, it can be entered in the "API Token" field.


Settings in Render Settings
-----------------------------

The attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the distance value.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

The geo distance attribute does not appear as an editable input field in the input form —
the distance value is calculated dynamically and is read-only.


Filter Rules
------------

The attribute is typically used together with the "Perimeter search" filter rule
(from the package ``filter_perimetersearch``):

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Perimeter search
     - Filters items based on a geographic radius around an entered address. The geo
       distance attribute provides the calculated distance values which can then be
       used for sorting.


Special Functions
-----------------

**Calculation**

The distance between the search point and the stored coordinate pair is calculated using
the Haversine formula, which accounts for the curvature of the earth. The result is
available in the template as a distance value (in km or miles).

**Caching**

Resolved coordinates (address → lat/lng) are cached in the table
``tl_metamodel_perimetersearch`` to avoid repeated API requests.

**Storage**

The attribute does not create its own column in the MetaModel table. It reads the
coordinates from the configured source attributes and calculates the distance at runtime.

**Sorting by distance**

The calculated distance values can be used as a sorting criterion in the MetaModels list
view, so that the nearest results appear at the top.


.. |br| raw:: html

   <br />
