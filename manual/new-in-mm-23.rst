.. _new_in_mm230:

Changes and Features in MM 2.3
===============================

The following is an overview of the changes and features in MetaModels 2.3, made possible by the
"early adopter program" — more information under Fundraising on the
`MM website <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-3>`_.

For a checklist after upgrading to MM 2.3, see :ref:`further notes below <check_upgrade_mm230>`.

.. note:: To create mm_* tables and attribute columns, a DB migration must be performed —
   see :ref:`Schema Manager <component_schema-manager>`. |br|
   After creating or modifying the labels of models, attributes, or legends, please clear the (translation) cache —
   see :ref:`component_translations`.


General and Core
----------------

The installation requirements for MetaModels 2.3 are:

* a running Contao 4.13.x (LTS)
* PHP 8.1 or higher
* MySQL 5.5.5 or higher (InnoDB), MariaDB (incl. "strict mode")
* ``memory_limit`` of 512MB or more (recommended)

* Integration of a new schema manager — :ref:`More info <component_schema-manager>`
* Entries for sorting/grouping now have a toggle button and can be enabled/disabled
  (`#1380 <https://github.com/MetaModels/core/issues/1380>`_)
* Note for developers: there is a new class for sorting attributes by name
  (src/CoreBundle/Sorter/AttributeSorter.php) — used for example in attribute selection for sorting
  (which is now sorted in ascending order)
* When the first sorting entry is created, the "Default" checkbox is now pre-selected
  (`#1472 <https://github.com/MetaModels/core/issues/1472>`_)
* If the render mode is set to "Hierarchy" in the input mask, a notice now appears that sorting
  must be set to "Manual" (`#1324 <https://github.com/MetaModels/core/issues/1324>`_)
* The "Variant" checkbox for attributes is disabled when the model is non-variant
  (`#884 <https://github.com/MetaModels/core/issues/884>`_)
* For variant items, the move buttons are disabled (`#871 <https://github.com/MetaModels/core/issues/871#issuecomment-2558575073>`_)
* For variant items, non-variant attributes in the mask are no longer hidden but displayed as readonly
* The "getSearchablePages" class (indexing of detail pages) has been completely rewritten and now
  runs more efficiently/faster
* There is a new event for manipulating the input mask headline:
  `GetEditMaskSubHeadlineEvent <https://github.com/contao-community-alliance/dc-general/blob/39ec68cee8b7034e5c1900692cd1b0eeaa7d4c7e/src/Contao/View/Contao2BackendView/Event/GetEditMaskSubHeadlineEvent.php>`_
* The input mask can be configured to display item values in the mask headline when editing
* Insert tags have been completely revised — :ref:`please note partially changed syntax <component_inserttags>`
* Adjustment to Contao's change of locale notation (now ``_`` instead of ``-``) — all uses of
  $GLOBALS[TL_LANGUAGE] marked as deprecated
* The sorting in the CE/module now has a setting for appending a URL fragment to target an anchor
  for ``generateSortingLink`` and ``renderSortingLink``
* In the list template ``metamodels_prerendered``, two methods are available to output links for
  toggling the sort order of an attribute — more in the :ref:`"Cookbook" <rst_cookbook_templates_fe_list_sorting>`
* Support for the new routing introduced in Contao 4.10 — legacy routing can now be disabled via
  config.yml (``legacy_routing: false``)
* Session handling has been migrated from Contao session to Symfony session
* Route priority handling — see :ref:`rst_cookbook_tips_set-route-priority`
* Widget template selection for the input mask (BE) — see Attributes
* Models linked as child tables can now contain variants (`#1054 <https://github.com/MetaModels/core/issues/1054>`_)
* The BE list can now be grouped by calendar week — formatting can be customized individually per
  language via a language key
* Translations have been migrated from the `CCA-Translator <https://github.com/contao-community-alliance/translator>`_
  and `Global-Lang-Arrays <https://symfony.com/doc/current/translation.html>`_ to the Symfony Translator.
  Translations are now stored in the corresponding Symfony message catalog, speeding up backend page loading.
  Custom translations can now also be maintained in XLIFF format. |br|
  Only a few places in the BE are noticeably affected by the switch — one fix enabled is the table view of
  items when an attribute in the list was not present in the associated input mask: previously only the
  translation key appeared, now the attribute's title is shown. |br|
  Existing custom translation texts created as PHP arrays
  `must be migrated to an XLIFF file <https://metamodels.readthedocs.io/de/latest/manual/component/translations.html#eigene-anpassung-von-ubersetzungen>`_. |br|
  More on this topic under :ref:`component_translations`
* A routing change has been made: MM masks in the BE are no longer accessed via the GET parameter
  ``...contao?do=metamodels_<model-name>`` but via the route ``...contao/metamodels/<model-name>``.
  This allowed a simplification of, for example, permission management in the BE. Previously, for user groups,
  clicks were required both in the input/render assignments ("last icon") and in Contao's user group settings —
  the Contao-side settings have been removed, and now only MM permissions need to be assigned
  (input mask + assignments). |br|
  The new routing introduces a problem with toggling the debug mode in the BE — Contao expects the referer
  value in a specific form that cannot currently be easily rewritten; after toggling, you land on a Contao
  "default page" — this has no further effects (see "Known Issues").
* In the render settings, the reference type for generating the URL for redirect links (jumpTo) can now be
  specified — for example, it is now possible to define an absolute URL including domain; see
  `Symfony UrlGeneratorInterface <https://github.com/symfony/routing/blob/f8dd6f80c96aeec9b13fc13757842342e05c4878/Generator/UrlGeneratorInterface.php#L34-L55>`_
* The templates for attributes in the render settings are now a required field
* For multilingual models, the panel (filter/search) in the BE list now responds to the list's language setting
* Core, attributes, and filters have been checked with the toolset `PHPCQ <https://github.com/phpcq/phpcq>`_ and
  adjusted accordingly — see `GitHub <https://github.com/MetaModels/core/issues/1502>`_
* :ref:`File-Usage Integration <rst_extended_file-usage>`


Attributes
----------

* For all attributes, the HTML5 templates have been revised: CSS class with attribute type and output type,
  PHP shortcode, surrounding HTML tag with optional CSS class output
* For all attributes, the backend template can now be selected via a dropdown — for the frontend see FEE

* Alias
    * Check for the variant/unique combination — `see news January 2025 <https://now.metamodel.me/de/mm-eap-newsletter-2-3/details/eap-info-mm-2-3-januar-i-2025>`_
* File
    * Support for predefined image size dimensions from `config.yaml` —
      see `contao.image.sizes:... <https://docs.contao.org/dev/framework/image-processing/image-sizes/#size-configuration>`_
    * Support for file search in the backend — search by filename or UUID; items where the file is
      embedded are shown, either as a direct file selection or when the parent folder was selected
    * :ref:`File-Usage Integration <rst_extended_file-usage>`
* Content Article
    * Template adjustment
    * Change to the backend list view output — instead of an overview of element types, the original
      rendering is now used; this is required for example for full-text search indexing — the output
      template can be customized individually
    * :ref:`File-Usage Integration <rst_extended_file-usage>`
* Combined Values
    * Check for the variant/unique combination — `see news January 2025 <https://now.metamodel.me/de/mm-eap-newsletter-2-3/details/eap-info-mm-2-3-januar-i-2025>`_
* Long Text
    * Long text supports readonly for TinyMCE and ACE — `see <https://github.com/contao/contao/pull/5985>`_
    * Fix of templates for text output: all HTML tags are stripped
    * :ref:`File-Usage Integration <rst_extended_file-usage>`
* Multi-Table (MCW)
    * Support for readonly and CSS classes for tl_class of the widget
* Text Table
    * Support for readonly
* Translated Alias
    * Check for the variant/unique combination — `see news January 2025 <https://now.metamodel.me/de/mm-eap-newsletter-2-3/details/eap-info-mm-2-3-januar-i-2025>`_
* Translated File
    * Support for predefined image size dimensions from `config.yaml` —
      see `contao.image.sizes:... <https://docs.contao.org/dev/framework/image-processing/image-sizes/#size-configuration>`_
    * Support for file search in the backend — search by filename or UUID; items where the file is
      embedded are shown, either as a direct file selection or when the parent folder was selected
    * :ref:`File-Usage Integration <rst_extended_file-usage>`
* Translated Content Article
    * Template adjustment
    * Change to the backend list view output — instead of an overview of element types, the original
      rendering is now used; this is required for example for full-text search indexing — the output
      template can be customized individually
    * :ref:`File-Usage Integration <rst_extended_file-usage>`
* Translated Combined Values
    * Check for the variant/unique combination — `see news January 2025 <https://now.metamodel.me/de/mm-eap-newsletter-2-3/details/eap-info-mm-2-3-januar-i-2025>`_
* Translated Long Text
    * Long text supports readonly for TinyMCE and ACE — `see <https://github.com/contao/contao/pull/5985>`_
    * Fix of templates for text output: all HTML tags are stripped
    * :ref:`File-Usage Integration <rst_extended_file-usage>`
* Translated Text Table
    * Support for readonly
* Translated Multi-Table (MCW)
    * Support for readonly and CSS classes for tl_class of the widget


Filter
------

* In the CE/FE module Filter, the filter rule labels now also show the type
  (`#1473 <https://github.com/MetaModels/core/issues/1473>`_)
* In the CE/FE module Filter, the ID for "FORM_SUBMIT" can be overridden — see :ref:`rst_cookbook_filter_filter-with-forwarding`
* Matching the FEE rights management, a new filter rule is available that filters the list by the
  items belonging to the logged-in member
* The template for filter output as a link list has been revised so that the Contao crawler no longer
  follows the links for search indexing
* Checkbox Status (formerly Publish Status) and Translated Checkbox Status (formerly Translated Publish Status)
    * The filter rule has been renamed from "Publish status" to "Checkbox status", since a checkbox
      does not necessarily control publishing
    * The option "Do not use filter in frontend preview" now responds to Contao's "Preview" status —
      previously it responded to backend login
* Custom SQL
   * The insert tag parameter "aggregate" now includes the type "list" — this was always described in
     the info box but was not previously implemented; it now allows comma-separated list values to be
     passed as GET values
   * Check of custom SQL queries using ``SUBSTRING_INDEX(SUBSTRING_INDEX('{{env::request}}', '/', -1), '?', 1)``
     and adjustment for the new routing — see :ref:`rst_cookbook_filter_custom-sql`
   * It is now possible to restrict execution to specific environments such as the frontend
* Simple Lookup
    * If the "Static parameter" option is set, a value can be selected in the CE/module MM list for
      the filter rule — new is the option "without data value [null]", when no selection — not even
      an empty string — should be set
* Single Select [select]
    * Attribute type Numeric (Integer) and Combined Values now supported
    * Template list output: attribute ``data-escargot-ignore`` added to prevent links from being indexed
* Multi Select [Tags]
    * Attribute type Numeric (Integer) now supported
    * Template list output: attribute ``data-escargot-ignore`` added to prevent links from being indexed
* Register
    * The template for filter output as a link list has been revised so that the Contao crawler no longer
      follows the links for search indexing (``data-escargot-ignore``)
    * Blocks for `formlabel` and `formfield` added to the template
    * A URL fragment can now be specified — after reload, the page jumps to the anchor point


Frontend Editing (FEE)
-----------------------

* A simple rights management system has been added which, when activated, allows each logged-in member
  to edit only their own entries (`#14 <https://github.com/MetaModels/contao-frontend-editing/issues/14>`_)
* Matching the rights management, a new filter rule is available that filters the list by the items
  belonging to the logged-in member
* There is a new event for manipulating the input mask headline:
  `GetEditMaskSubHeadlineEvent <https://github.com/contao-community-alliance/dc-general/blob/39ec68cee8b7034e5c1900692cd1b0eeaa7d4c7e/src/Contao/View/Contao2BackendView/Event/GetEditMaskSubHeadlineEvent.php>`_
* The input mask can be configured to display item values in the mask headline when editing
  (`#43 <https://github.com/MetaModels/contao-frontend-editing/issues/43>`_) — :ref:`see FEE <extended_frontend_editing_headlines>`
* The "Create" link is no longer included in the default template of the FE module — the template
  has been aligned with the CE template
* Change to insert tag resolution during :ref:`file upload <extended_frontend_editing_upload>` — adjust if necessary
* Thumbnails of image files in the Dropzone are now displayed after a page reload
* Form template selection for the input mask (FEE) is available for all non-translated attributes
* When overriding the buttons for the input mask, an insert tag can now be added to "Parameter"
  in addition to "Simple Tokens"
* The Dropzone template has been adjusted — check any custom modifications
* Support for predefined image size dimensions from `config.yaml` for thumbnails —
  see `contao.image.sizes:... <https://docs.contao.org/dev/framework/image-processing/image-sizes/#size-configuration>`_
* Option "Single file upload" is supported again
* Adjustment of the BE list output for "Content Article" and "Translated Content Article"
* Support for Notification Center version 2.x for notifications when creating or modifying records —
  when upgrading to NC 2.x, existing keys from 1.7 in the table "tl_nc_notification" may be migrated;
  see the message during DB migration execution
* Link generation for edit links in the FE has been revised so that the Contao crawler no longer
  follows the links for search indexing (``data-escargot-ignore``) — with the "Delete" link, this
  could lead to data loss


Extensions
----------

* :ref:`Note List (Notelist) <rst_extended_notelist>` has been released for MM 2.3



Known Issues
------------

* When toggling to/from debug mode in the BE via the button, the reference page is no longer correct
  and the page must be navigated to again — e.g. with "back" in the browser and reloading the page |br|
  Contao currently provides no way to influence the referer at that point


.. _check_upgrade_mm230:

Checklist for Upgrading to MM 2.3
-----------------------------------

In general, an upgrade within the MM 2.x branch is straightforward; any necessary label adjustments
and DB changes are handled via migrations. However, there are a few things that cannot or can only very
difficultly be caught by migrations. For this reason, the following points should be kept in mind when
upgrading to MM 2.3:

* Please follow all notes from :ref:`MM 2.2 <check_upgrade_mm220>`
* After upgrading, please clear the session data for backend users to avoid display of "pseudo-errors"
  (e.g. `Cannot assign null ... $intAmount of type int <https://now.metamodel.me/de/mm-eap-newsletter-2-3/details/eap-info-mm-2-3-dezember-ii-2023>`_)
* For upgrades from below 2.2, please follow the :ref:`checklist for MM 2.2 <check_upgrade_mm220>`
* Run a DB migration to create mm_* tables and attribute columns —
  :ref:`see Schema Manager <component_schema-manager>`
* Saved bookmarks to MM in the BE are no longer valid due to the new routing —
  `see newsletter <https://now.metamodel.me/de/mm-eap-newsletter/details/eap-info-mm-2-3-juli-ii-2024#nl-reader>`_
* Permissions for user groups are now assigned only in MM (see "Routing change" above) — remove
  obsolete checkboxes in user groups under "Backend modules"
* Check HTML5 templates — they have been revised (see Attributes, Filter, and FEE)
* Check HTML5 templates of filter widgets that output link lists — URL crawling has been prevented
* Check HTML5 templates with translations — e.g. ContentArticle
* Fix templates for text output of both long text attributes: all HTML tags are stripped
* Check filters with "auto_item" route priority — see :ref:`rst_cookbook_tips_set-route-priority`
* For FEE and FE module, adjust the template for the "Create" link if needed
* For FEE: check the upload mode :ref:`file upload <extended_frontend_editing_upload>`
* For FEE: check insert tag resolution during :ref:`file upload <extended_frontend_editing_upload>`
* For FEE: check whether the list and edit pages are excluded from Contao search indexing
* Check custom translations — `migrate to XLIFF format <https://metamodels.readthedocs.io/de/latest/manual/component/translations.html#eigene-anpassung-von-ubersetzungen>`_
* Check :ref:`custom default values for input masks <rst_cookbook_inputmask_default-values>`
* Check :ref:`input mask labels with custom adjustments — "LABEL NOT SET" <component_translations_lns>`
* Check custom SQL queries using ``SUBSTRING_INDEX(SUBSTRING_INDEX('{{env::request}}', '/', -1), '?', 1)``
  and adjust for the new routing — see :ref:`rst_cookbook_filter_custom-sql`
* When the "Variant" option of the model is activated, a migration checks attributes for unsupported
  variant/unique combinations —
  `see news January 2025 <https://now.metamodel.me/de/mm-eap-newsletter-2-3/details/eap-info-mm-2-3-januar-i-2025>`_
* In MM list and preset via "Simple Lookup" with "Static Parameter", note the new setting
  "- without data value [null] -"


Re-Financing
------------
.. seealso:: To re-finance the extensive work, the MM team asks for financial contributions. As a
   guideline, take the scope of the project to be realized and budget approximately 10% — based on
   the experience of past contributions, these are amounts between €100 and €500 (net) — an invoice
   including VAT is of course always issued. `More... <https://now.metamodel.me/de/unterstuetzer/spenden>`_


.. |br| raw:: html

   <br />
