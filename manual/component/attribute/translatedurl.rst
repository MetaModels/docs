.. _component_attribute_translatedurl:

|svg_attr_translatedurl_22| |img_url| Translated URL
====================================================

The "Translated URL" attribute is the multilingual variant of the
:ref:`URL <component_attribute_url>` attribute. It stores a separate link per language,
consisting of a title and a URL address. The values are not stored in the MetaModel
table, but in the translation table ``tl_metamodel_translatedurl``.

Typical use cases:

* Language-dependent links to external websites (e.g. German and English product pages
  of a manufacturer)
* Translated download links (e.g. DE and EN data sheets as separate URLs)
* Language-specific internal links to different Contao pages

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_url`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedurl


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings, the attribute offers the following
specific option:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Remove title
     - If this option is enabled, only the URL field is displayed and stored —
       the title field is omitted. Useful when only the pure URL without descriptive
       text is needed.


Settings in Render Settings
-----------------------------

The attribute has its own render setting:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Do not open in new tab
     - If this option is active, the link is output **without** ``target="_blank"`` —
       it therefore opens in the same browser window. Default (not enabled): links open
       in a new tab.
   * - Template
     - Selection of a custom template for the output of the link.
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
       ``long`` for full width).
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


Filter Rules
------------

The translated URL attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search; requires the package ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Database storage**

The URL values are stored per language in ``tl_metamodel_translatedurl``. Two columns
are stored per entry per language: ``href`` (the URL address) and ``title`` (the link
text). No column is created in the MetaModel table.

**Language-dependent links**

Title and URL can be completely different for each language version. In the backend, a
separate input form with title field and URL field (incl. page picker wizard) appears
for each language.

**Fallback language**

If a value is missing for a language, MetaModels falls back to the fallback language.

**Input wizard**

In the backend, a page picker wizard is automatically displayed next to the text field,
allowing internal Contao pages to be selected. If "Remove title" is enabled, only a
simple text field is available.


.. |svg_attr_translatedurl_22| image:: /_img/icons_svg/url.svg
   :width: 22px
.. |img_url| image:: /_img/icons/url.png
.. |br| raw:: html

   <br />
