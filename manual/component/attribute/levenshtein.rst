.. _component_attribute_levenshtein:

|svg_attr_levenshtein_22| |img_levenshtein| Levenshtein
=======================================================

The "Levenshtein" attribute creates a full-text search index for selected MetaModels attributes
and enables similarity search with configurable error tolerance (typos, similar spellings).
Typical use cases:

* Product search with tolerance for typos
* Name search (e.g. "Meyer" also finds "Mayer", "Maier")
* Full-text search across multiple attributes simultaneously

The attribute itself does not store any data values, but creates and maintains a separate
search index. The actual search is performed via a dedicated filter rule in the frontend.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_levenshtein


Settings when Creating the Attribute
--------------------------------------

The Levenshtein attribute offers the following specific settings:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Attributes to index
     - Selection of all MetaModels attributes whose values should be included in the
       search index. Multiple attributes can be selected simultaneously (checkbox list).
   * - Maximum Levenshtein distance
     - Multi-column wizard for configuring the allowed deviation per minimum word length.
       For each minimum length, the maximum Levenshtein distance is defined:

       * **Minimum word length** — From which word length the distance applies
       * **Allowed distance** — Number of allowed character deviations (0–10)

       Default: words from 3 characters → distance 1, from 5 characters → 2,
       from 9 characters → 5.
   * - Minimum word length
     - Words with fewer characters than this value are not included in the search index
       (default: ``3``).
   * - Maximum word length
     - Words with more characters than this value are not included in the search index
       (default: ``20``).


Settings in Render Settings
-----------------------------

The attribute has no specific render settings, as it contains no data values to display.
It is typically not listed in render settings.


Settings in the Input Form
----------------------------

The Levenshtein attribute does not appear as an input field in the input form —
the search index is automatically updated when records are saved.


Filter Rules
------------

The Levenshtein attribute provides its own filter rule:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Levenshtein-based search
     - Similarity search in the search index with typo tolerance. Supports wildcards
       (``*`` and ``?``). In the filter settings, a URL parameter, a template, and
       autocomplete options (minimum character count, automatic submission) can be configured.


Special Functions
-----------------

**Storage**

The attribute does not store any values of its own in the MetaModel table. The search index
is stored in two dedicated tables: ``tl_metamodel_levenshtein`` holds one entry per indexed
record, while ``tl_metamodel_levenshtein_index`` holds the words derived from it (columns:
``word``, ``transliterated``). The MetaModel table does not receive its own column.

.. note:: Up to MetaModels 2.4, the attribute type, the two tables, and two columns in
   ``tl_metamodel_attribute`` were misspelled as ``levensthein`` (``h`` and ``t`` swapped).
   From MetaModels 2.5 onward, everything consistently uses ``levenshtein``; a migration
   automatically renames existing installations while preserving the search index — see
   :ref:`new_in_mm250`.

**Index update**

The search index is automatically updated via a ``modelSaved`` hook whenever a MetaModels
record is saved. Via the backend list operation "Rebuild index" (icon in the attribute list),
the index can be completely regenerated manually.

**Autocomplete**

The filter rule supports autocomplete in the frontend. A minimum character count can be
configured in the filter settings from which suggestions appear, as well as whether the
form is submitted automatically.

**Stop words**

Very short or common words (stop words) are not indexed. In addition to the configured
minimum length, the extension contains a stop word list (e.g. English articles like "a",
"an", "are").

**Language support**

The index is created language-aware — for multilingual MetaModels, the active language is
taken into account when building the index.


.. |svg_attr_levenshtein_22| image:: /_img/icons_svg/levenshtein.svg
   :width: 22px
.. |img_levenshtein| image:: /_img/icons/levenshtein.png
.. |br| raw:: html

   <br />
