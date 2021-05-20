How to manage analytics?
========================

Sulu gives the content-manger an easy way to manage analytic-codes and appends
them automatically to the website output without any changes in the
twig-template. You can find the list of analytics under the webspace section.

The analytics consist of:

.. list-table::
    :header-rows: 1

    * - Title
      - To identify it.
    * - Domains
      - On which domain this analytics should be appended.
    * - All Domains
      - Should it appended to all domains.
    * - Type
      - The type (google, google_tag_manager, matomo, custom).
    * - Content
      - The code or key of the analytic.

Sulu can handle different types of analytic-systems like google or matomo.
This codes will be automatically added with the given key and site-id (for
matomo). To add other systems simply choose type custom and copy and paste
the code into the textarea.

.. warning::

    Be aware that custom analytics will not be evaluated and appended without
    validation - therefore it could break the website directly after saving.

Override analytics template
---------------------------

You are able to override the analytics template with the
`symfony template overriding mechanism <http://symfony.com/doc/current/book/templating.html#overriding-bundle-templates>`_.

There are three relevant templates folders:

* ``SuluWebsiteBundle:Analytics/type/google``
* ``SuluWebsiteBundle:Analytics/type/google_tag_manager``
* ``SuluWebsiteBundle:Analytics/type/matomo``
* ``SuluWebsiteBundle:Analytics/type/custom``

Each of these folders can contain multiple templates according to the desired
position of its content:

* ``body-open.html.twig``
* ``body-close.html.twig``
* ``head-open.html.twig``
* ``head-close.html.twig``

You can access the following information in the twig variable ``analytics``.

.. list-table::
    :header-rows: 1

    * - Name
      - Type
      - Description
    * - id
      - int
      - A unique identifier of the analytics.
    * - title
      - string
      - The title of the analytics.
    * - allDomains
      - boolean
      - Indicates whether the analytics is on all domains or only a specific one.
    * - content
      - mixed
      - Differs for the type.
    * - type
      - string
      - google / google_tag_manager / matomo / custom
    * - domains
      - array
      - Array of associated domains.

.. note::

    The ``content`` property contains for type google / google_tag_manager the key,
    for matomo an associated array of ``url`` and ``siteId`` and for the custom type
    the whole script (except the ``<script>`` tag).
