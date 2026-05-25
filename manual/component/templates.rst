.. _component_templates:

Templates in MetaModels
========================

For input and output of data as well as filtering, MetaModels provides various templates. All
templates can be customized and loaded as custom template variants.

In addition to the templates listed here, individual attributes or extensions may bring their own
templates.


.. _component_templates_fe-list:
Frontend List
-------------

A three-level hierarchy of templates is available for frontend output.

The **first level** consists of the MetaModels list templates as content element ``ce_metamodel_list``
or frontend module ``mod_metamodel_list``. This template serves as a "wrapper" for the output and
is selected in the respective content element or frontend module. The list output ("second level")
and pagination are included here as standard.

The **second level** is formed by the rendering template ``metamodel_prerendered`` or
``metamodel_unrendered`` — here all records are output in a loop. In this template, most adjustments
for custom output are typically made. These templates are selected in the render settings.

.. note:: The standard template ``metamodel_prerendered`` is sufficient for initial output and all
   attributes activated in the render settings are output. For custom output, you can create your own
   template based on ``metamodel_prerendered_debug`` and insert your HTML markup.

In the template, various data can be accessed — e.g.

* ``$this->total``: :ref:`total number of items <rst_cookbook_frontend_output-item-count>`
* ``$this->data``: array with all records (see below)
* ``$this->generateSortingLink``: :ref:`links for toggling the sorting <rst_cookbook_templates_fe_list_sorting>`
* ``$this->renderSortingLink``: :ref:`links for toggling the sorting <rst_cookbook_templates_fe_list_sorting>`
* ``$this->filterParams``: when the list is filtered, you have access to the filter data here
* ``$this->parameter``: :ref:`rst_cookbook_templates_fe_list_parameters`

For the output of records, a loop over the data from ``foreach ($this->data as $item)`` is built
into the template. In each ``$item``, you have access to the following nodes of the array of a record:

* ``$this->raw``: raw data of the record including system columns such as ``id``, ``tstamp``, etc.;
  numeric values and dates are output here as stored in the DB; for relation attributes, it is
  possible to access the linked record further (see ":ref:`component_relations_standard-relations`");
  for files, access to metadata, path information, UUID, etc.
* ``$this->text``: list with output in text representation (template from "third level")
* ``$this->html5``: list with output in HTML representation (template from "third level")
* ``$this->attributes``: list with output of the "Name" value from the configuration of the respective
  attribute (:ref:`for multilingual support in corresponding translation <component_multi-language_attribute>`)
* ``$this->actions``: node ``jumpTo`` with link from render settings — usually link to detail page;
  but further information may also be present, e.g. from the :ref:`wish list <rst_extended_notelist>`
* ``$this->jumpTo``: node is deprecated — use node ``$this->actions``
* further nodes, e.g. from the :ref:`wish list <rst_extended_notelist>`

The output of pre-rendered (prerendered) widget outputs makes the output very easy — but rendering
costs computing time accordingly. With very many simultaneous outputs, this can lead to server load
issues. As an alternative, unrendered outputs can be activated in the CE/module MM list.

If you want to build certain display conditions into the list template, e.g. display of blocks only
when values are set or have a certain value, the condition check should always use values from the
raw node (or text node if templates have not been modified). In the html5 node, tags are usually
always present, making them mostly unusable for checking.

Parameters for frontend output can be passed to the list template, e.g. for controlling a slider
or translations, etc. — see ":ref:`rst_cookbook_templates_fe_list_parameters`".

The **third level** consists of the attribute templates ``mm_attr_<attribute type>``. Selection is
made in the render settings for individual attributes. These templates are modified when this
customization also applies to different render settings — e.g. for the file attribute there is a
template as output with "ul" and one as "div". An individual CSS class can also be passed to the
template in the attribute settings of render settings.

In MM templates, Contao templates can also be included, for example to get output as a YouTube
content element for the Text attribute — see ":ref:`rst_cookbook_templates_fe_template_ce_elements`".

For list and attribute templates ("levels two and three"), there are **templates in the types/extensions**
``.text`` **and** ``.html5`` with always the same filename. The ``.text`` rendering is always present
and is used in the output in both the ``text`` and ``raw`` nodes. Whether ``.html5`` is also used
depends on the render settings. The output can be influenced by the choice of "Output format". If no
selection is made there, the standard output of the website is used — in backend and frontend typically
``HTML5``. But it is also possible to set the output to a specific format such as ``Text``.

If a custom template was created as ``html5``, e.g. ``mm_attr_text_special.html5``, a search is
also made for ``mm_attr_text_special.text`` — if not found, the standard template ``mm_attr_text.html5``
is used. The display with custom html5 templates can be optimized by creating a corresponding text
template — this shortens the search for a matching template.

To make text templates editable in the backend under templates, the following entries should be
created in your own ``tl_templates.php``:

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/tl_templates.php
   if (!empty($GLOBALS['TL_DCA']['tl_templates']['config']['validFileTypes'])) {
       $GLOBALS['TL_DCA']['tl_templates']['config']['validFileTypes'] .= ',text';
   }
   if (!empty($GLOBALS['TL_DCA']['tl_templates']['config']['editableFileTypes'])) {
       $GLOBALS['TL_DCA']['tl_templates']['config']['editableFileTypes'] .= ',text';
   }

In addition to ``.text`` and ``.html5``, there could be further formats in the future such as
``.json`` or ``.xml`` — the ``.xhtml`` format is no longer included.

**Additionally**, there is a template for pagination output ``mm_pagination`` and one for action
buttons ``mm_actionbutton``.


Frontend Filtering
------------------

For the output of frontend filters, there is a "wrapper template" as ``mm_filter_default``, which
is selected in the CE or frontend module "MetaModels frontend filter". In the template, the form is
built and all individual filters are output in a loop.

For filter rules, a corresponding template ``mm_filteritem_*`` can be activated — the default is
``mm_filteritem_default``. Instead of "default", there are also further predefined templates such as
"..checkbox", "..linklist", "..radiobuttons".

For filter rules, an individual ID or CSS classes can also be passed.

The output of the "MetaModel filter reset" can be customized with the template ``mm_clearall_default``.


Backend List
------------

In the backend, the output can be influenced via the render setting settings. In the list display
via ``metamodel_prerendered`` — but only if the output is not in table form — as well as for
attributes with ``mm_attr_<attribute type>``.


Backend Input Form
------------------

In the attribute settings of the input form, custom templates for backend widgets can be loaded.
Customization would also be possible via events of the `DC_G <https://github.com/contao-community-alliance/dc-general/>`_.


Frontend Editing (FEE)
----------------------

To show a link for creating a new record, there is a template ``ce_metamodel_list_edit`` or
``mod_metamodel_list_edit`` for the "list wrapper".

On the page for displaying the FEE input form, there is a "wrapper template"
``ce_metamodel_frontend_edit`` or ``mod_metamodel_frontend_edit``. The template outputs all input
widgets and contains a JavaScript snippet for :ref:`visibility conditions <mm_special_visibility-conditions>`
— the templates also exist without JavaScript as ``*_nojs``.

Customization of input widgets can be done for attributes in the render settings — note that
(FE) form widgets, not BE widgets, must be created and selected here.

.. |br| raw:: html

   <br />
