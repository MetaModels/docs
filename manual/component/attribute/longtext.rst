.. _component_attribute_longtext:

Long Text
=========

The "Long text" attribute is intended for longer text input. It is displayed as a textarea
widget in the backend and can optionally be equipped with a rich text editor (RTE such as
TinyMCE). Typical use cases:

* Description texts, product descriptions, article texts
* Free text fields for multi-line input
* HTML content (when RTE is enabled)

The maximum length is 65,535 characters (MySQL type ``TEXT``).

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedlongtext` is available.

.. seealso:: This attribute is supported by the :ref:`File-Usage integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_longtext


Settings when Creating the Attribute
--------------------------------------

The long text attribute has no specific settings when creating. Only the general attribute
settings are used:

* Name, column name, description
* Override variants


Settings in Render Settings
-----------------------------

The long text attribute has no specific render settings. In the attribute list of a render
setting, the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the long text.
       If no template is specified, the output is as plain text or HTML.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the long text attribute is added to an input form, the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form (e.g.
       ``long clr`` for full width with line break).
   * - Template for backend
     - Selection of a custom widget template for the backend form.
   * - Template for frontend
     - Selection of a custom widget template for frontend editing
       (only available if the "Frontend Editing" extension is installed).
   * - Rich text editor (RTE)
     - Activation and selection of a rich text editor (e.g. ``tinyMCE`` or ``ace``).
       If no RTE is selected, a simple textarea field appears.
   * - Syntax highlighting mode
     - Selection of the syntax highlighting for the source code editor ``ace``.
   * - Rows
     - Number of visible rows of the textarea field (height) — typically overridden by
       Contao CSS. When the template ``be_tinyMCE_mm`` or ``be_ace_mm`` is selected,
       the height values are passed to the widget as an option — for ``tinyMCE`` as pixels
       and for ``ace`` as rows.
   * - Columns
     - Number of visible characters per row of the textarea field (width) — typically
       overridden by Contao CSS.

**Functions**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Required field
     - Makes the field a required field.
   * - Allow HTML input
     - Allows the input of HTML tags in the text field (without RTE).
   * - Keep tags
     - HTML tags are not filtered or encoded when saving.
   * - Decode entities
     - HTML entities are decoded when saving.

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

The long text attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Text search
     - Free text input for full-text search in the long text field.
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search; requires the package ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Rich text editor (RTE)**

A RTE such as TinyMCE can be enabled via the input form. Note that the RTE formats the
entered HTML content and encodes entities when saving. The "Allow HTML input" option
should also be enabled in the functions in that case.

The input widget as TinyMCE can be equipped with a :ref:`custom jumpTo picker for the detail page
<rst_cookbook_specials_picker-for-tinymce>`.

**Database storage**

The text is stored as ``text NULL`` (up to 65,535 characters). An empty value is stored as
``NULL`` (compatible with MySQL Strict Mode). For longer content, a migration to
``mediumtext`` or ``longtext`` directly at the database level may be necessary — this is
described in the cookbook under :ref:`rst_cookbook_inputmask_manipulate-select-values`.


.. |br| raw:: html

   <br />
