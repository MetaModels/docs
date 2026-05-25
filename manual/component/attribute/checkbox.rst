.. _component_attribute_checkbox:

|img_checkbox| Checkbox
=======================

The "Checkbox" attribute stores a boolean value (0 or 1). Typical use cases:

* Publication status of a record (active/inactive)
* Yes/No fields (e.g. "Featured", "Show on homepage")
* Binary state values such as "Available", "Subscribed", etc.

The stored value in the database is ``'1'`` (active) or ``''`` (empty = inactive).
In the backend, the attribute appears as a checkbox widget. When the publication option is
enabled, an additional toggle button ("eye icon") is displayed in the record list.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedcheckbox` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_checkbox


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the checkbox attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Toggle icon
     - If this option is set, an additional icon ("eye") is added to the backend list view.
       This allows the publication status of a record to be toggled directly with a click.
       The column name ``published`` is typically used for this purpose. The actual filtering
       of published records in the frontend must be configured separately via a filter set
       with the "Checkbox status" filter rule.
   * - Inverted display option
     - Reverses the toggle status of the icon: an enabled checkbox (value ``1``) then shows
       the inactive symbol, a disabled one shows the active symbol. Useful for example for a
       "Hide" field analogous to the Contao content element.
   * - Custom icon
     - Enables the selection of custom icons for the backend list view. If this option is set,
       two additional fields appear:

       * **Icon active** — Icon displayed when value is ``1`` (select image file)
       * **Icon inactive** — Icon displayed when value is empty (select image file)

       Supported formats: jpg, jpeg, gif, png, tif, tiff, svg.


Settings in Render Settings
-----------------------------

The checkbox attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the checkbox value. If no template
       is specified, the output is as plain text (``1`` when active or ``''`` when inactive).

       Primarily for the list display in the backend, the template ``mm_attr_checkbox_icon``
       displays the status with UTF-8 icons as ☐ or ☑ (from MM 2.4).
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the checkbox attribute is added to an input form, the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form (e.g.
       ``w50`` for half width, ``cbx m12`` for checkbox-typical spacing).
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
     - Makes the field a required field (rarely useful for checkboxes, since an unchecked
       box already corresponds to a defined value).
   * - Submit on change
     - The input form is reloaded via Ajax as soon as the checkbox is toggled
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

The checkbox attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Checkbox status
     - Checks whether the checkbox value equals ``1``. Typically used for publication control.
       The filter rule offers two additional options:

       * **Allow override** — A URL parameter can override the filter rule
         (e.g. for preview links).
       * **Do not use filter in frontend preview** — The filter rule is skipped when a
         backend user uses the Contao frontend preview.
   * - Simple lookup
     - Filters by a specific checkbox value via a URL parameter; useful when visitors
       should be able to filter by active/inactive themselves.


Special Functions
-----------------

**Publication status (toggle icon)**

When "Toggle icon" is active, MetaModels registers a toggle operation in the backend list view.
Clicking the icon switches the record's value directly between ``1`` and ``''`` without having
to open the edit form. Filtering published records in the frontend must be done via a separate
filter set with the "Checkbox status" filter rule (package ``filter_checkbox``).

**Inverted mode**

The "Inverted display option" only reverses the *display* of the icon —
the stored value remains unchanged (``1`` = active, ``''`` = inactive).
This is useful when the semantics of a field are formulated in reverse
(e.g. "Hidden" instead of "Visible") analogous to the Contao content element.

**Database storage**

The value is stored as ``char(1) NOT NULL default ''``:
``'1'`` means active, ``''`` (empty string) means inactive.


.. |img_checkbox| image:: /_img/icons/checkbox.png

.. |br| raw:: html

   <br />
