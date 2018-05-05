.. _rst_extended_notelist:

MetaModels watch list
=====================

.. warning:: The watch list feature is still in fundraising stage. It will not be published until the fundraising target of x.000 € is reached. |br|
   We have a beta program for early installations. If you are interested please contact info@e-spin.de

The watch list feature (Note list) extends MetaModels and enables you to add individual data records (items) to a watch list.

Watchlist enables you to create e.g. a normal reminder list, comparison lists for product properties or even shopping cart functionalities.

If you have saved a data record in the watch list, you can of course also remove it again.

With the watch list comes a new filter rule, which enables you to filter for existing watch list data records. 

There is also a new widget for the form generator that enables you to list data records from the watch list and transfer them via email. It is also possible to send the email via Notification Center.

You can create several watch lists in each MetaModels. This allows to add a data record to two watch lists at the same time, e.g. to a "like list" and to an "order list". You can also transfer a data record from a "like list" to an "order list".

In the configuration of a watch list you can set up a filter, which ensures that only specific data records can be added to the watch list, e.g. only employees from the sales department.

Watch list also works with translated MetaModels. This means that data records of a watch list will remain preserved, when you switch the language.


Installation via Composer
-------------------------

Prerequisites for installation:

* PHP 7.x
* Contao 3.5.x or Contao 4.4.x
* MetaModels from core 2.0.0-alpha16 respecively 2.1 and DCG 2.0.0-beta39
* (Zip file of the extension, pls send a request to info@e-spin.de)

In the package manager type `metamodels/notelist` into the search field, then install and update the database. 


Create a watch list
-------------------

After you have successfully installed watch list, you will see a new icon in the row of metaModels icons. Click on it to set up and edit watch lists.

|img_notelist_icon_en|

When you create a new watch list choose a name first. 
At the moment you can select the PHP session or Contao session as "storage adapter".
If you select Contao session, the values of a watch list of logged in users will automatically be saved in the session values of the database. They are made available again, when the user logs in again.

You can restrict the recording of data records to only records with certain properties, e.g. the "department" or particular member groups with the filter selection. Filtering for member groups is possible by using the following extension "`condition membergroup filter
<https://github.com/cboelter/metamodels-filter_condition_membergroup>`_". 

|img_nodelist_config_en|

Via the list view you'll get access to all watch lists that have been set up.

|img_notelist_overview_en|


Activate watch list in MetaModels list
--------------------------------------

You will find a new section called "note list" in the CE MetaModels list, respectively in the FE module, where you can activate one or more of the watch lists, which you have set up.

|img_notelist_ce_mm-list_en|

You can change the order of the "action outputs" by drag & drop via the watch list sorting.

If you are using the default template for output you don't have to make any further changes. You should then see a new link in the FE list view to add data records to the watch list.

If you are using a customized template you'll have to make some changes to be able to add the new watch list links.
The links are available in the `action` node and can be output with the following code:

.. code-block:: html
   :linenos:

   <a href="<?= $arrItem['actions']['notelist_1']['href'] ?>" class="<?= $arrItem['actions']['notelist_1']['class'] ?>"><?= $arrItem['actions']['notelist_1']['label'] ?></a>

|img_notelist_fe_list_en|


Watch list output via filter
----------------------------

You can output the watch list on the front end with a normal MetaModels list, which filters out elements from the watch list.

To be able to filter you just need to create a filter with the new filter setting  "Notelist". In the filter settings just select the watch list with the elements you'd like to output.

|img_notelist_filterrule_en|

In the filtered list on the front end you should now see only employees from the  watch list.

|img_notelist_filtered_list_en|

In the list view it would be e.g. possible to activate a second watch list, to transfer elements from one watch list to another - e.g. from a "like" list to an "order" list.

In the settings for a watch list you can optionally set a filter for the inclusion onto a watch list. E.g if there are only employees allowed who belong to the sales department, the list looks as folllows:

|img_notelist_fe_list_with_filter_en|


Data display and inclusion into the form
----------------------------------------

There is also a new widget `MetaModels note list` available in the form generator. With its settings you can control the display in the form as well as in the email.

You can activate one or more watch lists and select a render setting for the FE output as well as for the email output.
Additionally, by checking the checkbox "clear list", you can define for each watch list whether the list should be cleared after the form processing.

|img_nodelist_form_widget_en|

„Custom email template“ is an optional template which contains all renderings of the mail output of a watch list and "encloses" them.
Please note that you have to specify the extension "text2 for " Supported template formats" in the Contao settings!
Watch list data can only be sent as (plain) text in an email at the moment - the render setting "output format" for the listing within the email has to be set to "text" respectively.

In the form the respective data records are output with the selected render setting.


|img_nodelist_form_fe_list_en|

It is not possible e.g. to delete elements of the watch list within the form, because by reloading the page all the data already entered in the form would be lost .

You can output a list with all elements of the watch list before you output the form. There you could edit theme seprately or delete the whole list.

.. code-block:: html
   :linenos:

   <p><a href="de/metamodels/note-list-contact-form.html?notelist_2_action=clear">Clear List 2</a></p></a>
   
|img_nodelist_form_fe_list_edit_items_en|

Data is sent by email and output can be customized with the email template. The Contao form options or the Notification Center are available to you for transmission.

|img_notelist_email_list_en|


Transfer of additional data for each item
-----------------------------------------

Optionally you can transfer additional data to the watchlist for each item, such as a number, tet or similar. To do this you'll have to create a form using the form generator, which contains the fields to display, e.g. field for a number and text field for a short information text - a submit button is not necessary and will be generated automatically.

This form will then be available in the watchlist settings - forms which already contain a watchlist formelement wll not be displayed (recursion!).

In the list view the form will be displayed with an "add/edit button" beneath each item. Data will be processed with the form and e.g. sent by email.

|img_notelist_fe_list_with_form_en|



InsertTags
----------

There are different InsertTags implemented for the output of the number of items in the watch lists. They output the number as follows ('mm_mitarbeiterliste' 
is the respective MetaModels):

* Number of all items: {{metamodels_notelist::sum::mm_employeelist}}
* Number of all items of the watch list with ID 1: {{metamodels_notelist::sum::mm_employeelist::1}}
* Number of all items of the watch list with ID 1 and 2: {{metamodels_notelist::sum::mm_employeelist::1,2}}

If there is no item on the watch list, 0 (Null) will be output.


Events
------

There is an event listener available, if you need to monitor manipulation of a watch list (add, remove, clear).

The event listener allows you to trigger feedback to the website or logging /tracking of actions.

As an example for a feedback you can insert the following code to a custom Contao module e.g. at ``/system/modules/myModule/config/event_listeners.php`` 

.. code-block:: php
   :linenos:

   <?php

use MetaModels\NoteList\Event\ManipulateNoteListEvent;
use MetaModels\NoteList\Event\NoteListEvents;

return [
    NoteListEvents::MANIPULATE_NOTE_LIST => [
        function (ManipulateNoteListEvent $event) {
            // Only handle note list "1".
            if ('1' !== ($listId = $event->getNoteList()->getStorageKey())) {
                return;
            }

            switch ($event->getOperation()) {
                case ManipulateNoteListEvent::OPERATION_ADD:
                    Message::addConfirmation('Added ' . $event->getItem()->get('id') . ' to ' . $listId);
                    // Add your own notes in metaData.
                    $metaData = $event->getNoteList()->getMetaDataFor($event->getItem());
                    $metaData['tstamp'] = time();
                    $event->getNoteList()->updateMetaDataFor($event->getItem(), $metaData);
                    break;
                case ManipulateNoteListEvent::OPERATION_REMOVE:
                    Message::addConfirmation('Removed ' . $event->getItem()->get('id') . ' to ' . $listId);
                    break;
                case ManipulateNoteListEvent::OPERATION_CLEAR:
                    Message::addConfirmation('Cleared ' . $listId);
                    break;
                default:
                    throw new \RuntimeException('Unknown note list operation: ' . $event->getOperation());
            }
        }
    ]
];

On the front end the feedback can be shown in a template with the output of the Contao message - e.g.

.. code-block:: php
   :linenos:
   
   <?php
   echo Message::generate();
   ?>


Known Issues and Next Features
------------------------------

* Translation in DE (if project is released via Transifex)
* Data transfer to a form as HTML (currently only available as text)
* 

Donations
---------

Thanks for the donations * for this extension to:

* `Sebastian Krull <http://www.sebastiankrull.de>`_: 350 €
* `Carsten Merz <http://www.fitkurs.de>`_: 350 € 
* `Westwerk GmbH & Co. KG: <https://www.westwerk.ac>`_: 350 €
* `Niels Hegmanns <http://www.heimseiten.de>`_: 350 € 
* `Hofer Werbung <http://www.hofer-werbung.de>`_: 350 € 


(donations are stated at their net value)


.. |br| raw:: html

   <br />


.. |img_notelist_icon_en| image:: /_img/screenshots/extended/notelist/notelist_icon_en.png
.. |img_nodelist_config_en| image:: /_img/screenshots/extended/notelist/nodelist_config_en.png
.. |img_notelist_overview_en| image:: /_img/screenshots/extended/notelist/notelist_overview_en.png
.. |img_notelist_ce_mm-list_en| image:: /_img/screenshots/extended/notelist/notelist_ce_mm-list_en.png
.. |img_notelist_fe_list_en| image:: /_img/screenshots/extended/notelist/notelist_fe_list_en.png
.. |img_notelist_filterrule_en| image:: /_img/screenshots/extended/notelist/notelist_filterrule_en.png
.. |img_notelist_filtered_list_en| image:: /_img/screenshots/extended/notelist/notelist_filtered_list_en.png
.. |img_notelist_fe_list_with_filter_en| image:: /_img/screenshots/extended/notelist/notelist_fe_list_with_filter_en.png
.. |img_nodelist_form_widget_en| image:: /_img/screenshots/extended/notelist/nodelist_form_widget_en.png
.. |img_nodelist_form_fe_list_en| image:: /_img/screenshots/extended/notelist/nodelist_form_fe_list_en.png
.. |img_notelist_email_list_en| image:: /_img/screenshots/extended/notelist/notelist_email_list_en.png
.. |img_notelist_fe_list_with_form_en| image:: /_img/screenshots/extended/notelist/notelist_fe_list_with_form_en.png
.. |img_nodelist_form_fe_list_edit_items_en| image:: /_img/screenshots/extended/notelist/nodelist_form_fe_list_edit_items_en.png