.. _manual_new_icons-25:

New Icons for MetaModels as SVG
================================

All backend icons have been switched from **PNG to SVG**. This keeps them crisp at any size -
even when the browser display is enlarged or on high-resolution screens.

.. seealso:: Anyone who generally finds the icons in the backend too small can enlarge them with
   the `contao-backend-size-bundle <https://github.com/e-spin/contao-backend-size-bundle>`_
   extension in their own user profile - the setting applies per user, not for the whole
   installation. The extension is **not** part of MetaModels and can be used independently of it;
   thanks to the switch to SVG, the MetaModels icons stay crisp even so.


What Else Has Changed
----------------------

**Color by area.** The icons of the structural level are colored so that the areas in the
backend can be distinguished at a glance: the MetaModel and its input masks in ochre, the
attributes in blue, the filters in red, the views in green. The icons of the individual
attribute, filter, and condition types deliberately remain neutral dark gray - they represent
the content, not the area.

The colors chosen were selected so that they fit into Contao's color scheme, are distinguishable
from one another, work as well as possible in both light and dark mode, and remain
distinguishable even for red-green color blindness.

**Own variant for dark mode.** Each icon has a file with the ``--dark`` suffix. Contao selects it
automatically when the backend is running in the dark color scheme - no setting is required.

**Pale variant for "disabled".** The file with the ``_1`` suffix is the grayed-out version.
MetaModels uses it everywhere something is set up but not active - a disabled filter rule, a
deactivated condition, an untranslated MetaModel.

The following tables compare the previous icon with the new one for each type. A "-" in the
*Previous* column means that this type had no icon of its own before.

.. note:: Contao embeds the icons at 16 pixels. In the tables they are shown at 22 pixels - i.e.
   as they appear in the enlarged backend with the extension mentioned above. This incidentally
   also shows exactly the reason for the switch: the old PNGs are raster graphics and become
   blurry when enlarged, while the new SVGs stay crisp.


Core and Structure
-------------------

The icons of the tree structure and the menus - everything that identifies a MetaModel itself,
its input masks, filter sets, and views.

.. list-table::
   :header-rows: 1
   :widths: 44 9 9 38

   * - Meaning
     - Previous
     - New
     - Note
   * - MetaModels in the breadcrumb
     - |alt_logo_png|
     - |neu_mm_logo_small_svg|
     -
   * - Attributes
     - |alt_fields_png|
     - |neu_fields_svg|
     -
   * - Render settings
     - |alt_rendersettings_png|
     - |neu_rendersettings_svg|
     -
   * - Fields of a render setting
     - |alt_rendersetting_png|
     - |neu_rendersetting_svg|
     -
   * - "Add all" in the render setting
     - |alt_rendersettings_add_png|
     - |neu_rendersettings_add_svg|
     -
   * - Input masks
     - |alt_dca_png|
     - |neu_dca_svg|
     -
   * - Fields of an input mask
     - |alt_dca_setting_png|
     - |neu_dca_setting_svg|
     -
   * - Display condition of a field
     - |alt_dca_condition_png|
     - |neu_dca_condition_svg|
     -
   * - Grouping and sorting
     - |alt_dca_groupsortsettings_png|
     - |neu_dca_groupsortsettings_svg|
     -
   * - "Add all" in the input mask
     - |alt_dca_add_png|
     - |neu_dca_add_svg|
     -
   * - Search settings
     - |alt_searchable_pages_png|
     - |neu_searchable_pages_svg|
     -
   * - Filter set
     - |alt_filter_png|
     - |neu_filter_svg|
     -
   * - Filter rules
     - |alt_filter_setting_png|
     - |neu_filter_setting_svg|
     -
   * - Permission assignment
     - |alt_dca_combine_png|
     - |neu_dca_combine_svg|
     -
   * - Variants
     - |alt_variants_png|
     - |neu_variants_svg|
     -
   * - Translated MetaModel
     - |alt_locale_png|
     - |neu_locale_svg|
     -
   * - Child table without its own icon
     - |alt_metamodels_png|
     - |neu_child_table_svg|
     - previously the default icon of a MetaModel, which was meaningless here
   * - Default icon of a MetaModel
     - |alt_metamodels_png|
     - |neu_metamodels_svg|
     - fallback when a MetaModel has no icon of its own set
   * - "MetaModels" menu group in the backend menu
     - |alt_mm_group_icon_contour_svg|
     - |neu_mm_group_icon_svg|
     - previously only the outline, now filled


Attributes
----------

The type icons of the attributes, as they appear in the attribute list and in the attribute type
selection.

.. list-table::
   :header-rows: 1
   :widths: 22 30 9 9 30

   * - Type
     - Backend label
     - Previous
     - New
     - Note
   * - ``alias``
     - Alias
     - |alt_alias_png|
     - |neu_alias_svg|
     -
   * - ``checkbox``
     - Checkbox
     - |alt_checkbox_png|
     - |neu_checkbox_svg|
     -
   * - ``color``
     - Color picker
     - |alt_color_png|
     - |neu_color_svg|
     -
   * - ``combinedvalues``
     - Combined values
     - |alt_combinedvalues_png|
     - |neu_combinedvalues_svg|
     -
   * - ``contentarticle``
     - Content of an article
     - |alt_article_png|
     - |neu_article_svg|
     -
   * - ``country``
     - Country
     - |alt_country_png|
     - |neu_country_svg|
     -
   * - ``decimal``
     - Decimal
     - |alt_decimal_png|
     - |neu_decimal_svg|
     -
   * - ``file``
     - File
     - |alt_file_png|
     - |neu_file_svg|
     -
   * - ``geodistance``
     - Geo distance
     - |alt_geodistance_png|
     - |neu_geodistance_svg|
     -
   * - ``langcode``
     - Language code
     - |alt_langcode_png|
     - |neu_langcode_svg|
     -
   * - ``levenshtein``
     - Levenshtein-based search
     - |alt_levenshtein_png|
     - |neu_levenshtein_svg|
     -
   * - ``longtext``
     - Long text
     - |alt_longtext_png|
     - |neu_longtext_svg|
     -
   * - ``marker_icon``
     - Cowegis marker
     - |alt_marker_png|
     - |neu_marker_svg|
     -
   * - ``numeric``
     - Numeric
     - |alt_numeric_png|
     - |neu_numeric_svg|
     -
   * - ``rating``
     - Rating
     - |alt_star_full_png|
     - |neu_star_svg|
     - up to MM 2.4 the same icon as the filled frontend star (``star-full``); gets its own file
       with 2.5
   * - ``select``
     - Single select [select]
     - |alt_select_png|
     - |neu_select_svg|
     -
   * - ``tablemulti``
     - Multi-table (MCW)
     - |alt_tablemulti_png|
     - |neu_tablemulti_svg|
     -
   * - ``tabletext``
     - Text table
     - |alt_tabletext_png|
     - |neu_tabletext_svg|
     -
   * - ``tags``
     - Multi select [tags]
     - |alt_tags_png|
     - |neu_tags_svg|
     -
   * - ``text``
     - Text
     - |alt_text_png|
     - |neu_text_svg|
     -
   * - ``timestamp``
     - Date/time
     - |alt_timestamp_png|
     - |neu_timestamp_svg|
     -
   * - ``token``
     - Token
     - |alt_token_png|
     - |neu_token_svg|
     -
   * - ``url``
     - URL
     - |alt_url_png|
     - |neu_url_svg|
     -


Translated Attributes
......................

The translated attributes use the same icon as their non-translated counterpart; only
``translatedtablemulti`` and ``translatedtabletext`` have one of their own.

.. list-table::
   :header-rows: 1
   :widths: 22 30 9 9 30

   * - Type
     - Backend label
     - Previous
     - New
     - Note
   * - ``translatedalias``
     - Translated alias
     - |alt_alias_png|
     - |neu_alias_svg|
     -
   * - ``translatedcheckbox``
     - Translated checkbox
     - |alt_checkbox_png|
     - |neu_checkbox_svg|
     -
   * - ``translatedcombinedvalues``
     - Translated combined values
     - |alt_combinedvalues_png|
     - |neu_combinedvalues_svg|
     -
   * - ``translatedcontentarticle``
     - Translated content of an article
     - |alt_article_png|
     - |neu_article_svg|
     -
   * - ``translatedfile``
     - Translated file
     - |alt_file_png|
     - |neu_file_svg|
     -
   * - ``translatedlongtext``
     - Translated long text
     - |alt_longtext_png|
     - |neu_longtext_svg|
     -
   * - ``translatedselect``
     - Translated single select [select]
     - |alt_select_png|
     - |neu_select_svg|
     -
   * - ``translatedtablemulti``
     - Translated multi-table (MCW)
     - |alt_translatedtablemulti_png|
     - |neu_translatedtablemulti_svg|
     - own icon
   * - ``translatedtabletext``
     - Translated text table
     - |alt_translatedtabletext_png|
     - |neu_translatedtabletext_svg|
     - own icon
   * - ``translatedtags``
     - Translated multi select [tags]
     - |alt_tags_png|
     - |neu_tags_svg|
     -
   * - ``translatedtext``
     - Translated text
     - |alt_text_png|
     - |neu_text_svg|
     -
   * - ``translatedurl``
     - Translated URL
     - |alt_url_png|
     - |neu_url_svg|
     -


Filter Rules
-------------

All filter rules selectable in the backend, alphabetically by type - regardless of which package
they come from.

.. list-table::
   :header-rows: 1
   :widths: 22 30 9 9 30

   * - Type
     - Backend label
     - Previous
     - New
     - Note
   * - ``checkbox``
     - Yes / No
     - |alt_filter_checkbox_png|
     - |neu_filter_yes_no_svg|
     - the file is now called ``filter_yes-no``
   * - ``checkbox_published``
     - Checkbox status
     - |alt_visible_png|
     - |neu_filter_checkbox_svg|
     - previously the eye (``visible.png``)
   * - ``conditionand``
     - AND condition
     - |alt_filter_and_png|
     - |neu_filter_and_svg|
     - from the core
   * - ``conditionor``
     - OR condition
     - |alt_filter_or_png|
     - |neu_filter_or_svg|
     - from the core
   * - ``customsql``
     - Custom SQL
     - |alt_filter_customsql_png|
     - |neu_filter_customsql_svg|
     - from the core
   * - ``expression_rule``
     - Expression rule
     - |alt_filter_expression_png|
     - |neu_filter_expression_svg|
     - from the core
   * - ``fromto``
     - Value from/to for one attribute
     - |alt_filter_fromto_png|
     - |neu_filter_fromto_svg|
     -
   * - ``fromtodate``
     - Value from/to for a date attribute
     - |alt_filter_fromto_png|
     - |neu_filter_fromto_date_svg|
     - own icon, previously the same as ``fromto``
   * - ``idlist``
     - Predefined set of items
     - |alt_filter_default_png|
     - |neu_filter_idlist_svg|
     - own icon, previously the fallback icon
   * - ``levenshtein``
     - Levenshtein-based search
     - |alt_filter_levenshtein_png|
     - |neu_filter_levenshtein_svg|
     -
   * - ``loupe``
     - Loupe-based search
     - —
     - |neu_loupe_emblem_svg|
     - was already SVG before, unchanged
   * - ``member_filter``
     - Permission for frontend members
     - |alt_filter_member_png|
     - |neu_filter_member_svg|
     - from contao-frontend-editing
   * - ``perimetersearch``
     - Perimeter search
     - |alt_filter_perimetersearch_png|
     - |neu_filter_perimetersearch_svg|
     -
   * - ``range``
     - Value from/to for two attributes
     - |alt_filter_range_png|
     - |neu_filter_range_svg|
     -
   * - ``rangedate``
     - Value from/to for two date attributes
     - |alt_filter_range_png|
     - |neu_filter_rangedate_svg|
     - own icon, previously the same as ``range``
   * - ``register``
     - Register
     - |alt_filter_register_png|
     - |neu_filter_register_svg|
     -
   * - ``related``
     - Filter on an attribute of the model via a relation
     - |alt_filter_by_related_png|
     - |neu_filter_by_related_svg|
     -
   * - ``select``
     - Single select
     - |alt_filter_select_png|
     - |neu_filter_select_svg|
     -
   * - ``simplelookup``
     - Simple lookup
     - |alt_filter_default_png|
     - |neu_filter_simplelookup_svg|
     - own icon, previously the fallback icon
   * - ``tags``
     - Multi select
     - |alt_filter_tags_png|
     - |neu_filter_tags_svg|
     -
   * - ``text``
     - Text filter
     - |alt_filter_text_png|
     - |neu_filter_text_svg|
     -
   * - ``translatedcheckbox_published``
     - Translated checkbox status
     - |alt_visible_png|
     - |neu_filter_checkbox_svg|
     - shares the icon with ``checkbox_published``
   * - —
     - Filter rule without its own icon
     - |alt_filter_default_png|
     - |neu_filter_default_svg|
     - fallback value; the types ``idlist`` and ``simplelookup`` now have their own icons


View Conditions
-----------------

The conditions used to show and hide individual fields of the input mask (*Manage conditions* on
an input mask setting) previously had no icon at all - the list showed the same symbol for every
condition. Each condition type now gets its own, so that AND, OR, and NOT combinations can be
told apart even in a nested list.

.. note:: If a condition type from a third-party package does not bring its own icon,
   ``condition_default.svg`` is shown. A new type therefore only needs a file
   ``condition_<name>.svg`` in the core - no registration is required.

.. list-table::
   :header-rows: 1
   :widths: 22 30 9 9 30

   * - Type
     - Backend label
     - Previous
     - New
     - Note
   * - ``conditionand``
     - AND
     - —
     - |neu_condition_and_svg|
     - combines several conditions, all of which must match
   * - ``conditionor``
     - OR
     - —
     - |neu_condition_or_svg|
     - combines several conditions, one of which is sufficient
   * - ``conditionnot``
     - NOT
     - —
     - |neu_condition_not_svg|
     - inverts the contained condition
   * - ``conditionpropertyvalueis``
     - Property value is …
     - —
     - |neu_condition_propertyvalueis_svg|
     - checks for a specific value
   * - ``conditionpropertycontainanyof``
     - Property value can contain …
     - —
     - |neu_condition_propertycontainanyof_svg|
     - checks for one of several values
   * - ``conditionpropertyvisible``
     - Property is visible …
     - —
     - |neu_condition_propertyvisible_svg|
     - ties into the visibility of another property
   * - —
     - Fallback icon
     - —
     - |neu_condition_default_svg|
     - for condition types from third-party packages that do not bring their own icon


Other Symbols
--------------

Status and frontend symbols that do not stand for a type.

.. list-table::
   :header-rows: 1
   :widths: 44 9 9 38

   * - Meaning
     - Previous
     - New
     - Note
   * - Checkbox active (list view)
     - |alt_visible_svg|
     - |neu_checkbox_active_svg|
     - previously Contao's own ``visible.svg``, now a dedicated one
   * - Checkbox inactive (list view)
     - |alt_invisible_svg|
     - |neu_checkbox_inactive_svg|
     - previously Contao's own ``invisible.svg``, now a dedicated one
   * - Rating - empty star
     - |alt_star_empty_png|
     - |neu_star_empty_svg|
     - frontend display
   * - Rating - filled star
     - |alt_star_full_png|
     - |neu_star_full_svg|
     - frontend display
   * - Rating - star on hover
     - |alt_star_hover_png|
     - |neu_star_hover_svg|
     - frontend display
   * - Levenshtein - index
     - |alt_levenshtein_index_png|
     - |neu_levenshtein_index_svg|
     -


Extensions
----------

Two extensions bring their own symbols. They follow the same rule as the core: whatever denotes
an entity is colored; whatever stands for a type stays neutral gray. With the note list, both
can be seen - the note list itself in yellow, its filter rule in gray.

The ``marker_icon`` attribute is additionally listed above under Attributes, because it shares
its icon with the extension.

.. list-table::
   :header-rows: 1
   :widths: 44 9 9 38

   * - Meaning
     - Previous
     - New
     - Note
   * - Note list
     - |alt_notelist_png|
     - |neu_notelist_svg|
     - the note list itself - yellow, because the icon stands for the entity
   * - Note list - entry contained
     - |alt_notelist_png|
     - |neu_notelist_filled_svg|
     - previously the same icon as the note list itself - the filled state is new
   * - Note list filter rule
     - |alt_notelist_png|
     - |neu_filter_notelist_svg|
     - own gray type icon
   * - Cowegis - MetaModels layer
     - |alt_metamodels_marker_svg|
     - |neu_metamodels_marker_svg|
     - layer type in the Cowegis map; was already SVG before
   * - Cowegis - marker
     - |alt_marker_png|
     - |neu_marker_svg|
     - also the type icon of the ``marker_icon`` attribute


.. Image substitutions

.. |alt_alias_png| image:: /_img/icons/alias.png
   :width: 22px
.. |alt_article_png| image:: /_img/icons/article.png
   :width: 22px
.. |alt_checkbox_png| image:: /_img/icons/checkbox.png
   :width: 22px
.. |alt_color_png| image:: /_img/icons/color.png
   :width: 22px
.. |alt_combinedvalues_png| image:: /_img/icons/combinedvalues.png
   :width: 22px
.. |alt_country_png| image:: /_img/icons/country.png
   :width: 22px
.. |alt_dca_add_png| image:: /_img/icons/dca_add.png
   :width: 22px
.. |alt_dca_combine_png| image:: /_img/icons/dca_combine.png
   :width: 22px
.. |alt_dca_condition_png| image:: /_img/icons/dca_condition.png
   :width: 22px
.. |alt_dca_groupsortsettings_png| image:: /_img/icons/dca_groupsortsettings.png
   :width: 22px
.. |alt_dca_png| image:: /_img/icons/dca.png
   :width: 22px
.. |alt_dca_setting_png| image:: /_img/icons/dca_setting.png
   :width: 22px
.. |alt_decimal_png| image:: /_img/icons/decimal.png
   :width: 22px
.. |alt_fields_png| image:: /_img/icons/fields.png
   :width: 22px
.. |alt_file_png| image:: /_img/icons/file.png
   :width: 22px
.. |alt_filter_and_png| image:: /_img/icons/filter_and.png
   :width: 22px
.. |alt_filter_by_related_png| image:: /_img/icons/filter_by_related.png
   :width: 22px
.. |alt_filter_checkbox_png| image:: /_img/icons/filter_checkbox.png
   :width: 22px
.. |alt_filter_customsql_png| image:: /_img/icons/filter_customsql.png
   :width: 22px
.. |alt_filter_default_png| image:: /_img/icons/filter_default.png
   :width: 22px
.. |alt_filter_expression_png| image:: /_img/icons/filter_expression.png
   :width: 22px
.. |alt_filter_fromto_png| image:: /_img/icons/filter_fromto.png
   :width: 22px
.. |alt_filter_levenshtein_png| image:: /_img/icons/filter_levenshtein.png
   :width: 22px
.. |alt_filter_member_png| image:: /_img/icons/filter_member.png
   :width: 22px
.. |alt_filter_or_png| image:: /_img/icons/filter_or.png
   :width: 22px
.. |alt_filter_perimetersearch_png| image:: /_img/icons/filter_perimetersearch.png
   :width: 22px
.. |alt_filter_png| image:: /_img/icons/filter.png
   :width: 22px
.. |alt_filter_range_png| image:: /_img/icons/filter_range.png
   :width: 22px
.. |alt_filter_register_png| image:: /_img/icons/filter_register.png
   :width: 22px
.. |alt_filter_select_png| image:: /_img/icons/filter_select.png
   :width: 22px
.. |alt_filter_setting_png| image:: /_img/icons/filter_setting.png
   :width: 22px
.. |alt_filter_tags_png| image:: /_img/icons/filter_tags.png
   :width: 22px
.. |alt_filter_text_png| image:: /_img/icons/filter_text.png
   :width: 22px
.. |alt_geodistance_png| image:: /_img/icons/geodistance.png
   :width: 22px
.. |alt_invisible_svg| image:: /_img/icons/invisible.svg
   :width: 22px
.. |alt_langcode_png| image:: /_img/icons/langcode.png
   :width: 22px
.. |alt_levenshtein_index_png| image:: /_img/icons/levenshtein_index.png
   :width: 22px
.. |alt_levenshtein_png| image:: /_img/icons/levenshtein.png
   :width: 22px
.. |alt_locale_png| image:: /_img/icons/locale.png
   :width: 22px
.. |alt_logo_png| image:: /_img/icons/logo.png
   :width: 22px
.. |alt_longtext_png| image:: /_img/icons/longtext.png
   :width: 22px
.. |alt_marker_png| image:: /_img/icons/marker.png
   :width: 22px
.. |alt_metamodels_marker_svg| image:: /_img/icons/metamodels_marker.svg
   :width: 22px
.. |alt_metamodels_png| image:: /_img/icons/metamodels.png
   :width: 22px
.. |alt_mm_group_icon_contour_svg| image:: /_img/icons/mm_group_icon_contour.svg
   :width: 22px
.. |alt_notelist_png| image:: /_img/icons/notelist.png
   :width: 22px
.. |alt_numeric_png| image:: /_img/icons/numeric.png
   :width: 22px
.. |alt_rendersetting_png| image:: /_img/icons/rendersetting.png
   :width: 22px
.. |alt_rendersettings_add_png| image:: /_img/icons/rendersettings_add.png
   :width: 22px
.. |alt_rendersettings_png| image:: /_img/icons/rendersettings.png
   :width: 22px
.. |alt_searchable_pages_png| image:: /_img/icons/searchable_pages.png
   :width: 22px
.. |alt_select_png| image:: /_img/icons/select.png
   :width: 22px
.. |alt_star_empty_png| image:: /_img/icons/star-empty.png
   :width: 22px
.. |alt_star_full_png| image:: /_img/icons/star-full.png
   :width: 22px
.. |alt_star_hover_png| image:: /_img/icons/star-hover.png
   :width: 22px
.. |alt_tablemulti_png| image:: /_img/icons/tablemulti.png
   :width: 22px
.. |alt_tabletext_png| image:: /_img/icons/tabletext.png
   :width: 22px
.. |alt_tags_png| image:: /_img/icons/tags.png
   :width: 22px
.. |alt_text_png| image:: /_img/icons/text.png
   :width: 22px
.. |alt_timestamp_png| image:: /_img/icons/timestamp.png
   :width: 22px
.. |alt_token_png| image:: /_img/icons/token.png
   :width: 22px
.. |alt_translatedtablemulti_png| image:: /_img/icons/translatedtablemulti.png
   :width: 22px
.. |alt_translatedtabletext_png| image:: /_img/icons/translatedtabletext.png
   :width: 22px
.. |alt_url_png| image:: /_img/icons/url.png
   :width: 22px
.. |alt_variants_png| image:: /_img/icons/variants.png
   :width: 22px
.. |alt_visible_png| image:: /_img/icons/visible.png
   :width: 22px
.. |alt_visible_svg| image:: /_img/icons/visible.svg
   :width: 22px
.. |neu_alias_svg| image:: /_img/icons_svg/alias.svg
   :width: 22px
.. |neu_article_svg| image:: /_img/icons_svg/article.svg
   :width: 22px
.. |neu_checkbox_active_svg| image:: /_img/icons_svg/checkbox_active.svg
   :width: 22px
.. |neu_checkbox_inactive_svg| image:: /_img/icons_svg/checkbox_inactive.svg
   :width: 22px
.. |neu_checkbox_svg| image:: /_img/icons_svg/checkbox.svg
   :width: 22px
.. |neu_child_table_svg| image:: /_img/icons_svg/child_table.svg
   :width: 22px
.. |neu_color_svg| image:: /_img/icons_svg/color.svg
   :width: 22px
.. |neu_combinedvalues_svg| image:: /_img/icons_svg/combinedvalues.svg
   :width: 22px
.. |neu_condition_and_svg| image:: /_img/icons_svg/condition_and.svg
   :width: 22px
.. |neu_condition_default_svg| image:: /_img/icons_svg/condition_default.svg
   :width: 22px
.. |neu_condition_not_svg| image:: /_img/icons_svg/condition_not.svg
   :width: 22px
.. |neu_condition_or_svg| image:: /_img/icons_svg/condition_or.svg
   :width: 22px
.. |neu_condition_propertycontainanyof_svg| image:: /_img/icons_svg/condition_propertycontainanyof.svg
   :width: 22px
.. |neu_condition_propertyvalueis_svg| image:: /_img/icons_svg/condition_propertyvalueis.svg
   :width: 22px
.. |neu_condition_propertyvisible_svg| image:: /_img/icons_svg/condition_propertyvisible.svg
   :width: 22px
.. |neu_country_svg| image:: /_img/icons_svg/country.svg
   :width: 22px
.. |neu_dca_add_svg| image:: /_img/icons_svg/dca_add.svg
   :width: 22px
.. |neu_dca_combine_svg| image:: /_img/icons_svg/dca_combine.svg
   :width: 22px
.. |neu_dca_condition_svg| image:: /_img/icons_svg/dca_condition.svg
   :width: 22px
.. |neu_dca_groupsortsettings_svg| image:: /_img/icons_svg/dca_groupsortsettings.svg
   :width: 22px
.. |neu_dca_setting_svg| image:: /_img/icons_svg/dca_setting.svg
   :width: 22px
.. |neu_dca_svg| image:: /_img/icons_svg/dca.svg
   :width: 22px
.. |neu_decimal_svg| image:: /_img/icons_svg/decimal.svg
   :width: 22px
.. |neu_fields_svg| image:: /_img/icons_svg/fields.svg
   :width: 22px
.. |neu_file_svg| image:: /_img/icons_svg/file.svg
   :width: 22px
.. |neu_filter_and_svg| image:: /_img/icons_svg/filter_and.svg
   :width: 22px
.. |neu_filter_by_related_svg| image:: /_img/icons_svg/filter_by_related.svg
   :width: 22px
.. |neu_filter_checkbox_svg| image:: /_img/icons_svg/filter_checkbox.svg
   :width: 22px
.. |neu_filter_customsql_svg| image:: /_img/icons_svg/filter_customsql.svg
   :width: 22px
.. |neu_filter_default_svg| image:: /_img/icons_svg/filter_default.svg
   :width: 22px
.. |neu_filter_expression_svg| image:: /_img/icons_svg/filter_expression.svg
   :width: 22px
.. |neu_filter_fromto_date_svg| image:: /_img/icons_svg/filter_fromto_date.svg
   :width: 22px
.. |neu_filter_fromto_svg| image:: /_img/icons_svg/filter_fromto.svg
   :width: 22px
.. |neu_filter_idlist_svg| image:: /_img/icons_svg/filter_idlist.svg
   :width: 22px
.. |neu_filter_levenshtein_svg| image:: /_img/icons_svg/filter_levenshtein.svg
   :width: 22px
.. |neu_filter_member_svg| image:: /_img/icons_svg/filter_member.svg
   :width: 22px
.. |neu_filter_notelist_svg| image:: /_img/icons_svg/filter_notelist.svg
   :width: 22px
.. |neu_filter_or_svg| image:: /_img/icons_svg/filter_or.svg
   :width: 22px
.. |neu_filter_perimetersearch_svg| image:: /_img/icons_svg/filter_perimetersearch.svg
   :width: 22px
.. |neu_filter_range_svg| image:: /_img/icons_svg/filter_range.svg
   :width: 22px
.. |neu_filter_rangedate_svg| image:: /_img/icons_svg/filter_rangedate.svg
   :width: 22px
.. |neu_filter_register_svg| image:: /_img/icons_svg/filter_register.svg
   :width: 22px
.. |neu_filter_select_svg| image:: /_img/icons_svg/filter_select.svg
   :width: 22px
.. |neu_filter_setting_svg| image:: /_img/icons_svg/filter_setting.svg
   :width: 22px
.. |neu_filter_simplelookup_svg| image:: /_img/icons_svg/filter_simplelookup.svg
   :width: 22px
.. |neu_filter_svg| image:: /_img/icons_svg/filter.svg
   :width: 22px
.. |neu_filter_tags_svg| image:: /_img/icons_svg/filter_tags.svg
   :width: 22px
.. |neu_filter_text_svg| image:: /_img/icons_svg/filter_text.svg
   :width: 22px
.. |neu_filter_yes_no_svg| image:: /_img/icons_svg/filter_yes-no.svg
   :width: 22px
.. |neu_geodistance_svg| image:: /_img/icons_svg/geodistance.svg
   :width: 22px
.. |neu_langcode_svg| image:: /_img/icons_svg/langcode.svg
   :width: 22px
.. |neu_levenshtein_index_svg| image:: /_img/icons_svg/levenshtein_index.svg
   :width: 22px
.. |neu_levenshtein_svg| image:: /_img/icons_svg/levenshtein.svg
   :width: 22px
.. |neu_locale_svg| image:: /_img/icons_svg/locale.svg
   :width: 22px
.. |neu_longtext_svg| image:: /_img/icons_svg/longtext.svg
   :width: 22px
.. |neu_loupe_emblem_svg| image:: /_img/icons_svg/loupe-emblem.svg
   :width: 22px
.. |neu_marker_svg| image:: /_img/icons_svg/marker.svg
   :width: 22px
.. |neu_metamodels_marker_svg| image:: /_img/icons_svg/metamodels_marker.svg
   :width: 22px
.. |neu_metamodels_svg| image:: /_img/icons_svg/metamodels.svg
   :width: 22px
.. |neu_mm_group_icon_svg| image:: /_img/icons_svg/mm_group_icon.svg
   :width: 22px
.. |neu_mm_logo_small_svg| image:: /_img/icons_svg/mm_logo_small.svg
   :width: 22px
.. |neu_notelist_filled_svg| image:: /_img/icons_svg/notelist_filled.svg
   :width: 22px
.. |neu_notelist_svg| image:: /_img/icons_svg/notelist.svg
   :width: 22px
.. |neu_numeric_svg| image:: /_img/icons_svg/numeric.svg
   :width: 22px
.. |neu_rendersetting_svg| image:: /_img/icons_svg/rendersetting.svg
   :width: 22px
.. |neu_rendersettings_add_svg| image:: /_img/icons_svg/rendersettings_add.svg
   :width: 22px
.. |neu_rendersettings_svg| image:: /_img/icons_svg/rendersettings.svg
   :width: 22px
.. |neu_searchable_pages_svg| image:: /_img/icons_svg/searchable_pages.svg
   :width: 22px
.. |neu_select_svg| image:: /_img/icons_svg/select.svg
   :width: 22px
.. |neu_star_empty_svg| image:: /_img/icons_svg/star-empty.svg
   :width: 22px
.. |neu_star_full_svg| image:: /_img/icons_svg/star-full.svg
   :width: 22px
.. |neu_star_hover_svg| image:: /_img/icons_svg/star-hover.svg
   :width: 22px
.. |neu_star_svg| image:: /_img/icons_svg/star.svg
   :width: 22px
.. |neu_tablemulti_svg| image:: /_img/icons_svg/tablemulti.svg
   :width: 22px
.. |neu_tabletext_svg| image:: /_img/icons_svg/tabletext.svg
   :width: 22px
.. |neu_tags_svg| image:: /_img/icons_svg/tags.svg
   :width: 22px
.. |neu_text_svg| image:: /_img/icons_svg/text.svg
   :width: 22px
.. |neu_timestamp_svg| image:: /_img/icons_svg/timestamp.svg
   :width: 22px
.. |neu_token_svg| image:: /_img/icons_svg/token.svg
   :width: 22px
.. |neu_translatedtablemulti_svg| image:: /_img/icons_svg/translatedtablemulti.svg
   :width: 22px
.. |neu_translatedtabletext_svg| image:: /_img/icons_svg/translatedtabletext.svg
   :width: 22px
.. |neu_url_svg| image:: /_img/icons_svg/url.svg
   :width: 22px
.. |neu_variants_svg| image:: /_img/icons_svg/variants.svg
   :width: 22px
