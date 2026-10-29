.. _rst_extended_mcp-admin:

MetaModels MCP Administration
=============================

With the MCP administration, an **AI agent** (e.g. Claude or Codex) can **read and set up** MetaModels via the MCP
server of Contao: it learns which building blocks exist, checks which packages are needed for a task,
and, on request, creates a MetaModel with attributes, input masks, render settings and filters.

MCP (*Model Context Protocol*) is an open standard through which AI programs talk to applications. Contao 6
ships its own MCP server (``/contao/mcp``); the MCP administration extends it with knowledge about
MetaModels. The **records** of the MetaModels are accessible via the :ref:`Contao API <rst_extended_contao-api>`,
which is also offered through this server.

.. note:: The extension is part of the **standard package** in the "early adopter program" (EAP) — more information
   under **Fundraising** on the `MM website <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-6>`_.

.. note:: The extension is at an early stage (*early*). The underlying bundles of Contao (API, MCP,
   OAuth server) are not yet stable — there is only the development branch ``dev-main``.

.. warning:: The agent acts with the permissions of the backend user who connected it. **Writing the
   configuration is therefore disabled by default** and intended for development systems — not for
   live sites.

Requirements
------------

* MetaModels core from 2.6
* Contao 6.x and PHP 8.4
* ``contao/mcp-bundle`` and ``contao/oauth-server-bundle`` from Contao (together with the ``contao/api-bundle``)
* for access to records, additionally :ref:`metamodels/contao-api <rst_extended_contao-api>`
* An administrator account in the backend — all tools require the role ``ROLE_ADMIN``


Installation via Composer
-------------------------

As long as the bundles of Contao are not published on Packagist, the repositories must be added to the
``composer.json`` of the project:

.. code-block:: bash

   composer config repositories.contao-api-bundle vcs https://github.com/contao/api-bundle.git
   composer config repositories.contao-mcp-bundle vcs https://github.com/contao/mcp-bundle.git
   composer config repositories.contao-oauth-server-bundle vcs https://github.com/contao/oauth-server-bundle.git
   composer require contao/api-bundle:dev-main contao/mcp-bundle:dev-main contao/oauth-server-bundle:dev-main
   composer require metamodels/contao-api metamodels/mcp-admin

Then update the database (Contao Manager, ``contao-setup`` or
``php vendor/bin/contao-console contao:migrate``) — the OAuth server creates the table ``oauth_server_token`` in the process.

.. note:: The addresses under ``/.well-known/`` must be passed on to Contao by the web server. Some
   configurations block all files starting with a dot — in that case, the agent cannot log in
   (the server responds with ``403``).


Connecting an agent
-------------------

The MCP server has the address ``https://<domain>/contao/mcp``. Authentication runs via OAuth: the agent opens the
backend, and after login Contao asks *"Connect … with Contao?"* and points out that the agent may do everything
the user may do in the backend. With consent, the agent receives a token that it renews itself.

With Codex, this looks as follows:

.. code-block:: bash

   codex mcp add contao --url https://example.org/contao/mcp
   codex mcp login contao

Contao only allows clients that identify themselves with a public document (*Client ID Metadata Document*) and
whose domain is approved. ``chatgpt.com`` (Codex), ``claude.ai`` and ``vscode.dev`` are preconfigured. Further clients
are approved with:

.. code-block:: yaml

   # config/config.yaml
   contao_oauth_server:
       cimd_trusted_domains: ['example.org']

The server fetches the client's document from the internet itself — the system needs an outgoing
connection for this. Login and workflow have been tested with Codex; a test with Claude is still pending.

.. note:: For calling ``contao_api_execute`` (reading and writing data), Codex requires a confirmation for each call,
   because the tool is not marked as *read-only*.


What the agent can do
---------------------

The MCP administration adds the following elements to the Contao server:

.. list-table::
   :header-rows: 1
   :widths: 35 15 50

   * - Name
     - Type
     - Purpose
   * - ``metamodels://guide/overview``
     - Resource
     - Guide: the building blocks of a MetaModel, the order when creating, and how to find the required packages.
   * - ``metamodels://catalog/types``
     - Resource
     - All known attribute and filter types with package, short description, availability and whether they are
       installed here.
   * - ``metamodels://catalog/articles``
     - Resource
     - Manual pages on topics (e.g. relations, multilingualism) and cookbook recipes.
   * - ``metamodels://schema/{kind}/{type}``
     - Resource
     - The settings a type knows: input, rules, defaults, selection values and labels.
   * - ``metamodels_search_catalog``
     - Tool
     - Finds types, extensions and manual pages for a task, from a few words in any language.
   * - ``metamodels_list``
     - Tool
     - All MetaModels with table, languages and the number of attributes, input masks, render settings and
       filters.
   * - ``metamodels_describe``
     - Tool
     - The configuration of a MetaModel: attributes, input masks with fields, render settings, filters.
   * - ``metamodels_check_types``
     - Tool
     - Checks the types a task needs: missing ones are named together with the ``composer require`` command.
   * - ``metamodels_plan``
     - Tool
     - Checks a description of a MetaModel and shows the database rows and commands. Writes nothing.
   * - ``metamodels_apply``
     - Tool
     - Writes the planned rows in a transaction. Requires the ``write`` switch.
   * - ``metamodels_migrate_schema``
     - Tool
     - Creates tables and columns of the MetaModel. Requires the ``write`` switch.
   * - ``metamodels_journal``
     - Tool
     - Shows what ``metamodels_apply`` created and changed.
   * - ``metamodels_undo``
     - Tool
     - Reverts a write operation. Requires the ``write`` switch.

Installing packages always stays with the human: the agent learns what is missing and names the command — Composer
is not executed via MCP.

Examples of tasks for the agent:

* *"Which MetaModels exist and which attributes does the MetaModel Employees have?"*
* *"I need a MetaModel for events with title, date, location and a price. Which packages do I need?"*
* *"Create the MetaModel and show me the plan first."*
* *"Show me all employees of the Sales department."* (via the Contao API)


Checking packages and access
----------------------------

Not every package is freely available — many extensions are still in fundraising. The agent gets this
information from the metadata (see below): for a package in the early adopter program, it does not recommend a
``composer require``, but names the page where access can be obtained.


Writing configuration
---------------------

Writing is intended for development systems and is disabled by default:

.. code-block:: yaml

   # config/config.yaml
   meta_models_mcp_admin:
       write: true

The process consists of three steps:

1. ``metamodels_plan`` takes the description (MetaModel, attributes, input masks, render settings, filters and
   the assignment to the backend) and checks it like the backend: installed types, table and column names,
   settings of the type. The response contains the problems or the planned rows with a ``plan_id``.
2. ``metamodels_apply`` writes the rows in a transaction — only for the ``plan_id`` of the current plan.
3. ``metamodels_migrate_schema`` executes Contao's database commands for the tables of this MetaModel
   (``mm_<name>`` and ``mm_<name>_*``). Commands that delete, rename or empty something are never executed.

A MetaModel needs an input mask and render settings that are assigned to the backend users so that it can be
opened in the backend. Both are created automatically for the first MetaModel.

After creation, the Contao API knows the MetaModel immediately. To make it appear in the frontend, the agent creates a page,
an article and a content element of the type *MetaModel list* (``metamodel_content``).

The description is the **desired state**, also for existing items: the plan shows each row as *create*,
*change* (with the new and the previous values) or *existing*. Only what the description names is changed;
everything else remains untouched.

Not supported (reported as a problem): a different type or a different column for an existing attribute,
a different type or a different attribute for an existing filter rule, adding languages, switching variants
and changing the order. Nothing is deleted — except when reverting a write operation.


Reverting
---------

Most configuration tables have no versions in the backend. Therefore every ``metamodels_apply`` writes a
**journal** (a JSON file in ``var/mm_mcp_admin/journal``) with the created and changed rows — in the same
transaction: if the journal cannot be written, nothing of the write remains.

``metamodels_undo`` deletes the created rows and restores the values of changed rows. Without ``confirm``
it only shows what would happen. It refuses to revert and changes nothing if

* a row has been changed since writing,
* something has been added below it (an attribute, a field, a filter rule) or
* a MetaModel is to be deleted whose table contains records.

Tables and columns remain; the database update deletes nothing. A journal can be reverted
once. All write operations are recorded in Contao's system log.


Knowledge base (metadata)
-------------------------

A package that is not installed cannot tell which types it would bring. The information about the packages and
their attribute and filter types therefore comes from a metadata file of the repository
`MetaModels/mcp-metadata <https://github.com/MetaModels/mcp-metadata>`_:

* per package: whether it is freely available or in the early adopter program, where to get access, a short description,
  keywords and the page in the manual,
* per type: the package, a short description, keywords in the languages of the users and the manual page,
* per article: a page of the manual on a topic or a cookbook recipe with title, path and keywords
  — just a reference, the text stays in the manual.

The file is published at ``https://metamodels.github.io/mcp-metadata/metadata.json``. The extension fetches it
from there (https only), keeps it for one day and remembers the last good version. If the server is not reachable,
the file shipped with the extension is used.

A system without internet access, or one that must not send such requests, disables fetching:

.. code-block:: yaml

   meta_models_mcp_admin:
       metadata:
           url: null        # default: 'https://metamodels.github.io/mcp-metadata/metadata.json'
           ttl: 86400       # seconds, how long the file is kept

.. note:: The metadata is data, not instructions: texts with markup or with more than 240 characters and links that
   do not start with ``https`` are discarded when reading.


Configuration
-------------

.. code-block:: yaml

   # config/config.yaml
   meta_models_mcp_admin:
       write: false                     # allow writing the configuration (default: false)
       journal_dir: '%kernel.project_dir%/var/mm_mcp_admin/journal'
       metadata:
           url: 'https://metamodels.github.io/mcp-metadata/metadata.json'
           ttl: 86400

``journal_dir`` defines where the journals of the write operations are stored. ``write`` enables writing.
