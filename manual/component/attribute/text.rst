.. _component_attribute_text:

|svg_attr_text_22| |img_text| Text
==================================

The "Text" attribute is the simplest text field in MetaModels and stores short texts up to 255
characters. Typical use cases:

* Names, titles, headings
* Short descriptions, subtitles
* Codes, article numbers, references
* Phone numbers, postal codes (as text, not number)

.. note:: For longer texts (over 255 characters), the attribute
   :ref:`component_attribute_longtext` should be used.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedtext` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_text


Settings when Creating the Attribute
--------------------------------------

The text attribute has no specific settings when creating. Only the general attribute settings are used:

* Name, column name, description
* Unique values
* Override variants


Settings in Render Settings
-----------------------------

The text attribute has no specific render settings. In the attribute list of a render setting, the
usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the text value.
       If no template is specified, the output is as plain text.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the text attribute is added to an input form, the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form (e.g.
       ``w50`` for half width, ``long`` for full width).
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
   * - Regular expression
     - Validation of input with a predefined regular expression.
       Available patterns:

       * **digit** — Digits only
       * **natural** — Positive integers
       * **alpha** — Letters only
       * **alnum** — Letters and digits
       * **extnd** — Everything except ``#`` and ``<>``
       * **date** — Date in configured format
       * **time** — Time in configured format
       * **datim** — Date and time
       * **friendly** — Friendly name (for email)
       * **email** — Email address
       * **emails** — Comma-separated email addresses
       * **url** — URL address
       * **alias** — Alias-compatible characters
       * **phone** — Phone number
       * **prcnt** — Percentage (0–100)
       * **locale** — Language code (e.g. ``de``, ``de_DE``)
       * **language** — Language code
       * **fieldname** — Valid field name

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

The text attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Text search
     - Free text input for searching in the text field.
   * - Simple lookup
     - Filters by an exact or partial value via a URL parameter.
   * - Single select
     - Selection of a value from a list of existing text values.
   * - Multi-select
     - Multiple selection from existing text values.
   * - Register
     - Filters by initial letters of the text value.
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package
       ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search; requires the package ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Icon picker**
The text attribute is also suitable for integrating an :ref:`icon picker for the input form
<rst_cookbook_specials_picker-for-icons>`.


**Database storage**

The text is stored as ``varchar(255) NULL``. An empty value is stored as ``NULL`` (compatible
with MySQL strict mode). The 255-character limit is fixed by the database type.

**HTML entities**

The attribute automatically handles Contao HTML entities (``basicEntities``). Special characters
are correctly encoded and decoded when saving and outputting.


.. |svg_attr_text_22| image:: /_img/icons_svg/text.svg
   :width: 22px
.. |img_text| image:: /_img/icons/text.png
.. |br| raw:: html

   <br />
