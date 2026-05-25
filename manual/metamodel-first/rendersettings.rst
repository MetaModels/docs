.. _mm_first_rendersettings:

|img_rendersettings_32| Render Settings
========================================

In this step, the render settings for the MetaModel "Employee List" are created. A render
setting is needed for the backend (data entry) and for the frontend (data output).

To access the render settings, activate the MetaModels overview so that the "Employee List"
entry is visible. Then click the icon "|img_rendersettings| Render settings" and the view
switches to the render settings overview — which is currently still empty.

After clicking "|img_new| New", the input mask for the first render setting opens immediately.
In the "Name" input field, enter a descriptive name such as "BE List" (see screenshot), check
the "Default" checkbox, and save the entry with "Save and close".

|img_rendersettings_01|

The render settings overview should now show the first entry "BE List" — see screenshot.

|img_rendersettings_02|

Clicking the icon "|img_rendersetting| Render settings for attributes" opens the next level for
the attributes. Here, the attributes to be displayed in the respective render setting list are
selected or activated.

A simple way to add all created attributes is via the header icon
"|img_rendersettings_add| Add all" — after clicking the "Continue" and "Save and close" buttons,
all existing attributes are added to the render setting. By default, the attributes are not
activated — this can easily be done via the "eye icon". In this example, the attributes "Last
name" and "First name" are activated — the attribute list should now look like the screenshot.

|img_rendersettings_03|

The render settings for the backend display are now complete. The render settings for the
frontend display can follow next.

The procedure is analogous to that for the "BE List" — in the render settings, "FE List" could
be entered as the name. Additionally, the display of attribute labels is disabled via the
"Hide labels" checkbox (see screenshot).

|img_rendersettings_04|

For the frontend display, all necessary attributes are activated — all except the "Published"
attribute, which is needed for the filter and does not need to be (or should not be) output
(see screenshot).

|img_rendersettings_05|

The preparations for the backend and frontend listings are now complete and the render settings
overview should now show the two lists (see screenshot).

|img_rendersettings_06|


.. |img_rendersettings_32| image:: /_img/icons/rendersettings_32.png
.. |img_rendersettings| image:: /_img/icons/rendersettings.png
.. |img_rendersetting| image:: /_img/icons/rendersetting.png
.. |img_rendersettings_add| image:: /_img/icons/rendersettings_add.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_edit| image:: /_img/icons/edit.gif

.. |img_rendersettings_01| image:: /_img/screenshots/metamodel_first/rendersettings_01.png
.. |img_rendersettings_02| image:: /_img/screenshots/metamodel_first/rendersettings_02.png
.. |img_rendersettings_03| image:: /_img/screenshots/metamodel_first/rendersettings_03.png
.. |img_rendersettings_04| image:: /_img/screenshots/metamodel_first/rendersettings_04.png
.. |img_rendersettings_05| image:: /_img/screenshots/metamodel_first/rendersettings_05.png
.. |img_rendersettings_06| image:: /_img/screenshots/metamodel_first/rendersettings_06.png
