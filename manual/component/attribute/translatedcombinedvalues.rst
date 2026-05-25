.. _component_attribute_translatedcombinedvalues:

Translated Combined Values
==========================

The "Translated Combined Values" attribute is the multilingual variant of the
:ref:`Combined Values <component_attribute_combinedvalues>` attribute. It combines
values from multiple attributes into a stored text value — separately per language.
When translated source attributes are used, the combined value is calculated using
the language-specific variant of the source fields. The values are stored in the
translation table ``tl_metamodel_translatedtext``.

Typical use cases:

* Combining first and last name in language-dependent order
  (e.g. "Hans Müller" in German, "Müller Hans" in other languages)
* Combined identifiers based on translated source attributes
* Language-specific search or display values

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_combinedvalues`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedcombinedvalues


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the translated combined values attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Format
     - ``sprintf`` format string that defines how the source values are combined.
       A placeholder ``%s`` must be present for each selected field value. Examples:

       * ``%s, %s`` → "Müller, Hans"
       * ``%s (%s)`` → "Product A (Category B)"
       * ``%s-%s-%s`` → "2024-01-15"

       All placeholder variants of ``sprintf`` are possible (see
       https://www.php.net/sprintf).
   * - Fields
     - Selection of source attributes in the order in which they are inserted into
       the format. In addition to MetaModels attributes, system meta-fields are also
       available: ID, PID, sorting, timestamp, variant group, variant base.
   * - Force update
     - If this checkbox is active, the combined value is automatically regenerated
       whenever one of the source attributes changes. The field is then displayed as
       read-only in the backend.


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
     - Selection of a custom template for the output of the combined value.
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
   * - Always save
     - The field is saved even if its value has not changed. Set automatically when
       "Force update" is active.

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
     - Free text input for searching within the combined value of the active language.
   * - Simple lookup
     - Filters by an exact or partial value via a URL parameter.
   * - Single select
     - Selection of a value from a list of existing combined entries.
   * - Multi-select
     - Multiple selection from a list of existing combined entries.
   * - Register
     - Filters by the initial letter of the combined value.
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search; requires the package ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Database storage**

The combined values are stored per language in ``tl_metamodel_translatedtext``
(fields: ``att_id``, ``item_id``, ``langcode``, ``value`` as ``varchar(255)``).
No column is created in the MetaModel table.

**Language-dependent combination**

The combination is calculated separately per language. When translated attributes are
used as source fields (e.g. "Translated text"), the language-specific values of the
respective source attribute are automatically used.

**Fallback language**

If a combined value is missing for a language, MetaModels falls back to the fallback
language.

**Uniqueness with automatic numbering**

If "Unique values" is active, MetaModels checks separately per language whether the
combined value already exists. For duplicates, a counter is automatically appended:
``Müller, Hans (2)``, ``Müller, Hans (3)``, etc.


.. |br| raw:: html

   <br />
