Introduction to MetaModels
==========================

.. _introdution_was-ist-metamodels:

What is MetaModels?
-------------------

MetaModels is an extension for the Contao CMS that allows you to manage any kind of structured content
without programming. Whether it's a product catalog, event calendar, staff list, rental properties or
menu — anyone who needs custom data management with individual fields, filtering, sorting and
multilingual output in Contao will find a ready-made solution in MetaModels.

The decisive advantage: Instead of having a custom extension developed for each new requirement,
administrators can set up new data structures entirely in the Contao backend — without programming and
without external developers. Individual customizations can be implemented quickly and independently,
even if requirements change during ongoing operation.

MetaModels supports many field types — texts, select fields, dates, files, yes/no fields and many more —
and displays them in the frontend as lists, detail views or filtered outputs. Editors manage their content
in clear input forms, just as they know from other areas of Contao.

More details about the features can be found in the :ref:`rst_features`.

What has been created with MetaModels in practice is shown in the
`MetaModels Showcase <https://now.metamodel.me/de/showcase>`_ or the
`presentation at CK17 <https://www.e-spin.de/metamodels-vortrag-contao-konferenz-2017.html>`_.

An `active MetaModels team <https://now.metamodel.me/de/ueber-uns/team>`_ helps with implementation
through this manual or the `Contao forum <https://community.contao.org/de/forumdisplay.php?149-MetaModels>`_
and `Slack channel <https://contao.slack.com/archives/CKGEBDV60>`_.


MetaModels Compared to Other Tools
------------------------------------

MetaModels is especially the right choice when a website needs individual data areas that go beyond
simple content elements — and when this solution should be maintainable, extensible and familiar
for editors to use.

The clear strength lies in the division of work: An administrator sets up the data model once — fields,
input forms, filters and output. Afterwards, editors maintain the content independently, without needing
any technical background knowledge.

Compared to a custom-built extension, MetaModels offers a decisive advantage: Changes to structure and
output are always possible in the backend, without programming knowledge and without deployment. For a
distributable, packaged product (e.g. a commercial extension), a custom extension makes more sense.

Anyone who wants to fully exploit the possibilities of MetaModels — e.g. custom templates, complex SQL
filters or hooks — benefits from basic knowledge of HTML, SQL, Contao templating and PHP. For standard
use cases, the backend alone is sufficient.


How Do I Get Started?
---------------------

Those new to MetaModels will find the easiest entry through these steps:

**1. Installation** |br|
MetaModels is set up via the Contao Manager or Composer. Instructions can be found in the section
:ref:`manual_install`.

**2. Creating Your First MetaModel** |br|
The section ":ref:`mm_first_index`" guides you through the most important concepts using a concrete
example:

- Create a data model
- Define attributes (fields)
- Set up an input form
- Configure output in the frontend
- Create and integrate filters

**3. Checklist for Getting Started** |br|
The page with the :ref:`component_workflow` provides a compact summary of the typical steps when
creating a new MetaModel — useful as a reference for your first projects.

**Tip for getting started:** |br|
A :ref:`simple example <mm_first_index>` — such as a staff list with name, function and photo — is ideal
for trying things out. It contains the essential building blocks (attributes, input form, output,
filtering), but remains manageable enough to understand the relationships. For orientation on where
things are, keep the
:download:`"MM layout map" </_download/MM_Lageplan_e-spin-Berlin.pdf>` handy.

If you have questions, the `Contao forum <https://community.contao.org/de/forumdisplay.php?149-MetaModels>`_
and the `Slack channel #metamodels <https://contao.slack.com/archives/CKGEBDV60>`_ can help — for complex
tasks, someone from the MM team can provide advice or act as an "MM coach"; contact via
`mail@metamodels.me <mailto:mail@metamodels.me>`_.


History of MetaModels
---------------------

MetaModels started in 2012 as the "next generation" of the well-known and widely appreciated extension
'Catalog' — version 1.0 was released in May 2013.

The 'Catalog' had grown into a very complex extension over time and offered many possibilities when used
in conjunction with Contao. Unfortunately, it had become increasingly difficult over time to maintain the
code or implement new features.

From the experience gained in developing Catalog 1 and Catalog 2, it became clear that a "Catalog 3"
required a complete restart.

On this basis, a completely new extension was developed under the name "MetaModels", incorporating many
modern programming paradigms. The goal was to create an extension on a flexible and easily extensible
codebase.

With version 2.0 of MetaModels for Contao 3.x, the result of many hours of discussion about the
"best solution" and hard programming was available.

Version 2.1 followed as a migration of 2.0 for Contao 4.4. During these changes, much of the
"underpinnings" was adapted and "old habits" were dropped, many small bugs were fixed and the DB queries
were converted Symfony-like. A compilation for MM 2.2 can be :ref:`found here <new_in_mm220>`.

The further path led through :ref:`MM 2.3 for Contao 4.13 <new_in_mm230>` to :ref:`MM 2.4 for Contao
5.3 <new_in_mm240>` with more standard components from Symfony — an MM 2.5 for Contao 5.7 is in
progress.

There are already plans for MM 3.0 — :ref:`see here <planning_mm30>`.


Resources
---------

* `MetaModels project page <https://now.metamodel.me>`_
* `MetaModels on Github <https://github.com/MetaModels>`_
* `MetaModels manual on Github <https://github.com/MetaModels/docs-de>`_
* `MetaModels Contao Community Subforum <https://community.contao.org/de/forumdisplay.php?149-MetaModels>`_
* `MetaModels Contao Slack #metamodels <https://contao.slack.com/archives/CKGEBDV60>`_
* :download:`"MM layout map" </_download/MM_Lageplan_e-spin-Berlin.pdf>`


.. |br| raw:: html

   <br />
