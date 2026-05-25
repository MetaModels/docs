.. _ref_api:

MetaModels Reference and API
=============================

.. warning:: Still under construction!

The MetaModels API forms the interface for custom programming and extension.


.. _ref_api_interf:
Interfaces MetaModels
---------------------

The MetaModels API provides interfaces for accessing various classes as
"`Interfaces <http://php.net/manual/de/language.oop5.interfaces.php>`_".

The interfaces can be used, for example, in custom programming or functions in
events/hooks or in templates. Using the interfaces, various data or properties
can be easily retrieved or manipulated.

The following groups of interfaces are available:

.. _index_api_interfaces:

.. toctree::
    :maxdepth: 1

    interfaces/metamodels
    interfaces/attribute
    interfaces/filter
    interfaces/dcgeneral-datadefinition

The documentation for each group includes basic examples. Additional examples can be found in the
":ref:`Cookbook <rst_cookbook>`",
`talk by Ingolf Steinhardt at CK23 <https://www.e-spin.de/contao-metamodels/metamodels-vortrag-contao-konferenz-2023.html>`_ and
":ref:`rst_cookbook_specials_register-services`".


.. _ref_api_dcg:
DC_General (DCG)
----------------

DC_General handles display and data processing in the backend and partially for frontend editing
(FEE). More on `DC_General <https://dc-general.readthedocs.io>`_.
