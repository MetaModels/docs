.. _rst_cookbook_specials_generate-geocoordinates:

Automatically Generating and Storing Coordinates
=================================================

Introduction
------------

This guide describes how coordinates can be automatically generated from address
data and stored in a MetaModels installation.


Requirements
------------

* Contao 5.3 with MetaModels 2.4, also successfully tested with Contao 4.13 and MetaModels 2.3
* Google Maps API key for coordinate generation → ``API_KEY_SERVER`` (Application: IP addresses; API: Geocoding API)
* Fields in the MetaModel: ``street``, ``postcode``, ``city``, ``latitude``, ``longitude``
* The fields ``latitude`` and ``longitude`` are of type "Decimal" (metamodels/attribute_decimal)


Step 1: Creating the EventListener
------------------------------------

To automatically retrieve and store coordinates from address data, a listener for
the PrePersistModelEvent is created. The event is fired before the model is
written to the database.

Create a file ``src/EventListener/PrePersistModelEventListener.php`` with the
following content. Note: ``API_KEY_SERVER`` must be replaced with the
corresponding API key.

.. code-block:: php

  <?php
  // src/EventListener/PrePersistModelEventListener.php
  namespace App\EventListener;

  use ContaoCommunityAlliance\DcGeneral\Event\PrePersistModelEvent;
  use App\Service\GoogleMaps;

  class PrePersistModelEventListener
  {
    public function __invoke(PrePersistModelEvent $event) {
      $model = $event->getModel();
      if (!$model->getProperty('latitude') || !$model->getProperty('longitude')) {
        $street      = $model->getProperty('street');
        $postcode    = $model->getProperty('postcode');
        $city        = $model->getProperty('city');
        $address     = $street . ', ' . $postcode . ' ' . $city;
        $googleMaps  = new GoogleMaps();
        $apiToken    = 'API_KEY_SERVER';
        $coordinates = $googleMaps->getCoordinates($address, $apiToken);

        if (null !== $coordinates) {
          $model->setProperty('latitude', $coordinates->getLatitude());
          $model->setProperty('longitude', $coordinates->getLongitude());
        }
      }
    }
  }


Step 2: Creating the GoogleMaps Class
--------------------------------------

Create the file ``src/Service/GoogleMaps.php`` with the following content:

.. code-block:: php

  <?php
  // src/Service/GoogleMaps.php
  namespace App\Service;

  use App\Service\Coordinates;

  class GoogleMaps
  {
    public function getCoordinates($address, $apiKey) {
      $url      = \sprintf('https://maps.googleapis.com/maps/api/geocode/json?address=%s&key=%s', urlencode($address), $apiKey);
      $response = file_get_contents($url);
      $data     = json_decode($response);

      if ($data->status === 'OK') {
        $loc = $data->results[0]->geometry->location;

        return new Coordinates($loc->lat, $loc->lng);
      }

      return null;
    }
  }


Step 3: Creating the Coordinates Class
---------------------------------------

Create the file ``src/Service/Coordinates.php`` with the following content:

.. code-block:: php

  <?php
  // src/Service/Coordinates.php
  namespace App\Service;

  class Coordinates
  {
    private $lat;
    private $lng;

    public function __construct($lat, $lng) {
      $this->lat = $lat;
      $this->lng = $lng;
    }

    public function getLatitude() {
      return $this->lat;
    }

    public function getLongitude() {
      return $this->lng;
    }
  }


Step 4: Registering the EventListener
---------------------------------------

Create or edit the file ``src/Resources/config/service.yml``:

.. code-block:: yaml

  # src/Resources/config/service.yml
  services:
    App\EventListener\PrePersistModelEventListener:
      public: true
      tags:
        - { name: kernel.event_listener, event: dc-general.model.pre-persist }

Ensure that this file is included in the main configuration, e.g. in
``config/services.yaml``:

.. code-block:: yaml

  # config/services.yaml
  imports:
    - { resource: '../src/Resources/config/service.yml' }


Testing
-------

Edit an existing record in the backend, fill in the ``street``, ``postcode``,
and ``city`` fields and leave the ``latitude`` and ``longitude`` fields empty.
The coordinates will be generated automatically on saving.


Notes
-----

* The ``PrePersistModelEvent`` is only triggered for changed records.
* How the stored coordinates can be displayed as markers on a map is described
  under ":ref:`rst_cookbook_specials_marker-for-gmap`".
* If the :ref:`perimeter search filter (metamodels/filter_perimetersearch) <extended_perimetersearch>`
  is implemented, one of its implemented services can also be used to determine
  the coordinates.
* The EventListener can also be registered via PHP annotations or attributes —
  see :ref:`rst_cookbook_specials_register-services`.


**Thanks**

Thanks to `Nicole Weiß - Webstylisten.de <https://webstylisten.de>`_ for the article.
