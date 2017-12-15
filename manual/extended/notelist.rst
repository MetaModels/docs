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
* Contao 3.5.x
* MetaModels core dev-hotfix/2.0.0-alpha16 (b326aed1) und DCG 2.0.0-beta39

In the package manager type `metamodels/notelist` into the search field, then install and update the database. 


Create a watch list
-------------------

After you have successfully installed watch list, you will see a new icon in the row of metaModels icons. Click on it to set up and edit watch lists.

|img_notelist_icon|

When you create a new watch list choose a name first. 
At the moment you can select the PHP session or Contao session as "storage adapter".

If you select Contao session, the values of a watch list of logged in users will automatically be saved in the session values of the database. They are made available again, when the user logs in again.

You can restrict the recording of data records to only records with certain properties, e.g. the "department" or particular member groups with the filter selection. Filtering for member groups is possible by using the following extension "`condition membergroup filter
<https://github.com/cboelter/metamodels-filter_condition_membergroup>`_". 

|img_nodelist_config|

Via the list view you'll get access to all watch lists that have been set up.

|img_notelist_overview|


Activate watch list in MetaModels list
--------------------------------------

You will find a new section called "note list" in the CE MetaModels list, respectively in the FE module, where you can activate one or more of the watch lists, which you have set up.

|img_notelist_ce_mm-list|

You can change the order of the "action outputs" by drag & drop via the watch list sorting.

If you are using the default template for output you don't have to make any further changes. You should then see a new link in the FE list view to add data records to the watch list.

If you are using a customized template you'll have to make some changes to be able to add the new watch list links.
The links are available in the `action` node and can be output with the following code:

.. code-block:: html
   :linenos:

   <a href="<?= $arrItem['actions']['notelist_1']['href'] ?>" class="<?= $arrItem['actions']['notelist_1']['class'] ?>"><?= $arrItem['actions']['notelist_1']['label'] ?></a>

|img_notelist_fe_list|


Watch list output via filter
----------------------------

You can output the watch list on the front end with a normal MetaModels list, which filters out elements from the watch list.

To be able to filter you just need to create a filter with the new filter setting  "Notelist". In the filter settings just select the watch list with the elements you'd like to output.

|img_notelist_filterrule|

In the filtered list on the front end you should now see only employees from the  watch list.

|img_notelist_filtered_list|

In the list view it would be e.g. possible to activate a second watch list, to transfer elements from one watch list to another - e.g. from a "like" list to an "order" list.

In the settings for a watch list you can optionally set a filter for the inclusion onto a watch list. E.g if there are only employees allowed who belong to the sales department, the list looks as folllows:

|img_notelist_fe_list_with_filter|


Data display and inclusion into the form
----------------------------------------

There is also a new widget `MetaModels note list` available in the form generator. With its settings you can control the display in the form as well as in the email.

You can activate one or more watch lists and select a render setting for the FE output as well as for the email output.

|img_nodelist_form_widget|

The respective data records are output in the form with the possibility to delete the whole list or individual items.

|img_nodelist_form_fe_list|

Data is sent by email and output can be customized with the email template. The Contao form options or the Notification Center are available to you for transmission.

|img_notelist_email_list|


Known Issues and Next Features
------------------------------

* after sending the form the elements are not removed from the watch list
* optional specification of a number per watchlist item is missing


InsertTags
----------

There are different InsertTags implemented for the output of the number of items in the watch lists. They output the number as follows ('mm_mitarbeiterliste' 
is the respective MetaModels):

* Number of all items: {{metamodels_notelist::sum::mm_mitarbeiterliste}}
* Number of all items of the watch list with ID 1: {{metamodels_notelist::sum::mm_mitarbeiterliste::1}}
* Number of all items of the watch list with ID 1 and 2: {{metamodels_notelist::sum::mm_mitarbeiterliste::1,2}}

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


Donations
---------

Thanks for the donations * for this extension to:

* `Sebastian Krull <http://www.sebastiankrull.de>`_: 350 €
* `Carsten Merz <http://www.fitkurs.de>`_: 350 € 


(donations are stated at their net value)


.. |br| raw:: html

   <br />


.. |img_notelist_icon| image:: /_img/screenshots/extended/notelist/notelist_icon.png
.. |img_nodelist_config| image:: /_img/screenshots/extended/notelist/nodelist_config.png
.. |img_notelist_overview| image:: /_img/screenshots/extended/notelist/notelist_overview.png
.. |img_notelist_ce_mm-list| image:: /_img/screenshots/extended/notelist/notelist_ce_mm-list.png
.. |img_notelist_fe_list| image:: /_img/screenshots/extended/notelist/notelist_fe_list.png
.. |img_notelist_filterrule| image:: /_img/screenshots/extended/notelist/notelist_filterrule.png
.. |img_notelist_filtered_list| image:: /_img/screenshots/extended/notelist/notelist_filtered_list.png
.. |img_notelist_fe_list_with_filter| image:: /_img/screenshots/extended/notelist/notelist_fe_list_with_filter.png
.. |img_nodelist_form_widget| image:: /_img/screenshots/extended/notelist/nodelist_form_widget.png
.. |img_nodelist_form_fe_list| image:: /_img/screenshots/extended/notelist/nodelist_form_fe_list.png
.. |img_notelist_email_list| image:: /_img/screenshots/extended/notelist/notelist_email_list.png