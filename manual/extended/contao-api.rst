.. _rst_extended_contao-api:

MetaModels Contao API
=====================

The Contao API provides the **records (items) of the MetaModels as a REST interface** — for reading, creating,
changing and deleting. It is built on the API bundle of Contao 6 (API Platform). This also makes the MetaModels
accessible to the :ref:`MCP server of Contao <rst_extended_mcp-admin>` and thus to AI agents.

The interface works like editing in the backend: the same permissions and the same validation apply, and the same
events are triggered (versions, log, search index).

.. note:: The extension is part of the **standard package** in the "early adopter program" (EAP) — more information
   under **Fundraising** on the `MM website <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-6>`_.

.. note:: The API bundle, the MCP bundle and the OAuth server of Contao are not yet stable: currently there is only
   the development branch ``dev-main``, and Contao marks the interface as *experimental*. Until Contao releases
   versions, the bundles can only be installed with an additional setting in the ``composer.json`` —
   `see below <#installation-via-composer>`_.

Requirements
------------

* MetaModels core from 2.6
* Contao 6.x and PHP 8.4
* ``contao/api-bundle`` from Contao


Installation via Composer
-------------------------

As long as ``contao/api-bundle`` is not published on Packagist, the repository must be added to the
``composer.json`` of the project:

.. code-block:: bash

   composer config repositories.contao-api-bundle vcs https://github.com/contao/api-bundle.git
   composer require contao/api-bundle:dev-main metamodels/contao-api

Then update the database as usual (Contao Manager, ``contao-setup`` or
``php vendor/bin/contao-console contao:migrate``).


Calling the interface
---------------------

Each MetaModel has its own resource under

``/contao/api/mm/<table name>``  or  ``/contao/api/mm/<table name>/<id>``

e.g. ``/contao/api/mm/mm_employees/12``. The operations:

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - Method
     - Purpose
   * - ``GET`` (list)
     - List of items. Use ``page`` and ``itemsPerPage`` for paging.
   * - ``GET`` (``…/<id>``)
     - Read one item.
   * - ``POST``
     - Create an item (content type ``application/ld+json``).
   * - ``PATCH``
     - Change an item — only the fields that change are sent (content type
       ``application/merge-patch+json``).
   * - ``DELETE``
     - Delete an item.

The description of all resources in OpenAPI format with all attributes of the MetaModels is available at
``/contao/api`` — the Swagger interface can also be reached there.


Authentication via token
------------------------

For access without a browser, a **personal access token** from Contao is used. It is generated in the backend module
*Security* (button for generating a token, with name and expiry date). The token starts with
``ct_pat_`` and is **shown only once**. It applies to the backend user who generated it.

The token is sent with every request as a bearer token:

.. code-block:: bash

   curl -H "Authorization: Bearer ct_pat_…" \
        "https://example.org/contao/api/mm/mm_employees?itemsPerPage=10"

   curl -X PATCH -H "Authorization: Bearer ct_pat_…" \
        -H "Content-Type: application/merge-patch+json" \
        -d '{"name": "New name"}' \
        "https://example.org/contao/api/mm/mm_employees/12"

.. warning:: A token grants the same permissions as the user has in the backend. It does not belong in a repository
   or in files that are passed on. A token that is no longer needed is deleted in the backend.


Permissions
-----------

The permissions follow the backend:

* A user needs an **input mask** for the MetaModel (assignment of the input mask to users or groups in the
  :ref:`input/render assignments <component_dca-combine>`). Without access, the interface responds
  with ``403``, and the MetaModel does not appear in the OpenAPI description and in the MCP for this user.
* Only the **attributes of the input mask** are visible and writable.
* The settings of the input mask also apply here: *read-only*, *mandatory* as well as the permissions for
  creating, editing and deleting records.
* The attribute types *Token*, *Geodistance* and *Levenshtein* are not output.


Reading
-------

The values are converted into a suitable JSON format per attribute type, e.g. numbers as numbers, tags and
multi-selects as a list and files as a list of UUIDs. The response is a Hydra collection with
``totalItems`` and the links to the pages.

Attributes of the type *Timestamp* (date, time) are output and expected as a **Unix timestamp**: an integer in seconds,
e.g. ``1798675200`` for 31.12.2026, 00:00 UTC.


Multilingual MetaModels
-----------------------

For MetaModels with several languages, the language is specified with the query parameter ``lang``:

* **Writing** (``POST``, ``PATCH``, ``DELETE``): ``lang`` is **mandatory**, otherwise the fallback language would be
  overwritten unnoticed. Without ``lang`` or with an unknown language, the interface responds with ``400``.
* **Reading**: ``lang`` is optional (``GET …?lang=en``); if omitted, the active language is used.


Writing
-------

Saving is always done via the MetaModel itself (as in the backend), not directly into the database. The content is
validated before saving:

* Unknown or non-writable fields, wrong types and missing mandatory fields are rejected with ``422`` and a
  message about the affected field.
* **Relations** are resolved before saving: an unknown alias in a select, unknown tags
  or columns of a table are **not silently discarded**, but rejected.
* **Files** are specified as UUID (no paths). The file must exist, be located in the folder defined in the
  attribute, and have an allowed extension.
* After saving, a **version** is created as in the backend (only if the record has changed) — with the
  name of the logged-in user. The versions are visible in the backend and can be restored there.
  Deleting does not create a version.
* The actions are written to the **system log** as in the backend, if logging is enabled in the MetaModel.
* The **search index** of Contao's backend search is updated (only if ``contao.backend_search`` is enabled
  and the worker is running), as is the index of the :ref:`Loupe filter <rst_extended_loupe>`. An error there does
  not abort saving; it is noted as a warning in the log.
* **Derived attributes** (alias, combined values, token, index of the Levenshtein filter) are created when an item is
  created, even if they are not in the input mask and the request does not name them. The alias is built from the source
  fields of the attribute, as in the backend. This requires MetaModels from 2.6 (or 2.5.4): before that, ``saveItem()``
  saved only the attributes that were set for a new item, so an item created via API or script had no alias. For an
  existing item the setting of the attribute applies, as in the backend.
* The **HTTP cache** is informed as in the backend: after saving and deleting, the cache tags of the table
  (``contao.db.<table>``) and of the item (``contao.db.<table>.<id>``) expire. The lists of MetaModels (module and
  content element) carry both tags, so that new items, items that only match the filter after a change, and empty lists
  expire in the cache as well. Without an HTTP cache with tag support nothing happens.

Other extensions can react to saving and deleting: for this, the interface triggers the Symfony events
``metamodels.contao-api.item-saved`` (arguments ``metamodel`` and ``created``) and
``metamodels.contao-api.item-deleted`` (argument ``metamodel``). The item is the subject of the event in each case.


Variants
--------

For MetaModels with variants, the items have the fields ``varbase`` (``true`` for a
base item) and ``vargroup`` (the ID of the base item).

* A **new variant** is created when the ID of the base item is specified as ``vargroup`` in the ``POST``. It starts
  as a copy of the base and may only contain the variant attributes.
* The **shared attributes** are changed on the base item and passed on to the variants.
* The ``vargroup`` of an existing item cannot be changed.
* When a base item is deleted, its variants are deleted as well.


Fields with the MultiColumnWizard
---------------------------------

Contao's API bundle does not know the MultiColumnWizard. The bundle ``menatwork/contao-multicolumnwizard-bundle``
therefore ships its own converter (from version 3.8, currently the ``develop`` branch), which it loads only when the API
bundle is active. This way fields with this widget appear in Contao's resources (``/contao/api/dc/…``), for example the
parameters of a MetaModels module or content element:

* A field is a **list of objects**, one property per column. Each column is converted like the widget it uses in the
  backend (text, select, file, etc.). ``minCount`` and ``maxCount`` of the widget count as the minimum and maximum
  number of rows.
* When writing, the widget checks every cell as in the backend (e.g. mandatory fields); errors are in the response with
  the path of the cell (``422``).
* Not supported are fields whose columns come from a function (``columnsCallback``) and fields with a column for which
  there is no converter. They are missing in the interface so that no values are lost when writing.
* If a column is read-only, the whole field is read-only: when adding, removing or re-sorting rows, the value of this
  column could not be preserved.


Public read access
------------------

Contao itself does not yet offer an interface for the frontend: all resources of the API bundle are under
``/contao/api`` and require a backend user. For pages that should read MetaModels as JSON without login (e.g. with Vue
or Stimulus), the extension therefore brings a **read-only access** that **does not run via API Platform** and lies
outside the backend:

* ``GET /api/mm/<table>`` returns a list, ``GET /api/mm/<table>/<id>`` a single item.
* **Nothing is public unless it is in the configuration.** For each MetaModel a **render setting** and a **filter
  setting** are specified:

  .. code-block:: yaml

     # config/config.yaml
     meta_models_contao_api:
         public:
             mm_events:
                 render_setting: 35   # id of the render setting
                 filter: 16           # id of the filter setting
                 sort_by: startdate   # optional, default: id
                 sort_direction: ASC  # optional
                 per_page: 30         # optional, upper limit max_per_page (default 100)
                 cache_ttl: 3600      # optional, seconds in the shared cache

* The **attributes** of the render setting are the ones that are output, as rendered texts, just as a list in the
  frontend shows them (e.g. a date as a date and a selection with its label).
* The **filter setting** is mandatory. It limits the items (e.g. with the rule *Published*, otherwise unpublished items
  would be visible as well) and defines the parameters accepted as query parameters. Other parameters are ignored. A
  single item must satisfy the same filter as the list, otherwise the interface responds with ``404``.
* The list knows ``page``, ``itemsPerPage`` and, for multilingual MetaModels, ``lang``. The response contains
  ``total``, ``page``, ``itemsPerPage`` and ``items``. Errors also come as JSON.
* The responses are released for the HTTP cache and marked with ``contao.db.<table>`` and
  ``contao.db.<table>.<id>``. Saving an item in the backend or via the interface makes them expire.
* There is no CORS header: the page that fetches the data is on the same host.

Example: display with Vue
^^^^^^^^^^^^^^^^^^^^^^^^^

A list with search, paging and detail view needs only a little HTML and JavaScript that fetches the JSON. The team's
example is built as part of the page structure, not as a finished HTML file:

* **A page** (e.g. "API Demo") in the page tree with a content element **HTML**. As the template of the element, a
  custom variant is selected, ``ce_html_api_demo_events`` (file ``templates/ce_html_api_demo_events.html.twig``). It
  contains the markup, the styles and the script. The default template of the element would remove ``<script>``, the
  variant outputs its content unchanged. The *HTML* field of the element therefore stays empty or takes a comment.
* **The library** (``vue.global.prod.min.js``) is located in a public subfolder of ``files/`` (e.g.
  ``files/api-demo/``). The folder must be **public**: in the file manager set the switch *Public*, which creates the
  file ``.public``. Then run ``contao:symlinks``. This way the example needs no CDN.
* **Vue expressions** are written with ``v-text`` and bindings (``:href``), not with ``{{ … }}``: Contao takes these
  braces for insert tags.
* **Links with ``#``:** Contao puts ``<base href>`` into the page. A link consisting only of ``#/4`` therefore leads to
  the start page. The example prepends the path of the current page (``window.location.pathname``).
* The page calls the interface on the same host, without login:

  .. code-block:: javascript

     const query = new URLSearchParams({ page: 1, itemsPerPage: 5, q: 'api' });
     const response = await fetch('/api/mm/mm_events?' + query);
     const { total, items } = await response.json();

     // Detail view of an item
     const item = await (await fetch('/api/mm/mm_events/4')).json();

* ``q`` and further parameters are the parameters of the filter setting (in the example a text search in the title and
  one in the place). An unpublished item returns ``404``, the example then shows a message instead of the detail page.

.. warning:: Everything that is in the render setting and that the filter lets through can be fetched by anyone. An
   attribute that should not be public does not belong in the render setting.


Access for logged-in members
----------------------------

Members of the website can read, create, change and delete their **own records** via the same interface, for example for
frontend editing with Vue or Stimulus. Contao's core cannot (yet) issue tokens for members, so the **session of the
logged-in member** applies: the page calls the interface on the same host, the browser sends the session cookie along.

* The addresses: ``/api/member/mm/<table>`` (list with ``GET``, create with ``POST``) and
  ``/api/member/mm/<table>/<id>`` (``GET``, ``PATCH``, ``DELETE``). ``GET /api/member/me`` returns the username,
  without login all addresses respond with ``401``.
* Here too **nothing is reachable unless it is in the configuration**:

  .. code-block:: yaml

     # config/config.yaml
     meta_models_contao_api:
         member:
             - mm_events

* The permissions are those of :ref:`frontend editing <rst_extended_frontend_editing>`: the **input mask** assigned to
  the member groups (:ref:`input/render assignments <component_dca-combine>`) determines which attributes members see
  and write and whether they may create, edit and delete records.
* With the **permission check** setting of the input mask (attribute for the member: a single select on ``tl_member``
  with the alias ``username``), members see and change only records in which they are named as owner. Other people's
  records respond with ``404``. A created record belongs to the logged-in member, the content of the request cannot
  name another owner. Without this setting all members of the group may edit all records, as in frontend editing.
* Writing works as with the interface in the backend (validation of the values, versions, search index, cache). Errors
  in the values come with ``422`` and a list of the problems. The alias of a new record is created automatically, even
  if the attribute is not in the members' input mask.

.. warning:: An assignment for the member group ``*`` applies to **every** member and wins by its position. Anyone who
   creates a separate row for the backend users therefore gives it a member group in which no member is (e.g.
   "Guests"), so that the members get their own input mask.

The example for members is a **subpage of a protected page** (in the test "API Demo FEE" under "My account TEST",
protected for the member group). The subpage inherits the protection, only logged-in members get there. Like the public
page it has a content element **HTML** with a template variant (``ce_html_api_demo_member``) that outputs the form for
creating and changing and the list of the member's own records. If the session expires, the page shows a note with the
link to the login.

Against the injection of requests by foreign sites (CSRF), every modifying request requires ``Content-Type:
application/json`` and the header ``X-Requested-With``. Both trigger a preflight check on a foreign site, which the
interface does not allow (there is no CORS header). A token is not needed for this. The responses are not intended for
the shared cache.


Known limitations
-----------------

* Unknown attribute types (e.g. from extensions) are output with the value of the widget and an untyped schema.
* The translations of the variant attributes are not copied from the base when a variant is created.
* The bundles of Contao are not yet stable (see above). The extension is therefore closely tied to the current
  state and may still change until the Contao bundles are released.
