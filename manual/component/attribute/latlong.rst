.. _component_attribute_latlong:

|svg_attr_latlong_22| LatLong (from MM 2.5)
===========================================

The "LatLong" attribute stores a coordinate pair (latitude and longitude) in a single
column of the native MySQL/MariaDB data type ``POINT``. Unlike two separate decimal
attributes or a text attribute with comma-separated values, this provides a genuine
spatial column type on which the database can optionally create a spatial index.

Typical use cases:

* Storing the location of a record (branch, venue, marker on a map)
* Basis for a :ref:`perimeter search <component_filter_perimeter-search>` or the
  :ref:`Geo distance <component_attribute_geodistance>` attribute
* Marker position in the :ref:`Cowegis layer integration <extended_cowegis-layer-marker>`

.. seealso::

   * :ref:`component_filter_perimeter-search`
   * :ref:`component_attribute_geodistance`
   * :ref:`extended_cowegis-layer-marker`


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_latlong


Settings when Creating the Attribute
--------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Create spatial index
     - Creates a ``SPATIAL INDEX`` on the column, so that queries (e.g. the
       :ref:`perimeter search <component_filter_perimeter-search>`) can use it for a
       significantly faster perimeter search — see :ref:`Special Functions
       <component_attribute_latlong_special>` below. This requires the column to be
       ``NOT NULL`` — the attribute therefore automatically becomes a required field,
       both when saving a record and visibly in the input form (see below). If the
       attribute already contains empty values, enabling this fails until they have
       been filled in.


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
     - Selection of a custom template for the output of the coordinate pair.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

By default, the input form displays two fields side by side (latitude, longitude). If
"Create spatial index" is enabled on the attribute, the field is additionally marked as
required — this cannot be changed as long as the index is active.

**Address lookup via the Geocode widget**

If the package `cowegis/cowegis-contao-geocode-widget-bundle
<https://github.com/cowegis/cowegis-contao-geocode-widget-bundle>`_ is installed, an
additional legend is available in the input form that lets manual coordinate entry be
replaced or supplemented by an address search with map selection:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Field layout
     - Defines, independently of the address lookup, whether the coordinates are
       displayed in two separate fields (latitude, longitude — default) or in a single
       field as a comma-separated value (``latitude,longitude``).
   * - Determine coordinates from an address |br| (`Cowegis Geocode widget
       <https://github.com/cowegis/cowegis-contao-geocode-widget-bundle>`_ required)
     - Adds a popup with address search and map that lets the coordinates be determined
       from an entered address and adjusted on the map (``submitOnChange`` — immediately
       reveals the following options).
   * - Attribute street / house number / |br| postal code / city / country
     - Selection of attributes of the same MetaModel from whose values the search query
       for the address lookup is assembled. All five fields are optional and can be
       selected independently of each other — at least one must be filled in for the
       popup to appear.
   * - Tile server URL (optional) |br| Map attribution (optional)
     - Both fields can be left empty — the widget then uses its own default tile server
       along with its attribution. Only fill these in if a different map provider should
       be used (e.g. due to its terms of use for your own traffic).

.. note:: The Geocode widget is a separate package, independent of MetaModels — see its
   `documentation <https://github.com/cowegis/cowegis-contao-geocode-widget-bundle>`_ for
   further details on operating the popup.


Filter Rules
------------

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - :ref:`Perimeter search <component_filter_perimeter-search>`
     - In the "Data mode" of the filter rule, the "Single attribute" option is
       available — only an attribute of type LatLong can be selected there. If a spatial
       index is defined on the attribute, it is automatically used for the perimeter
       search.


.. _component_attribute_latlong_special:

Special Functions
-----------------

**Storage format**

The coordinates are stored as a native ``POINT`` (WKB binary format including SRID), not
as two decimal values or as text. This makes all spatial functions of MySQL/MariaDB
(e.g. ``ST_Distance_Sphere()``, ``ST_X()``, ``ST_Y()``) directly available on the column.

Existing data can easily be migrated into the new attribute — e.g.

.. code-block:: mysql
   :linenos:

   -- copy from geo_lat/geo_long into the new field geo_latlong_single
   UPDATE mm_map
   SET geo_latlong_single = POINT(geo_long, geo_lat)
   WHERE geo_lat IS NOT NULL
     AND geo_long IS NOT NULL
     AND geo_latlong_single IS NULL;


**Performance with spatial index**

A plain ``WHERE ST_Distance_Sphere(...) <= x`` fundamentally **cannot** use a spatial
index — when the index is enabled, the perimeter search therefore combines a coarse,
index-capable bounding-box pre-filter (``MBRContains``) with the exact
``ST_Distance_Sphere()`` calculation. Measured on a table with 500,000 records and a
50 km perimeter search (median of 5 runs):

.. list-table::
   :header-rows: 1
   :widths: 50 25 25

   * - Variant
     - Time
     - Factor
   * - Old: Haversine approximation, two separate decimal attributes, no index
     - 0.402 s
     - Baseline
   * - New: ``ST_Distance_Sphere()``, LatLong attribute without index
     - 0.181 s
     - ~2.2× faster
   * - New: ``ST_Distance_Sphere()`` + bounding-box pre-filter + spatial index
     - 0.014 s
     - ~28× faster

The formula change alone already roughly doubles performance; the spatial index with
bounding-box pre-filter adds another ~13× on top of that.


.. |svg_attr_latlong_22| image:: /_img/icons_svg/latlong.svg
   :width: 22px

.. |br| raw:: html

   <br />
