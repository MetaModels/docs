.. _component_attribute_token:

|svg_attr_token_22| |img_token| Token (from MM 2.4)
===================================================

The "Token" attribute generates a cryptographically random, immutable string (token) when
a record is saved for the first time. Typical use cases:

* Unique order numbers or reference numbers (e.g. ``TKN-aB3xYqUK``)
* Access keys or release links in the frontend
* Internal reference IDs that must remain stable

The token is generated exactly once — when first saved, as long as the field is still empty.
Every subsequent save of the record leaves the existing token unchanged. Even a direct call
to ``setDataFor()`` does not overwrite an existing token (write-once protection at the
database level).

.. note:: The token is always unique. The "Unique values" option in the general attribute
   settings is therefore permanently active and cannot be disabled.

.. warning:: When duplicating (copying) a record in the backend, no token is carried over.
   The new item automatically receives its own new token when saved.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_token


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the token attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Token character set
     - Characters used for random generation. Possible inputs:

       * Simple characters: ``123ABC`` (each character individually)
       * Range in square brackets: ``[a-z][A-Z][0-9]``
       * Special characters directly: ``$%=``
       * Combined: ``[A-F][0-9]`` (hexadecimal)

       Default: ``[a-z][A-Z][0-9]`` (alphanumeric). If the field is left empty, this
       default also applies.
   * - Token character length
     - Number of randomly generated characters (minimum: 3, default: 8).
   * - Token prefix
     - Optional fixed text prepended to every token
       (e.g. ``TKN-`` results in ``TKN-aB3xYqUK``). |br|
       The prefix does not count towards the token length.

Examples of unique identifiers include the `German ID and passport specifications
<https://de.wikipedia.org/wiki/Ausweisnummer>`_ with the characters ``123456789CFGHJKMNPRTVWXYZ``
and a length of 26 characters, or the `Record Locator <https://en.wikipedia.org/wiki/Record_locator>`_
known from airline tickets with six characters from ``23456789ABCDEFGHJKMNPQRSTXYZ``
— 0, 1, O, I, L are omitted for better readability.

The number of possible combinations ``V`` is derived from the number of possible characters
``n`` and the token character length ``k`` — the formula is ``V = n**k``.

With the default setting ``[a-z][A-Z][0-9]``, there are 62 characters ``n`` and a character
length ``k`` of 8, resulting in approximately 218 trillion possible combinations — with
three characters, there are only 238,328.


Settings in Render Settings
-----------------------------

The token attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the token value. If no template
       is specified, the output is as plain text.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the token attribute is added to an input form, the field is fundamentally read-only
(``readonly``) in the backend. The following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form (e.g.
       ``w50`` for half width, ``clr`` for line break, ``long`` for full width).

**Overview (backend search)**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Searchable
     - The attribute is available in the backend as a search field.

.. note:: The options "Required field", "Always save", and "Filterable" are not available
   for the token attribute, as the field is always saved internally and is read-only.


Filter Rules
------------

The token attribute can be used with the following filter rules:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Filter rule
     - Note
   * - Simple lookup
     - Filters records by an exact token value; useful for URL-based access checks |br|
       (e.g. ``?token=TKN-aB3xYqUK``).
   * - Custom SQL
     - For more complex filters for combinations with additional parameters.


Special Functions
-----------------

**Configuration via config.yaml**

The maximum number of generation attempts before an exception is thrown can be adjusted
project-specifically in ``config/config.yaml``:

.. code-block:: yaml
   :linenos:

   meta_models_attribute_token:
       max_retries: 5   # Default: 3

Each attempt checks whether the generated token already exists in the database. If the
uniqueness check fails three times (or as many times as configured), a ``RuntimeException``
is thrown.

**Custom token generation via event**

Via the event ``MetaModels\AttributeTokenBundle\Event\GenerateTokenEvent``, the token
generation can be replaced or extended by custom code. If ``setToken()`` is called in the
listener, MetaModels skips the built-in random generation.

Example listener as ``src/EventListener/MyTokenListener.php``:

.. code-block:: php
   :linenos:

   <?php

   use MetaModels\AttributeTokenBundle\Event\GenerateTokenEvent;

   class MyTokenListener
   {
       public function onGenerateToken(GenerateTokenEvent $event): void
       {
           $prefix = $event->getAttribute()->get('token_prefix') ?? '';
           $event->setToken($prefix . strtoupper(bin2hex(random_bytes(8))));
       }
   }

Registration in ``config/services.yaml``:

.. code-block:: yaml
   :linenos:

   App\EventListener\MyTokenListener:
       tags:
           - name: kernel.event_listener
             event: MetaModels\AttributeTokenBundle\Event\GenerateTokenEvent
             method: onGenerateToken

The available methods of the event:

.. list-table::
   :header-rows: 1
   :widths: 40 60

   * - Method
     - Description
   * - ``getAttribute(): Token``
     - Returns the token attribute (access to character set, length, prefix).
   * - ``getItem(): IItem``
     - The MetaModel item that is currently being saved.
   * - ``setToken(string $token)``
     - Sets a custom token; prevents the built-in generation.
   * - ``getToken(): ?string``
     - Returns the token set by the listener, or ``null``.
   * - ``isTokenProvided(): bool``
     - ``true`` if a listener has already called ``setToken()``.

**Write-once protection at the database level**

Even if ``setDataFor()`` is called directly, MetaModels does not overwrite an existing
token value. The UPDATE query contains a condition ``WHERE column IS NULL OR column = ''``,
so a filled field always remains unchanged.

**Database storage**

The token is stored as ``varchar(255) NULL`` in the MetaModel table. An empty value is
stored as ``NULL`` (compatible with MySQL Strict Mode).


.. |svg_attr_token_22| image:: /_img/icons_svg/token.svg
   :width: 22px
.. |img_token| image:: /_img/icons/token.png

.. |br| raw:: html

   <br />
