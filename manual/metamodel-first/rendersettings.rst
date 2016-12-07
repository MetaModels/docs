.. _mm_first_rendersettings:

|img_rendersettings_32| Render settings
=======================================

In this step we'll set up the render settings for our MetModel "Employee list". We need a render setting for the back end (data input) and for the front end (data output).

To set up a new render setting go to the MetaModels overview. Beneath the MetaModel "Employee list" click on the icon "|img_rendersettings| Render settings". The render setting overview for this MetaModel will open but currently there is no render setting available yet.

With a click on "|img_new| New" the input mask for a new render setting opens. Here you set an appropriate name for the setting, e.g. "BE list" (for back end list, see screenshot below). Then check the checkbox "Is default" and hit "Save and close". 

|img_rendersettings_01_en|

Now you can see your first entry named "BE list" in the render settings overview -  see screenshot below.

|img_rendersettings_02_en|

Next click on the icon "|img_rendersetting| Define attribute settings". Here you can select and activate the attributes which you want to be shown in the render setting.

The easiest way to add attributes to a render setting is with a click on the icon "|img_rendersettings_add| Add all" in the header. Then hit "Continue" and "Save and close" and all attributes available will be added to your render setting. Please note that the attributes are set to "unpublished" by default (grey "eye-icon"). But you can easily activate them with a click on the "eye icon".
In our example we will just activate the attributes "name" and "first name". 
Now your attributes should be activated as shown in the screenshot below.

|img_rendersettings_03_en|

Now you have successfully set up a render setting for the backend. 

Let's go on with the one for the front end:
The procedure is pretty much the same as for the back end. But this time you might want to choose "FE list" (for front end list) as the appropriate name.
Additionally we want to hide all labels by checking the checkbox "Hide labels" (see screenshot). 

|img_rendersettings_04_en|

For the front end view we will activate all the attributes except the attribute "published". As this is only required for the filtering, it doesn't need to be displayed in the front end (see screenshot).

|img_rendersettings_05_en|

This completes our preparations for the back end and front end listings. No you should see the overview of our two render settings as shown in the screenshot below.

|img_rendersettings_06_en|


.. |img_rendersettings_32| image:: /_img/icons/rendersettings_32.png
.. |img_rendersettings| image:: /_img/icons/rendersettings.png
.. |img_rendersetting| image:: /_img/icons/rendersetting.png
.. |img_rendersettings_add| image:: /_img/icons/rendersettings_add.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_edit| image:: /_img/icons/edit.gif

.. |img_rendersettings_01_en| image:: /_img/screenshots/metamodel_first/rendersettings_01_en.png
.. |img_rendersettings_02_en| image:: /_img/screenshots/metamodel_first/rendersettings_02_en.png
.. |img_rendersettings_03_en| image:: /_img/screenshots/metamodel_first/rendersettings_03_en.png
.. |img_rendersettings_04_en| image:: /_img/screenshots/metamodel_first/rendersettings_04_en.png
.. |img_rendersettings_05_en| image:: /_img/screenshots/metamodel_first/rendersettings_05_en.png
.. |img_rendersettings_06_en| image:: /_img/screenshots/metamodel_first/rendersettings_06_en.png

