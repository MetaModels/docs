.. _rst_extended_changelanguage-bridge:

ChangeLanguage-Bridge for MetaModels
=====================================

Makes the `"ChangeLanguage" <https://github.com/terminal42/contao-changelanguage>`_ extension
item-aware on a MetaModel's detail pages: instead of falling back to the language start page on a
language switch, the language switcher links directly to the same record in the target language -
including the filter parameters matching there (e.g. alias).

The extension covers two separate cases, see ":ref:`rst_extended_changelanguage-bridge_slug-get`"
further below for the distinction:

* **Translated attribute** (the filter value differs per language, e.g. ``adam-de-adonis-de`` vs.
  ``adam-en-adonis-en``): the "Support language switcher" checkbox is needed per render setting,
  see ":ref:`rst_extended_changelanguage-bridge_activation`".
* **GET filter parameter, even for monolingual models** (e.g. ``?alias=...``): works automatically
  without any configuration, see ":ref:`rst_extended_changelanguage-bridge_get-auto`".

More on the topic :ref:`Multilingualism in MetaModels <component_multi-language>`, in particular
the section ":ref:`component_multi-language_fe-output`" - it also describes the two previous
workarounds (the "Search all languages" filter rule and a custom ``changelanguageNavigation``
hook) that become unnecessary with this extension.


Prerequisites
----------------

* as of MetaModels 2.5
* `terminal42/contao-changelanguage <https://github.com/terminal42/contao-changelanguage>`_
* only for the "translated attribute" case: a separate "Jump to page" row with its own filter
  setting per language in the render setting - the usual configuration for multilingual jump
  links. For the GET case, no special configuration of the render setting is needed.


Installation via Contao Manager or Composer
----------------------------------------------

.. code-block:: bash

   composer require metamodels/changelanguage-bridge


.. _rst_extended_changelanguage-bridge_slug-get:

How ChangeLanguage Handles Slug and GET Parameters
------------------------------------------------------

Whether `"ChangeLanguage" <https://github.com/terminal42/contao-changelanguage>`_ picks up a
filter parameter on a language switch by itself depends on how it appears in the URL - this is
purely ChangeLanguage's own behaviour, independent of this extension or MetaModels in general:

* **As a path segment** (Contao's "Folder" URLs, e.g. ``/alias/hihi-huhusss-2``),
  ``ChangeLanguageModule::createUrlParameterBag()`` reads every ``/key/value/`` pair from the
  current request and carries it over into the target URL without being asked. **No**
  configuration is needed for this - as long as the value is also valid in the target language
  (see below).
* **As a GET parameter** (e.g. ``?alias=hihi-huhusss-2``), the same method only carries over what
  is explicitly entered as a name in the page field "Keep query parameters"
  (``tl_page.languageQuery``). Without an entry, the parameter is lost on the language switch, and
  the language switcher then lands on the plain target page without any relation to the record.

Contao's ``auto_item`` (the parameter-less case, e.g. ``/hihi-huhusss-2`` with no key at all) is
explicitly excluded from the path segment carry-over - ``createUrlParameterBag()`` removes it
again. A filter parameter that is meant to be carried over on a language switch must therefore not
be entered as ``auto_item``, but needs an actual URL parameter name (e.g. "alias").

This also explains why a **monolingual** MetaModel whose detail page uses the same alias in every
language root works in the path segment case without this extension: there is no
language-specific value to translate, and ChangeLanguage already carries over the identical value
by itself. Only once the value differs per language (translated attribute) or the parameter is
passed as GET does one of the following two sections become relevant.


.. _rst_extended_changelanguage-bridge_get-auto:

Automatic GET Parameters (independent of the checkbox)
------------------------------------------------------------

This extension solves the GET case from above automatically, entirely without the "Support
language switcher" checkbox and without a manual entry under "Keep query parameters": for the
current page, it determines which MetaModels content element (or embedded module) is filtering
records there, and which of its filter parameters are declared as "GET" (or the lenient
"slug-or-GET"). Their current value is passed on to the language switcher unchanged.

Deliberately separate from the item translation in the next section: this part does not translate
anything, it merely passes the raw value through - for a monolingual model (identical value in
every language) this is always correct. If "Support language switcher" is additionally active for
the same render setting and already supplies a language-specific translated value, that value
takes precedence and is not overwritten by the automatic pass-through.


.. _rst_extended_changelanguage-bridge_activation:

Activation for Translated Attributes
---------------------------------------

For each render setting whose jump target should support the language switcher with the
translated record, the **"Support language switcher"** option is checked in the "Jump to page"
section - off by default, so that an already existing custom ``changelanguageNavigation`` hook for
the same render setting does not collide.


How It Works
--------------

When building the language switcher, the extension independently checks whether the current page
is the jump target of a render setting with the option enabled. If so, the currently displayed
record is determined, the MetaModel language is switched to the respective target language, and
the target page and slug for that language are determined via the same internal function that
MetaModels also otherwise uses to generate its jump links. The render setting with its filter
configuration thus remains the single place where the jump target is maintained.
