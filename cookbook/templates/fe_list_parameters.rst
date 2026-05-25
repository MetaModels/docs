.. _rst_cookbook_templates_fe_list_parameters:

Custom Parameters for the MM List Output in the Frontend
=========================================================

.. note:: This feature is available from MM 2.2.

In the list output settings (CE/FE module) there is an option to pass custom values to the
template. These can be text or numeric values. This makes it easy for editors to control or
vary the list output using parameters while using the same FE template. It allows a list
template to be further generalised and controlled from the backend — for example with labels,
translations, or parameters for the output or JavaScript content.

Using an MCW, custom key-value pairs can be created that are available in the template as an
array via ``$this->parameter``.

The following two screenshots show a possible input in the backend and what is passed to the
template:

|img_settings-wizard_01|

|img_settings-wizard_02|

The following code can be placed in the header section of the
:ref:`list template <component_templates_fe-list>` and shows as an example how to access the
values including a default value:

.. code-block:: php
   :linenos:

   <?php
   // Get value for "key".
   $extract = fn(string $keyName, string $default = ''): string =>
       (false !== $index = \array_search($keyName, \array_column($this->parameter, 'key'), true))
       ? $this->parameter[$index]['value']
       : $default;
   $valueOne = $extract('key1', 'value0');
   $valueTwo = $extract('key2', '');
   // dump($this->parameter, $valueOne, $valueTwo);


.. |img_settings-wizard_01| image:: /_img/screenshots/metamodel_new_features/settings-wizard_01.jpg
.. |img_settings-wizard_02| image:: /_img/screenshots/metamodel_new_features/settings-wizard_02.jpg


