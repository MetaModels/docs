.. _rst_cookbook_specials_register-services:

Registering Services
====================

.. note:: The options presented below are merely suggestions or examples — for
   your own work, you should develop the optimal configuration for your needs.
   More on the topic can be found in the
   `Symfony documentation <https://symfony.com/doc/6.4/index.html>`_.

MetaModels comes with many functions that only need to be activated or configured
in the backend. However, not every conceivable setting and function can be
covered. For individual project tasks, the built-in options may not be sufficient
and must be supplemented with custom adjustments.

Various MM and DC_General (DCG) methods are available here to accomplish these
tasks in just a few lines.

In particular, the provided events offer a simple way to implement custom logic
or hook into the existing logic. An introduction to working with the
:ref:`ref_api` is provided e.g. by the
`CK23 talk by Ingolf Steinhardt <https://www.e-spin.de/contao-metamodels/metamodels-vortrag-contao-konferenz-2023.html>`_.

The following presents various implementation approaches using the PrePersistModelEvent
as an example. The event is called by the input mask "just before saving to the
DB", provided that a field value has changed. With this event, entered data can
e.g. be manipulated or new data dynamically generated.

Event listeners and other services are registered analogously to
`Contao hooks <https://docs.contao.org/dev/framework/hooks/#registering-hooks>`_.

.. note:: Requires at least Contao 4.13 and PHP 8


.. _register-services-with-attribute:

1. Registration via Attribute
------------------------------

Registration via attribute is the simplest implementation option — only the
following file needs to be created and the cache cleared.

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/PrePersistModelEventListener.php
   namespace App\EventListener;

   use ContaoCommunityAlliance\DcGeneral\Event\PrePersistModelEvent;
   use Symfony\Component\EventDispatcher\Attribute\AsEventListener;

   #[AsEventListener(PrePersistModelEvent::NAME)]
   class PrePersistModelEventListener
   {
       public function __invoke(PrePersistModelEvent $event)
       {
           if ('mm_employees' !== $event->getEnvironment()->getDataDefinition()?->getName()) {
               return;
           }

           $model = $event->getModel();
       }
   }

After clearing the cache, the registration can be verified as follows:

``php vendor/bin/contao-console debug:event-dispatcher dc-general.model.pre-persist``

The key ``dc-general.model.pre-persist`` is defined in the respective class and
can also be used as a parameter in the attribute. If registration was successful,
the marked entry should be found.

|img_register-services_01.png|

If this is not yet the case, running ``composer install`` may resolve the issue.

If the executing method is named ``__invoke``, the attribute key can be written
at the class name as in the example — if you want to use a custom method name,
e.g. when multiple methods for different events exist in one class, the attribute
key must be placed on the respective method name.

This approach works in this simple form only if no further events or similar are
registered via ``services.yml``. If this is the case, you can either switch
entirely to registration via ``services.yml`` — see item 2 — or add the
following lines to ``services.yml`` to enable automatic loading:

.. code-block:: yaml
   :linenos:

   # config/services.yml
   services:
     _defaults:
       autowire: true
       autoconfigure: true
       public: false

     App\:
       resource: '../src/*'


.. _register-services-with-services:

2. Registration Without Attribute via services.yml
---------------------------------------------------

As an alternative to registration via attribute, the call can be included via
``services.yml`` — especially if you have various settings and do not want to
rely on automatic registration.

The class then looks as follows:

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/PrePersistModelEventListener.php
   namespace App\EventListener;

   use ContaoCommunityAlliance\DcGeneral\Event\PrePersistModelEvent;

   class PrePersistModelEventListener
   {
       public function __invoke(PrePersistModelEvent $event)
       {
           if ('mm_employees' !== $event->getEnvironment()->getDataDefinition()?->getName()) {
               return;
           }

           $model = $event->getModel();
       }
   }

The following entry must also be added to ``services.yml``:

.. code-block:: yaml
   :linenos:

   # config/services.yml
   services:
     App\EventListener\PrePersistModelEventListener:
       tags:
         - { name: kernel.event_listener, event: dc-general.model.pre-persist }

If the method is not named ``__invoke``, the method name must be added to the
tags in ``services.yml`` — a priority can also be specified. More at
`Symfony <https://symfony.com/doc/6.4/event_dispatcher.html>`_.


.. _register-services-with-attribute-and-other:

3. Registration via Attribute with Additional Services
------------------------------------------------------

If access to further services is needed in the class, they can be automatically
injected via the ``constructor``.

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/PrePersistModelEventListener.php
   namespace App\EventListener;

   use ContaoCommunityAlliance\DcGeneral\Event\PrePersistModelEvent;
   use MetaModels\IFactory;
   use Symfony\Component\EventDispatcher\Attribute\AsEventListener;

   #[AsEventListener(PrePersistModelEvent::NAME)]
   class PrePersistModelEventListener
   {
       public function __construct(private readonly IFactory $factory)
       {
       }

       public function __invoke(PrePersistModelEvent $event)
       {
           if ('mm_employees' !== $event->getEnvironment()->getDataDefinition()?->getName()) {
               return;
           }

           $model = $event->getModel();

           $anotherMetaModel = $this->factory->getMetaModel('mm_another_model');
       }
   }


.. _register-services-with-services-and-other:

4. Registration Without Attribute via services.yml with Additional Services
---------------------------------------------------------------------------

If access to further services is needed in the class, they can be injected via
the ``constructor`` by passing the service as an argument in ``services.yml``.

.. code-block:: php
   :linenos:

   <?php
   // src/EventListener/PrePersistModelEventListener.php
   namespace App\EventListener;

   use ContaoCommunityAlliance\DcGeneral\Event\PrePersistModelEvent;
   use MetaModels\IFactory;

   class PrePersistModelEventListener
   {
       public function __construct(private readonly IFactory $factory)
       {
       }

       public function __invoke(PrePersistModelEvent $event)
       {
           if ('mm_employees' !== $event->getEnvironment()->getDataDefinition()?->getName()) {
               return;
           }

           $model = $event->getModel();

           $anotherMetaModel = $this->factory->getMetaModel('mm_another_model');
       }
   }

.. code-block:: yaml
   :linenos:

   # config/services.yml
   services:
     App\EventListener\PrePersistModelEventListener:
     arguments:
       - '@metamodels.factory'
       tags:
         - { name: kernel.event_listener, event: dc-general.model.pre-persist }


.. _register-services-all-in-src:

5. All Files in src/ with Namespace App
----------------------------------------

If you want to keep all files — including e.g. ``service.yml`` — compactly in
the ``src/`` folder while still working with the ``App`` namespace, you can look
at the CK23 talk example on
`Github <https://github.com/e-spin/vortrag-contao-konferenz-2023/tree/main/src>`_
or download the ``src/`` folder for testing and adjust ``composer.json``
accordingly.

Note the entry
`foo <https://github.com/e-spin/vortrag-contao-konferenz-2023/blob/main/src/Resources/config/foo.yml>`_ —
it is required to work around some "Contao magic" for the namespace...


.. _register-services-all-in-src-own-namespace:

6. All Files in src/ with Custom Bundles
-----------------------------------------

If you want to work with your own namespace and less Contao/Symfony magic, more
files need to be created in ``src/``. This can be useful e.g. when working with
multiple separate bundles and their namespaces. In that case, additional
subfolders such as ``src/ProjectOneBundle`` would be created.

If this is not the case, all files can be placed directly in ``src/`` with a
namespace such as ``AppBundle``.

The following shows an example structure:

|img_register-services_02.png|

For the files and namespace to be found correctly, ``composer.json`` must be
extended as follows:

.. code-block:: json
   :linenos:

   "autoload": {
     "psr-4": {
         "AppBundle\\": "src/"
     }
   },

The following two files are essential for the basic setup:

.. code-block:: php
   :linenos:

   <?php
   // src/AppBundle.php
   namespace AppBundle;

   use Symfony\Component\HttpKernel\Bundle\Bundle;

   /**
    * This is the local customization bundle.
    */
   class AppBundle extends Bundle
   {
   }

.. code-block:: php
   :linenos:

   <?php
   // src/DependencyInjection/AppExtension.php
   namespace AppBundle\DependencyInjection;

   use Symfony\Component\Config\FileLocator;
   use Symfony\Component\DependencyInjection\ContainerBuilder;
   use Symfony\Component\DependencyInjection\Loader\YamlFileLoader;
   use Symfony\Component\HttpKernel\DependencyInjection\Extension;

   class AppExtension extends Extension
   {

       /**
        * Loads a specific configuration.
        *
        * @throws \InvalidArgumentException When provided tag is not defined in this extension
        */
       public function load(array $configs, ContainerBuilder $container)
       {
           $loader = new YamlFileLoader($container, new FileLocator(__DIR__ . '/../Resources/config'));
           $loader->load('services.yml');
       }
   }

The following file ``ContaoManagerPlugin.php`` is optional and controls the
order in which the custom bundle is loaded relative to other bundles. Here you
can e.g. specify that your own bundle is loaded after Contao — other bundles
such as the NotificationCenter can also be specified. For the file to be
recognised, this must be stated in ``composer.json`` — see below.

.. code-block:: php
   :linenos:

   <?php
   // src/ContaoManager/ContaoManagerPlugin.php

   use Contao\CoreBundle\ContaoCoreBundle;
   use Contao\ManagerBundle\ContaoManagerBundle;
   use Contao\ManagerPlugin\Bundle\BundlePluginInterface;
   use Contao\ManagerPlugin\Bundle\Config\BundleConfig;
   use Contao\ManagerPlugin\Bundle\Config\ConfigInterface;
   use Contao\ManagerPlugin\Bundle\Parser\ParserInterface;

   class ContaoManagerPlugin implements BundlePluginInterface
   {
       /**
        * Gets a list of autoload configurations for this bundle.
        *
        * @param ParserInterface $parser
        *
        * @return array<ConfigInterface>
        */
       public function getBundles(ParserInterface $parser): array
       {
           return [
               BundleConfig::create(AppBundle::class)
                   ->setLoadAfter(
                       [
                           ContaoCoreBundle::class,
                           ContaoManagerBundle::class
                       ]
                   )
           ];
       }
   }

.. code-block:: json
   :linenos:

   "autoload": {
     "psr-4": {
         "AppBundle\\": "src/"
     },
     "classmap": [
         "src/ContaoManager/ContaoManagerPlugin.php"
     ]
   },




.. |img_register-services_01.png| image:: /_img/screenshots/cookbook/specials/register-services_01.png
.. |img_register-services_02.png| image:: /_img/screenshots/cookbook/specials/register-services_02.png
