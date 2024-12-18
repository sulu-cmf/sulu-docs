Store Media in an external Storage
==================================

Sulu stores its media with the `flysystem file system abstraction`_. This allows you to configure different storage backends easily.

By default this uses the local file system storage. A list of other supported storage backends and their installation instructions
can be found here: https://github.com/thephpleague/flysystem-bundle/blob/3.x/docs/2-cloud-storage-providers.md

If you want to set up multiple storage backends you have to configure which of those to use for sulu media which can be done like this:

.. code-block:: yaml

    sulu_media:
        storage: s3
        storages:
            s3:
                key: 'your aws s3 key'
                secret: 'your aws s3 secret'
                bucket_name: 'your aws s3 bucket name'
                path_prefix: 'optional path prefix'
                region: 'eu-west-1'

If you use s3 compatible services (e.g. minio) you can pass additional ``arguments`` and ``endpoint`` to the
configuration.

.. _flysystem file system abstraction: https://github.com/thephpleague/flysystem
