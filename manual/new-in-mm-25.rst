.. _new_in_mm250:

Changes and Features in MM 2.5
===============================

The following is an overview of the changes and features in MetaModels 2.5, made possible by the
"early adopter program" — more information under Fundraising on the
`MM website <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-5>`_.

For a checklist after upgrading to MM 2.5, see :ref:`further notes below <check_upgrade_mm250>`.


General and Core
----------------

MetaModels 2.5 requires **Contao 5.7** and **PHP 8.4**.

The most important new features are:

- Support for **Twig templates** in addition to the existing ``.html5`` templates
- New SVG icons for the backend
- No more MooTools
- Custom backend sections via configuration
- Breadcrumb for child tables
- Attribute templates now output the label together with the value
- New attribute for lat/long values
- Variants with pagination
- Record changes in the system log
- Version management for MM configuration and MM items


Twig Templates (NEW)
.....................

Every MetaModels template can now optionally be offered as a **Twig template** as well. If a Twig
variant exists for a template, it takes **precedence** over the classic ``.html5`` template — exactly
as in Contao itself. If the Twig variant is missing, the ``.html5`` template is rendered unchanged
(full backward-compatibility fallback).

Precedence applies both in the **frontend and the backend**, and for **both output formats**: the
visible output (format ``html5``) and the text output (format ``text``, used for the search index and
sorting). The text output is rendered via its own template with the ``.text.html.twig`` suffix — so
the former ``mm_attr_text.text`` becomes ``@Contao/metamodels/attribute/text.text.html.twig``. The
doubling in the name of the text attribute is not a typo: the first part is the attribute type, the
second the format. If a Twig variant is missing here too, the legacy template is used unchanged.

.. note:: MetaModels ships Twig text variants **only for the** ``attribute`` **group** — that is
   where the content for the ``text`` node in ``raw`` or for the search index is generated. There are
   none for ``filter`` and ``item``.

**Naming scheme:** The Twig templates reside in the managed ``@Contao`` namespace, in their own
subgroup ``metamodels/``. The Twig identifier is derived from the former (flat) template name by
removing the conventional prefix and prepending the group (``attribute``, ``filter``, or ``item``):

============================  ===========  =========================================================
Previously (``.html5``)       Group        Twig
============================  ===========  =========================================================
``mm_attr_text``               attribute    ``@Contao/metamodels/attribute/text.html.twig``
``mm_filteritem_default``      filter       ``@Contao/metamodels/filter/default.html.twig``
``metamodel_prerendered``      item         ``@Contao/metamodels/item/prerendered.html.twig``
============================  ===========  =========================================================

Because the templates reside in the ``@Contao`` namespace, they can be edited without any extra
effort in Contao's **Template Studio** and can be overridden via **theme folders** and the global
project ``templates/`` directory.

**Providing your own Twig templates:** Custom Twig templates are placed — as in Contao's own
bundles — under a *namespace root*: a ``twig/`` folder with an empty marker file ``.twig-root``. In
the project, the ``templates/`` folder is already a namespace root, so the subfolder structure can
be used there directly:

.. code-block:: text

   templates/
   └── metamodels/
       ├── attribute/
       │   └── text.html.twig
       ├── filter/
       │   └── default.html.twig
       └── item/
           └── prerendered.html.twig

The same variables are available within a template as in the ``.html5`` version (e.g. ``{{ raw }}``
for an attribute). The blocks known from the ``.html5`` templates are real Twig blocks — for example
a filter widget extends the standard template with
``{% extends "@Contao/metamodels/filter/default.html.twig" %}`` and overrides the blocks
``formlabel`` and ``formfield``.

**Existing overrides:** An existing override of a **flat** template name in the project's
``templates/`` folder (or in a theme folder) — e.g. ``templates/metamodel_prerendered.html5`` — still
**takes precedence** over a shipped Twig template. Existing customizations therefore continue to
work unchanged after the upgrade.

.. note:: This consideration for flat overrides is a **transitional solution** and will be dropped
   in MetaModels 2.6 (Contao 6.3) together with the ``.html5`` templates. Custom customizations
   should therefore be moved to ``templates/metamodels/<group>/…`` — either as ``.html.twig`` or
   (as a transitional measure) as ``.html5`` under the new path.

**The Frontend Editing templates are now Twig-capable too.** This affects the input mask itself
(``dcfe_general_edit``) as well as the widgets for files (``form_upload-on-steroids``), the
MultiColumnWizard (``form_mcw``), and the multi-text field (``form_text_multiple``).

These do **not** follow the ``metamodels/<group>/<leaf>`` scheme described above, but Contao's own
convention with a flat name: an ``@Contao/dcfe_general_edit.html.twig`` takes precedence over the
``.html5`` file of the same name. This is not a MetaModels mechanism, but Contao's built-in Twig
precedence for legacy templates — it applies to every template output via Contao's template class.

In practice this means: anyone who wants to override one of these templates creates either a
``.html5`` as before or, now, a ``.html.twig`` under the same name. An existing ``.html5`` override
with higher priority keeps its precedence, so existing customizations continue to run unchanged.

There is one rule to note for a custom Twig version of the widget templates — the ``label`` block is
replaced for fields with a language badge, see :ref:`rst_extended_frontend_editing`.


Backend Icons (Reworked)
..........................

All backend icons have been switched from **PNG to SVG**. This keeps them crisp at any size — even
when the browser display is enlarged or on high-resolution screens.

**The six areas of a MetaModel can be told apart by color:** attributes (blue), render settings
(green), input mask (orange), search settings (purple), filters (red), and assignments (magenta).
The icons of the individual attribute and filter types deliberately remain a plain gray — they sit
next to each other in long lists, where added color would only look restless.

|mm_list_with_icons|

The colors chosen were selected so that they fit into Contao's color scheme, are distinguishable
from one another, work as well as possible in both light and dark mode, and remain distinguishable
even for red-green color blindness.

**Dark mode is supported throughout.** For every symbol whose color does not work in the dark
design, a dedicated version is provided; Contao automatically shows the right one. Disabled buttons
appear in a pale version of the same symbol, again for both designs.

An **overview page** is available here: :ref:`manual_new_icons-25`.

Three places have also received new icons:

* **Conditions in the input mask** previously all shared the same symbol. Each type now has its own
  — AND, OR, NOT, "Property is visible", "Property has the value", and "Property contains one of".
  In a nested condition, this makes it possible to see at a glance how it is built up.
* **The checkbox attribute** is no longer toggled in the item list with an eye icon, but with a
  checked or empty checkbox. An eye implies visibility, but a checkbox attribute can mean anything —
  "paid", "reviewed", "member". The colors match those Contao uses for published and unpublished.
* **The note list** now shows on the MetaModel whether a note list is set up at all: the symbol is
  filled as soon as any exist, and stays empty otherwise. This applies in the list of MetaModels as
  well as in the breadcrumb.

.. seealso:: Anyone who generally finds the icons in the backend too small can enlarge them with the
   `contao-backend-size-bundle <https://github.com/e-spin/contao-backend-size-bundle>`_ extension in
   their own user profile — the setting applies per user, not for the whole installation. The
   extension is **not** part of MetaModels and can be used independently of it; thanks to the switch
   to SVG, the MetaModels icons stay crisp even so.


Quick Access to a MetaModel's Areas (NEW)
............................................

Anyone configuring a MetaModel constantly jumps back and forth between its areas: from the
attributes to the input mask, from there to the render settings, then to the filters. Until now,
every such switch went back through the full list of all MetaModels.

On the right of the breadcrumb, the icons for **all areas of the MetaModel** you are currently in
now appear — attributes, render settings, input mask, search settings, filters, assignments, and,
if installed, the note lists. A click takes you straight there.

|mm_breadcrumb_icons|

This quick access appears everywhere it is clear which MetaModel is meant: in the lists of the
individual areas as well as in the edit masks within them. It does not appear in the **overall
list** of all MetaModels — no single MetaModel is meant there. A newly created MetaModel gets it as
soon as it has been saved.

Which icons appear depends on the areas that exist; if an extension adds another one, it
automatically appears in the row as well.


Custom Backend Sections via Configuration (NEW)
..................................................

A custom section (group) in the backend navigation — previously only buildable by hand via a custom
``MenuEvent`` listener plus an SVG icon — can now be created directly via configuration
(`ticket core#1519 <https://github.com/MetaModels/core/issues/1519>`_):

.. code-block:: yaml

   meta_models_core:
       be_sections:
           products:
               name:
                   de: 'Produkte'
                   en: 'Products'
               tooltip:
                   de: 'Produkte erstellen'
                   en: 'Create products'
               icon: 'files/theme/mm/products.svg'
               add:
                   before: design

``products`` is the unique alias of the section. ``name`` and ``tooltip`` are language maps — the
current backend language is shown, otherwise English, otherwise the first existing entry. ``icon``
is a web path (typically under Contao's file manager, ``files/…``); if omitted or if the file cannot
be found, the gray MetaModels default icon is shown instead (the same one also used by the
MetaModels' own sections, but there in blue). Under ``add``, exactly one of ``before`` or ``after``
sets the position relative to an existing navigation entry (e.g. ``design``, ``content``,
``accounts``, or another section created via configuration); ``collapsed: true`` makes the section
start collapsed on first load.

.. note:: The target alias is the **internal** Contao group name, not the displayed label — the
   former "Layout" section has been called ``design`` internally since Contao 4/5, not ``layout``.
   If the given alias is not found in the navigation, the custom section is appended at the end
   instead.

This configuration only creates the **empty group** — it is populated as before: via custom modules
or, for standalone MetaModels screens, by specifying the section on the input mask itself.

.. seealso::


Disabled Entries Are Shown Struck Through
.............................................

In the backend lists it was previously hard to see whether an entry is disabled — the symbol at the
end of the row indicated it, but the name itself did not. The name of a disabled entry is now shown
**struck through**. This affects render settings, the input mask, filter rules, and file selection.

In file lists, the suffix "[Default]" appears after the name. It stays readable and is not struck
through as well, so the two pieces of information do not run into one another.


Breadcrumb for Child Tables (NEW)
.....................................

When viewing the list of a **child table**, the header previously only showed the name of the
module. Where you came from and which record the list belongs to was not shown anywhere — with
multiple levels of nesting, it was easy to lose your orientation.

The full path now appears there, from the base model down to the current level:

.. code-block:: text

   Employee: Mayer, Herbert  ›  Business Trips

Each link in the path is clickable and leads to the list view of its level, so switching between
levels no longer requires going through the overall list of all MetaModels. With deep nesting, the
base model, the last parent level, and the current level remain visible; anything in between
collapses into ``…`` and can be expanded:

.. code-block:: text

   Employee  ›  …  ›  Child 3: Third Child  ›  Business Trips

In the **edit mask**, the record being edited is added as the last link.

What names a record in the path is determined by its level's input mask setting **"Additions to the
mask headline"** — the same field that already extended the edit mask's headline before. It accepts
Simple Tokens for the attributes, and ``##model_id##`` outputs the ID:

.. code-block:: text

   ##model_name##, ##model_firstname##
   ##model_city## [##model_id##]

Without a value, only the name of the MetaModel appears at that point. There is therefore no new
field, and any values already maintained take effect immediately.

.. note:: Lists **without** a parent keep their previous headline. Contao shows either a breadcrumb
   or a headline at this point, and at the top level the breadcrumb previously said the same thing
   as the headline anyway.

The look and behavior come from Contao itself — it is the same breadcrumb as in the core modules,
including the expand menu behind the ellipsis. See also :ref:`component_relations` for child tables.


DC_General
----------

DC_General was upgraded to **version 2.5** for **Contao 5.7**. The key changes:

* **New referer handling:** Contao 5.7 no longer determines the reference page via the session, but
  via the ``DcaUrlAnalyzer``. This does not work for the MetaModels tables, so the links for "Back"
  and "Save and close" are now generated **deterministically** (new: ``ViewHelpers::getBackUrl()``).
  The former ``StoreRefererListener`` is dropped.
* **"Save and back" button (saveNback) removed:** DC_General follows Contao Core 5.7.0 here — the
  ``saveNback`` button has been removed from the input mask and "Edit all". "Save and close"
  (``saveNclose``) remains available.
* **Contao legacy code removed:** Classes and paths no longer needed from Contao versions below 5.7
  have been removed (among others ``TreeSelect``, ``FileSelect``, and the dead file-selector path in
  the ``FileTree`` widget).
* **Sortable selection lists switched to Contao 5 technology:** The ``fileTree`` widget and the tree
  pickers now use Contao's Stimulus controllers ``contao--sortable`` and ``contao--input-map``
  instead of ``Backend.makeMultiSrcSortable()``, which is marked deprecated as of Contao 5.7. The
  button for removing a file is now rendered server-side instead of being added afterwards via
  JavaScript. In addition, the ``fileTree`` widget now evaluates the widget option ``isSortable``,
  with which Contao has replaced the removed ``orderField`` option since version 5.0 — the latter is
  still supported. Nothing changes in how it is used.
* **Tree pickers can now be restricted (NEW):** A tree picker builds its own view of the target
  table and therefore always offered all records — even where the list next to it showed only a
  filtered selection. Via the new widget option ``sourceFilter``, the caller can supply the allowed
  IDs; the picker then restricts itself to those. An empty list here means "nothing matches", not
  "no filter" — anyone specifying a restriction gets it, even in that case. Nothing changes without
  the option. It is used by the Select and Tags attributes, see there.
* **Tooltips of the operation buttons fixed:** In the list views, the display of the tooltips
  (including the icons for opening child tables) has been fixed.
* **Cycle in visibility conditions caught:** If the visibility conditions of two fields referenced
  each other (field A only visible if field B is, and vice versa), the input mask previously crashed
  with a fatal error. Such a cycle is now detected; the fields involved simply stay hidden, and the
  mask remains usable.
* **Understandable feedback for missing permissions:** If a record could not be deleted, created, or
  edited, DC_General previously showed a technical error screen with English developer text. Logged-in
  editors without the required permission now see an understandable message, while visitors who are
  not logged in (e.g. in Frontend Editing) instead see the login page.
* **Technical errors during automatic reload remain visible:** If an input mask triggered a technical
  error while processing a field value during an automatic reload (e.g. due to a visibility
  condition) — for instance caused by a faulty extension — the message previously vanished silently
  when the mask was rebuilt. It now stays visible until the field is edited again.
* **Languages outside Contao's language list:** If a MetaModel supports a language that is not
  enabled as a language in Contao's settings (e.g. ``en_DE``), the input mask's language selector
  produced a PHP warning and an empty entry. It now falls back to the ICU display name and, as a
  last resort, the language code itself, so the language always remains selectable.
* **Services are now passed via the constructor:** Several DC_General classes previously fetched
  required services from the Symfony container at runtime; they now receive them as constructor
  arguments. Nothing changes in operation, and **no MetaModels package is affected**. This is only
  relevant for **custom extensions** built on top of DC_General: the event listener ``WidgetBuilder``
  has been changed from a static to a regular method, and its constructor now takes four required
  arguments. Anyone calling ``WidgetBuilder::handleEvent()`` statically or instantiating the class
  with two arguments needs to adjust — details in DC_General's ``docs/upgrade-2.5.md``.
* **MooTools completely removed:** All of DC_General's backend JavaScript has been switched to
  vanilla JS and Contao 5.7's Stimulus controllers. This also affects the **markup**: the deprecated
  marker classes ``click2edit`` and ``picker_selector`` and the ID ``sbtog`` have disappeared from
  the templates, as have all ``onclick`` attributes with ``Backend.*`` calls. The shipped JS files
  have been renamed in the process (``dcGeneralAjax.js`` → ``generalAjax.js``, ``vanillaGeneral.js``
  → ``generalBase.js``, ``generalDriver_src.js`` → ``generalDriver.js``); there is no longer a build
  step, the shipped file **is** the source. Anyone overriding custom DC_General templates or
  including these files directly needs to follow suit — details in DC_General's
  ``docs/upgrade-2.5.md``.
* **Variants now follow the sort order of their base record:** In tree views, child entries were
  previously always output according to the internal ``sorting`` column, while the top level
  followed the sort order configured in the input mask. In a list sorted by an attribute, the base
  records therefore appeared in the desired order while their variants beneath them seemed to be in
  arbitrary order. Both levels now use the same sort order.

  This only affects lists with an **attribute sort order**. Nothing changes with manual sorting, nor
  when no sort order is set at all — in that case the order from ``sorting`` still applies as
  before. The change affects all tree views in DC_General, not only variants.

* **The visibility toggle now follows Contao's model:** Previously, clicking the toggle only swapped
  the icon of the clicked entry via JavaScript. That could only be correct for that one entry: in a
  **variant hierarchy**, variants inherit the value from the non-variant record — toggling the parent
  record changed the state of the variants logically as well, but their icons stayed as they were
  until the page was reloaded. The toggle is now a regular link: the server saves the new state and
  re-delivers the list, so that **all** affected rows are immediately shown correctly. Nothing
  changes in how it is used.
* **Derived values in variants now follow changes to the parent record:** If a non-variable value
  was subsequently changed in the parent record, it was, as before, propagated to the child records
  — but derived variable attributes such as Combined Values or Alias that include this value were
  not recalculated in the child records and remained outdated until the child record itself was
  edited (`issue #657 <https://github.com/MetaModels/core/issues/657>`_). See also
  :ref:`component_relations_variants`.
* **"Toggle all" in the tree view (NEW):** Next to the root entry there is now a link that expands
  or collapses all nodes of the tree view at once — analogous to what Contao itself offers in its
  own page tree. This affects the variants view as well as MetaModels with render mode "Hierarchy"
  (`issue #560 <https://github.com/contao-community-alliance/dc-general/issues/560>`_). See also
  :ref:`component_relations_variants`.
* **Record changes in the system log (NEW):** Creating, duplicating, and deleting MetaModels records
  now appear under "System → Log" — exactly as Contao has always done for its own tables. Previously,
  changes to MetaModels data did not appear there at all (`issue #577
  <https://github.com/contao-community-alliance/dc-general/issues/577>`_,
  `issue #1461 <https://github.com/MetaModels/core/issues/1461>`_). The entry names the record, not
  just the table and ID — the same name also shown by the input mask and the breadcrumb navigation.
  As with Contao's own tables, editing is deliberately not logged this way — that is what versioning
  is for. Logging can be disabled via an option on the MetaModel, but is active by default.
* **Version management for MM configuration and items (NEW):** The input mask now shows a selection
  of earlier versions of a record with date and editor, from which an older state can be restored —
  as known from Contao's own tables. This applies to MetaModel records themselves as well as to the
  configuration of input masks, render settings, and filter rules. Previously, this toggle had no
  effect despite the existing option (`issue #52
  <https://github.com/contao-community-alliance/dc-general/issues/52>`_, open since 2014) — for
  MetaModel records, this connection did not exist at all before. It can be disabled per table via
  the table's own DCA as usual. For translated MetaModels, all language variants of a record share a
  common version history.
* **Pagination bar below list views (NEW):** If a list contains more records than the page block
  shows, a bar with "Page x of y" and the page numbers now appears below the table — as known from
  the Contao backend. In addition, "First", "Previous", "Next", and "Last" jump to the outer pages.
  The page-block selector in the panel remains unchanged; the bar is added without replacing
  anything. If everything fits on one page, it is not shown. The selected page is retained like the
  other panel settings, but is deliberately reset as soon as a filter or the block size is changed —
  otherwise you would end up on a page that no longer exists after the new selection.

  **The tree view** also now has a pagination bar, and there the page-block selector is new as well
  — previously it was hidden and the tree was always loaded in full. Only the **top-level records**
  are counted here, i.e. the base records for variants. Each base record appears together with all
  its variants; "Page 1 of 3" therefore means "bases 1-3 of 7", not "rows 1-3 of 20". Paging through
  all nodes would separate bases from their variants. Whatever is expanded stays expanded while
  paging.
* **Turbo Drive is active in the backend:** Navigation between the MetaModels backend pages now runs
  through Contao's Turbo Drive, i.e. without a full page rebuild; the scroll position is preserved.
  DC_General's **forms** are deliberately excluded (``data-turbo="false"``), because automatic
  submission triggered by visibility conditions re-renders the mask without a redirect — a response
  that Turbo would discard.
* **Input mask and save operations sped up:** When building the input mask, the data model was
  previously reassembled completely for **each individual field** — for a mask with 27 fields, that
  meant 27 times. This now happens once per pass. As a result, the number of attribute conversions
  drops substantially — in the test case from 1,785 to 221 calls per save operation. Also mitigated:
  determining whether a request originates from the backend was previously repeated around 6,000
  times per save operation and is now cached once per request. Nothing changes in behavior or
  result, this is purely about runtime.

.. note:: **Anyone who finds the backend slow should check the environment first** — not
   MetaModels. In one measurement on the same input mask, a save operation took around 10.9 seconds
   in Symfony **dev** mode with Xdebug active, 4.2 seconds in dev mode without Xdebug, and 0.75
   seconds in **prod** mode. Having ``xdebug.mode=debug`` with ``start_with_request=yes`` permanently
   enabled alone costs a factor of 2.6, because a connection attempt to the debugger is made on
   **every** request. On production systems, Xdebug should be disabled and ``APP_ENV=prod`` set.


Attributes
----------

**Twig variants** are now available throughout for the attribute templates, under
``metamodels/attribute/<type>`` (see the "Twig Templates" section). The existing ``.html5``
templates remain in place as a fallback and are only used if they have been overridden in the
project's ``templates/`` directory; they will be dropped in MetaModels 3.0.

* **The surrounding block now comes from the attribute template:** Up to 2.4, the list template
  output the block ``<div class="field …"><div class="label">…</div><div class="value">…</div></div>``
  around every value, while the attribute template only delivered the innermost snippet. Anyone
  wanting to customize the output could not reach the surrounding container this way
  (`core#660 <https://github.com/MetaModels/core/issues/660>`_). As of 2.5, the attribute template
  outputs the block itself.

  **Nothing changes for existing output.** For this purpose, there is an option in render settings,
  "Wrapper in list template (legacy behavior, deprecated)", and a migration sets it during the
  upgrade for all existing render settings. Only newly created render settings start without the
  option. It is marked deprecated from the outset and will be dropped in 3.0.

  Note: custom attribute templates do not output the block unless they have been adjusted — so it
  is missing in a newly created render setting. In the column mode of the backend list, no block is
  deliberately output, because the column heading already carries the label. And anyone using the
  ``html5`` node outside the list template, e.g. via ``parseAll()`` in custom code, gets different
  values for new render settings; ``text``, ``raw``, and ``attributes`` remain unchanged.

  For custom attribute templates, there are four new values for this: ``label``, ``colName``,
  ``hideLabels``, and ``legacyAttributeWrapper``. The pattern, with an example, is documented under
  :ref:`component_templates_attribute-wrapper`.

* **Render attributes only on demand (NEW):** New option "Render attributes only on demand (Lazy)"
  per render setting. Previously, MetaModels always rendered both output formats (HTML5 and text)
  for every attribute, regardless of whether the list template actually used them. When Lazy is
  enabled, an attribute is only rendered when the template actually accesses it — and separately per
  format, so accessing only one format does not also render the other.

  This is worthwhile for templates that only output part of the configured attributes or
  consistently use only one output format — depending on how large the unused portion is, this can
  bring a noticeable to significant speed gain. If a template accesses all attributes in both
  formats anyway, the option provides no benefit and can add a small overhead, because Twig's
  general object access is somewhat slower than a plain array.

  Unlike the wrapper block above, this option is **not** legacy behavior applied to existing render
  settings by a migration, and it is **not** deprecated: whether Lazy is worthwhile depends on the
  specific template, so there is no generally "better" side. The default is therefore off for both
  new and existing render settings alike — for more on the mechanics and benchmarks, see
  :ref:`component_rendersettings`.

* Select, Translated Select, Tags, and Translated Tags
    * **Jump to the relation table (NEW):** Next to the field label in the backend there is now a
      symbol that opens the table the attribute refers to — in a new tab, so the unsaved input mask
      is preserved.
    * If the attribute refers to a **MetaModel**, the jump leads to its backend module; if it refers
      to a **Contao table**, to the relevant Contao module. For the latter, a mapping is stored
      (among others ``tl_page``, ``tl_article``, ``tl_news``, ``tl_calendar_events``, ``tl_faq``,
      ``tl_member``, ``tl_user``).
    * **Permissions are respected.** The symbol always appears, but is grayed out and not clickable
      if the target table is not available for the user's own user group, if the field is read-only,
      or if no module is stored for a Contao table. The reason is shown in the tooltip in each case.
      Administrators see everything.
    * There is no symbol if the target MetaModel is only maintained as a **child table** — that is
      not reached via its own call, but via the operation in the parent list.
    * The symbol does not appear in **Frontend Editing**.
    * **The configured filter now also applies in the tree-picker popup (NEW):** If the attribute is
      set up as a tree picker, the popup previously opened the full target table — the restriction
      only affected the selection list next to it. This made it possible to select items that
      should never have been selectable in the first place. The popup now honors the same
      restriction.
    * This applies to **both ways** of restricting a target: the *filter setting*, when the
      attribute refers to a MetaModel, and the *condition* (SQL) for a Contao table. Both are served
      from the same query the selection list draws its entries from — the two views can therefore
      not diverge.
    * For the condition, note the **table alias**; it differs between attribute types. For Tags it
      is ``t.``, for Select it is ``sourceTable.`` — e.g. ``sourceTable.username='tester'``. The
      applicable alias is shown in the description of the input field.
    * If **no** filter is set, nothing changes. In that case, the restriction is not even
      determined — a tree picker is used precisely where the target table is too large for a
      selection list.
    * **To note for Select and Translated Select:** If a restriction is tightened afterwards,
      already saved values that no longer match it disappear from the mask — both in the picker and
      in the selection list, as was already the case before. The next time the record is saved, they
      are then also gone from the stored data. Before tightening a restriction, it is therefore
      worth checking whether existing records are affected.
    * **This no longer applies to Tags and Translated Tags (NEW):** References hidden by a filter
      setting of the mask now survive saving unchanged — regardless of whether the restriction has
      just been tightened or has been in place for a while. Previously, every save of a record
      deleted all references missing from the currently visible subset, even if the editor never had
      them available to select and consequently could never have deselected them either. This
      affected the ``tags`` attribute as well as Tags references to another MetaModel.

* Levenshtein (levenshtein)
    * **Spelling corrected throughout:** The attribute type had been called ``levensthein`` — with
      the ``h`` and ``t`` swapped — since its very first version. The class names, Composer package,
      and template had already been fixed earlier, but not the type name itself, the two index
      tables, and two columns in ``tl_metamodel_attribute``. This has now been caught up: it is
      ``levenshtein`` everywhere.
    * Affected are the tables ``tl_metamodel_levensthein`` and ``tl_metamodel_levensthein_index``,
      the columns ``levensthein_distance`` and ``levensthein_attributes``, as well as the type name
      stored in ``tl_metamodel_attribute`` and ``tl_metamodel_filtersetting``. The attribute's
      filter rule also carried the wrong name.
    * A **migration** renames both and carries over the stored type names — the existing search
      index is preserved and does **not** need to be rebuilt. Nothing needs to be done manually.

* Rating (rating)
    * the **MooTools variant has been removed** (template ``mm_attr_rating_moo.html5`` as well as
      the MooTools JS files ``moostarrating.js``/``moostarrating_src.js``) — the vanilla star-rating
      variant remains
    * new Twig templates ``metamodels/attribute/rating`` (includes the JS via ``{% add … to body %}``)
      and ``metamodels/attribute/rating_raw``

* File and Translated File
    * the **separate sorting column is gone** — the order of multiple files is now embedded in the
      value itself, exactly as Contao has handled it since version 5.0
    * previously, MetaModels created an additional column ``<column-name>__sort`` in the item table
      when the *Multiple files* (``file_multiple``) option was set; for *Translated File*, the
      column ``value_sorting`` in ``tl_metamodel_translatedlongblob`` took on this role
    * background: Contao removed the widget option ``orderField`` in version 5.0 — the ``fileTree``
      widget now only knows ``isSortable`` and stores the order directly in the field value. Manual
      sorting had therefore effectively become ineffective under Contao 5
    * a **migration** transfers the existing order into the value and then deletes the column:
      entries from the sort column first, followed by the remaining files in their previous order
    * the virtual helper attributes ``<column-name>__sort`` are dropped; the classes ``FileOrder``
      and ``TranslatedFileOrder`` are marked *deprecated* and will be removed in MM 3.0
    * nothing changes in how it is used: multiple files are still sorted in the input mask via drag
      and drop and removed individually from the selection via the button on the preview image

* **LatLong (NEW):** new attribute ``metamodels/attribute_latlong`` — stores a coordinate pair
  (latitude/longitude) as a native ``POINT`` in a single column instead of two decimal attributes or
  a text attribute with comma-separated values — :ref:`more... <component_attribute_latlong>`

    * optionally a **spatial index** on the column, which the :ref:`perimeter search
      <component_filter_perimeter-search>` automatically uses for significantly faster searches
      (see "Filter" below)
    * if `cowegis/cowegis-contao-geocode-widget-bundle
      <https://github.com/cowegis/cowegis-contao-geocode-widget-bundle>`_ is installed, manual
      coordinate entry can be replaced by an **address search with map selection** — either still as
      two fields or as a single comma-separated value

* Geo Distance (geodistance)
    * the distance calculation now uses the native spatial function ``ST_Distance_Sphere()`` instead
      of the previous formula — the latter was called "Haversine" but was in fact only a flat
      approximation without real earth curvature
    * in single mode, only a :ref:`LatLong attribute <component_attribute_latlong>` can be selected
      (previously unusable — the select filtered on an attribute type that never existed)
    * new option **Rounding step (km)** — rounds the displayed distance value to a multiple of this
      value, without affecting the sort order (which always remains exact)


Filter
------

* The filter widgets in the frontend are now rendered via the MetaModels template engine, and
  therefore follow the same ``@Contao/metamodels/filter/<name>`` scheme as attributes and items (see
  the "Twig Templates" section).
* **Static parameter with multilingual attributes:** The "simple lookup" filter rule with "Static
  parameter" set allows a preselection in the content element or FE module. If the rule is backed by
  an attribute whose values are translated, this preselection was previously tied to the language in
  which it was set: anyone who set it in German and later opened the element with an English profile
  language saw an unreadable entry "Unknown option: …" instead of the selection — and lost the
  setting as soon as they touched the field.

  The selection is now resolved via the referenced record rather than the value itself. This means
  the field shows the correct entry regardless of the profile language. This also applies if the
  MetaModel carries a language for which there is no backend profile language at all — the fallback
  language then applies.

  **Filtering was never affected.** The stored value is converted to the ID of the referenced record
  before the query, regardless of language. Existing preselections remain valid, no migration is
  needed. One thing to note: saving an element whose value was found in a different language rewrites
  it to your own language — equivalent, but the stored value moves with you.

  The same applies to the preselection in a MetaModel's **search settings**, which use the same
  filter parameters.
* **Perimeter search: radius disappears with the address.** If the address field was cleared, the
  previously selected radius stayed visible in the widget — even though it was never evaluated
  without an address anyway (`issue #31
  <https://github.com/MetaModels/filter_perimetersearch/issues/31>`_). It is now reset together with
  the address. Only the widget display is affected; the filtering was never incorrect. See also
  :ref:`component_filter_perimeter-search`.
* **Perimeter search: significantly faster with the new LatLong attribute.** The distance
  calculation now uses ``ST_Distance_Sphere()`` instead of the previous formula (see "Attributes"
  above) — that alone already roughly doubles the speed. When used with data mode "Single attribute"
  and a :ref:`LatLong attribute <component_attribute_latlong>` that has a spatial index, the
  perimeter search additionally combines an index-backed bounding-box pre-filter with the exact
  calculation. Measured on 500,000 records and a 50 km search: from 0.40 s to 0.014 s — **around 28×
  faster** than before. Details:
  :ref:`Special functions with the LatLong attribute <component_attribute_latlong_special>`.


Frontend Editing (FEE)
-----------------------

Frontend Editing itself has not changed in MM 2.5 — the feature set matches that of MM 2.4. Two
points affect it indirectly, though:

* The **templates of the input mask and its widgets** can now also be overridden as Twig templates —
  see the "Twig Templates" section.
* The **marking of translated fields** with a colored badge next to the label continues to work
  unchanged — green for a translation of its own, orange for a value inherited from the fallback
  language, with the explanatory sentence as a tooltip. Nothing changes for editors.

  Under the hood, this required some work: Contao 5.7 renders form fields in the frontend via Twig
  templates that escape the label — HTML in the label would appear there as source code. Up to
  Contao 5.3, the old ``.html5`` templates output it unchanged, which is why the badge could simply
  be placed in the label. It is now output via a dedicated template that MetaModels assigns only to
  the affected fields; forms outside of Frontend Editing remain unaffected. Fields still output via
  an ``.html5`` template continue to get the badge directly in the label.

For the overall handling of Frontend Editing, see :ref:`rst_extended_frontend_editing`.


Known Issues
------------

* When toggling to/from debug mode in the BE via the button, the reference page is no longer correct
  and the page must be navigated to again — e.g. with "back" in the browser and reloading the page |br|
  The cause is the toggle link generated by Contao, ``?do=debug&key=enable&referer=…``: on the
  route-based MetaModels backend pages (e.g. ``/contao/metamodel/mm_employees``), the ``referer``
  parameter stays **empty**, so Contao returns to the backend dashboard after toggling instead of the
  original page. This affects Contao's own debug toggle and is not caught by DC_General's new referer
  handling (its own "Back" buttons) — Contao offers no way to influence the referer at this point.


.. _check_upgrade_mm250:

Checklist for Upgrading to MM 2.5
-----------------------------------

In general, an upgrade within the MM 2.x branch is straightforward; any necessary label adjustments
and DB changes are handled via migrations. However, there are a few things that cannot or can only very
difficultly be caught by migrations. For this reason, the following points should be kept in mind when
upgrading to MM 2.5:

* Please follow all notes from :ref:`MM 2.4 <check_upgrade_mm240>`
* Check the requirements: **Contao 5.7** and **PHP 8.4**
* **Custom template overrides** using the flat name (e.g. ``templates/metamodel_prerendered.html5``)
  continue to work, but should be moved to ``templates/metamodels/<group>/…`` — precedence for flat
  overrides is dropped in MM 3.0
* Anyone shipping custom Twig templates: use a ``twig/`` folder with a ``.twig-root`` marker and the
  structure ``metamodels/<group>/<leaf>.html.twig``
* **DC_General:** note the changes to referer handling and the removal of the "Save and back"
  (saveNback) button; adjust custom templates/code that rely on ``saveNback``
* **Sorting links:** anyone who overrode the Contao keys ``MSC.orderMetaModelListByAscending``/
  ``…Descending`` for the labels of the sort links should switch to ``sorting_direction_label``
  (with ``%attribute_name%`` and ``%direction%``), ``sorting_direction_asc``, and
  ``sorting_direction_desc`` in the ``metamodels_default`` domain
* **Custom DC_General extensions:** the event listener ``WidgetBuilder`` has a changed signature
  (``handleEvent()`` no longer static, four required constructor arguments). Only relevant to those
  who call or instantiate it themselves — the shipped MetaModels packages do not
* **DC_General backend JavaScript:** anyone overriding custom DC_General templates, running custom
  JavaScript against their markup, or including the shipped JS files directly needs to follow suit —
  MooTools is gone, the marker classes (``click2edit``, ``picker_selector``, ``sbtog``) and the
  ``onclick`` attributes have been replaced, and the JS files renamed
* **Levenshtein attribute:** the consistent correction from ``levensthein`` to ``levenshtein`` is
  handled via migration, and the search index is preserved. Only those who use the old names
  **themselves** need to adjust: custom SQL queries or exports against
  ``tl_metamodel_levensthein``/``tl_metamodel_levensthein_index`` or the columns
  ``levensthein_distance``/``levensthein_attributes``, as well as custom PHP code that hard-checks
  the type name ``levensthein``. The class ``LevenstheinSearchRule`` remains available as a
  *deprecated* alias for the transition and will be dropped in MM 3.0
* **Icons:** the icons are now available as SVG, and the replaced PNG files have been **removed**.
  Nothing changes in how they are used. Only those who use the old files themselves need to adjust:
  custom CSS that embeds a MetaModels symbol as a background image, or custom DCA entries pointing to
  a ``.png`` path under ``bundles/metamodels…/images/``. The extension needs to be changed to
  ``.svg`` there
* **File attributes:** the sort columns ``<column-name>__sort`` and ``value_sorting`` are migrated
  into the value and then **deleted** — be sure to make a backup beforehand, as deleting the columns
  cannot be undone. Custom code or evaluations that access these columns directly must be adjusted;
  the order is now embedded in the value itself. In the parsed value, the previous keys
  ``bin_sorted``/``value_sorted``/``path_sorted``/``meta_sorted`` (File) and ``value_sorting``
  (Translated File) are retained, and now correspond to the unsorted counterpart. The key ``sort``,
  already marked deprecated since 2.1, is dropped


Re-Financing
------------
.. seealso:: To re-finance the extensive work, the MM team asks for financial contributions. As a
   guideline, take the scope of the project to be realized and budget approximately 10% — based on
   the experience of past contributions, these are amounts between €100 and €500 (net) — an invoice
   including VAT is of course always issued. `More... <https://now.metamodel.me/de/unterstuetzer/spenden>`_


.. |mm_list_with_icons| image:: /_img/screenshots/new_in_2-5/mm_list_with_icons.png
.. |mm_breadcrumb_icons| image:: /_img/screenshots/new_in_2-5/mm_breadcrumb_icons.png


.. |br| raw:: html

   <br />
