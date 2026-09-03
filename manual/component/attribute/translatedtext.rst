.. _component_attribute_translatedtext:

|svg_attr_translatedtext_22| |img_text| Translated Text
=======================================================

The "Translated Text" attribute is the multilingual variant of the
:ref:`Text <component_attribute_text>` attribute. It stores a separate short text value
per language (up to 255 characters). The values are not stored in the MetaModel table,
but in the translation table ``tl_metamodel_translatedtext``.

Typical use cases:

* Multilingual product names, titles, or headings
* Language-dependent short descriptions or subtitles
* Translated article numbers or references (when language-dependent)

.. note:: For longer texts (over 255 characters), the attribute
   :ref:`component_attribute_translatedlongtext` should be used.

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_text`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedtext


Settings when Creating the Attribute
--------------------------------------

The attribute has no specific settings when creating it.
Only the general attribute settings are used:

* Name, column name, description
* Unique values
* Override variants


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
     - Selection of a custom template for the output of the text value.
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
     - Validation of the input with a predefined regular expression.
       Available patterns:

       * **digit** — Digits only
       * **natural** — Positive integers
       * **alpha** — Letters only
       * **alnum** — Letters and digits
       * **extnd** — Everything except ``#`` and ``<>``
       * **date** — Date in the configured format
       * **time** — Time in the configured format
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

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Text search
     - Free text input for searching within the text field of the active language.
   * - Simple lookup
     - Filters by an exact or partial value via a URL parameter.
   * - Single select
     - Selection of a value from a list of existing text values.
   * - Multi-select
     - Multiple selection from existing text values.
   * - Register
     - Filters by the initial letter of the text value.
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search; requires the package ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Database storage**

The text values are stored per language in ``tl_metamodel_translatedtext``
(fields: ``att_id``, ``item_id``, ``langcode``, ``value`` as ``varchar(255)``).
No column is created in the MetaModel table.

**Fallback language**

If a value is missing for a language, MetaModels falls back to the fallback language.
IDs without a value in the fallback language are treated as empty strings.

**HTML entities**

The attribute handles Contao HTML entities automatically (``basicEntities``).
Special characters are correctly encoded and decoded when saving and outputting.

**Uniqueness per language**

If "Unique values" is active, MetaModels checks uniqueness separately per language.


.. |svg_attr_translatedtext_22| image:: /_img/icons_svg/text.svg
   :width: 22px
.. |img_text| image:: /_img/icons/text.png
.. |br| raw:: html

   <br />
