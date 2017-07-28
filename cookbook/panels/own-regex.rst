.. _rst_cookbook_panels_regex:

Input screens: custom RegEx test
================================

You can implement your own RegEx validation for a text input field in an input screen with the following event listener.

To implement it, respectively to activate it for the field in the input screen, this validation must be made available for Contao on-board functionality.

In order to do so, we'll create the following hook "addCustomRegex" as follows - see `API: addCustomRegex <https://docs.contao.org/books/api/extensions/hooks/addCustomRegexp.html>`_

* create a folder for your custom module in /system/modules  - e.g. "/metamodels_mycustoms"
* in the folder metamodels_mycustoms add two more folders named "/config" and "/classes"
* in the folder /classes add the file "MyClass.php" as described in Contao API
* in the folder /config add the file "config.php" as described in Contao API
* additionally in the folder /config the file "event_listeners.php" - the key of the array $options must be the same as the value obtained from testing of $strRegexp in /MyClass ('zip')
* after you have created all the files and filled them with code, you can create the autoload.php by using "Autoload creator" under "developer tools" in the Contao back end.

The entry "ZIP" should now be available in the settings of an input field of an attribute of type "text" when the Regex test is selected. If not, purge all the caches in the back end and check the data if necessary.


|img_own-regex|

Source codes
------------

You'll find the following source code in the files:

File /system/modules/metamodels_mycustoms/classes/MyClass.php

.. code-block:: php
   :linenos:
   
   <?php
   class MyClass
   {
       public function myAddCustomRegexp($strRegexp, $varValue, Widget $objWidget)
       {
           if ($strRegexp == 'plz')
           {
               if (!preg_match('/^[0-9]{4,6}$/', $varValue))
               {
                   $objWidget->addError('Feld ' . $objWidget->label . ' should contain a valid ZIP postcode.');
               }
       
               return true;
           }
       
           return false;
       }
   }


File /system/modules/metamodels_mycustoms/config/config.php

.. code-block:: php
   :linenos:
   
   <?php
   $GLOBALS['TL_HOOKS']['addCustomRegexp'][] = array('MyClass', 'myAddCustomRegexp');


File /system/modules/metamodels_mycustoms/config/event_listeners.php

.. code-block:: php
   :linenos:

   <?php 
   use ContaoCommunityAlliance\DcGeneral\Contao\View\Contao2BackendView\Event\GetPropertyOptionsEvent;
   
   // Event Listener with priority "-1"
   return array
   (
       GetPropertyOptionsEvent::NAME => array(
           array(
               function (GetPropertyOptionsEvent $event) {
                   if (($event->getEnvironment()->getDataDefinition()->getName() !== 'tl_metamodel_dcasetting')
                       || ($event->getPropertyName() !== 'rgxp')) {
                       return;
                   }
       
                   $options = $event->getOptions();
                   
                   // Key "zip" equals $strRegexp test in myAddCustomRegexp
                   $options['zip'] = 'ZIP';
       
                   $event->setOptions($options);
               },
               -1
           )
       )
   );


The file autoload.php in /system/modules/metamodels_mycustoms/config should look as follows after its generation:

.. code-block:: php
   :linenos:

   <?php 
   ClassLoader::addClasses(array
   (
       // Classes
       'MyClass' => 'system/modules/metamodels_mycustoms/classes/MyClass.php',
   ));


**Notice:** the RegEx validation was taken from the Contao manual und represents just a simple test method for german ZIP codes. 
You can find more accurate RegEx checks online or you could also implement a check against a list with assigned zip code numbers.


.. |img_own-regex| image:: /_img/screenshots/cookbook/panels/own-regex.jpg

