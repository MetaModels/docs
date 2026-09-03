.. _component_attribute_alias:

|svg_attr_alias_22| |img_alias| Alias
=====================================

The "Alias" attribute generates a unique, URL-compatible short identifier derived from one or more
existing attributes. Typical use cases:

* URL parameters in frontend filtering (e.g. ``/products/my-product``)
* Readable, stable identifiers for deep links or :ref:`SEO URLs <rst_cookbook_tips_seo_url>`
* Unique slugs automatically generated from names or titles

The alias is automatically composed from the configured source attributes when saving a record.
A character set and a conversion language can be specified so that special characters
(e.g. umlauts) are correctly converted.

.. note:: An alias is not automatically unique. To ensure uniqueness, the "Unique values"
   option must be enabled in the general attribute settings.

.. warning:: If the "Force alias regeneration" option is enabled, existing alias values will be
   regenerated every time the source attributes change. This may invalidate already-published URLs.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_alias


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, unique values,
override variants), the alias attribute offers the following specific options:

**Alias Fields**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Alias fields
     - Selection of one or more attributes from which the alias is built.
       In addition to MetaModels attributes, system meta fields such as ID,
       PID, sorting, or timestamp are also available. If multiple fields are
       selected, their values are joined with a hyphen.

**Generation and Character Set**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Force alias regeneration
     - If this checkbox is enabled, the alias is automatically regenerated every time
       one of the source attributes changes. The alias field is then displayed as
       read-only in the backend. Without this option, a once-generated alias remains unchanged.
   * - Valid alias characters
     - Defines which character set is used for automatic generation.
       Possible values: *Unicode digits and lowercase letters* (default),
       *Unicode digits and letters*, *ASCII digits and lowercase letters*,
       *ASCII digits and letters*.
   * - Conversion language
     - ISO 639-1 language code (e.g. ``de`` or ``de-DE``) used to convert special
       characters during generation. If left empty, the default conversion is used.
   * - Alias prefix
     - Optional text prepended to the generated alias (only alphanumeric characters allowed).
   * - Alias postfix
     - Optional text appended to the generated alias.
   * - No integer prefix
     - If this checkbox is active (default), no ``id-`` prefix is prepended when the
       resulting alias is purely numeric.


Settings in Render Settings
-----------------------------

The alias attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the alias value. If no template
       is specified, the output is as plain text.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the alias attribute is added to an input form (DCA setting), the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form (e.g.
       ``w50`` for half width, ``clr`` for line break, ``long`` for full width).
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
     - Makes the field a required field. If "Unique values" is active in the attribute
       settings, this option is set automatically.
   * - Always save
     - The field is saved even if its value has not changed. If "Force alias regeneration"
       is active, this option is also set automatically.

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

The alias attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Simple lookup
     - Filters records by an exact or partial alias value; the alias column name is used as the
       URL parameter by default (e.g. ``?alias=my-product`` or with ``auto_item`` as
       ``/my-product``) — see e.g. :ref:`Detail page <mm_first_contentelements_detailpage>`.
   * - Text search
     - Free text input in the frontend for searching within the alias field.
   * - Levenshtein-based search
     - Similarity search with typo tolerance; requires the package ``attribute_levenshtein``.
   * - Loupe
     - Full-text index search based on an SQLite database; requires the package
       ``filter_loupe`` (from MM 2.4).


Special Functions
-----------------

**Insert tags in source attributes**

If a source attribute contains insert tags (e.g. ``{{env::page}}``), these are resolved before
slug generation. This allows dynamic alias components to be incorporated.

**Behavior when copying a record**

If "Force alias regeneration" is active, an existing alias value is not transferred when
duplicating a record in the backend. Instead, a new alias is automatically generated after
saving (``doNotCopy`` behavior).

**Uniqueness with automatic suffix assignment**

If "Unique values" is active, the slug generator checks after generation whether the alias
already exists. If a duplicate is found, Contao automatically appends a numeric suffix
(e.g. ``my-product-2``, ``my-product-3``, etc.) until a unique value is found.

**Database storage**

The alias is stored as ``varchar(255) NULL`` in the MetaModel table. An empty value is
stored as ``NULL`` (compatible with MySQL Strict Mode).

**Beautifier special characters**

The character sequences ``[-]``, ``[zwsp]``, ``&shy;``, and ``&ZeroWidthSpace;`` (conditional
hyphens used in Contao for text formatting) are automatically removed before slug generation
so they do not appear in the alias.


.. |svg_attr_alias_22| image:: /_img/icons_svg/alias.svg
   :width: 22px
.. |img_alias| image:: /_img/icons/alias.png

.. |br| raw:: html

   <br />
