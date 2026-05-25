.. _rst_cookbook_specials_marker-for-gmap:

Displaying Locations as Markers on a Google Map
================================================

.. note:: As an alternative to output via a template, the
   :ref:`Cowegis Layer <extended_cowegis-layer-marker>` extension is available.


Goal
----
The (optionally filtered) entries of a MetaModel should be displayed as markers
on a Google Map on a page.


Requirements
------------

* Contao 5.3 with MetaModels 2.4, also successfully tested with Contao 4.13 and MetaModels 2.3
* Google Maps API key for map display → ``API_KEY_WEBSITE`` (Application: Websites; API: Maps JavaScript API)
* MetaModels template with access to ``latitude``, ``longitude`` and optionally custom fields such as ``name`` in this example

The output is generated in the MetaModels template of the render settings. This
is the template used to output the MetaModels list in the frontend — see
:ref:`Templates <component_templates>`. A variant of the template
``metamodel_prerendered.html5`` can be created, e.g. as
``metamodel_pre_gmap-with-marker.html5``, and selected in the render settings.

Preparing Markers
-----------------

In the template, an array with all coordinates is first created:

.. code-block:: php

  <?php $markers = []; ?>
  <?php foreach ($this->data as $arrItem): ?>
    <?php
      if (!empty($arrItem['text']['latitude']) && !empty($arrItem['text']['longitude'])) {
          $markers[] = [
              'lat'   => (float) $arrItem['text']['latitude'],
              'lng'   => (float) $arrItem['text']['longitude'],
              'title' => htmlspecialchars($arrItem['text']['name'], ENT_QUOTES, 'UTF-8'),
          ];
      }
    ?>
    <?php // Additional list output ... ?>
  <?php endforeach; ?>


Embedding the Map
-----------------

The map is also output in the created template after the "foreach loop". Note:
``API_KEY_WEBSITE`` must be replaced with the corresponding API key. The PHP
array is converted to a JSON string for the JavaScript loop output.

.. code-block:: html

      <div class="map_wrapper">
      <script async src="https://maps.googleapis.com/maps/api/js?key=API_KEY_WEBSITE&callback=initMap&language=en&region=US"></script>

      <div id="map" style="height: 400px; width: 100%;"></div>

      <script>
        function initMap() {
          const mapOptions = {
            center: { lat: 52.553807, lng: 13.405007 }, // Default for Berlin
            zoom: 10,
            zoomControl: true,
            streetViewControl: true,
            mapTypeControl: true,
            mapTypeId: google.maps.MapTypeId.ROADMAP,
            styles: [
              {
                "featureType": "poi.business",
                "stylers": [
                  { "visibility": "off" }
                ]
              },
              {
                "featureType": "poi.park",
                "elementType": "labels.text",
                "stylers": [
                  { "visibility": "off" }
                ]
              }
            ]
          };

          // Initialise map
          const map = new google.maps.Map(document.getElementById("map"), mapOptions);
          // Marker array as JSON
          var markers = <?= json_encode($markers, JSON_HEX_TAG | JSON_HEX_APOS | JSON_HEX_QUOT | JSON_HEX_AMP) ?>;

          var bounds = new google.maps.LatLngBounds();
          var infowindow = new google.maps.InfoWindow();
          var minZoom = 11;

          if (markers.length === 0) {
            map.setCenter({ lat: 52.553807, lng: 13.405007 });
            map.setZoom(minZoom);
            return;
          }

          // Create markers from JSON data
          markers.forEach(function(markerData) {
            let marker = new google.maps.Marker({
              position: { lat: markerData.lat, lng: markerData.lng },
              map: map,
              title: markerData.title,
              clickable: true
            });

            // Create info output e.g. with title and URL
            marker.addListener("click", function() {
              let content = "<strong>" + markerData.title + "</strong>";
              if (markerData.website) {
                content += "<br>" + markerData.website;
              }
              infowindow.setContent(content);
              infowindow.open(map, marker);
            });

            bounds.extend(marker.position);
          });

          map.fitBounds(bounds);

          google.maps.event.addListenerOnce(map, 'zoom_changed', function() {
            if (map.getZoom() > minZoom) {
              map.setZoom(minZoom);
            }
          });
        }
      </script>
    </div>


Notes
-----

* For privacy-compliant use, the map should be enabled via a consent tool.
* Rendering output can be sped up by selecting the option
  ``'Do not output parsed items via "$data"'`` in the CE MM List settings —
  rendering the data is not necessary.
* How coordinates for an address can be automatically retrieved and stored on
  save is described under ":ref:`rst_cookbook_specials_generate-geocoordinates`".


**Thanks**

Thanks to `Nicole Weiß - Webstylisten.de <https://webstylisten.de>`_ for the article.
