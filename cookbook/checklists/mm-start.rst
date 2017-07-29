.. _rst_cookbook_checklists_mm-start:

Start with MetaModels
=====================

You should consider some basic things when you start with MetaModels.

The MetaModels project is running quite stable - nevertheless it is in constant development. In interaction with other components, such as the DC_general (DCG) or the Contao core, there may be a data loss. That's why it is highly recommended to set up a regular backup.

Checklist:

   |box| Did you install the current version of MetaModels and DCG (preferably via Composer)?
   
   |box| In Contao "System settings" activate the checkboxes "Bypass the internal cache" in the section "Global configuration" and also "Display error messages" in the section "Security settings". Subsequently purge all the caches.
   
   |box| Set up a regular backup
   
   |box| For known bugs and errors take a look on our `forum <https://community.contao.org/en/forumdisplay.php?184-MetaModels>`_ or on `Github <https://github.com/issues?user=MetaModels>`_


.. |box| raw:: html

   <span>&#9634;</span>



