.. _component_inserttags:

Insert Tags
===========

.. note:: Output of values of a model, item, attribute via insert tag |br|
  The insert tags were completely reworked in MM 2.3 and are partially only available from
  that version (functional).



Introduction
------------

To enable various outputs from MetaModels in standard Contao content, various insert tags are
available, such as the total number of (published) records or individual values of one or more items.

Custom insert tags can easily be created via the Contao hook
`replaceInsertTags <https://docs.contao.org/dev/reference/hooks/replaceInsertTags/>`_ in combination
with the :ref:`MM API <ref_api>`.


.. _component_inserttags_count:
Total Number of Items
---------------------

If no items are found, e.g. due to filtering, the digit ``0`` is output.

* FE module MM list: ``{{mm::total::mod::[ID]}}`` — e.g. ``{{mm::total::mod::22}}``
* CE MM list: ``{{mm::total::ce::[ID]}}`` — e.g. ``{{mm::total::ce::33}}``
* MetaModel: ``{{mm::total::mm::[MM Table-Name|ID](::[ID filter])}}`` — e.g.
  ``{{mm::total::mm::mm_employees}}``, ``{{mm::total::mm::1}}``, ``{{mm::total::mm::1::44}}``


.. _component_inserttags_items:
Output of One or More Items
-----------------------------

Output of one or more items with optional specification of the output type.

* ``{{mm::item::[MM Table-Name|ID]::[Item ID|ID,ID,ID]::[ID render setting](::[Output (Default:text)|html5])`` — e.g.
  ``{{mm::item::mm_employees::1,3,42::5}}``, ``{{mm::item::1::1,3,42::5}}``


.. _component_inserttags_attributes:
Output of an Attribute
----------------------

Output of an attribute with optional specification of the output type.

* ``{{mm::attribute::[MM Table-Name|ID]::[Item ID]::[ID render setting]::[Attribute Col-Name|ID](::[Output (Default:text)|html5|raw])`` — e.g.
  ``{{mm::attribute::mm_employees::42::5::combined_name}}``, ``{{mm::attribute::mm_employees::42::5::date_start::raw}}``


.. _component_inserttags_jumpto:
Output of Parameters for the Detail Page Redirect
---------------------------------------------------

Output of the URL, label, page ID or values of the "param" node, such as the alias of a redirect
(jumpTo) of an item — the redirect must be configured accordingly in the render settings.

* ``{{mm::jumpTo::[MM Table-Name|ID]::[Item ID]::[ID render setting](::[Parameter (Default:url)|label|page|params.attname])}}`` — e.g.
  ``{{mm::jumpTo::mm_employees::42::5}}``, ``{{mm::jumpTo::mm_employees::42::5::page}}``, ``{{mm::jumpTo::mm_employees::42::5::params.alias}}``

The linking with the :ref:`output of a URL to the detail page can also be done via a picker
<rst_cookbook_specials_picker-for-tinymce>`, which is easier for editors to handle.



.. |br| raw:: html

   <br />
