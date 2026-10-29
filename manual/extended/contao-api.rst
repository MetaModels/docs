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


Known limitations
-----------------

* Unknown attribute types (e.g. from extensions) are output with the value of the widget and an untyped schema.
* The translations of the variant attributes are not copied from the base when a variant is created.
* The bundles of Contao are not yet stable (see above). The extension is therefore closely tied to the current
  state and may still change until the Contao bundles are released.
