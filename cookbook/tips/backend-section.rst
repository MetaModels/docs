.. _rst_cookbook_tips_backend-section:

Custom Section in the Backend Navigation
=========================================

Access to MetaModel data entry is often best placed in a dedicated section in the backend
navigation. To do this, a corresponding group must be created, to which the desired model(s)
can be assigned in the input mask properties under "Backend section".

|img_be-section|

This requires an SVG icon and an assignment via the Contao MenuEvent — it is planned that this
will be
`configurable via an entry in config.yaml <https://github.com/MetaModels/core/issues/1519>`_
in the future.

SVG icons can be downloaded from e.g. `material.io <https://material.io/tools/icons/>`_ — the
width, height, and fill colour should be adjusted as shown in the example using a text editor:

.. code-block:: svg
   :linenos:

    <svg xmlns="http://www.w3.org/2000/svg" fill="#91979c" width="15" height="15" viewBox="0 0 24 24">
        <path d="...."/>
    </svg>

The file can be saved e.g. at ``files/backend/group_icon_mm-test.svg`` (make the folder public).

An event listener is also required to create the entry — the group can be configured via the
following parameters (lines 30 to 34):

* $nodeName — alias of the entry
* $nodeTitle — title
* $nodeIcon — path to the icon
* $targetNode — search for an existing entry such as "content" for content items
* $targetType — whether the entry should appear ``before`` or ``after`` the "targetNode"

Place the listener at ``src/EventListener/BackendMenuListener.php`` and run ``composer install``.

.. code-block:: php
   :linenos:

   <?php

   namespace App\EventListener;

   use Contao\CoreBundle\Event\MenuEvent;
   use Knp\Menu\Util\MenuManipulator;
   use Symfony\Component\EventDispatcher\Attribute\AsEventListener;
   use Symfony\Component\HttpFoundation\RequestStack;
   use Symfony\Component\HttpFoundation\Session\Attribute\AttributeBagInterface;

   #[AsEventListener(priority: -1)]
   class BackendMenuListener
   {
       private array $targetTypes = ['before' => 0, 'after' => 1];

       public function __construct(
           private readonly RequestStack $requestStack,
       ) {
       }

       public function __invoke(MenuEvent $event): void
       {
           $factory = $event->getFactory();
           $tree    = $event->getTree();

           if ('mainMenu' !== $tree->getName()) {
               return;
           }

           $nodeName   = 'mm-test';
           $nodeTitle  = 'My MM Category';
           $nodeIcon   = '/files/backend/group_icon_mm-test.svg';
           $targetNode = 'content';
           $targetType = 'after';

           $categoryNode = $tree->getChild($nodeName);
           if (!$categoryNode) {
               $sessionBag  = $this->requestStack->getSession()->getBag('contao_backend');
               $status      = ($sessionBag instanceof AttributeBagInterface) ? $sessionBag->get('backend_modules') : [];
               $isCollapsed = ($status[$nodeName] ?? 1) < 1;

               $categoryNode = $factory
                   ->createItem($nodeName)
                   ->setLabel($nodeTitle)
                   ->setUri('/contao?mtg=' . $nodeName)
                   ->setLinkAttribute('class', 'group-' . $nodeName)
                   ->setLinkAttribute('title', $nodeTitle)
                   ->setLinkAttribute('data-action', 'contao--toggle-navigation#toggle:prevent')
                   ->setLinkAttribute('data-contao--toggle-navigation-category-param', $nodeName)
                   ->setLinkAttribute('aria-controls', $nodeName)
                   ->setLinkAttribute('aria-expanded', $isCollapsed ? 'false' : 'true')
                   ->setChildrenAttribute('id', $nodeName)
                   ->setLinkAttribute('style', \sprintf('background: url(%s) 3px 2px no-repeat;', $nodeIcon))
                   ->setExtra('translation_domain', false);

               if ($isCollapsed) {
                   $categoryNode->setAttribute('class', 'collapsed');
               }

               $tree->addChild($categoryNode);

               $targetPosition = \array_search($targetNode, \array_keys($tree->getChildren()), true);
               $targetPosition = false === $targetPosition ? 0 : $targetPosition + $this->targetTypes[$targetType];
               $manipulator    = new MenuManipulator();
               $manipulator->moveToPosition($categoryNode, $targetPosition);
           }
       }
   }
   ?>

The listener and a dummy SVG are available :download:`here for download </_download/BE-section.zip>`.


.. |img_be-section| image:: /_img/screenshots/cookbook/tips/be-section_01.png
