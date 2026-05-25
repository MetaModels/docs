.. _component_attribute_translatedcheckbox:

Translated Checkbox
===================

The "Translated Checkbox" attribute is the multilingual variant of the
:ref:`Checkbox <component_attribute_checkbox>` attribute. It stores a separate boolean
value (0 or 1) per language. The values are stored in the dedicated translation table
``tl_metamodel_translatedcheckbox``.

Typical use cases:

* Language-dependent publication (e.g. published in German, not yet in English)
* Yes/No fields that can be set differently per language

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_checkbox`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedcheckbox


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the translated checkbox attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Toggle icon
     - Adds an additional icon ("eye") in the backend list view to toggle the status
       directly (language-dependent). The column name ``published`` is typically used.
   * - Inverted display option
     - Reverses the toggle icon status: an active checkbox (value ``1``) then shows the
       inactive symbol, a deactivated one shows the active symbol. Useful e.g. for a
       "Hide" field analogous to Contao content elements.
   * - Custom icon
     - Enables the selection of custom icons. Unlike the monolingual variant, the icons
       can be set **separately per language** (multi-column wizard with language selection,
       active icon, and inactive icon).


Settings in Render Settings
-----------------------------

In the attribute list of a render setting, the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the checkbox value. If no template
       is specified, the output is plain text (``1`` when active or empty when inactive).

       Primarily for list display in the backend, the template ``mm_attr_checkbox_icon``
       shows the status with UTF-8 icons as ☐ or ☑ (from MM 2.4).
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
     - CSS classes for the display (e.g. ``w50 cbx m12``).
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
   * - Submit on change
     - The input form is reloaded via Ajax when the checkbox is toggled
       (``submitOnChange``). The data is not yet saved at this point.

**Overview (backend filter)**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Filterable
     - The attribute is available in the backend as a filter criterion.


Filter Rules
------------

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Translated checkbox status
     - Checks whether the checkbox value in the active language is equal to ``1``.
       Typically used for language-dependent publication control.


Special Functions
-----------------

**Database storage**

The values are stored per language in ``tl_metamodel_translatedcheckbox``
(fields: ``att_id``, ``item_id``, ``langcode``, ``value`` as ``char(1)``).
No column is created in the MetaModel table.

**Language-dependent icons**

The custom icons for active/inactive can be chosen differently for each language version —
e.g. a DE flag for the German and a GB flag for the English publication.

**Fallback language**

If a value is missing for a language, MetaModels falls back to the fallback language.
IDs without a value in the fallback language are treated as inactive (``''``).


.. |br| raw:: html

   <br />
