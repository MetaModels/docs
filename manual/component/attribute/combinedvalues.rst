.. _component_attribute_combinedvalues:

|svg_attr_combinedvalues_22| |img_combinedvalues| Combined Values
=================================================================

The "Combined values" attribute combines values from multiple existing attributes into a new,
stored text value. Typical use cases:

* Combining first and last name into "Last name, First name" for display or search
* Creating composite identifiers from multiple fields, e.g. for the attribute
  :ref:`Single select <component_attribute_select>` or :ref:`Multi-select <component_attribute_tags>`
* Preparing output texts that should be stored as fixed combinations

The combination is done via a ``sprintf`` format string. All existing MetaModels attributes
as well as system meta fields (ID, PID, sorting, etc.) can be selected as source fields.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedcombinedvalues` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_combinedvalues


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, unique values,
override variants), the attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Format
     - ``sprintf`` format string that defines how the source values are combined.
       A placeholder ``%s`` must be present for each selected field value. Examples:

       * ``%s, %s`` → "Smith, John"
       * ``%s (%s)`` → "Product A (Category B)"
       * ``%s-%s-%s`` → "2024-01-15"

       All placeholder variants of ``sprintf`` are possible (see
       https://www.php.net/sprintf).
   * - Fields
     - Selection of the source attributes in the order in which they are inserted into
       the format. In addition to MetaModels attributes, system meta fields are also
       available: ID, PID, sorting, timestamp, variant group, variant base.
   * - Force update
     - If this checkbox is active, the combined value is automatically regenerated every
       time one of the source attributes changes. The field is then displayed as read-only
       in the backend. Without this option, a once-generated value remains unchanged.


Settings in Render Settings
-----------------------------

The "Combined values" attribute has no specific render settings. In the attribute list of a
render setting, the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the combined value.
       If no template is specified, the output is as plain text.
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
     - Makes the field a required field. If "Unique values" is enabled, this is set automatically.
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

The "Combined values" attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Text search
     - Free text input for searching in the combined value.
   * - Simple lookup
     - Filters by an exact or partial value via a URL parameter.
   * - Single select
     - Selection of a value from a list of existing combined entries.
   * - Multi-select
     - Multiple selection from a list of existing combined entries.
   * - Register
     - Filters by initial letters of the combined value.
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search; requires the package ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Uniqueness with automatic numbering**

If "Unique values" is active, MetaModels checks after generation whether the combined value
already exists. If a duplicate is found, a counter in parentheses is automatically appended:
``Smith, John (2)``, ``Smith, John (3)``, etc.

**Behavior when copying a record**

If "Force update" is active, no value is carried over when duplicating a record. The combined
value is automatically regenerated when the new record is saved (``doNotCopy`` behavior).

**Format placeholders**

All ``sprintf`` placeholders can be used, not just ``%s``. For example, ``%05d`` formats
a number with leading zeros to 5 digits. The number of placeholders must match the number
of selected fields.

**Database storage**

The combined value is stored as ``text NULL``. An empty value is stored as ``NULL``
(compatible with MySQL Strict Mode).


.. |svg_attr_combinedvalues_22| image:: /_img/icons_svg/combinedvalues.svg
   :width: 22px
.. |img_combinedvalues| image:: /_img/icons/combinedvalues.png

.. |br| raw:: html

   <br />
