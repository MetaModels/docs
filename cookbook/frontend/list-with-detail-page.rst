.. _rst_cookbook_frontend_list-with-detail-page:

List with Detail Page
=====================

A typical output consists of a list page and a detail page: the list contains a link for each entry, the detail page
shows the one entry with all its details. The following steps apply equally to a monolingual and a multilingual
MetaModel - with a multilingual MetaModel there are only the variants of the attributes and one page per language.

An event management serves as the example, with the attributes title, start date and "published".

.. contents::
   :local:
   :depth: 1


1. Alias as a unique key
------------------------

The entry on the detail page is found via an alias that is part of the URL, e.g.
``/event/forum-for-sport-2026-11-07``. For this, an :ref:`Alias <component_attribute_alias>` attribute is created
- for a multilingual MetaModel :ref:`Translated Alias <component_attribute_translatedalias>`.

Four settings are important:

* **Fields for the alias**: title and start date. The date keeps the alias distinguishable even if two events have the
  same title.
* **Required fields**: The attributes the alias is built from are set to "Mandatory field" in the input mask. If one
  of them stays empty, the alias is incomplete and changes as soon as it is filled later - and with it the link to the
  detail page.
* **Unique values**: The option in the general attribute settings ensures that the alias occurs only once.
* **Always update**: The alias is regenerated on every save. Without this option, an alias that has been assigned once
  stays even if the title changes.

.. note:: The alias is generated when saving in the backend. Anyone who creates entries in another way (e.g. via the
   :ref:`Contao API <rst_extended_contao-api>`) must supply the value of the alias.


2. Two filters
--------------

The list and the detail page each need their own filter. The rules of a level are always linked with AND. So there is
**no** need to create an "AND" rule and nest the other rules below it - that is only necessary for a group within an
"OR" rule. The same applies to the view conditions of the input mask.

**Filter "List"**

* :ref:`Checkbox status <component_attribute_checkbox>` rule on the attribute "published": only entries with the
  checkbox set appear.
* Next to it the rules the visitors use, e.g. category, tags and a date range (each with a URL parameter).

**Filter "Detail"**

* :ref:`Checkbox status <component_attribute_checkbox>` rule on the attribute "published": this way an entry that is
  set to "not published" can no longer be accessed, not even via a bookmarked link.
* :ref:`Simple lookup <component_filter_simplelookup>` rule on the attribute "Alias" with the URL parameter
  ``auto_item``. As a result, the URL contains only the value of the alias, without a key.

.. warning:: For "only published entries" use the "Checkbox status" rule of the package ``attribute_checkbox`` and not
   the "Yes / No" rule (package ``filter_checkbox``). The "Yes / No" rule is a widget for the visitors: it only filters
   once someone makes a choice, and it puts ``published/1`` into every URL the filter generates - so the links to the
   detail page also end in ``/published/1``.


3. Render settings
------------------

At least three :ref:`render settings <component_rendersettings>` have proven useful:

* **Backend list**: the defaults of the list and the attributes are sufficient. If the list should appear as a table,
  set the option "Display in table form" in the :ref:`input mask <component_dca>`.
* **Frontend list**: the attributes that should appear in the list (e.g. title, preview image, date, time, place).
* **Detail**: all details of the entry.

For the output in the frontend you usually need customized templates, see :ref:`Templates <rst_cookbook_templates_index>`:
the template of the render setting (e.g. ``metamodel_prerendered``), the template of the list element
(``ce_metamodel_list``) and, if necessary, the attribute templates ``mm_attr_*``. The size of a preview image is also not
defined in MetaModels but in Contao under *Themes > Image sizes*. In the render setting you only select it.


4. Jump-to page in the list
---------------------------

In the render setting of the list you define the **jump-to page**: the detail page and the **filter of the detail
page** (for a multilingual MetaModel one row per language). MetaModels generates the links of the list from the filter:
the value of the alias is inserted into the URL of the detail page.


5. Pages and content elements
-----------------------------

* **List page**: An article with the content element "MetaModel Filter" (if visitors should be able to filter) and
  below it the content element "MetaModel List". In the list you select the MetaModel, the render setting of the list,
  the filter of the list and the sorting (e.g. by start date descending).
* **Detail page**: A page, usually not shown in the navigation, with the content element "MetaModel List" with the
  render setting and the filter of the detail page.

The list and the detail page use the same technique, only the filter and the render setting differ.


6. Checking
-----------

* In the list, the links point to ``/<detail page>/<alias>`` and not to ``/<detail page>/<alias>/published/1``.
* The detail page of an entry shows all details.
* An unknown alias and an unpublished entry return an empty page ("There are no entries …").
* In a multilingual MetaModel this applies to every language and every page separately.
