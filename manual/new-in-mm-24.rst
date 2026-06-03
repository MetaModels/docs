.. _new_in_mm240:

Changes and Features in MM 2.4
===============================

The following is an overview of the changes and features in MetaModels 2.4, made possible by the
"early adopter program" — more information under Fundraising on the
`MM website <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-4>`_.

For a checklist after upgrading to MM 2.4, see :ref:`further notes below <check_upgrade_mm240>`.

.. note:: To create mm_* tables and attribute columns, a DB migration must be performed —
   see :ref:`Schema Manager <component_schema-manager>`. |br|
   After creating or modifying the labels of models, attributes, or legends, please clear the (translation) cache —
   see :ref:`component_translations`.


General and Core
----------------

With Contao 5, a new version of Symfony comes into play and the minimum PHP version has been set to 8.2. With
Contao 5, the most noticeable change is the slightly revised backend with full width and new icons. The new
`widget width specifications <https://docs.contao.org/dev/reference/dca/palettes/#arranging-fields>`_ in the
input mask such as "w25" or "w66" can of course also be used in MM. MetaModels supports the "Dark Mode" in the
backend, including icon variants with the suffix "--dark".

For custom modifications or programming, several things must be considered that have changed in Contao, such as
the deprecations from `C 4.13 <https://github.com/contao/contao/blob/4.13/DEPRECATED.md>`_
and `C 5 <https://github.com/contao/contao/blob/5.x/DEPRECATED.md>`_, absolute path specifications for files
like icons or CSS/JS, or full method call signatures e.g. ``\Contao\Input::get('myvariable')``.

For image sizes, various default presets such as "Center-Center" no longer exist — define custom image sizes
and adjust them e.g. in the render settings.

Links in the CE and module to model, filter, etc. now open in a separate browser tab.

The MM core now supports a custom
"`route_prefix <https://docs.contao.org/5.x/manual/de/system/einstellungen/#zus%C3%A4tzliche-backend-einstellungen>`_",
to call the backend with e.g. ``admin/`` instead of ``contao/``.

Support for Content Security Policy (CSP) has been added for secure JavaScript and inline styles in frontend output —
`more in the November 2025 newsletter
<https://now.metamodel.me/de/mm-eap-newsletter-2-4/details/eap-info-mm-2-4-november-i-2025>`_

A link picker for TinyMCE on detail pages can be configured — see
:ref:`rst_cookbook_specials_picker-for-tinymce`.

The "Contao File Usage" extension is supported for searching used files — see
:ref:`rst_extended_file-usage`.


Multilingualism
---------------

With MM 2.4, some design principles for multilingualism have been implemented more consistently and corrected —
:ref:`more on the multilingualism architecture in MetaModels <component_multi-language>`

The adjustments include the strict output of fallback language content when the target translation language has
no own content — this applies for example also to the File and Content Article attributes.

When multilingual records are copied, all other languages are now copied in addition to the fallback language.

The backend display of which language is the fallback language has been improved. When switching from the fallback
language to the target translation language in the input mask, the attributes now show which content will be
output — i.e. whether |img_fallback| or |img_translated|.

For translations, a :ref:`translation tool for DeepL & Co. <rst_extended_translator-bridge>` has been added
to complement the :ref:`XLIFF Export/Import <rst_extended_xliff_ex-import>`.


Attributes
----------

* Checkbox
    * Dark mode support for icons — create an additional icon file with the suffix "--dark"
    * Template ``mm_attr_checkbox_icon.html5`` for displaying in the backend as ☑ or ☐ in list view
* Cowegis Marker (NEW)
    * Selection of markers in Cowegis for display on a map —
      :ref:`see "Cowegis Layer Integration for Markers" <extended_cowegis-layer-marker>`
* File
    * Template adjustments for output of `title`, `alt`, `caption` from the `metafile` node
    * Two new templates: ``mm_attr_file_contao_image.html5`` for standard output as in Contao, including
      JSON-LD data output, and ``mm_attr_file_contao_image_ofpage.html5`` for standard output as in Contao
      where the first image is output as ``primaryImageOfPage``; see also
      :ref:`SEO adjustments <rst_cookbook_tips_seo_structured-data>`
* Single Select [select]
    * Support for use with popup widget in a child table
* Content Article
    * Support for use in a child table
    * Template change — passing an array of content objects
* Combined Values
    * Option "Always save" (alwaysSave) activated — saves even without value changes
* Country
    * Country code notation changed to uppercase letters
* Long Text
    * Migration for `basicEntities` — `see Contao manual <https://docs.contao.org/manual/de/artikelverwaltung/insert-tags/#basic-entities>`_
* Multi Select [tags]
    * Support for use in a child table
* Multi-Table (MCW)
    * Support for ``'inputType' => 'fileTree'`` with ``'multiple' => 'true'`` including file moving
* Text
    * Migration for `basicEntities` — `see Contao manual <https://docs.contao.org/manual/de/artikelverwaltung/insert-tags/#basic-entities>`_
* Token (NEW)
    * Generates a cryptographically random, immutable string (token) when a record is first saved —
      see :ref:`component_attribute_token`
* Translated Alias
    * Column ``langcode`` changed to ``varchar(64)``
* Translated Checkbox
    * Dark mode support for icons — create an additional icon file with the suffix "--dark"
    * Template ``mm_attr_translatedcheckbox_icon.html5`` for displaying in the backend as ☑ or ☐ in list view
    * Column ``langcode`` changed to ``varchar(64)``
* Translated File
    * Template adjustments for output of `title`, `alt`, `caption` from the `metafile` node
    * Two new templates: ``mm_attr_file_contao_image.html5`` for standard output as in Contao, including
      JSON-LD data output, and ``mm_attr_file_contao_image_ofpage.html5`` for standard output as in Contao
      where the first image is output as ``primaryImageOfPage``; see also
      :ref:`SEO adjustments <rst_cookbook_tips_seo_structured-data>`
    * Column ``langcode`` changed to ``varchar(64)``
* Translated Content Article
    * Support for use in a child table
    * Column ``mm_lang`` changed to ``varchar(64)``
    * Template change — passing an array of content objects
* Translated Combined Values
    * Option "Always save" (alwaysSave) activated — saves even without value changes
* Translated Long Text
    * Migration for `basicEntities` — `see Contao manual <https://docs.contao.org/manual/de/artikelverwaltung/insert-tags/#basic-entities>`_
    * Column ``langcode`` changed to ``varchar(64)``
* Translated Multi-Table (MCW)
    * Support for ``'inputType' => 'fileTree'`` with ``'multiple' => 'true'`` including file moving
    * Column ``langcode`` changed to ``varchar(64)``
* Translated Text Table
    * Column ``langcode`` changed to ``varchar(64)``
* Translated Text
    * Migration for `basicEntities` — `see Contao manual <https://docs.contao.org/manual/de/artikelverwaltung/insert-tags/#basic-entities>`_
    * Column ``langcode`` changed to ``varchar(64)``
* Translated URL
    * Column name changed from ``language`` to ``langcode`` — migration available
    * Column ``langcode`` changed to ``varchar(64)``


Filter
------

All filter rules that generate a URL now have a new setting ("URL type for parameter") to specify whether
parameters should appear as slug or GET parameters in the URL. For backwards compatibility, after an upgrade
the setting is "Slug or GET" — this setting is deprecated and should be changed for each corresponding
filter rule to either Slug OR GET. More on this in the
:ref:`SEO tips <rst_cookbook_tips_seo_filter-url>`

* Simple Lookup
    * If the "Static parameter" option is set, a value can now be selected as a preset in both the
      CE/module MM list and in the MM filter for the filter rule — see `Ticket #345 <https://github.com/MetaModels/core/issues/345>`_
* Expression Rule (New)
    * With the "Expression" filter rule, the execution of further filter rules can be tied to conditions —
      see :rel:`rst_cookbook_filter_expression-rule`
* Filter-by-related
    * :ref:`Replaces the "Filter-Parent" filter <rst_extended_filter_by_related>`
    * The filter rule allows filtering items by properties of a related (relation) MetaModel. A single
      select (Select) or child table can be used as relations.
* Perimeter Search
    * The map providers ``GoogleMaps`` and ``OpenStreetMaps`` now require an ``HttpClientInterface`` as parameter
* Value from/to for one attribute (from-to)
    * Min and max values are available in the template as ``optionsMin`` and ``optionsMax``
    * New template with type ``date`` as ``mm_filteritem_datepicker.html5``
* Value from/to for two attributes (range)
    * Min and max values are available in the template as ``optionsMin`` and ``optionsMax``
    * New template with type ``date`` as ``mm_filteritem_datepicker.html5``
* Full-text search with "Loupe"
    * The new filter rule creates an index over selected attributes, which can then be searched —
      see :ref:`Loupe <rst_extended_loupe>`
* Expression Rule (New)
    * Allows the execution of further filter rules to be tied to conditions; more at the
      :ref:`Expression filter rule <rst_cookbook_filter_expression-rule>`


Frontend Editing (FEE)
-----------------------

* Template name change from `form_textfield_multiple` to `form_text_multiple` in "FormTextFieldMultipleBundle"
  (alignment with Contao 5)
* In the input mask settings for file upload, the widget modes now only show the relevant settings for single
  or multiple upload depending on the "Multiple editing" setting — if the attribute setting is changed, the
  upload must be adjusted accordingly
* Customizing the target directory or filename with the insert tag ``{{post::<attribute-column-name>}}`` is
  no longer possible since this tag no longer exists in Contao 5 — a Simple Token can now be used instead as
  ``##post::<attribute-column-name>##`` — :ref:`see FEE <extended_frontend_editing_upload>`
* Support for multilingual MetaModels — the FE mask has a language switcher like in the BE; see
  :ref:`FEE <extended_frontend_editing_multilanguage>`
* Form template selection for the input mask (FEE) is now available for all translated attributes
* Support for Content Security Policy (CSP)


Known Issues
------------

* When toggling to/from debug mode in the BE via the button, the reference page is no longer correct
  and the page must be navigated to again — e.g. with "back" in the browser and reloading the page |br|
  Contao currently provides no way to influence the referer at that point


.. _check_upgrade_mm240:

Checklist for Upgrading to MM 2.4
-----------------------------------

In general, an upgrade within the MM 2.x branch is straightforward; any necessary label adjustments
and DB changes are handled via migrations. However, there are a few things that cannot or can only very
difficultly be caught by migrations. For this reason, the following points should be kept in mind when
upgrading to MM 2.4:

* Please follow all notes from :ref:`MM 2.3 <check_upgrade_mm230>` and :ref:`MM 2.2 <check_upgrade_mm220>`
* DC_General template changes
* Template name change from `form_textfield_multiple` to `form_text_multiple` in "FormTextFieldMultipleBundle" (FEE)
* Template changes for File and Translated File for metadata output
* Check custom programming for Contao 5 compatibility (see above)
* For FEE with file upload: check widget mode in attribute settings of the input mask (see above)
* For FEE with file upload: check whether insert tag ``{{post::*}}`` was used and adjust (see above)
* For FEE and the delete item link: consider changes due to CSP support and adjust CSS if needed (see above)
* For dark mode: create additional variants of custom icons with suffix "--dark" — e.g. for
  `flag_enabled.svg` and `flag_disabled.svg` create `flag_enabled--dark.svg` and `flag_disabled--dark.svg` — see
  `EAP News October II 2024 <https://now.metamodel.me/de/mm-eap-newsletter-2-4/details/eap-info-mm-2-4-oktober-ii-2024>`_
* For the Country attribute, country code notation has been changed to uppercase as in Contao — existing
  data is adjusted via migration; adjust any custom checks or storage logic
* For the Translated URL attribute, the language code column name has been changed to ``langcode`` — adjust
  any custom SQL queries or template outputs
* Check image size selections — various default presets such as "Center-Center" no longer exist
* If the "Static parameter" option is set in a filter rule, check the default value for "Filter value for
  attribute" in the MM list — if no items appear in the list, set to "without data value [null]"
* For custom queries for perimeter search or geocoordinate lookup, pass an ``HttpClientInterface`` as
  parameter to the map provider
* For filter rules, check the "URL type for parameter" setting and set to Slug OR GET
* New templates for Content Article (also multilingual) passing an array of content objects
* Output of fallback language content when no translated content exists



Re-Financing
------------
.. seealso:: To re-finance the extensive work, the MM team asks for financial contributions. As a
   guideline, take the scope of the project to be realized and budget approximately 10% — based on
   the experience of past contributions, these are amounts between €100 and €500 (net) — an invoice
   including VAT is of course always issued. `More... <https://now.metamodel.me/de/unterstuetzer/spenden>`_


.. |img_fallback| image:: /_img/icons/fallback.png
.. |img_translated| image:: /_img/icons/translated.png

.. |br| raw:: html

   <br />
