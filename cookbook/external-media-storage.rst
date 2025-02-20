Store Media in an external Storage
==================================

Sulu stores its media with the `flysystem file system abstraction`_. This allows you to configure different storage backends easily.

By default this uses the local file system storage. A list of other supported storage backends and their installation instructions
can be found in the Flysystem Documentation here: https://github.com/thephpleague/flysystem-bundle/blob/3.x/docs/2-cloud-storage-providers.md

The following represents the default configuration for Sulu and can be adjusted to your needs:

.. code-block:: yaml

    # config/packages/flysystem.yaml
    flysystem:
        storages:
            default.storage:
                adapter: 'local'
                options:
                    directory: '%kernel.project_dir%/var/storage/default'

    # config/packages/sulu_media.yaml
    sulu_media:
        storage:
            flysystem_service: 'default.storage'


.. warning::

    Please check the section :ref:`what-about-image-formats` to avoid confusion how image formats are handled in Sulu,
    and why they can't be stored in the configured flysystem storage.

.. _what-about-image-formats:

What About Image Formats?
-------------------------

.. note::

    Only the original files will be uploaded to the external storage. Image Formats / Thumbnails will still be generated
    in the local directory. This is because image formats are generated in Sulu on demand, to explain in detail: when
    an image format is requested for the first time, Sulu generates the image from the original file and stores it in the public
    directory. The web server then acts as a kind of proxy. If the image is requested again, it checks the public
    directory and directly returns the previously generated image instead of regenerating it. External storages like S3,
    Google Cloud Storage or Azure Blob Storage do not support some kind of proxy or CDN functionality.

    If you want to store the image formats in an external service you have to use a CDN like Fastly, Cloudflare or others
    that support caching generated image formats for a long time. Some hosters allow to configure CDN directly on specific URLs
    in Sulu all URLs under ``/uploads/media/*`` need to be routed through a proxy or CDN. If your chosen CDN
    requires a custom Domain, you can use Symfony's CDN feature via:

    ``{{ asset(media.thumbnail['40x40']) }}``

    and `configure a CDN Domain`_ in the Symfony configuration ``framework.assets`` configuration. If you have tested your proxy or CDN and it correctly
    caches the generated images for a long time, you can disable saving the thumbnails to the local filesystem by setting
    ``sulu_media.format_cache.save_image`` to ``false`` in ``config/packages/sulu_media.yaml``, it is recommend to use an ENV variable
    to locally still use it.

    Important: Never disable the format cache unless you have set up a CDN or proxy. Otherwise, your server will
    regenerate the image format on every request, which can overwhelm your server, as image generation is resource-intensive.


.. _Configure a CDN Domain: https://symfony.com/doc/6.4/reference/configuration/framework.html#base-urls
.. _flysystem file system abstraction: https://github.com/thephpleague/flysystem
