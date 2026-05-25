.. _component_attribute_color:

Color Picker
============

The "Color picker" attribute enables the selection of a web color including an opacity value
via an integrated color picker widget. Typical use cases:

* Background colors, font colors, or accent colors for design elements
* Color coding of categories or events
* Transparency-controlled color values (color + opacity)

In the backend, two input fields appear: a text field for the hex color code (6 characters,
e.g. ``ff0000``) and a second field for the opacity value. The color can be selected visually
via the color picker icon.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_color


Settings when Creating the Attribute
--------------------------------------

The color picker attribute has no specific settings when creating. Only the general attribute
settings are used:

* Name, column name, description
* Override variants

.. note:: The "Unique values" option is not available for this attribute, since color values
   are stored as serialized data.


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
     - Selection of a custom template for the output of the color value.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the attribute is added to an input form, the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form.
   * - Template for backend
     - Selection of a custom widget template for the backend form.
   * - Template for frontend
     - Selection of a custom widget template for frontend editing
       (only if "Frontend Editing" is installed).

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

The color picker attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Simple lookup
     - Filters by an exact color value via a URL parameter.
   * - Single select
     - Selection of a color value from a list of existing values.


Special Functions
-----------------

**Database storage**

The color value is stored as a serialized PHP array in a ``TINYBLOB NULL`` field.
The array contains two elements: the hex color code (without ``#``) and the opacity value.
An empty value is stored as ``NULL``.

**Color picker widget**

In the backend, a color picker wizard is displayed next to the text field, allowing the color
to be selected visually. The first text field accepts the six-character hex color code,
the second accepts the opacity value.

**Sorting by color value**

Sorting by color values is done by converting the hexadecimal values into numeric sort keys,
so that colors can be meaningfully ordered by their numeric color value.


.. |br| raw:: html

   <br />
