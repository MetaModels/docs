.. _component_attribute_country:

|svg_attr_country_22| |img_country| Country
===========================================

The "Country" attribute provides a selection list of all countries in the world.
Country names are displayed in the currently active backend language and sorted
alphabetically. The two-letter ISO 3166-1 Alpha-2 code is stored
(e.g. ``DE`` for Germany, ``AT`` for Austria). Typical use cases:

* Country field in address forms
* Country of origin or destination for products or shipping options
* Nationality information for person data

The available countries can be restricted if only a selection of specific countries
is needed.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_country


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the country attribute offers the following specific option:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Filter available countries
     - Restricts the country selection to specific countries. Multiple selection via a
       searchable dropdown list is possible. If no selection is made, all countries are available.


Settings in Render Settings
-----------------------------

The country attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output. If no template is specified,
       the localized country name in the active language is output.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the country attribute is added to an input form, the following options are available:

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
   * - Show empty option
     - Displays an empty selection option at the beginning of the country list so that
       no country is pre-selected.

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

There is currently no dedicated filter rule for the country attribute.
For filtering by a country, the "Simple lookup" filter rule can be used
with the country code (e.g. ``DE``) as parameter.


Special Functions
-----------------

**Localized output**

The stored ISO code (e.g. ``DE``) is automatically converted to the localized country name
of the active language when output. The attribute resolves the code internally via the
Contao service ``contao.intl.countries`` and caches the result per language.

**Sorting by country name**

Sorting of records by the country attribute is done using the localized country name
(not the stored ISO code), so that alphabetical sorting is language-dependently correct.

**Database storage**

The country value is stored as ``varchar(2) NULL`` (two-letter ISO code).
An empty value is stored as ``NULL`` (compatible with MySQL Strict Mode).


.. |svg_attr_country_22| image:: /_img/icons_svg/country.svg
   :width: 22px
.. |img_country| image:: /_img/icons/country.png

.. |br| raw:: html

   <br />
