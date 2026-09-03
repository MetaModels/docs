.. _component_attribute_langcode:

|svg_attr_langcode_22| |img_langcode| Language Code
===================================================

The "Language code" attribute provides a selection list of ISO language codes (locales).
Language names are displayed in the currently active backend language. The language code
is stored (e.g. ``de``, ``en``, ``fr`` or also ``de_DE``, ``en_US``). Typical use cases:

* Language of a document, article, or product
* Target language for translation assignments
* Language preference for user or member data

The available language codes can be restricted if only a specific selection of languages
is needed. Input is done via checkboxes.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_langcode


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the language code attribute offers the following specific option:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Language codes
     - Restricts the language selection to specific language codes. The available languages
       are displayed as a checkbox list. If no selection is made, all languages available
       in Contao are selectable.


Settings in Render Settings
-----------------------------

The language code attribute has no specific render settings. In the attribute list of a
render setting, the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output. If no template is specified,
       the localized language name in the active language is output.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the language code attribute is added to an input form, the following options are available:

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
     - Displays an empty selection option so that no language code is pre-selected.

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

There is currently no dedicated filter rule for the language code attribute.
For filtering by a language code, the "Simple lookup" filter rule can be used
with the language code (e.g. ``de``) as parameter.


Special Functions
-----------------

**Localized output**

The stored language code (e.g. ``de``) is automatically converted to the localized language
name of the active language when output. The attribute uses the Contao service
``contao.intl.locales`` for this and caches the result per language.

**Fallback languages**

If a language name is not available for the active language, the attribute automatically
falls back to fallback languages to still display a readable name.

**Database storage**

The language code is stored as ``varchar(5) NULL`` (up to 5 characters, e.g. ``de_DE``).
An empty value is stored as ``NULL`` (compatible with MySQL Strict Mode).


.. |svg_attr_langcode_22| image:: /_img/icons_svg/langcode.svg
   :width: 22px
.. |img_langcode| image:: /_img/icons/langcode.png
.. |br| raw:: html

   <br />
