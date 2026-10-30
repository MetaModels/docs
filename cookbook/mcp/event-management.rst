.. _rst_cookbook_mcp_event-management:

Event Management with an AI Agent
=================================

This recipe shows how an AI agent sets up a complete event management via :ref:`MCP <rst_extended_mcp-admin>`:
multilingual MetaModels, input mask, render settings, filters, pages and sample data. It is a **conversation in
steps**, not a script: the agent works best when it is guided in small steps and the result is checked after each step.
How a list and a detail page are built by hand is described in the recipe
:ref:`List with Detail Page <rst_cookbook_frontend_list-with-detail-page>`.

.. note:: The answers depend on the agent and the language model and will differ from those described here. The prompts
   are templates to adapt and no guarantee for a particular result.

.. contents::
   :local:
   :depth: 1


Prerequisites
-------------

* The agent is connected to the MCP server and writing is enabled, see
  :ref:`MCP administration <rst_extended_mcp-admin>`. Writing is off by default because the agent acts with the rights
  of the backend user - it is meant for development and test systems, not for a live site.
* In the first prompt, give the address of the site and of the MCP server - e.g. ``https://example.org/`` and
  ``https://example.org/contao/mcp``.


1. Describe the wish and have it checked
----------------------------------------

Describe what is needed and have the agent **check** first, not build:

.. code-block:: text

   I would like to create an event management with the following functions:

   * Multilingual with DE, EN, FR - DE is default
   * Assignment to a category: Conference, Workshop, Seminar
   * Event status: published and cancelled
   * Assignment to one or more tags: Sport, Music, Theater, personal development, Ecology
   * Fields of an event: title, description, image, date, multi-day, time, place, price,
     max. number of participants, contact information
   * If multi-day, there should also be an end date
   * A list of the published events with title, image (thumbnail), date, time, place,
     sorted by date in descending order
   * Filtering by category, by tag, by date range from to
   * Link to the event details - with its own page showing all details of the event

   Contao runs at https://example.org/, the MCP server at https://example.org/contao/mcp
   Check my specifications first and give feedback on how we could proceed.

**What to check:** Does the agent report missing packages? Multilingualism needs e.g. ``attribute_translatedtext`` and
``attribute_translatedalias``. The agent names the packages and the access (many are in the early adopter program) and
does not install them itself. You install them, update the database and have the agent check the types again with
``metamodels_check_types`` before continuing with step 2. A monolingual fallback is not an option because it misses the
task.

Does the agent ask when something is ambiguous (e.g. "event status")? Does it say what it cannot do? For the condition
"end date only if multi-day" it creates a :ref:`view condition <component_dca_visibility-conditions>`. It should name it
in the plan.


2. Have it plan, write nothing
------------------------------

.. code-block:: text

   Plan the MetaModels (write nothing) and show me the plan and the database commands.

The plan names the tables, attributes, input mask, render settings and filters and contains notes from the planner.
This is the right moment for corrections:

.. code-block:: text

   Change the plan: The main table is called mm_event_events so that all MetaModels have the prefix mm_event_.
   Instead of an auxiliary model for the status use two checkboxes published and cancelled. published controls
   what the list shows (filter), cancelled is only a note in the output. The price is a decimal number
   (type decimal), max_participants of type numeric. The place is translated (translatedtext), the contact fields
   stay the same for all languages. Also plan the input mask, the render setting and the filter
   (category, tag, date from-to) and show me the change.

**What to check:** Are table names, types and translations correct? The plan shows each row as *create*, *change* or
*existing* - only what it names is changed.


3. Apply and create the tables
------------------------------

.. code-block:: text

   Apply the plans for categories, tags and events and create the tables. Show me the result
   with metamodels_describe for all three and tell me the journal_id of each write operation.
   Pages and records come in the next step.

**What to check:** Are the three MetaModels visible in the backend and can they be opened? If not, the assignment to the
backend is missing - then ask the agent to check and complete the ``assignments`` with ``metamodels_describe``.

Does the end date of a new event in the backend appear only when you click "Multi-day"? This requires that the checkbox
has "Submit on change", the planner points this out. If the condition is missing, ask the agent to add it, e.g. "The end
date should only appear if Multi-day is set. Plan first and show me the plan." If title and start date, from which the
alias is built, are not mandatory fields, the planner points this out as well.


4. Pages, articles and content elements
---------------------------------------

.. code-block:: text

   Now create the pages, tell me briefly beforehand what you create. One page with the event list (filter and
   list, descending by start date, only published events) and a detail page for an event.
   The detail page is reachable via the link in the list. Use the contao_api_* tools for this. The records
   come in the next step. Where something is not possible with the tools, tell me.

.. tip:: If the agent looks for a tool in the Contao API, a **single word in the singular** helps as a search term
   (e.g. "page" instead of "create pages").

For the pages and content elements the agent needs the Contao API. Codex asks for confirmation on every call of
``contao_api_execute``.


5. Create sample data
---------------------

.. code-block:: text

   Create the records: the 3 categories (Conference, Workshop, Seminar) and the 5 tags each in DE, EN
   and FR, then 10 dummy events, also in DE, EN and FR, with one category and one to three
   tags, some multi-day with an end date, one or two cancelled, two not published.
   Use contao_api_mm_<table>_post and lang for this. Each event gets an image: simple color areas.
   If you are missing something, tell me.

With a multilingual MetaModel the agent writes each language individually - for 10 events in 3 languages that is 30
write operations. This takes a while.

.. warning:: MetaModels generates the **alias** only when saving in the backend. Via the Contao API the agent must supply
   the value, otherwise the detail links are missing, see
   :ref:`List with Detail Page <rst_cookbook_frontend_list-with-detail-page>`.


6. Check the output and improve
-------------------------------

Open the list page in the browser and check it. Typical improvements and the matching prompt:

.. list-table::
   :header-rows: 1
   :widths: 40 60

   * - Observation
     - Prompt (core)
   * - The list also shows unpublished events.
     - "Find out why and fix it." (The filter rule *Checkbox status* on ``published`` is missing.)
   * - The image appears as a file name.
     - "The image should appear as a thumbnail in the list."
   * - The image size is wrong.
     - "Create the image size 120 x 68 pixels (proportional) in Contao and select it in the render setting of the
       list." (Image sizes are a Contao setting, MetaModels only selects them.)
   * - The links end in ``/published/1``.
     - "Find the cause: the links should be /en/event/<alias>, an unpublished event should not appear even via a
       bookmarked link, the aliases should be built in the backend from title and start date."
   * - The "Cancelled" badge is missing.
     - That is a matter of the template, the tools do not write templates - see
       :ref:`Templates <rst_cookbook_templates_index>`.

For corrections, ask the agent to **reload the help and the planner's notes** (``metamodels://guide/overview``) if the
system has changed since the conversation started.


7. Journal and reverting
------------------------

.. code-block:: text

   Show me the journal. What would happen if I revert everything (only show, don't confirm anything yet)?
   Tell me what can be reverted, what cannot and why.

As long as records are in the tables, ``metamodels_undo`` refuses to revert the MetaModels. Pages, articles and content
elements are not in the journal, they are created via the Contao API. More under
:ref:`Reverting <rst_extended_mcp-admin>`.
