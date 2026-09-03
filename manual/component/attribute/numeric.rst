.. _component_attribute_numeric:

|svg_attr_numeric_22| |img_numeric| Numeric
===========================================

The "Numeric" attribute stores integer values. Typical use cases:

* Quantities, amounts, stock levels
* Years, ages
* Sorting or priority values
* Counters and rankings

.. note:: For postal codes or phone numbers, the "Text" attribute should be used, as these
   are not real numeric values and would lose leading zeros.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_numeric


Settings when Creating the Attribute
--------------------------------------

The numeric attribute has no specific settings when creating. Only the general attribute
settings are used:

* Name, column name, description
* Unique values
* Override variants


Settings in Render Settings
-----------------------------

The numeric attribute has no specific render settings. In the attribute list of a render
setting, the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the numeric value.
       If no template is specified, the output is as plain text.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the numeric attribute is added to an input form, the following options are available:

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

The numeric attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Value from/to for one field
     - From/to range filter for a single numeric attribute; e.g. for an age range
       search or quantity filtering.
   * - Value from/to for two fields
     - Range filter across two numeric attributes; e.g. when a value range is
       represented by a "from" and a "to" attribute.


Special Functions
-----------------

**Input validation**

The input field uses the ``digit`` regex check and only accepts integer numeric input.
The maximum length is 10 characters (corresponding to the value range of a 32-bit integer).

**Database storage**

The value is stored as ``int(10) NULL default NULL``. An empty value is stored as ``NULL``
(compatible with MySQL Strict Mode).


.. |svg_attr_numeric_22| image:: /_img/icons_svg/numeric.svg
   :width: 22px
.. |img_numeric| image:: /_img/icons/numeric.png
.. |br| raw:: html

   <br />
