.. _component_data-in-attributes:

Storing Data Types as Attributes
==================================

When planning how to structure your MetaModels, in addition to the :ref:`database structure
<component_relations_database_structure>`, it is important to know what options are available for
storing your real data such as texts, numbers, dates, postal codes, etc. Both the data types of the
database (MySQL/MariaDB) and the input options via Contao widgets must be taken into account.

The following is an overview of which attributes can be used to store the desired data. Additionally,
"filter rule" indicates which :ref:`filter rules for filtering/search <component_filter>` can be used
in the frontend. It also shows which attributes are available for :ref:`frontend editing (FEE)
<rst_extended_frontend_editing>` (✔) — additional repositories may need to be installed (🗹).

Texts
-----

.. csv-table::
   :header: "Data type", "Attribute", "Package name", "Filter rule", "FEE", "Note"
   :widths: 10, 10, 10, 10, 10, 10

    "Short texts", ":ref:`Text <component_attribute_text>`", `attribute_text <https://github.com/MetaModels/attribute_text>`_, "Text search, |br| Single select, |br| Multi-select, |br| Simple lookup, |br| Register, |br| Levenshtein, |br| Loupe", "✔", "up to 255 characters; |br| number of attribute limited |br| by DB"
    "Long texts", ":ref:`Long text <component_attribute_longtext>`", `attribute_longtext <https://github.com/MetaModels/attribute_longtext>`_, "Text search, |br| Levenshtein, |br| Loupe", "✔", "up to 65535 characters; |br| :ref:`adjustable <rst_cookbook_inputmask_manipulate-select-values>`; |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"
    "Text as alias", ":ref:`Alias <component_attribute_alias>`", `attribute_alias <https://github.com/MetaModels/attribute_alias>`_, "Text search, |br| Simple lookup, |br| Levenshtein, |br| Loupe", "✔", "up to 255 characters; |br| Generation from one |br| or more attributes"
    "Combined values", ":ref:`Combined values <component_attribute_combinedvalues>`", `attribute_combinedvalues <https://github.com/MetaModels/attribute_combinedvalues>`_, "Text search, |br| Single select, |br| Multi-select, |br| Simple lookup, |br| Register, |br| Levenshtein, |br| Loupe", "✔", "up to 255 characters; |br| Result string definable via ``sprintf``"
    "Text as table", ":ref:`Table text <component_attribute_tabletext>`", `attribute_tabletext <https://github.com/MetaModels/attribute_tabletext>`_, "Levenshtein, |br| Loupe", ":ref:`🗹 <rst_extended_frontend_editing_installation>`", "up to 255 characters per cell"
    "Text as URL", ":ref:`URL <component_attribute_url>`", `attribute_url <https://github.com/MetaModels/attribute_url>`_, "Levenshtein, |br| Loupe", ":ref:`🗹 <rst_extended_frontend_editing_installation>`", "up to 255 characters; |br| Character count for title and URL"
    "Text as token", ":ref:`Token <component_attribute_token>`", `attribute_token <https://github.com/MetaModels/attribute_token>`_, "Text search, |br| Single select, |br| Multi-select, |br| Simple lookup, |br| Register, |br| Levenshtein, |br| Loupe", "✔", "up to 255 characters; |br| Character count incl. optional prefix"
    "*Multilingual*"
    "Short texts multilingual", ":ref:`Translated text <component_attribute_translatedtext>`", `attribute_translatedtext <https://github.com/MetaModels/attribute_translatedtext>`_, "see Text", "✔", "up to 255 characters"
    "Long texts multilingual", ":ref:`Translated long text <component_attribute_translatedlongtext>`", `attribute_translatedlongtext <https://github.com/MetaModels/attribute_translatedlongtext>`_, "see Long text", "✔", "see Long text; |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"
    "Text as alias multilingual", ":ref:`Translated alias <component_attribute_translatedalias>`", `attribute_translatedalias <https://github.com/MetaModels/attribute_translatedalias>`_, "see Alias", "✔", "see Alias"
    "Combined |br| values multilingual", ":ref:`Translated combined values <component_attribute_translatedcombinedvalues>`", `attribute_translatedcombinedvalues <https://github.com/MetaModels/attribute_translatedcombinedvalues>`_, "see |br| Combined values", "✔", "see Combined values"
    "Text as |br| table multilingual", ":ref:`Translated table text <component_attribute_translatedtabletext>`", `attribute_translatedtabletext <https://github.com/MetaModels/attribute_translatedtabletext>`_, "Levenshtein, |br| Loupe", ":ref:`🗹 <rst_extended_frontend_editing_installation>`", "see Table text"
    "Text as |br| URL multilingual", ":ref:`Translated URL <component_attribute_translatedurl>`", `attribute_translateurl <https://github.com/MetaModels/attribute_translateurl>`_, "Levenshtein, |br| Loupe", ":ref:`🗹 <rst_extended_frontend_editing_installation>`", "see URL"

Numbers
-------

.. csv-table::
   :header: "Data type", "Attribute", "Package name", "Filter rule", "FEE", "Note"
   :widths: 10, 10, 10, 10, 10, 10

    "Integer values", ":ref:`Numeric <component_attribute_numeric>`", `attribute_numeric <https://github.com/MetaModels/attribute_numeric>`_, "Value from/to for one attribute, |br| Value from/to for two attributes", "✔", "for postal codes or phone numbers use |br| Text attribute"
    "Decimal numbers", ":ref:`Decimal <component_attribute_decimal>`", `attribute_decimal <https://github.com/MetaModels/attribute_decimal>`_, "Value from/to for one attribute, |br| Value from/to for two attributes", "✔", "Input with period as decimal separator"
    "Date or time", ":ref:`Timestamp <component_attribute_timestamp>`", `attribute_timestamp <https://github.com/MetaModels/attribute_timestamp>`_, "Value from/to for one date attribute, |br| Value from/to for two date attributes", "✔", "Stored as UNIX timestamp; |br| Input can be limited to date only |br| or time only"
    "Geo coordinates (combined)", ":ref:`LatLong <component_attribute_latlong>`", `attribute_latlong <https://github.com/MetaModels/attribute_latlong>`_, "Perimeter search", "**—**", "Stored as native ``POINT``; |br| optional spatial index for faster |br| perimeter search; input optionally |br| via address search with map"
    "Geo coordinates (separate)", "see Decimal", , "Perimeter search", "**—**", "create one attribute each |br| for latitude and longitude"

Files
-----

.. csv-table::
   :header: "Data type", "Attribute", "Package name", "Filter rule", "FEE", "Note"
   :widths: 10, 10, 10, 10, 10, 10

    "File", ":ref:`File <component_attribute_file>`", `attribute_file <https://github.com/MetaModels/attribute_file>`_, , "✔ Upload", "searchable in BE by filename or UUID; |br| :ref:`image size selectable <rst_cookbook_templates_fe_work_with_images>` for image output; |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"
    "*Multilingual*"
    "File multilingual", ":ref:`Translated file <component_attribute_translatedfile>`", `attribute_translatedfile <https://github.com/MetaModels/attribute_translatedfile>`_, , "✔ Upload", "see File; |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"

Transfer e.g. to a :ref:`Rocksolid Slider <rst_cookbook_templates_fe_template_ce_elements_rstslider>`.

Boolean Value
-------------

.. csv-table::
   :header: "Data type", "Attribute", "Package name", "Filter rule", "FEE", "Note"
   :widths: 10, 10, 10, 10, 10, 10

    "Boolean value", ":ref:`Checkbox <component_attribute_checkbox>`", `attribute_checkbox <https://github.com/MetaModels/attribute_checkbox>`_, "Checkbox status", "✔", "Display in BE list as toggle icon possible"
    "*Multilingual*"
    "Boolean value multilingual", ":ref:`Translated checkbox <component_attribute_translatedcheckbox>`", `attribute_translatedcheckbox <https://github.com/MetaModels/attribute_translatedcheckbox>`_, "Translated checkbox status", "✔", "see Checkbox"


Relations
---------

.. csv-table::
   :header: "Data type", "Attribute", "Package name", "Filter rule", "FEE", "Note"
   :widths: 10, 10, 10, 10, 10, 10

    "1:n", ":ref:`Single select [select] <component_attribute_select>`", `attribute_select <https://github.com/MetaModels/attribute_select>`_, "Single select, |br| Filter on attribute of model with a relation", "✔", "Relation to another table for one value |br| MM tables or other Contao tables"
    "m:n", ":ref:`Multi-select [tags] <component_attribute_tags>`", `attribute_tags <https://github.com/MetaModels/attribute_tags>`_, "Multi-select, |br| Filter on attribute of model with a relation", "✔", "Relation to another table for multiple values |br| MM tables or other Contao tables"
    "*Multilingual* |br| Single and multi-select can |br| inherently handle multilingual MMs"
    "1:n", ":ref:`Translated single select [select] <component_attribute_translatedselect>`", `attribute_translatedselect <https://github.com/MetaModels/attribute_translatedselect>`_, "Single select", "✔", "only for special cases with own column for language key"
    "m:n", ":ref:`Translated multi-select [tags] <component_attribute_translatedtags>`", `attribute_translatedtags <https://github.com/MetaModels/attribute_translatedtags>`_, "Multi-select", "✔", "only for special cases with own column for language key"

Further information can be found on the page :ref:`component_relations`.

Further Data
------------

.. csv-table::
   :header: "Data type", "Attribute", "Package name", "Filter rule", "FEE", "Note"
   :widths: 10, 10, 10, 10, 10, 10

    "Color value", ":ref:`Color picker <component_attribute_color>`", `attribute_color <https://github.com/MetaModels/attribute_color>`_, , ":ref:`🗹 <rst_extended_frontend_editing_installation>`", "Opacity/transparency also selectable; |br| sorting by color possible; |br| :ref:`see attribute Color <rst_extended_attribute_color>`"
    "Content elements", ":ref:`Content of an article <component_attribute_contentarticle>`", `attribute_contentarticle <https://github.com/MetaModels/attribute_contentarticle>`_, , "**—**", "multiple content elements like an article; |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"
    "Country names", ":ref:`Country <component_attribute_country>`", `attribute_country <https://github.com/MetaModels/attribute_country>`_, , "✔", "available countries can be restricted"
    "Language codes", ":ref:`Language code <component_attribute_langcode>`", `attribute_langcode <https://github.com/MetaModels/attribute_langcode>`_, , "✔", "available languages can be restricted"
    "Geo distance", ":ref:`Geo distance <component_attribute_geodistance>`", `attribute_geodistance <https://github.com/MetaModels/attribute_geodistance>`_, , "**—**", "additional info for sorting |br| perimeter search results"
    "Star rating", ":ref:`Rating <component_attribute_rating>`", `attribute_rating <https://github.com/MetaModels/attribute_rating>`_, , "**—**", "number of stars selectable"
    "MCW table", ":ref:`Table multi (MCW) <component_attribute_tablemulti>`", `attribute_tablemulti <https://github.com/MetaModels/attribute_tablemulti>`_, , ":ref:`🗹 <rst_extended_frontend_editing_installation>`", ":ref:`see attribute for Multi-Column-Wizard <rst_extended_attribute_mcw>`; |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"
    "Pin for Cowegis map", "Cowegis marker", `cowegis-layer <https://github.com/MetaModels/cowegis-layer>`_, , "✔", ":ref:`see Cowegis layer integration for marker <extended_cowegis-layer-marker>`"
    "*Multilingual*"
    "Content elements |br| multilingual", ":ref:`Translated content of an article <component_attribute_translatedcontentarticle>`", `attribute_translatedcontentarticle <https://github.com/MetaModels/attribute_translatedcontentarticle>`_, , "**—**", "see Content of an article; |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"
    "MCW table |br| multilingual", ":ref:`Translated table multi (MCW) <component_attribute_translatedtablemulti>`", `attribute_translatedtablemulti <https://github.com/MetaModels/attribute_translatedtablemulti>`_, , ":ref:`🗹 <rst_extended_frontend_editing_installation>`", "see Table multi (MCW); |br| :ref:`File-Usage <rst_extended_file-usage>` ✔"

Output e.g. as :ref:`CE-YouTube <rst_cookbook_templates_fe_template_ce_elements_youtube>`.

.. |br| raw:: html

   <br />
