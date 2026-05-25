.. _rst_cookbook_checklists_attribut_change:

Attribute Not Displayed After Change
=====================================

After changing an attribute (e.g. attribute type), it is no longer displayed on
the website.

Note: When the attribute type is changed, existing data in the database will be deleted!

Checklist:

   |box| Check attribute listings in render settings and input masks

   |box| If needed, delete and re-add the attribute in the render settings

   |box| Check that the attribute is set to visible in render settings and input masks

   |box| Re-enter values in the input mask after the change if needed

   |box| Use the :ref:`debug output <rst_cookbook_debug_templates>` to check if the attribute is arriving in the template


.. |box| raw:: html

   <span>&#9634;</span>


