.. _component_filter_levenshtein:

|img_filter_default| Levenshtein-based Search
=================================================

The "Levenshtein-based Search" filter rule (package ``attribute_levenshtein``) creates
a full-text index over selected attributes and enables a similarity-based full-text
search with autocomplete. The search is based on the Levenshtein distance algorithm,
which also finds typos and similar-sounding terms.

A prerequisite is the installation of the
:ref:`Levenshtein <component_attribute_levenshtein>` attribute, which builds the search
index in its own table. The included template ``mm_filteritem_levenshtein.html5``
contains the necessary JavaScript logic for autocomplete.

.. seealso:: Detailed documentation on the Levenshtein attribute:
   :ref:`component_attribute_levenshtein`


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_levenshtein


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Levenshtein-based Search".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The Levenshtein attribute that provides the search index.


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
     - Template for the widget output. Default: ``mm_filteritem_levenshtein``
       (contains JavaScript for autocomplete).
   * - Placeholder
     - Placeholder text in the search input field.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.
   * - Enable autocomplete
     - Enables the JavaScript-based autocomplete while typing. Default: active.
   * - Minimum characters for autocomplete
     - Number of characters at which autocomplete is triggered. Default: 3.
   * - Auto-submit
     - The search form is automatically submitted when an autocomplete suggestion
       is selected.


Matching Attributes
------------------

The "Levenshtein-based Search" filter rule works exclusively with the special
Levenshtein attribute:

* :ref:`Levenshtein <component_attribute_levenshtein>`

The Levenshtein attribute can in turn index multiple other attributes (text, long text,
alias, etc.).


.. |img_filter_default| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
