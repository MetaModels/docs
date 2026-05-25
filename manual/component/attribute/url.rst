.. _component_attribute_url:

URL
===

The "URL" attribute stores a link consisting of a title and a URL address. Alternatively,
it can be set to pure URL output (without title). Typical use cases:

* External links to websites, documents, or social media profiles
* Internal links to Contao pages (via the page picker)
* Download links with descriptive title

External links must be entered including the protocol (e.g. ``https://www.example.com``).
Internal Contao links can be selected via the integrated page picker.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedurl` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_url


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the URL attribute offers the following specific option:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Remove title
     - If this option is enabled, only the URL field is displayed and stored — the title
       field is omitted. Useful when only the pure URL without descriptive text is needed.


Settings in Render Settings
-----------------------------

The URL attribute has its own render setting:

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

When the URL attribute is added to an input form, the following options are available:

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

The URL attribute can be used with the following filter rules:

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

If "Remove title" is disabled, the pair ``[title, URL]`` is stored as a serialized array
in a ``blob NULL`` field. If "Remove title" is enabled, only the URL is stored as a simple
string.

**Input wizard**

In the backend, a page picker wizard is automatically displayed next to the text field,
allowing internal Contao pages to be selected. If "Remove title" is enabled, only a simple
text field is available.


.. |br| raw:: html

   <br />
