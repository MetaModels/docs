.. _rst_cookbook_inputmask_manipulate-schema:

Customising the Schema for Attributes
=======================================

.. note:: The schema manager is implemented from version 2.3.

Database properties for attributes are manipulated via the schema manager —
see :ref:`component_schema-manager` for more details — and not via DCA
customisation. Changes are checked and applied during the database migration,
which can be triggered via the Contao Manager or the command line.

The following example changes the field for the long text attribute `vita` in
the model `mm_employees` from `TEXT` (65535) to `MEDIUMTEXT` (16777215) — see
`Doctrine <https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/types.html#mapping-matrix>`_ and
`Github <https://github.com/doctrine/dbal/blob/369ab24fc865939ff451c5214742cebac052f2f1/src/Platforms/AbstractMySQLPlatform.php#L40-L46>`_.

.. code-block:: php
   :linenos:

   <?php
   // src/SchemaManager/SchemaManager.php
   namespace App\SchemaManager;

   use Doctrine\DBAL\Platforms\AbstractMySQLPlatform;
   use MetaModels\Information\MetaModelCollectionInterface;
   use MetaModels\Schema\Doctrine\DoctrineSchemaGeneratorInterface;
   use MetaModels\Schema\Doctrine\DoctrineSchemaInformation;

   #[DoctrineSchemaProvider(-20)]
   final class SchemaManager implements DoctrineSchemaGeneratorInterface
   {
       public function generate(DoctrineSchemaInformation $schema, MetaModelCollectionInterface $collection): void
       {
           if (!$schema->getSchema()->hasTable('mm_employees')) {
               return;
           }

           $table = $schema->getSchema()->getTable('mm_employees');

           $table->getColumn('vita')->setLength(AbstractMySQLPlatform::LENGTH_LIMIT_MEDIUMTEXT);
       }
   }

If you cannot or do not want to use the ``DoctrineSchemaProvider`` attribute for
registration, an alternative is to register via ``services.yml``.

.. code-block:: yml
   :linenos:

   # config/services.yml
   services:
     App\SchemaManager\SchemaManager:
       tags:
         - { name: 'metamodels.schema-generator.doctrine', priority: -20 }

To verify that your own schema manager has been registered and loaded, use the
console:

``php vendor/bin/contao-console debug:container``

More information on ":ref:`rst_cookbook_specials_register-services`".

.. |br| raw:: html

   <br />
