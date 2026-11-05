.. _rst_cookbook_mcp_event-management:

Creating Event Management with an AI Agent
==========================================

**As of: 03.11.2026**

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
* With Claude, it is best to start the agent in the **console** (``claude`` in the project folder) and to check with
  ``/mcp`` before the first prompt that the server reports *connected* and *authenticated*. In the test, the desktop app
  lost the connection several times, see :ref:`MCP administration <rst_extended_mcp-admin>`. A ``codex mcp login contao``
  (Codex) or ``/mcp`` (Claude) is enough if the login has expired.
* The agent needs the **whole task in the session**. If you start a new session after an interruption, give the wish from
  step 1 and the decisions from the plan again.
* If the pages are to be created in several languages, Contao needs one **start page** (root page) per language. Without
  it the agent does not create pages for this language and asks - the texts of the events are multilingual anyway. You
  create the start page beforehand or have the agent create it as a separate step.


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

The agent applies the MetaModels one after the other: first categories and tags, then the events, because these refer to
them. If it reports that a plan is outdated, it plans again - this is normal and not an error.

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

If there are already pages for list and detail (e.g. from an earlier attempt), the agent keeps using them instead of
creating duplicates, and says so. You should know beforehand which pages exist, otherwise it remains unclear what comes
from it (important when reverting, see step 7). The filters are placed before the list in the same article, the detail
page is hidden and requires an item (``requireItem``). The filters work via the **address** (``/category/workshop``,
``/tag/music``, ``/date/2026-11-01,2026-11-30``), not via ``?category=workshop``.

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
   In the test the alias remained empty after creating. Only a second write operation (``PATCH``) built it from title and
   start date (e.g. ``climate-conference-2026-2026-11-20``); values supplied by the agent were overwritten in the process.
   After creating, have it check that every event has an alias in every language.


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
   * - The "Cancelled" badge is missing, or it says "Cancelled: 1".
     - "Build the note "Cancelled" in the list: instead of "Cancelled: 1", cancelled events should show a clearly
       visible note, the others nothing. Use a template for this." The MCP administration does not write templates, the
       agent creates the file with Contao's template tools or writes it itself and enters it in the render setting of the
       list. See below and :ref:`Templates <rst_cookbook_templates_index>`.
   * - The detail page still says "Cancelled: 1".
     - The agent changes only what was asked. With "Do this on the detail page too" it switches the render setting of the
       detail page.
   * - There are no pages for one language.
     - The start page of this language is missing, see prerequisites.
   * - The filter above the list is missing or does nothing.
     - "Check whether the filter element is above the list, and call the list with /category/workshop." The filters are a
       content element of their own and work via the address, not via ``?category=``.

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

In the test, reverting went like this, and this is how you should guide it:

1. The agent shows the journal (here five entries) and checks each entry without ``confirm``.
2. It reports that the configuration only reverts when the tables are empty, and first deletes its content elements,
   articles, pages, the image size and the template file, then the records (for translated MetaModels with ``lang``).
3. Then it reverts the journal entries, the newest first.
4. It checks the result (number of MetaModels, pages, content elements) and says what remains.

What remained are the **empty tables** ``mm_event_categories``, ``mm_event_tags`` and ``mm_event_events`` (``DROP TABLE``
by hand if you do not want them) and the pages that existed before. The agent did not delete them itself and said so -
that is how it should be. The instruction "revert everything" is, however, broad: the agent deleted the sample data and
its pages along with it without asking. If you do not want that, add it ("The records stay").


What came out of the test
-------------------------

The procedure was played through with Claude Code in the console (packages ``attribute_translated*`` installed, writing
enabled):

* **Planning:** Three plans, tables and database commands, nothing written. The agent itself reported what remains open
  (date filter, note "Cancelled", image size).
* **Applying:** Three MetaModels (de, en, fr), categories and tags, seven sample events in three languages, list and
  detail page, the condition for the end date in the backend. All statements of the agent matched the database.
* **Frontend:** The list showed the published events in descending order by date, without the draft. The filters by
  category, tag and date range worked via the address.
* **Note "Cancelled":** A template showed a red badge only for the cancelled event, otherwise nothing.
* **Reverting:** All journal entries could be reverted, the numbers matched the state before.

The hurdles and how to get around them:

.. list-table::
   :header-rows: 1
   :widths: 35 65

   * - Hurdle
     - What helps
   * - Packages for multilingualism are missing.
     - The agent names them and installs nothing. You install them, update the database and have the types checked again
       with ``metamodels_check_types`` before continuing.
   * - The tools are missing in the session or disappear.
     - Check ``/mcp`` in the console, log in again, start a **new** session, give the task completely. Do not use the
       desktop app for the test.
   * - The token expires (60 minutes).
     - In the console the client renews it itself. On errors like ``401`` log in again.
   * - The planner demands ``fe_memberAttribut``.
     - The agent enters an arbitrary column. No effect as long as the member permissions are off.
   * - The date filter does not find multi-day events that begin before the period.
     - The filter ``fromtodate`` checks only the start date. For overlaps you need a filter over start and end
       (``rangedate``) - decide deliberately and record it in the plan.
   * - The alias field is empty after creating.
     - Have the alias checked after creating. MetaModels builds it when writing from title and start date and
       overwrites supplied values.
   * - No pages in the third language.
     - The start page of this language is missing.
   * - "Cancelled: 1" instead of a note.
     - A template for the attribute, see step 6. The tools have no dry run for ``contao_template_create_override``: the
       agent should say beforehand which file will be created, and afterwards check that no empty override was created
       for all checkboxes.
   * - Reverting refuses.
     - First delete records, then ``metamodels_undo``, newest entry first. The empty tables remain.
   * - The journal records only a part.
     - Pages, content elements, image sizes, records and template files are not in it. Have the agent name what it
       created, and check ``git status`` and the page structure.

If you want to repeat the test, back up the database, ``files/`` and the journal beforehand and restore them when no
session of the agent is running.


How it can continue
-------------------

The recipe ends with the **setup**: MetaModels, input mask, render settings, filters, pages and sample data.
**Adapting the templates** is not part of it and was not covered in the test. It is the next step, because the output in
the frontend then corresponds to the default of the render setting. For example:

* a note "Cancelled" for events with the checkbox ``cancelled`` set, in the list and on the detail page,
* the presentation of image, date and place in the list and the structure of the detail page,
* classes and design for your own layout.

The tools do not write templates. You create them by hand, see :ref:`Templates <rst_cookbook_templates_index>`, or
discuss them with the agent in a separate step, stating what the template should show.
