.. _cookbook_install_mm2.1-alpha:

Installing MetaModels 2.1 for Alpha Testing
============================================

For installing MM 2.1, the requirements listed in :ref:`manual_install` apply.

There are currently still issues with the ``strict mode``, which is enabled by default in newer
MariaDB installations. For now, either the ``strict mode`` must be disabled, or the ``NOT NULL``
constraint must be manually removed from the fields of your MM tables that have a default value.

During the alpha test phase, the bundles ``bundle_start`` and ``bundle_all`` are not yet
available for the installation. In addition to the core, the required packages for attributes
and filters must be installed separately.

This can be done either via the Contao Manager or by updating ``composer.json`` directly.

As a base implementation, the following packages including version constraints must be installed
— either in ``composer.json`` directly or via the Contao Manager:

* MM Core ``^2.1.0@dev``
* DC_General with ``dev-feature/contao4-release as 2.1.0``
* MultiColumnWizard (MCW) with ``^3.4.0@beta``

The following entries under "require" in ``composer.json`` can be used as a template:

.. code-block:: json
   :linenos:

   "require": {
     "php": "^7.1",
     "contao-community-alliance/dc-general": "^2.1",
     "contao/manager-bundle": "<4.5",
     "contao/installation-bundle": "<4.5",
     "menatwork/contao-multicolumnwizard-bundle": "^3.4",
     "metamodels/core": "^2.1.0@dev",
     "metamodels/attribute_alias": "^2.1.0@dev",
     "metamodels/attribute_checkbox": "^2.1.0@dev",
     "metamodels/attribute_color": "^2.1.0@dev",
     "metamodels/attribute_combinedvalues": "^2.1.0@dev",
     "metamodels/attribute_country": "^2.1.0@dev",
     "metamodels/attribute_decimal": "^2.1.0@dev",
     "metamodels/attribute_file": "^2.1.0@dev",
     "metamodels/attribute_langcode": "^2.1.0@dev",
     "metamodels/attribute_levensthein": "^2.1.0@dev",
     "metamodels/attribute_longtext": "^2.1.0@dev",
     "metamodels/attribute_numeric": "^2.1.0@dev",
     "metamodels/attribute_rating": "^2.1.0@dev",
     "metamodels/attribute_select": "^2.1.0@dev",
     "metamodels/attribute_tabletext": "^2.1.0@dev",
     "metamodels/attribute_tags": "^2.1.0@dev",
     "metamodels/attribute_text": "^2.1.0@dev",
     "metamodels/attribute_timestamp": "^2.1.0@dev",
     "metamodels/attribute_url": "^2.1.0@dev",
     "metamodels/attribute_translatedalias": "^2.1.0@dev",
     "metamodels/attribute_translatedcheckbox": "^2.1.0@dev",
     "metamodels/attribute_translatedcombinedvalues": "^2.1.0@dev",
     "metamodels/attribute_translatedfile": "^2.1.0@dev",
     "metamodels/attribute_translatedlongtext": "^2.1.0@dev",
     "metamodels/attribute_translatedselect": "^2.1.0@dev",
     "metamodels/attribute_translatedtabletext": "^2.1.0@dev",
     "metamodels/attribute_translatedtags": "^2.1.0@dev",
     "metamodels/attribute_translatedtext": "^2.1.0@dev",
     "metamodels/attribute_translatedurl": "^2.1.0@dev",
     "metamodels/filter_checkbox": "^2.1.0@dev",
     "metamodels/filter_fromto": "^2.1.0@dev",
     "metamodels/filter_range": "^2.1.0@dev",
     "metamodels/filter_select": "^2.1.0@dev",
     "metamodels/filter_tags": "^2.1.0@dev",
     "metamodels/filter_text": "^2.1.0@dev",
     "metamodels/filter_register": "^2.1.0@dev"
   },

Note that only the packages that are actually needed should be installed — especially if
``bundle_all`` was previously used.

A database query can quickly reveal which attributes and filters are in use:

.. code-block:: sql
   :linenos:

   -- Attributes
   SELECT type FROM `tl_metamodel_attribute` GROUP BY type ORDER BY type

   -- Filters
   SELECT type FROM `tl_metamodel_filtersetting` GROUP BY type ORDER BY type
