.. _rst_cookbook_debug_templates:

Debug Templates
===============

If you need a custom template for output, e.g. for a frontend list, or want to know
which attributes are being passed to an existing template, you can conveniently output
them using the Symfony debug toolbar.

The default template is "metamodel_prerendered", or whichever template was selected
in the render setting for output.

If no custom template is in use yet, a copy of "metamodel_prerendered" must be created
in the Contao "/templates" folder.

The template is extended with the following lines at the top:

.. code-block:: php
   :linenos:

   <?php
   // Debug items.
   if (\Contao\System::getContainer()->get('kernel')->isDebug()) {
       dump($this->data);
   }
   ?>

The page must then be viewed in the frontend in debug mode. To do this, enable debug
mode in the backend header, or for persistent debug mode, create a `.env` or `.env.local`
file in the project folder with the content `APP_ENV=dev` (the page should then not be
publicly accessible).

The array can then be inspected in the debug toolbar via the "crosshair icon":

|img_symfony-toolbar|

The `isDebug()` check ensures that the `dump` does not interfere with the normal page call.

From MM 2.2 there is a dedicated template `metamodel_prerendered_debug.html5` that can
be selected in the render settings for frontend output — no MM list values will be output
with this template initially.

For easy copying of the array data into a frontend template, there is the
:ref:`rst_cookbook_frontend_array-helper`, which generates output in the source code for
`copy & paste`.


Debug in MM 2.0
---------------

In Contao 3, the Symfony toolbar is not available and `print_r` must be used instead.

The template is extended with some output lines and should start as follows:

.. code-block:: php
   :linenos:

   <?php
   echo "<!-- DEBUG START \n";
   echo "<pre>\n";
   print_r($this->items->parseAll($this->getFormat(), $this->view));
   echo "</pre>\n";
   echo "\n DEBUG END -->";
   ?>

When the corresponding page with the listing is opened in the browser, the debug output
should appear in the page source.

If the output is very large, rendering in the browser can become very slow — a workaround
is to only output a single item node:

.. code-block:: php
   :linenos:

   <?php
   echo "<!-- DEBUG START \n";
   echo "<pre>\n";
   // only 0th node
   print_r($this->items->parseAll($this->getFormat(), $this->view)[0]);
   echo "</pre>\n";
   echo "\n DEBUG END -->";
   ?>

If the redirect and filter for the detail page are configured in the render settings,
the array output in the source code can become very large and often causes an
"Allowed memory size..." error. A workaround is to temporarily disable the redirect filter.

The output can be removed by commenting out, deleting, or switching to a different template.


.. |img_symfony-toolbar| image:: /_img/screenshots/cookbook/debug/symfony-toolbar.jpg
