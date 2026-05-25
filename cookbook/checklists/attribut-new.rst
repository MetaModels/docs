.. _rst_cookbook_checklists_attribut_new:

Adding an Attribute to an Existing MetaModel
=============================================

If a MetaModel already exists and you want to add another attribute for output in
the template, the following points must be observed:

Checklist:

   |box| Go to the corresponding MetaModel and create the attribute (type, column name, name, label, etc.)

   |box| In render settings, open the attribute list for the frontend output e.g. "FE List" via the icon and add the attribute e.g. using "Add all" — check that it is set to visible

   |box| In input masks, select the appropriate mask and open the attribute list via the icon and add the attribute e.g. using "Add all" — check that it is set to visible

   |box| Fill in the new attribute in an existing or new record...

   |box| Use the :ref:`debug output <rst_cookbook_debug_templates>` to check if the attribute is arriving in the template and adjust the template as needed

Additional settings:

   |box| If the attribute should also appear in the backend list view, add the attribute in the render settings for backend output e.g. "BE List" (see above)

   |box| Configure settings such as image preview, template, CSS in render setting > attribute

   |box| Configure settings such as required field, TinyMCE, searchable/filterable in input mask > attribute


.. |box| raw:: html

   <span>&#9634;</span>


