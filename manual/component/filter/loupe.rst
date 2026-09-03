.. _component_filter_loupe:

|svg_filt_loupe_22| |img_filter_default| Loupe
==============================================

The "Loupe" filter rule (package ``filter_loupe``, from MM 2.4) creates a full-text
index over selected attributes in a dedicated SQLite database and enables powerful
full-text search with fuzzy search (similarity search). The implementation is based on
the PHP library `Loupe <https://github.com/loupe-php/loupe>`_.

In contrast to the :ref:`Levenshtein search <component_filter_levenshtein>`, Loupe uses
a standalone SQLite database for the index and offers extended configuration options for
fuzzy distance and ranking.

.. seealso:: Detailed documentation on Loupe:
   :ref:`rst_extended_loupe`


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_loupe


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Loupe".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attributes to index
     - Selection of attributes (checkboxWizard) to be included in the Loupe search
       index. Required field.
   * - Fuzzy distance
     - MCW table that defines the allowed Levenshtein distance (fuzzy distance, 0–10)
       for different word lengths (minimum characters). |br|
       Default: word length 5 → distance 1; word length 9 → distance 2.
   * - Equalize ranking
     - If this option is active, all matches are ranked equally regardless of their
       relevance (no relevance ranking).
   * - Use formatted values
     - If this option is active, the formatted output values of the attributes are
       indexed (instead of raw values from the database).


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
   * - Ignore this filter for remaining values
     - This filter does not return its own options as a restriction when calculating
       remaining values.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Loupe" filter rule can index the following attribute types:

* :ref:`Text <component_attribute_text>`
* :ref:`Long text <component_attribute_longtext>`
* :ref:`Alias <component_attribute_alias>`
* :ref:`Combined values <component_attribute_combinedvalues>`
* :ref:`Token <component_attribute_token>`
* :ref:`Translated text <component_attribute_translatedtext>`
* :ref:`Translated long text <component_attribute_translatedlongtext>`


Special Functions
----------------

**Rebuild index**

In the filter rule list, an additional operation icon (Loupe icon) appears for Loupe
filter rules to manually rebuild the SQLite search index. The index is also automatically
updated when indexed items are changed.

**SQLite database**

The Loupe index is stored in a standalone SQLite file (not in the main Contao database).
This enables fast full-text searches even with large amounts of data.


.. |svg_filt_loupe_22| image:: /_img/icons_svg/loupe-emblem.svg
   :width: 22px
.. |img_filter_default| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
