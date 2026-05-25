.. _rst_cookbook_inputmask_regex:

Input Mask: Custom RegEx Validation
=====================================

If you need custom regex validation for a text input field in an input mask,
this can be implemented using the following event listener.

To integrate it and activate it for the field in the input mask, the validation
must first be made available using Contao's built-in tools.

For this, the hook "addCustomRegex" is set up as follows — see
`API: addCustomRegex <https://docs.contao.org/books/api/extensions/hooks/addCustomRegexp.html>`_

.. note:: The following description still applies to Contao 4.4 — current
   implementations should be placed in the `src/` folder.

* Create a folder for your own module under /system/modules — e.g. "/metamodels_mycustoms"
* Create two further folders "/config" and "/classes" inside metamodels_mycustoms
* In the /classes folder, create the file "MyClass.php" as described in the Contao API
* In the /config folder, create the file "config.php" as described in the Contao API
* Additionally in the /config folder, create the file "event_listeners.php" — the key
  of the $options array must be identical to the value checked for $strRegexp in
  /MyClass ('plz')
* Once all files are created and filled with source code, the "autoload.php" can be
  generated via the developer tools in the Contao backend under "Autoload creator"

In the settings of an input field for a "Text" attribute, the entry "PLZ" should
then be available in the RegEx validation selection. If this is not the case,
try clearing all caches in the backend and checking the files.

|img_own-regex|


Source Code
-----------

The files contain the following source code:

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
                   $objWidget->addError('Field ' . $objWidget->label . ' should contain a valid postal code.');
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

   // Event listener with priority "-1"
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

                   // Key "plz" equals $strRegexp check from myAddCustomRegexp
                   $options['plz'] = 'PLZ';

                   $event->setOptions($options);
               },
               -1
           )
       )
   );


The autoload.php in /system/modules/metamodels_mycustoms/config should look like
this after generation:

.. code-block:: php
   :linenos:

   <?php
   ClassLoader::addClasses(array
   (
       // Classes
       'MyClass' => 'system/modules/metamodels_mycustoms/classes/MyClass.php',
   ));


**Note:** The regex validation was taken from the Contao documentation and
represents only a very basic check for postal codes. More precise regex
validations can be found online, or a check against a list of valid postal
codes could be implemented here.


.. |img_own-regex| image:: /_img/screenshots/cookbook/inputmask/own-regex.jpg
