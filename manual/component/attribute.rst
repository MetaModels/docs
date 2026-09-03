.. _component_attribute:

|svg_fields_32| |img_fields_32| Attributes
==========================================

.. note:: Create your own columns of the database table as attributes and configure them |br|
   To create the attribute columns in the mm_* table, run a DB migration — :ref:`see schema manager <component_schema-manager>`


Introduction
------------

The "Attributes" component is one of the most fundamental settings in a MetaModel.
Attributes are used to define your own specific data fields and create them as columns in the
database table. On the page :ref:`component_data-in-attributes` you can see which attribute
can be used for which database data type. In addition to the usual data types such as ``varchar``,
``int``, ``text``, etc., there are also attributes for special storage — more about this in the
following overview.

When creating an attribute "|img_new| New attribute", the mandatory fields are the selection of
the attribute type and the entry of the column name — the column name defines, as the name
suggests, the label of the column in the database table. As additional inputs, a name and a
description can be filled in, which also appear as label and description in the input form.

.. warning:: When changing the attribute type, as well as when deleting the attribute,
  the previously entered values in the database will be deleted! If an attribute type
  must be changed while retaining the values, this should be accompanied directly at the
  database level, e.g. via export/import of the attribute column via CSV. A changed attribute
  should then be added again in the render settings and input forms.

.. seealso:: Checklist for changing an attribute type: :ref:`rst_cookbook_checklists_attribut_change`

Depending on the attribute type, further input options and settings become available after reloading
the page. The following is a list of attribute types with notes on specific options:

* **Alias**: Alias field, e.g. for URL parameters during filtering |br|
  The alias can be created as a combination of various (existing) attributes;
  as an option, the recreation of the alias when the source attributes change can be forced
  (force alias recreation); an alias is not automatically created as a unique value — for that,
  the "Unique values" checkbox must be activated — :ref:`more... <component_attribute_alias>`
* **Checkbox**: single checkbox for boolean values |br|
  The checkbox can be used to set boolean values (0|1); a special variant
  is "publish" — this displays the "eye" icon in the backend, but the filter
  for the publishing itself must be created manually; the column name "published"
  is generally used for the publication value; the "Listview checkbox" option allows
  a custom icon in the backend to display the status — :ref:`more... <component_attribute_checkbox>`
* **Combined values**: combination of various attributes |br|
  All existing attributes and "system attributes" such as ID, PID, etc. can be combined
  into a new attribute; the combination is done using sprintf formatting;
  e.g. the two attributes "Last name" and "First name" can be combined via "%s, %s" to
  "Last name, First name"; the "Force update" option forces recreation when values change
  — :ref:`more... <component_attribute_combinedvalues>`
* **Country**: country selection |br|
  This attribute provides a country selection; the selection of countries can be
  narrowed with the "Filter available countries" option — :ref:`more... <component_attribute_country>`
* **Decimal**: decimal numbers |br|
  This attribute is for storing decimal numbers such as monetary amounts;
  it has two decimal places — :ref:`more... <component_attribute_decimal>`
* **File**: file picker |br|
  The "File" attribute provides a file picker for selecting a file or, if the
  "Multiple selection" option is set, multiple files; additional file options can be
  set during selection with the "Customize the file tree" option; when using with images,
  note that for (direct) display of preview images in the backend or frontend, the
  "Use as image field with preview image" option in the render settings of the file
  attribute must be set — :ref:`more... <component_attribute_file>`
* **Long text**: text input |br|
  Attribute for longer text input — :ref:`more... <component_attribute_longtext>`
* **Numeric**: input of integer values — :ref:`more... <component_attribute_numeric>`
* **Single select [select]**: relation (1:n) to another MetaModels or Contao table |br|
  The "Select" attribute creates a 1:n relation to another table; this can be either
  a MetaModels table or any other Contao table, e.g. tl_member — :ref:`more... <component_attribute_select>`
* **Table text**: input of values as a table |br|
  The "Table text" attribute defines a number of columns including column label and
  column width; any number of rows can then be created in the input form, e.g. to
  store multiple URLs or phone numbers — :ref:`more... <component_attribute_tabletext>`
* **Multi-select [tags]**: relation (m:n) to another MetaModels or Contao table |br|
  The "Tags" attribute creates an m:n relation to another table; this can be either
  a MetaModels table or any other Contao table, e.g. tl_page;
  the relation is resolved in a special MetaModels table, so no column is created
  in the MetaModel table for this attribute — :ref:`more... <component_attribute_tags>`
* **Text**: simple text field — :ref:`more... <component_attribute_text>`
* **Timestamp**: date or date and time |br|
  Data is stored as a Unix timestamp; conversions may need to be made for custom
  SQL filters — :ref:`more... <component_attribute_timestamp>`
* **URL**: link text and URL |br|
  Entry of external links (include "\http://") or internal links via the page picker;
  optionally, "Remove title" can output only the URL — :ref:`more... <component_attribute_url>`

If the "Translation" option is activated in the MetaModel, the following attributes are
additionally available for multilingual support:

* Translated checkbox — :ref:`more... <component_attribute_translatedcheckbox>`
* Translated combined values — :ref:`more... <component_attribute_translatedcombinedvalues>`
* Translated file — :ref:`more... <component_attribute_translatedfile>`
* Translated long text — :ref:`more... <component_attribute_translatedlongtext>`
* Translated select — :ref:`more... <component_attribute_translatedselect>`
* Translated table text — :ref:`more... <component_attribute_translatedtabletext>`
* Translated tags — :ref:`more... <component_attribute_translatedtags>`
* Translated text — :ref:`more... <component_attribute_translatedtext>`

These attributes differ from their monolingual counterparts essentially through the entry of
multilingual data for name and description. Special extension tables are used for translated
attributes, not the table created by the MetaModel.

Note that for relations via "Single select" or "Multi-select" between two MetaModels with
translations, it is generally *not* necessary to select "Translated single select" or
"Translated multi-select". MetaModels automatically handles language detection and switching
with the "Single select" and "Multi-select" attributes.

The two "translated variants" are mainly intended for connecting to tables that do not belong
to MetaModels and have a separate field for the language variant — or for the special case
where different items should be selected in the referenced MetaModel depending on the language.
More on this on a special page about multilingual support — sponsors sought!

In addition to the listed attributes, further attribute types may also be available via
additional MetaModels extensions. The attributes are installed via Composer or like normal
Contao extensions by copying to the "modules" folder (depending on how they are provided by
the developer).

Examples of additional attributes are:

* **Rating**: rating module with stars |br|
  The attribute module is used to output a "star rating" in the frontend;
  various options such as the number of stars, etc. can be set in the backend
  — :ref:`more... <component_attribute_rating>`
* **Color picker**: selection of web colors and transparency — :ref:`more... <component_attribute_color>`
* **Levenshtein**: word search by Levenshtein |br|
  The attribute calculates word similarity for flexible search — :ref:`more... <component_attribute_levenshtein>`
* **Country selection**: selection list with countries — :ref:`more... <component_attribute_country>`
* **Language code**: selection of ISO language codes |br|
  This attribute provides a selection of language codes; the language codes can be
  selected via checkbox — :ref:`more... <component_attribute_langcode>`
* **Content article**: possibility to create Contao content elements similar to an article in |br|
  a widget :ref:`more... <component_attribute_contentarticle>` |br|
  also available as translated variant :ref:`more... <component_attribute_translatedcontentarticle>`
* **Multi-table**: similar to "Table text" attribute but each "cell" can have its own |br|
  widget type such as select, radio buttons, checkboxes, etc. :ref:`more... <component_attribute_tablemulti>` |br|
  also available as translated variant :ref:`more... <component_attribute_translatedtablemulti>`
* **Geo-distance**: calculates the geographic distance to the search point for a perimeter search |br|
  The value can be used to sort lists by distance — :ref:`more... <component_attribute_geodistance>`
* **LatLong** (from MM 2.5): coordinate pair (latitude/longitude) as a native ``POINT`` in a single |br|
  column, optionally with a spatial index for a faster perimeter search; input optionally via an
  address search with a map — :ref:`more... <component_attribute_latlong>`
* **Token** (from MM 2.4): unique string |br|
  Creates unique strings that do not change again — :ref:`more... <component_attribute_token>`

The order in which attributes are created is freely selectable —
only for attributes that refer to other attributes, such as "Alias" or "Combined values",
a subsequent creation makes sense.

For the "Select" and "Multi-select" attributes, the MetaModels to be referenced must also
be created first.


Options
-------

Two options are available for all attributes: "Override variants" and "Unique values".

"Override variants" makes the attribute available in the input forms for variant entry.
The prerequisite for this is that the "Variants" option is set in the MetaModel —
otherwise the checkbox is inactive.

The "Unique values" option checks attribute inputs for uniqueness.


Procedure
---------

A new attribute is created via "|img_new| New attribute". After all necessary options have
been entered or selected, the setting is saved and appears in the attribute list of the
existing MetaModels. The order in the list has no further influence.
Run the database migration!

.. seealso:: In the cookbook:

   * :ref:`rst_cookbook_checklists_attribut_new`
   * :ref:`rst_cookbook_tips_speedup_backend`


Details of All Attributes
--------------------------

.. toctree::
   :maxdepth: 1

   attribute/alias
   attribute/checkbox
   attribute/combinedvalues
   attribute/decimal
   attribute/file
   attribute/longtext
   attribute/numeric
   attribute/select
   attribute/tabletext
   attribute/tags
   attribute/text
   attribute/timestamp
   attribute/url
   attribute/translatedalias
   attribute/translatedcheckbox
   attribute/translatedcombinedvalues
   attribute/translatedfile
   attribute/translatedlongtext
   attribute/translatedselect
   attribute/translatedtabletext
   attribute/translatedtags
   attribute/translatedtext
   attribute/translatedurl
   attribute/rating
   attribute/color
   attribute/levenshtein
   attribute/country
   attribute/langcode
   attribute/contentarticle
   attribute/translatedcontentarticle
   attribute/tablemulti
   attribute/translatedtablemulti
   attribute/geodistance
   attribute/latlong
   attribute/token


.. |svg_fields_32| image:: /_img/icons_svg/fields.svg
   :width: 32px
.. |img_fields_32| image:: /_img/icons/fields_32.png
.. |img_fields| image:: /_img/icons/fields.png
.. |img_new| image:: /_img/icons/new.gif

.. |br| raw:: html

   <br />

.. |nbsp| unicode:: 0xA0
   :trim:
