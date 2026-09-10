.. _rst_extended_erd-viewer:

MetaModels ERD
===============

Shows an automatically generated `entity-relationship diagram
<https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model>`_ of all MetaModels tables and
their relations in the backend - as a complement to the manually maintained sketch recommended
under :ref:`Database Structure <component_relations_database_structure>`. The diagram is freshly
generated from the database on every call and is therefore always up to date.

This view is helpful for quickly getting an overview of the existing tables and their relations -
for example on a project you have taken over, or one you have not worked on in a long time.

Note that not all tables from the database are shown here - e.g. for the Tags attribute (multiple
selection), there is a relation table between the two linked tables; this is not shown in the
diagram.


Prerequisites
--------------

* as of MetaModels 2.5
* own Composer package, no dependency on other extensions apart from MetaModels itself


Installation via Contao Manager or Composer
---------------------------------------------

.. code-block:: bash

   composer require metamodels/erd-viewer


Access
------

After installation, an additional menu item "ERD View" appears at the top right of the "All
MetaModels" list, next to "New MetaModel" and "Edit multiple".

|img_erd-button|

The "Back" arrow on the ERD page leads back to exactly this list.

Like the entire MetaModels administration, the **ERD view is only accessible to admins**.


The View
--------

|img_erd-overview|

**Diagram:** Each MetaModels table appears as a blue box, each referenced Contao table (e.g.
``tl_member`` or ``tl_page``) that is not itself a MetaModel appears as a grey, dashed box marked
"external". Clicking a box opens a detail panel on the right with the MetaModel name,
parent/child relation and the complete attribute list; at the same time all boxes not directly
connected to it are dimmed. Escape or a click elsewhere clears this again.

Two kinds of relations are shown:

* **Attribute relations** via the Select and Tags attributes as well as their translated variants
  (translated select, translated tags) - labelled with the attribute name and the cardinality in
  square brackets: ``[1:n]`` for select/translated select, ``[m:n]`` for tags/translated tags.
* **Parent-child relations** (:ref:`child tables <component_relations_child-tables>`) as a dashed
  orange arrow labelled "Child of [n:1]".

**Filter:** The search field or the checkbox list on the left can be used to restrict the diagram
to a subset of the tables - both act together and live on the diagram. The "All"/"None" buttons
set all checkboxes at once.

**Views:** A currently set table selection can be saved under a freely chosen name. Saved views
are visible and usable **for all admins in the backend**, can be applied by clicking their name,
and can be deleted again by the **creating user** via the "×" next to it.

**Pan/zoom:** The mouse wheel zooms, holding the mouse button down in an empty area pans (the
cursor turns into a hand); zoom buttons and a "reset all" icon are also available. The overview
map at the top right shows the complete graph together with a frame for the currently visible
section.

**Export:** The currently visible section of the map can be downloaded as SVG or PNG, the
currently filtered table selection additionally as a Graphviz ``.dot`` file or as GraphML.

.. tip:: The GraphML file can be opened free of charge and without installation in `yEd Live
   <https://www.yworks.com/yed-live/>`_ and freely edited further there - e.g. for a cleanly
   hand-adjusted layout or documentation outside the backend.


.. |img_erd-button| image:: /_img/screenshots/extended/erd-viewer/erd-button.png
.. |img_erd-overview| image:: /_img/screenshots/extended/erd-viewer/erd-overview.png
