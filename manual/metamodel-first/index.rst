.. _mm_first_index:

The First MetaModel
===================

Building the first MetaModel is intended to provide an easy introduction to the implementation.
The task for the first project is a simple employee list with only a few content fields. The list
should be maintainable in the backend and can be displayed as a table in the frontend. Some
aspects such as sorting, filtering, etc. have been intentionally omitted.

The implementation is based on the :ref:`component_index` — more notes on the templates used
and possible relations can also be found there. If you are unsure about the best way to start,
take a look at the :ref:`article on the workflow <component_workflow>`.

For an easier overview of where to find what, the
:download:`"MM site map" </_download/MM_Lageplan_e-spin-Berlin.pdf>` is available for download.

**Task:**

* Create an employee list that can be maintained in the backend
* Store the values: last name, first name, email, department
* Additional field for publishing a record
* Output the list as a table in the frontend

**Prerequisites:**

* Current Contao (LTS) — see :ref:`manual_install`
* Current MetaModels matching the Contao version — see :ref:`manual_install` and :ref:`rst_cookbook_checklists_mm-start`
* Confident use of Contao
* Understanding of :ref:`component_index`

.. toctree::
    :hidden:
    :maxdepth: 1

    new-mm
    attribute
    rendersettings
    dca
    searchable-pages
    filter
    dca-combine
    contentelements
    conclusion
