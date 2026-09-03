.. _component_rendersettings:

|svg_rendersettings_32| |img_rendersettings_32| Render Settings
==================================================================

.. note:: Create list views for backend and frontend;
  add and activate attributes

Introduction
------------

"Render settings" define the basic parameters for listing and displaying records both for the
backend and for the frontend — separately for each. The individual records stored in a MetaModel
are also referred to as "items".

In the backend, items must be listed for further input or changes, and in the frontend for
display or output. Although various aspects differ between backend and frontend, many things are
similar, so the settings are summarized in the "Render settings" component.

Every MetaModel requires a render setting for the backend, since only through render settings
can an input form for data entry and changes be accessed.

For the frontend, render settings only need to be created for MetaModels whose items are to be
listed or displayed as such. MetaModels connected to another MetaModel via a relation (attributes
"Select" or "Tags/Multi-select") therefore do not necessarily require a render setting for the
frontend.

In addition to different requirements for backend and frontend, render settings can also cover
further requirements. Any number of different render settings can be created for each MetaModel,
for example to produce differentiated outputs. So one render setting could prepare a list with
basic information and another render setting a detail view (a detail view is "a list with one
item"). Furthermore, individual render settings can be granted access for user and/or member
groups via :ref:`component_dca-combine`.

Once a render setting has been created and the basic settings have been entered, the attributes
must be activated for the setting as a further step. More about this under "Procedure". As a
further option, an individual template can be selected for each attribute in a render setting
(if it has been created beforehand) and a custom CSS class, e.g. for highlighting in the backend.

Options
-------

* **Name** |br|
  The name can be freely chosen; for better distinction, the abbreviations "BE" and "FE" for
  backend and frontend are often placed before the name, e.g. "BE list", "BE entry" or
  "FE full list"
* **Template** |br|
  Here a template is selected in which all items are output in a loop; the template is very
  easy to override in the Contao-typical way; note that templates for the backend must not
  be created in a template subdirectory; the template receives all attributes in "raw" type
  and only the active attributes in "html" and "text" types
* **Output format** |br|
  Possible selections are HTML5 and Text; unless there are special requirements, the selection
  can be left empty; the XHTML format is no longer supported with MM 2.2
* **Redirect page** |br|
  The redirect page with page selection and filter is only for frontend output, e.g. to link
  to a detail page; a list element with an appropriate filter should be present on the detail
  page; for multilingual MetaModels there is a setting for page selection and filter per language
* **Hide empty entries** |br|
  Empty attribute entries are skipped — important when attribute labels are also output
* **Hide labels** |br|
  The attribute names are not output as "labels"
* **Render attributes only when needed [Lazy] (from MM 2.5)** |br|
  An attribute is only rendered once the template actually accesses it, instead of both output
  formats (HTML5 and Text) being generated immediately for every attribute as before; worthwhile
  when a template only uses part of the attributes or consistently uses only one output format —
  if, on the other hand, a template accesses all attributes in both formats, the option brings no
  benefit and can mean a small additional overhead; default is off, selectable per render setting
  to match the template used
* **Wrapper in list template [legacy behavior, deprecated] (from MM 2.5)** |br|
  Outputs the enclosing block (field, label, value) in the list template as up to MM 2.4, instead
  of — as usual from 2.5 — in the attribute templates themselves; automatically activated for
  existing render settings during the upgrade so the output does not change, newly created render
  settings start without the option; marked as legacy behavior from the outset and removed in
  MetaModels 3.0 — pattern and example for custom attribute templates under
  :ref:`component_templates_attribute-wrapper`
* **Additional CSS/JavaScript files** |br|
  CSS and/or JavaScript files can be output with the list for output formatting and interaction;
  they are only included if at least one item is output in the list


Procedure
---------

A new render setting is created via "|img_new| New". After all necessary options have been
entered or selected, the setting is saved and appears in the list of existing render settings
for a MetaModel.

In addition to the "|img_edit| pencil icon", there is the icon for the
"|img_rendersetting| Render settings of the attributes". Clicking on the icon opens a list
of attributes activated for the render settings. If no attributes are present or need to be
added, this can be done via the "|img_rendersettings_add| Add all" icon — alternatively
via "|img_new| New". The "Add all" route requires confirmation twice.

The attributes of the render setting are then available and may need to be activated, or only
those that should be displayed in the list view should be activated.

For individual attributes, the template to be used can be changed and/or a special CSS class
entered ("|img_edit| Edit").


Notes on Rendering Attributes Only When Needed [Lazy] (from MM 2.5)
---------------------------------------------------------------------

Without this option, when building a list MetaModels renders **both** output formats — both the
requested format (usually HTML5) and the text value — for **every** activated attribute,
regardless of whether the list template ultimately outputs both or even just one of them. With a
large number of attributes and records, this noticeably costs time spent on values that are never
used.

If the option is activated, the template instead gets its own placeholder for each format, which
only actually renders an attribute once it is specifically accessed in the template — and this
happens independently per format: if the template only accesses ``html5``, ``text`` is not
calculated at all for this attribute, and vice versa. Nothing changes for template authors in
terms of usage — access to ``html5``, ``text``, ``raw`` and ``attributes`` works as usual.

**When this pays off:** The option always helps when a template does not access all activated
attributes in both formats anyway — for example because only part of the attributes are actually
output, or because the template consistently uses only one format (e.g. only ``text`` for a
search index, or only ``html5`` for the visible output). If, on the other hand, a template
accesses all attributes in both formats anyway, Lazy brings no benefit and can even be minimally
slower due to the somewhat more expensive, general access. So there is no fundamentally "better"
behavior — that's why the option can be switched on or off per render setting to match the
template used, and the default is off equally for new and existing render settings.

**Measurements:** For reference, several scenarios were measured on a real test page with 13 and
208 items respectively and 25 configured attributes (pure CPU time rather than wall-clock time,
to factor out system load on the measurement machine; each reproduced multiple times):

================================================  ==================  ==================
Scenario                                          13 items            208 items
================================================  ==================  ==================
All attributes, both formats used                 no difference       no difference
All attributes, only HTML5 used                   14-20 % faster      10-11 % faster
All attributes, only Text used                    63-69 % faster      70-71 % faster
Only 3 of 25 attributes used                      37-45 % faster      36-43 % faster
================================================  ==================  ==================

The effect is greatest with pure text usage, because HTML5 rendering takes noticeably more effort
per attribute than plain text output — if it is skipped entirely thanks to Lazy, this has a
correspondingly large impact.


.. seealso:: :ref:`rst_cookbook_rendering_encrypt-email`


.. |svg_rendersettings_32| image:: /_img/icons_svg/rendersettings.svg
   :width: 32px
.. |img_rendersettings_32| image:: /_img/icons/rendersettings_32.png
.. |img_rendersettings| image:: /_img/icons/rendersettings.png
.. |img_rendersetting| image:: /_img/icons/rendersetting.png
.. |img_rendersettings_add| image:: /_img/icons/rendersettings_add.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_edit| image:: /_img/icons/edit.gif

.. |br| raw:: html

   <br />
