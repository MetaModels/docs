.. _component_workflow:

Workflow in MetaModels
======================

The workflow for mapping your own data structure in MetaModels is broken down into individual steps
that must be carried out in sequence for each MetaModel. The following description is aimed at
beginners in MetaModels and was created based on "best practice" — experienced users will combine
and supplement certain steps directly.

The individual steps are described in more detail in the further articles of the :ref:`component_index`
section.

.. note:: Note: SVG icons were introduced for MetaModels in MM 2.5. During a transition period,
   both variants are shown in the manual — new first, then old — an overview can be found here:
   :ref:`manual_new_icons-25`

Step 0: Concept of the Data Structure
--------------------------------------

The total number of MetaModels and their links create a database structure with which data can be
stored, output and filtered in the desired way. Good planning helps to avoid subsequent changes,
especially for more complex tasks.

It is recommended to record the structure of MetaModels and their links graphically. This helps
both during creation and for documentation.

In addition to classic relations such as single (1:n) or multi-link (m:n), MetaModels also offers
further options — more about this in the article :ref:`component_relations`.

In the simplest case, you can sketch the schema on paper with a pen — but there are also various
tools available, such as `yEd <https://www.yworks.com/products/yed>`_ or the online version
`yEd live <https://www.yworks.com/yed-live/>`_.

As an example, a structure for employees including links to department and projects, and a
self-reference for holiday substitution:

|img_db-schema_01|

For data storage and relations in MetaModels, corresponding attributes are required — which ones
are available can be found on :ref:`component_data-in-attributes`.

This also allows you to select which MM packages need to be installed in addition to the core.

Step 1: Basic Settings for a MetaModel
---------------------------------------

For the basic settings, the most important settings are pre-selected, so only the most necessary
information and selections need to be made. For easier orientation of where to find what, the
:download:`"MM layout map" </_download/MM_Lageplan_e-spin-Berlin.pdf>` is available for download.

* 1: |img_new| :ref:`Create new MetaModel <mm_first_new-mm>`
    * Set up :ref:`multilingual support <component_multi-language>` if necessary
    * After saving, the icons can be accessed from left to right as follows |svg_workflow_01| |img_workflow_01|
* 2: |svg_fields_22| |img_fields| :ref:`Create attributes <mm_first_attribute>`
    * After creating all attributes, run the :ref:`DB migration <component_schema-manager>` (Contao Manager or console) and clear the cache
* 3.a: |svg_rendersettings_22| |img_rendersettings| :ref:`Create render setting <component_rendersettings>`
    * Basic setting for the list view
* 3.b: |svg_rendersetting_22| |img_rendersetting| :ref:`Add attributes to render setting <component_rendersettings>`
    * Determines which attributes are available in the respective list for a view
* 4.a: |svg_dca_22| |img_dca| :ref:`Create input form <component_dca>`
    * Basic setting for the input form
* 4.b: |svg_dca_setting_22| |img_dca_setting| :ref:`Add attributes to input form <component_dca>`
    * Determines which attributes are available in the respective input form for a view
* 5: |svg_dca_combine_22| |img_dca_combine| :ref:`Create input/render assignments <component_dca-combine>`
    * Select and save the created render setting and input form

When step 5 is completed, the new MetaModel should appear on the left in the Contao navigation
in the "METAMODELS" section.

**You can and should now enter the first test records.**

Step 2: Basic Output
---------------------

- 1: Create a page and article in Contao
- 2.a: Create a :ref:`content element "MetaModel list" <component_contentelements>` in the article
- 2.b: Select the created MetaModel and the render setting in the MetaModel list and save

**In the frontend, a list with the entered test records from step 1 should now be visible on the
created page.**

Step 3: Adjusting Settings from Step 1
---------------------------------------

* 1: |img_new| :ref:`Adjust MetaModel <mm_first_new-mm>`
    * Activate :ref:`variants <component_relations_variants>` if required for the data structure
* 3.a: |svg_rendersettings_22| |img_rendersettings| :ref:`Adjust render setting <component_rendersettings>`
    * Create specific render setting, e.g. for list output in the frontend
    * Select the :ref:`template variant "metamodels_prerendered" <component_templates>` for custom output
    * Set the "jumpTo" page for :ref:`detail view <component_contentelements>`
* 3.b: |svg_rendersetting_22| |img_rendersetting| :ref:`Adjust attributes in render setting <component_rendersettings>`
    * Make specific settings for attributes — e.g. :ref:`output of images including image size <rst_cookbook_templates_fe_work_with_images>`
    * Select :ref:`template variant "mm_attr_<type>" <component_templates>` for custom output
* 4.a: |svg_dca_22| |img_dca| :ref:`Adjust input form <component_dca>`
    * Specify keys for output of filter, search, sorting, limit
    * Select the backend section where the MetaModel should appear, e.g. content or custom section
    * Display as table in the backend
    * Permissions for editing
* 4.b: |svg_dca_setting_22| |img_dca_setting| :ref:`Adjust attributes for input form <component_dca>`
    * CSS class such as w50
    * Required field, read-only (Readonly)
    * Option whether the attribute should be filterable and/or searchable in the backend list
    * Add legends to logically subdivide larger input forms
* 4.c: |svg_dca_groupsortsettings_22| |img_dca_groupsortsettings| :ref:`Create sorting/grouping <component_dca>`
    * Create default sorting or further sortings for selection in list
* 4.d: |svg_dca_condition_22| |img_dca_condition| :ref:`Create visibility conditions <component_dca_visibility-conditions>`
    * Input widgets can be shown or hidden based on values of other widgets
* 5: |svg_dca_combine_22| |img_dca_combine| :ref:`Create input/render assignments <component_dca-combine>`
    * Assign selection of render settings and input forms to user groups (BE) or member groups (FE)
* 6.a: |svg_filter_22| |img_filter| :ref:`Create filter <component_filter>`
    * Assign a name for the filter
* 6.b: |svg_filter_setting_22| |img_filter_setting| :ref:`Create filter rules <component_filter>`
    * Insert filter rules
    * Nesting with AND or OR is possible
    * Without further specification, all filter rules are automatically linked with AND

**With the adjustments made, the display in the backend and frontend should match the individual
requirements.**

In addition to the options listed, there are further possibilities that can be read on the linked
pages.

Step 4: Further Output Options from Step 2
-------------------------------------------

- 2.c: Content element "MetaModel list" from step 2
    - Select filter
    - Define sorting by an attribute — :ref:`see also special sorting <rst_cookbook_filter_custom-sql_sortierung-der-ausgabe-nach-mehr-als-einem-attribut-fest>`
      or :ref:`sorting links <rst_cookbook_templates_fe_list_sorting>`
    - Set limit and pagination
- 3.a: Create a :ref:`content element "MetaModel filter" <component_contentelements>` in the article
- 3.b: Select MetaModel and the filter (usually the same as from the MM list) with the desired filter rules

**On the page, a filter with corresponding filter widgets should be visible in the frontend and the
list should respond to the filtering.**

The frontend output can be adjusted with various settings for :ref:`rst_cookbook_tips_seo`.

.. _component_workflow_tips:
Tips:
-----

* For "MM starters", it is recommended to build the :ref:`"First MetaModel" <mm_first_index>` example
* Print out and keep the :download:`"MM layout map" </_download/MM_Lageplan_e-spin-Berlin.pdf>` handy
* Represent the data structure graphically — not all attributes need to be entered — it helps
  during construction and communication with customers and support requests
* When creating models, work "from the outside in" — in the example above, first create Department
  and Projects, then Employees — so that the models are already available when creating attributes
  for references (here the single select)
* For larger data structures, related models can be given a common "prefix" such as "events",
  so that the tables are named e.g. "mm_events_categories", "mm_events_contacts", etc. — the
  model table can then be filtered by "mm_events_" and is easier to work with
* Create similar attributes one after another — with "Save and new", the previous attribute type
  is retained, saving the selection
* For "helper information" such as salutations, units of measurement, etc., you don't need to
  create a MetaModel as a reference each time — for example,
  :ref:`a helper model construct <rst_cookbook_specials_helper-models>` can solve this
* After creating a model or attribute, run DB migration and clear the cache
* Before starting, check whether you need the models or attributes to be multilingual —
  a later switch is not easy
* Adding attributes to render settings and input forms is simplified with the "Add all" button
* There are a number of :ref:`checklists <rst_cookbook_checklists_index>` that help with the work
* Help is available in the `Forum <https://community.contao.org/de/forumdisplay.php?149-MetaModels>`_
  and on `Slack (#metamodels) <https://contao.slack.com/archives/CKGEBDV60>`_ — you can also
  get coaching on projects from the MM team (`mail@metamodels.me <mailto:mail@metamodels.me>`_)


.. |br| raw:: html

   <br />

.. |img_db-schema_01| image:: /_img/screenshots/metamodel_first/db-schema_01.png
   :width: 400px

.. |img_new| image:: /_img/icons/new.gif
.. |img_fields| image:: /_img/icons/fields.png
.. |svg_fields_22| image:: /_img/icons_svg/fields.svg
   :width: 22px
.. |img_workflow_01| image:: /_img/screenshots/workflow/workflow_01.png
.. |svg_workflow_01| image:: /_img/screenshots/workflow/svg_workflow_01.png
.. |img_rendersettings| image:: /_img/icons/rendersettings.png
.. |svg_rendersettings_22| image:: /_img/icons_svg/rendersettings.svg
   :width: 22px
.. |img_rendersetting| image:: /_img/icons/rendersetting.png
.. |svg_rendersetting_22| image:: /_img/icons_svg/rendersetting.svg
   :width: 22px
.. |img_dca| image:: /_img/icons/dca.png
.. |svg_dca_22| image:: /_img/icons_svg/dca.svg
   :width: 22px
.. |img_dca_setting| image:: /_img/icons/dca_setting.png
.. |svg_dca_setting_22| image:: /_img/icons_svg/dca_setting.svg
   :width: 22px
.. |img_dca_groupsortsettings| image:: /_img/icons/dca_groupsortsettings.png
.. |svg_dca_groupsortsettings_22| image:: /_img/icons_svg/dca_groupsortsettings.svg
   :width: 22px
.. |img_dca_condition| image:: /_img/icons/dca_condition.png
.. |svg_dca_condition_22| image:: /_img/icons_svg/dca_condition.svg
   :width: 22px
.. |img_dca_combine| image:: /_img/icons/dca_combine.png
.. |svg_dca_combine_22| image:: /_img/icons_svg/dca_combine.svg
   :width: 22px
.. |img_filter| image:: /_img/icons/filter.png
.. |svg_filter_22| image:: /_img/icons_svg/filter.svg
   :width: 22px
.. |img_filter_setting| image:: /_img/icons/filter_setting.png
.. |svg_filter_setting_22| image:: /_img/icons_svg/filter_setting.svg
   :width: 22px
