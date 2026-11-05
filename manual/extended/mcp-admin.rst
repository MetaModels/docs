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

With Claude Code the server is registered as an HTTP server. You start the login in the program with ``/mcp``: select
the server, *Authenticate*, log in in the browser and agree.

.. code-block:: bash

   claude mcp add --transport http contao https://example.org/contao/mcp

.. note:: **Claude: better in the console than in the desktop app.** In the test the connection was stable in the console
   (``claude`` in the project folder), including the renewal of the token after about an hour. In the desktop app it broke
   off several times: overnight, after a restart of the database and once in the middle of the session, each time without an
   error in the server. There is no ``/mcp`` there, and the *Connectors* page requires a public https address - a system on
   ``127.0.0.1`` cannot be entered there. If you want to use the app, add the server with ``claude mcp add`` (or create a
   ``.mcp.json`` in the project folder), log it in once in the console with ``/mcp`` and then start a **new** session in
   the app.

   Before the first prompt, check in the console with ``/mcp`` that the server reports *connected* and *authenticated*.
   A running session does not reload the server: if it was not reachable when the session started (database restarted,
   login expired), even a *Reconnect* does not help, only a new session.

Contao only allows clients that identify themselves with a public document (*Client ID Metadata Document*) and
whose domain is approved. ``chatgpt.com`` (Codex), ``claude.ai`` and ``vscode.dev`` are preconfigured. Further clients
are approved with:

.. code-block:: yaml

   # config/config.yaml
   contao_oauth_server:
       cimd_trusted_domains: ['example.org']

The server fetches the client's document from the internet itself — the system needs an outgoing
connection for this. Login and workflow have been tested with Codex and with Claude Code (reading, writing, planning,
applying and reverting).

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
     - Finds types, extensions and manual pages for a task, from a few words. The language of the words is given in the ``language`` parameter (default ``en``).
   * - ``metamodels_list``
     - Tool
     - All MetaModels with table, languages and the number of attributes, input masks, render settings and
       filters.
   * - ``metamodels_describe``
     - Tool
     - The configuration of a MetaModel: attributes, input masks with fields and view conditions,
       render settings, filters.
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

A detailed example is in the cookbook: :ref:`Event management with an AI agent <rst_cookbook_mcp_event-management>`.


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

If the MetaModel should appear in its **own navigation group** in the backend, the agent sets the backend section on the
input mask. The group itself is configuration (``meta_models_core.be_sections`` in ``config/config.yaml``, see
:ref:`Own section in the backend navigation <rst_cookbook_tips_backend-section>`) and the agent cannot write it. It
names what has to be entered. Then clear the cache. Without the group the MetaModel does not appear in the
navigation.

For **checkboxes** the agent sets two things itself:

* The field of a new checkbox in the input mask gets the CSS classes ``w50 cbx m12`` so that it lines up with the fields
  next to it. A ``tl_class`` given in the settings of the field replaces them.
* If a filter uses the checkbox as "published" (rule ``checkbox_published`` or ``translatedcheckbox_published``), the
  agent switches on the option **Toggle icon** (``check_publish``) on the attribute. The list in the backend then shows
  the usual eye for toggling. With ``"check_publish": false`` in the settings of the attribute it stays off. This also
  applies to an existing attribute that still lacks the option.

A new **alias** attribute (``alias`` and ``translatedalias``) starts, as in the backend, with the options **Unique
values** (``isunique``) and **Force alias regenerating** (``force_alias``, ``force_talias`` for the translated alias). In a
MetaModel with variants the agent does not set them, since unique values are not possible there. With ``false`` in the
settings of the attribute an option stays off. Other attribute types do not get these defaults.

A field of the input mask can be tied to a :ref:`view condition <component_dca_visibility-conditions>`, e.g. the end
date of an event only if the checkbox "Multi-day" is set. The agent creates it on the field (``conditions``). The
following applies:

* The conditions of one level are linked with AND, a ``conditionand`` wrapper is not needed.
* The field of the attribute the condition refers to (here "Multi-day") needs the option "Submit on change". Without it
  the backend shows the effect only after saving. The planner points this out.
* The types come from the core: *Property value is*, *Property contains one of*, *Property is visible* and the
  containers *AND*, *OR* and *NOT*.

After creation, the Contao API knows the MetaModel immediately. To make it appear in the frontend, the agent creates a page,
an article and a content element of the type *MetaModel list* (``metamodel_content``).

The description is the **desired state**, also for existing items: the plan shows each row as *create*,
*change* (with the new and the previous values) or *existing*. Only what the description names is changed;
everything else remains untouched.

Not supported (reported as a problem): a different type or a different column for an existing attribute,
a different type or a different attribute for an existing filter rule or view condition, adding languages, switching variants
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

**What the journal does not know.** It contains only what ``metamodels_apply`` has written. Records, pages, articles,
content elements, image sizes and template files are created via the Contao API or by hand and are not in it.
If a test is to be reverted **completely**, proceed in this order and have the agent show each step before it executes it:

1. Remove the content elements, articles and pages that the agent created, as well as a created image size and a
   template file. Anything existing that the agent only used stays.
2. Delete the records of the MetaModels (for translated MetaModels per language, with ``lang``). Only then does
   ``metamodels_undo`` allow the MetaModels.
3. Start ``metamodels_undo`` with the **newest** entry of the journal and go backwards. First without ``confirm``,
   which shows what would happen.
4. Delete the empty tables (``DROP TABLE``) by hand if they bother you. ``metamodels_migrate_schema`` never deletes,
   and a later ``contao:migrate`` may show them as superfluous.

The agent should say beforehand what it wants to delete. It may understand an instruction "revert everything" as
including the sample data and pages it created itself.


Templates
---------

Templates are not written by the MCP administration but by Contao's MCP server with its template tools
(``contao_template_*``): find and read templates, check the code, determine the users of a template, create and save an
override, and create, compare and restore restore points. This way the agent can also override the templates of
MetaModels, for example a block of the page navigation (``mm_pagination``) or of the filter (``mm_filter_default``). The
:ref:`Twig blocks <component_templates_twig-blocks>` of these templates remain accessible in the override.

Important:

* The agent creates a **restore point** before every change. It applies to the whole ``templates/`` directory and is
  stored in the application cache - it is not a permanent backup.
* The check of the code only shows that the template can be compiled. Whether the output is correct is shown only by
  the page in the frontend.
* The analysis of the users does not find the templates of MetaModels: MetaModels loads them from PHP, not via Twig. The
  agent then reports only unresolvable references. Which lists use a template has to be checked in the frontend.
* ``contao_template_create_override`` has **no dry run**. If the agent sends ``dry_run: true`` anyway, the parameter is
  ignored without a message and the file is created. In the test this was an empty override for
  ``checkbox.html.twig``, which would have affected all checkboxes of the site. The agent noticed it and deleted it. Instruct
  it to say before every creation which file will be created, and afterwards to check ``git status`` or the folder
  ``templates/``.
* For the template of a **single attribute** (e.g. a badge only when a checkbox is set) the tools offer no variant. The
  agent then writes the file itself and enters the name in the render setting of the attribute (journal: one change, the
  file is not in it). In the test both worked: a Twig template under ``templates/metamodels/attribute/`` and a classic
  ``.html5`` directly in ``templates/`` (name ``mm_attr_<type>_…``). The file lies outside the journal and remains when
  reverting.


Knowledge base (metadata)
-------------------------

A package that is not installed cannot tell which types it would bring. The information about the packages and
their attribute and filter types therefore comes from a metadata file of the repository
`MetaModels/mcp-metadata <https://github.com/MetaModels/mcp-metadata>`_:

* per package: whether it is freely available or in the early adopter program, where to get access, a short description,
  keywords (per language) and the page in the manual,
* per type: the package, a short description, keywords per language and the manual page,
* per article: a page of the manual on a topic or a cookbook recipe with title, path and keywords
  — just a reference, the text stays in the manual.

The keywords are kept in a separate list per language, ``en`` is mandatory, each list has at most 12 entries::

    "keywords": {"en": ["date", "time"], "de": ["datum", "uhrzeit", "termin"]}

The lists are deliberately not mixed, because a word can mean something different depending on the language. In the search,
the keywords of the given language count fully, the English ones as a fallback with half weight; keywords of other languages
are not searched. The former flat array is no longer read.

The file is published at ``https://metamodels.github.io/mcp-metadata/metadata.json``. The extension fetches it
from there (https only), keeps it for one day and remembers the last valid version. If the server is not reachable,
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


Tips and Frequently Asked Questions
-----------------------------------

A complete example step by step is in the cookbook under
:ref:`Event management with an AI agent <rst_cookbook_mcp_event-management>`.

**What is the best way to proceed?**

* In small steps, checking the result after each step (backend, frontend).
* Have the agent **plan** first ("Plan …, don't write anything yet"), read and correct the plan, and only then apply it.
* Say what should **not** happen ("Pages and records come in the next step").
* Have the agent state the ``journal_id`` after each write operation.
* Have it ask when something is unclear: "Where something is not possible with the tools, tell me."

**The agent does not find the tools.**
Some agents load the tools only after a few seconds. In the prompt, name the server (e.g. "use the MCP server contao")
and give its address. If the login has expired, logging in again helps (with Codex ``codex mcp login contao``).

**The tools are gone in the middle of the conversation.**
The server is reachable, but the session has lost the tools (most often in the desktop app, see above). Check the status
in the console with ``/mcp``, log in again if necessary and start a **new session**. There, give the task again
**completely**, because the new session knows neither the wish nor the previous plan. Until then, only what is in the
journal (``metamodels_journal``) has been written.

**The plan is suddenly outdated.**
If the agent writes several plans one after the other or in parallel, an earlier plan can become invalid (the order of
the rows has changed). The agent recognizes this and plans again, you do not have to do anything. Therefore have it apply
the MetaModels one after the other: first categories and tags, then the events.

**The agent offers a dummy column (``fe_memberAttribut``).**
In the input mask the planner demands a field that is only needed for member permissions in the frontend. The agent fills
it with an arbitrary column; this has no effect as long as the member permissions are off.

**A test is to be repeated.**
The journal is not enough for that (see :ref:`Reverting <rst_extended_mcp-admin>`). On a test system, back up the
database, ``files/`` and the directory ``var/mm_mcp_admin/journal`` beforehand and restore them - not while a session of
the agent is running.

**The agent reports that a type is not installed.**
Then a package is missing, for example for multilingualism (``attribute_translated*``). The agent names the package and
the access. You install it yourself and have the types checked again with ``metamodels_check_types``. The agent should
not fall back to a variant that misses the task (such as monolingual instead of multilingual).

**Should you say "multilingual"?**
Yes, with the languages and the default language ("DE, EN, FR - DE is default"). This includes which fields should be
translated and which apply to all languages (e.g. contact fields).

**What can't the agent do?**

* Adapt templates only with Contao's template tools (see above), not with the MCP administration (e.g. a "Cancelled"
  badge in the output),
* change the order of attributes and fields, delete something and rename columns,
* add languages and switch variants,
* install packages.

It should say so and name a way in the backend. If it does not, ask for it.

**The data is in the backend, but the detail links are missing.**
MetaModels generates the alias when saving in the backend. Anyone who creates records via the Contao API must supply
the alias value. The attributes the alias is built from (e.g. title and date) should be mandatory fields, otherwise the
alias is incomplete and changes later (see :ref:`List with Detail Page <rst_cookbook_frontend_list-with-detail-page>`).

**The list also shows unpublished entries.**
The filter needs the *Checkbox status* rule (package ``attribute_checkbox``) on the attribute "published". The
"Yes / No" rule (``filter_checkbox``) is a widget for the visitors and only filters when someone uses it.

**What if the agent's answer changes after an update?**
The help (``metamodels://guide/overview``) and the planner's notes are loaded at the start of the conversation. Ask the
agent to reload them after the system or the extension has been updated.

**Is this suitable for live sites?**
No, writing is meant for development systems. The agent acts with the rights of the backend user.
