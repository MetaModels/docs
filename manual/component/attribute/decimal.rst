.. _component_attribute_decimal:

|img_decimal| Decimal
=====================

The "Decimal" attribute stores decimal numbers (double-precision floating-point numbers).
Typical use cases:

* Monetary amounts and prices (e.g. ``19.99``)
* Dimensions and weights (e.g. ``1.75``, ``0.5``)
* Geographic coordinates (latitude/longitude)
* Percentage values or rating values

.. note:: For postal codes or phone numbers, the "Text" attribute should be used, as these
   are not real numeric values and would lose leading zeros. For perimeter search, one
   decimal attribute each for latitude and longitude must be created.

.. note:: Input is done with a **period** as the decimal separator (not a comma).


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_decimal


Settings when Creating the Attribute
--------------------------------------

The decimal attribute has no specific settings when creating. Only the general attribute
settings are used:

* Name, column name, description
* Unique values
* Override variants


Settings in Render Settings
-----------------------------

The decimal attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the decimal value.
       If no template is specified, the output is as plain text.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the decimal attribute is added to an input form, the following options are available:

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

The decimal attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Value from/to for one field
     - From/to range filter for a single decimal attribute; e.g. for a price range
       search with minimum and maximum value.
   * - Value from/to for two fields
     - Range filter across two decimal attributes; e.g. when a value range is
       represented by a "from" attribute and a "to" attribute
       (e.g. price range of an offer).
   * - Perimeter search
     - For geo coordinates: filters by radius around a search point when latitude
       and longitude are stored as separate decimal attributes.


Special Functions
-----------------

**Database storage**

The value is stored as ``double NULL default NULL``. An empty value is stored as ``NULL``
(compatible with MySQL Strict Mode).

**Input validation**

The input field uses the ``digit`` regex check, which only accepts numeric input
(including decimal point and sign).


.. |img_decimal| image:: /_img/icons/decimal.png

.. |br| raw:: html

   <br />
