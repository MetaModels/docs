.. _component_attribute_translatedlongtext:

|svg_attr_translatedlongtext_22| |img_longtext| Translated Long Text
====================================================================

The "Translated Long Text" attribute is the multilingual variant of the
:ref:`Long Text <component_attribute_longtext>` attribute. It stores a separate long
text value per language (up to 65,535 characters). The values are not stored in the
MetaModel table, but in the translation table ``tl_metamodel_translatedlongtext``.

Typical use cases:

* Multilingual product descriptions
* Language-dependent article texts or news posts
* Translated HTML content (when a rich text editor is enabled)

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_longtext`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.

.. seealso:: This attribute is supported by the :ref:`File-Usage Integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedlongtext


Settings when Creating the Attribute
--------------------------------------

The attribute has no specific settings when creating it.
Only the general attribute settings are used:

* Name, column name, description
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
     - Selection of a custom template for the output of the long text.
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
     - Selection of syntax highlighting for the source code editor ``ace``.
   * - Rows
     - Number of visible rows of the textarea field (height) — usually overridden by
       Contao CSS. When the template ``be_tinyMCE_mm`` or ``be_ace_mm`` is selected,
       the height values are passed to the widget as an option — for ``tinyMCE`` as
       pixels and for ``ace`` as rows.
   * - Columns
     - Number of visible characters per row of the textarea field (width) — usually
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
   * - Preserve tags
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

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Text search
     - Free text input for full-text search in the long text field of the active language.
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search; requires the package ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Database storage**

The text values are stored per language in ``tl_metamodel_translatedlongtext``
(fields: ``att_id``, ``item_id``, ``langcode``, ``value`` as ``text``).
No column is created in the MetaModel table.

**Rich text editor (RTE)**

An RTE such as TinyMCE can be enabled via the input form. The RTE formats HTML content
and encodes entities when saving. The "Allow HTML input" option should also be enabled
in that case.

**Fallback language**

If a value is missing for a language, MetaModels falls back to the fallback language.


.. |svg_attr_translatedlongtext_22| image:: /_img/icons_svg/longtext.svg
   :width: 22px
.. |img_longtext| image:: /_img/icons/longtext.png
.. |br| raw:: html

   <br />
