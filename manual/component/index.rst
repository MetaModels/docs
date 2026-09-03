.. _component_index:

Components of a MetaModel
==========================

The following chapters describe the structure of MetaModels to help understand the "logic" of
how the extension is built.

First, a clarification of two terms: **MetaModel** (singular) will refer to a data table with its
attributes, input/output options, filters, etc. A MetaModel is written without "s" in the following
texts, even if this would be required grammatically.

The term **MetaModels** (plural) stands alone as the name for the extension package for Contao.

For newcomers or those returning to MetaModels, it may be somewhat difficult to find a suitable
workflow for creation. For this audience, there is a :ref:`simple workflow for working with MetaModels
<component_workflow>`. There are also some :ref:`tips for getting started <component_workflow_tips>` and
an :ref:`overview of which attributes can store what data <component_data-in-attributes>`.

Before creating more complex data structures in MetaModels, you should definitely think about an
"elegant" structure — especially the relations between models. There is an overview page
":ref:`component_relations`" for this.

.. note:: Note: SVG icons were introduced for MetaModels in MM 2.5. During a transition period,
   both variants are shown in the manual — new first, then old — an overview can be found here:
   :ref:`manual_new_icons-25`

After creating a MetaModel, the following main components are available for editing:

 |svg_fields_22| |img_fields|  :ref:`component_attribute` |br|
 |svg_rendersettings_22| |img_rendersettings|  :ref:`component_rendersettings` |br|
 |svg_dca_22| |img_dca|  :ref:`component_dca` |br|
 |svg_searchable_pages_22| |img_searchable_pages|  :ref:`component_searchable-pages` |br|
 |svg_filter_22| |img_filter|  :ref:`component_filter` |br|
 |svg_dca_combine_22| |img_dca_combine|  :ref:`component_dca-combine`

When creating a (simple) MetaModel, the components can be worked through in the order listed. As the
complexity of the MetaModel increases — i.e. when multiple MetaModels interact with each other — you
will inevitably need to supplement or modify individual entries in an existing MetaModel.

In addition to the main components, there are further configuration options, such as creating grouping/
sorting of items in a backend list or :ref:`display conditions for input widgets in an input form
<component_dca_visibility-conditions>`.

.. _rst_component_index_mm_lageplan:
For an easier overview of where to find what, there is the
:download:`"MM layout map" </_download/MM_Lageplan_e-spin-Berlin.pdf>` available for download.

The MetaModels extension adds two new content elements and modules to Contao for frontend output.
The content element/module "MetaModel list" allows records to be displayed individually or as a list
on the website, and the content element/module "MetaModel frontend filter" provides a filter for the
frontend — more about this under :ref:`component_contentelements`.

How the individual templates interact is described on the :ref:`component_templates` page.

MetaModels is very well suited for working with multilingual content —
:ref:`more about multilingual support in MM. <component_multi-language>`

To output individual values of a record (item) or the number of all records in the Contao context,
various :ref:`insert tags <component_inserttags>` are available.


.. toctree::
    :hidden:
    :maxdepth: 1

    workflow
    new-mm
    attribute
    rendersettings
    dca
    dca-visibility-conditions
    searchable-pages
    filter
    dca-combine
    contentelements
    relations
    schema-manager
    translations
    templates
    data-in-attributes
    multi-language
    inserttags

.. |br| raw:: html

   <br />

.. |nbsp| unicode:: 0xA0
   :trim:

.. |img_fields| image:: /_img/icons/fields.png
.. |svg_fields_22| image:: /_img/icons_svg/fields.svg
   :width: 22px
.. |img_rendersettings| image:: /_img/icons/rendersettings.png
.. |svg_rendersettings_22| image:: /_img/icons_svg/rendersettings.svg
   :width: 22px
.. |img_dca| image:: /_img/icons/dca.png
.. |svg_dca_22| image:: /_img/icons_svg/dca.svg
   :width: 22px
.. |img_searchable_pages| image:: /_img/icons/searchable_pages.png
.. |svg_searchable_pages_22| image:: /_img/icons_svg/searchable_pages.svg
   :width: 22px
.. |img_filter| image:: /_img/icons/filter.png
.. |svg_filter_22| image:: /_img/icons_svg/filter.svg
   :width: 22px
.. |img_dca_combine| image:: /_img/icons/dca_combine.png
.. |svg_dca_combine_22| image:: /_img/icons_svg/dca_combine.svg
   :width: 22px
