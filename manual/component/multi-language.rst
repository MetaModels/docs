.. _component_multi-language:

Multilingual Support in MetaModels
====================================

MetaModels is very well suited for multilingual content. MM provides its own attributes for
multilingual content, such as `Translated text`, `Translated alias`, `Translated file`, etc.
For attributes whose values are independent of a language, such as numeric values, product IDs,
etc., these variants do not exist.

Multilingual support in MetaModels is designed so that multilingual fields are filled in not only
in the fallback language but also in the desired translations. If this is not the case for a field,
the fallback value is automatically output in the frontend. In the template, it is not possible to
distinguish whether a value comes from a translation or from the fallback.

Before starting to create models, think carefully about whether content should be stored
multilingually. Multilingual content is stored in separate tables and not in the individual ``mm_*``
table, so switching to multilingual support later involves corresponding maintenance work.

If you have monolingual values that you want to store but have them output in different languages
in the frontend, switching to multilingual support is uncritical. Only the attribute labels are
extended according to the languages — :ref:`see below "Attributes" <component_multi-language_attribute>`.


Models
------

The multilingual support of a model is set during creation. By activating the "Translation" option,
a list of languages to maintain can be managed. If the "Support for territory specification in the
language" option is also activated, in addition to main language entries like ``de``, ``en``, ``fr``,
the territorial additions like ``de_DE``, ``de_AT``, ``de_CH``, etc. are also enabled in the list.

One language must be designated as the fallback language, for which all records must always be
present. If the fallback language is changed later, this must be checked and corrected in the DB.

If you create multiple models that are also connected by relations, it is advisable to define the
same language schema and the same fallback language for all models. It is also sensible to only
create the languages that Contao has defined via its root pages.

Multilingual MetaModels are highlighted with a colored country flag |img_locale|.


.. _component_multi-language_save:
Storage in Database
-------------------

Translated values are stored in separate tables ``tl_metamodel_translated*`` — depending on the
attribute type, these can be different tables. The values therefore do not appear in the individual
table of the created model ``mm_*``. The translation tables have a reference to the attribute
``att_id`` and the record ``item_id`` as well as the language code ``langcode``.

Data in the defined fallback language must be present so that output is possible even when a
translation is not available. Since MM 2.4, the fallback language is marked in the input form with
"|img_fallback|" in the language switcher as well as in the heading.

When switching in the input form from the fallback language to a translation language, the inputs
of the fallback language are displayed in text fields. This makes translation of texts easier. More
about the labeling in the section ":ref:`component_multi-language_input`".

.. note:: If a fallback text is not translated, it is also not saved in the translation language.
   This is particularly important when a term in the fallback language and the translation language
   is the same, e.g. "Marketing" in English and German. |br|
   If an entry is changed back to the fallback content after a translation, the entry for the
   translation language is deleted from the database.

.. note:: From MM 2.4, when copying a record, all languages are copied with it —
   `see issue <https://github.com/MetaModels/core/issues/598#issuecomment-1912422061>`_

.. note:: When multilingual models or attributes are deleted, not all content is deleted with them
   — :ref:`notes on checking and deleting here <rst_cookbook_specials_delete-superfluous-data>`

From MM 2.4, there is the parameter "Disable fallback mode" — if this is active, a value for a
language is still saved even if it is equal to the fallback value. This option is activated, for
example, for the attribute :ref:`Translated checkbox <component_attribute_translatedcheckbox>`.

.. _component_multi-language_attribute:
Attributes
----------

If the "Translation" option is activated for a model, multilingual variants are also available when
creating attributes. Depending on installation this can include:

* :ref:`Translated alias <component_attribute_translatedalias>`
* :ref:`Translated checkbox <component_attribute_translatedcheckbox>`
* :ref:`Translated file <component_attribute_translatedfile>`
* :ref:`Translated table multi (MCW) <component_attribute_translatedtablemulti>`
* :ref:`Translated table text <component_attribute_translatedtabletext>`
* :ref:`Translated URL <component_attribute_translatedurl>`
* :ref:`Translated combined values <component_attribute_translatedcombinedvalues>`
* :ref:`Translated content of an article <component_attribute_translatedcontentarticle>`
* :ref:`Translated long text <component_attribute_translatedlongtext>`
* :ref:`Translated text <component_attribute_translatedtext>`
* :ref:`Translated single select [select] <component_attribute_translatedselect>` |*note|
* :ref:`Translated multi-select [tags] <component_attribute_translatedtags>` |*note|

|*note|: the **non-translated attributes Single select and Multi-select inherently support
multilingual** for relations to MetaModel tables. The two attributes listed here are for special
cases, e.g. relations to non-MM tables with multilingual content. These tables must, however, have
a column with the language key. You can also use monolingual MetaModel tables with an attribute for
the language key. This makes it possible to deliver not only a corresponding translation per language,
but entirely different content. For example, a hiking tour could go "left" for English-speaking
visitors and "right" for German-speaking visitors.

For multilingual MetaModels, a field per language is available for all attribute definitions for
the "Name" and "Description" fields — the fallback language is shown highlighted. These translated
labels are automatically output in the corresponding language in the input form when the matching
backend language is selected in the user profile.

Additionally, the translated "Name" value can be accessed in the :ref:`rendering template
<component_templates_fe-list>` via the ``attributes`` node — the Contao frontend language or the
fallback value is automatically output. In the template, an output might look like:

.. code-block:: html
   :linenos:

    <p><strong><?= $arrItem['attributes']['name'] ?>:</strong> <?= $arrItem['text']['name'] ?></p>
    <p><strong><?= $arrItem['attributes']['city'] ?>:</strong> <?= $arrItem['text']['city'] ?></p>
    <p><strong><?= $arrItem['attributes']['description'] ?>:</strong> <?= $arrItem['text']['description'] ?></p>

This allows convenient handling of multilingual "labels" in a template.


.. _component_multi-language_input:
Input Form / Input
------------------

In the backend input form, the widgets of multilingual attributes have a colored flag icon
|img_locale| for distinction. Language switching is done directly in the header of the input form.
The fallback language is marked accordingly in both the language switcher and the heading.

When creating a new record, the fallback language is always filled in first — if a language other
than the fallback language is set in the form, switching to the fallback language occurs on saving.

.. warning:: Saving a record or an entry does not happen automatically when switching to another
   language — inputs must be saved with "Save" before switching!

.. note:: The following displays were introduced or adjusted in MM 2.4.

After the fields in the fallback language have been filled in and saved, you can switch to any other
language. The multilingual fields initially show the content from the fallback language. Additionally,
the hint |img_fallback| is shown in the title as long as no content has been saved that differs from
the fallback values. This is intended to make the translation status easier to recognize.

These displayed fallback contents are not saved in the respective translation language in the DB when
saved — see ":ref:`component_multi-language_save`".

When content in the translation language is created and saved, the hint on the corresponding input
fields switches to |img_translated|.

Depending on the status of an input field, it looks like this, for example:

|translation-hints|

**Extensions for translating:**

For **continuous translations**, the extension :ref:`rst_extended_xliff_ex-import` is recommended.
This uses the `XLIFF format <https://en.wikipedia.org/wiki/XLIFF>`_ for exchange.

The files are exported via the extension and re-imported after translation — appropriate agencies
or tools can be used for translation.

For **translation in the backend**, the extension :ref:`rst_extended_translator-bridge` is available,
which integrates various translation providers such as `DeepL <https://www.deepl.com>`_. Translation
can be done per input field or via a keyboard shortcut for the active input form of the selected language.


Backend List View
-----------------

In the backend list view, there is also a language switcher in the header — if multilingual attributes
are output in the list, the display is according to the language.


Filters
-------

Most filter rules search in the language that is currently the active (Contao) language in the frontend.
For some filter rules such as ":ref:`Simple lookup <component_filter_simplelookup>`",
":ref:`Single select <component_filter_select>`", ":ref:`Multi-select <component_filter_tags>`",
":ref:`Text filter <component_filter_text>`" there is the "Search all languages" option, if a
multilingual attribute is selected. This option can be used e.g. on the detail page (see below).

For the ":ref:`Translated checkbox <component_attribute_translatedcheckbox>`" attribute, there is a
dedicated filter rule ":ref:`Translated checkbox status <component_filter_translated-checkbox>`".

For the ":ref:`Levenshtein-based search <component_attribute_levenshtein>`" filter rule, it is also
possible to select multilingual attributes in the "Attributes to index" attribute setting.

The ":ref:`Loupe full-text search <rst_extended_loupe>`" filter rule currently supports the multilingual
attributes Text and Long text.


.. _component_multi-language_fe-output:
Frontend List / Detail View
----------------------------

In the frontend, the content of attributes is output in templates analogously to monolingual attributes.
Attribute labels are also translated and delivered to the template.

.. note:: If content is not translated, the content of the fallback language is output — this applies
   not only to attributes with text input, but also to attributes such as translated file or translated
   content of an article.

This means that if, for example, no language-specific file is selected for the translation language,
all files from the fallback language are output.

Which language the content comes from can be queried in the ``raw`` node of the
:ref:`output array <component_templates_fe-list>` in the ``langcode`` node.

If you have a detail page where you typically want to display a record via alias, you should be able
to switch to the detail page in another language using the language switcher.

Extensions like `"ChangeLanguage" <https://github.com/terminal42/contao-changelanguage>`_ "sees" only
the page created in Contao — e.g. ``https://my-domain.tld/en/dessert/details`` — without the alias of
the filter.

To pass the value for the other languages to the extension and filter accordingly, there are several
options - the least effort is required by the :ref:`metamodels/changelanguage-bridge
<rst_extended_changelanguage-bridge>` extension further below.

**1. Filter rule "Simple lookup" with the "Search all languages" option**

First, all detail pages of the individual languages must be linked via the page properties — button
"Page in main language". Additionally, the "URL parameter" from the filter rule must be entered in
the "Keep query parameters" field (alias). "auto_item" must not be entered as the URL parameter,
as ChangeLanguage cannot work with it.

The ":ref:`Simple lookup <component_filter_simplelookup>`" filter rule is also created or adjusted.
The URL parameter must not be entered as ``auto_item`` and the "Search all languages" option must be
activated. This allows filtering with all language variants, i.e. with

``https://my-domain.tld/en/dessert/details/alias/marinated-strawberries`` as well as |br|
``https://my-domain.tld/de/dessert/details/alias/marinated-strawberries`` or

``https://my-domain.tld/en/dessert/details/alias/marinierte-erdbeeren`` as well as |br|
``https://my-domain.tld/de/dessert/details/alias/marinierte-erdbeeren``.


**2. Hook "changelanguageNavigation"**

If you do not want to activate the "Search all languages" option or work with ``auto_item`` as the
"URL parameter", you can exchange the filter parameter (e.g. alias) appropriately for each language
in the "ChangeLanguage" language switcher via a hook — `see
docs <https://extensions.terminal42.ch/docs/changelanguage/en/developers/#rewriting-an-url-parameter>`_

As a starting point, the snippet: you need to check whether you are on the appropriate detail page,
e.g. ID 3, 15, 36 for the individual language pages. The appropriate value for the respective other
language can be determined from the current value of the filter parameter and the current language.
This query depends on the structure of the MetaModels. The hook is called once for each language
in the language switcher.

.. code-block:: php

   public function __invoke(ChangelanguageNavigationEvent $event)
   {
       // ...

       // Right page?
       $listPageIds = [3, 15, 36];
       if (!\in_array($targetPageId = $event->getNavigationItem()->getTargetPage()->id, $listPageIds, true)) {
           return;
       }

       // Get alias value.
       $currentAliasValue = Input::get('auto_item');

       // Search for attribute value in target language.
       // $newAliasValue = ....

       // Set alias value for target language/page.
       $event->getUrlParameterBag()->setUrlAttribute('auto_item', $newAliasValue);
   }

This variant also correctly sets the ``hreflang`` entries in the meta data —
:ref:`see SEO <rst_cookbook_tips_seo_metadata-hreflang>`.


**3. "ChangeLanguage-bridge" extension (from MM 2.5)**

The :ref:`metamodels/changelanguage-bridge <rst_extended_changelanguage-bridge>` extension takes
care of the hook from variant 2 for you: a single checkbox for "Support language switcher" per
render setting is enough, the matching filter parameter for the respective target language is then
determined automatically from the jump configuration already maintained on the render setting — no
fixed list of page IDs and no custom PHP code needed.

In addition, the same extension also picks up GET filter parameters (e.g. ``?alias=...``)
automatically into the language switcher, entirely without that checkbox — a manual entry under
"Keep query parameters" as in variant 1 is then no longer needed for monolingual models either. Why
ChangeLanguage picks up path segments (``/alias/...``) on its own but not GET parameters is
explained in ":ref:`rst_extended_changelanguage-bridge_slug-get`".


Frontend Editing (FEE)
----------------------

From MM 2.4, multilingual support is also supported in frontend editing — :ref:`more about this
<extended_frontend_editing_multilanguage>` — including the display of the fallback language and
translation status — see :ref:`Frontend Editing <extended_frontend_editing_multilanguage>`.


Custom Translation Adjustments
-------------------------------

The texts in MetaModels, such as the label of buttons, can be overridden with custom texts — more
about this under ":ref:`component_translations_modifications`".



.. |img_locale| image:: /_img/icons/locale.png
.. |img_fallback| image:: /_img/icons/fallback.png
.. |img_translated| image:: /_img/icons/translated.png
.. |translation-hints| image:: /_img/screenshots/components/translation-hints.png

.. |br| raw:: html

   <br />

.. |*note| raw:: html

   <strong>*</strong>
