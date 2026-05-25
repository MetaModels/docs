.. _component_filter_text:

|img_filter_text| Text Filter
=============================

The "Text Filter" filter rule (package ``filter_text``) filters items based on a text
input in the frontend. Visitors enter a search term into a text input field, and items
are filtered by matches with the value of the selected attribute. Various search modes
allow precise or flexible searches, including regular expressions.

Typical use cases: Free text search in title fields, keyword search in descriptions,
or combined searches with multiple filter rules.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_text


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Text Filter".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The text attribute to search within.


Settings for the Frontend Widget
--------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - URL parameter
     - The key of the URL parameter for passing the filter value. Without input,
       the column name of the attribute is used. With ``auto_item``, only the value —
       without key — is embedded in the URL.
   * - URL type for the parameter
     - Defines whether the parameter is passed as a slug (friendly URL) or as a
       GET parameter (from MM 2.4) — :ref:`see SEO <rst_cookbook_tips_seo_filter-url>`
   * - Label
     - Label of the search input field.
   * - Hide label at filter widget
     - Suppresses the output of the label.
   * - Template
     - Template for the widget output. Default: ``mm_filteritem_default``.
   * - Search mode
     - Defines how the search term is compared with the attribute value:

       * **Exact** — The search term must exactly match the attribute value.
       * **Begins with** — The attribute value must begin with the search term.
       * **Ends with** — The attribute value must end with the search term.
       * **Any words** — At least one of the words separated by delimiters must be
         contained in the attribute value. |br|
         Additional option: **Delimiter** (e.g. space or comma).
       * **All words** — All words separated by delimiters must be contained in the
         attribute value. |br|
         Additional option: **Delimiter**.
       * **Regular expression** — The attribute value is checked against a regular
         expression. |br|
         Additional option: **Pattern** (e.g. ``%s`` for the search term).
   * - Placeholder
     - Placeholder text displayed in the input field when it is empty.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Text Filter" filter rule is suitable for attributes that store text values:

* :ref:`Text <component_attribute_text>`
* :ref:`Long Text <component_attribute_longtext>`
* :ref:`Alias <component_attribute_alias>`
* :ref:`Combined Values <component_attribute_combinedvalues>`
* :ref:`Token <component_attribute_token>`
* :ref:`Translated Text <component_attribute_translatedtext>`
* :ref:`Translated Long Text <component_attribute_translatedlongtext>`


Special Functions
----------------

**Regular expressions**

In the "Regular expression" search mode, a PHP regex pattern can be specified.
The placeholder ``%s`` is replaced by the entered search term.
Example: ``^%s`` searches for values that begin with the search term.

**Multi-word search with delimiter**

In the "Any words" and "All words" modes, the search term is split into individual
words using the configured delimiter before each word is searched separately.


.. |img_filter_text| image:: /_img/icons/filter_text.png

.. |br| raw:: html

   <br />
