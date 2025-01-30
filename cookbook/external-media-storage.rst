Store Media in an external Storage
==================================

Sulu is able to upload newly created media files directly to an external storage provider (such as AWS S3 or
Google Cloud Storage).

.. warning::

    Please check the note about image formats / thumbnails at the end of this document.

AWS-S3
------

First install dependencies.

.. code-block:: bash

    composer require "league/flysystem:^1.0" "league/flysystem-aws-s3-v3:^1.0.1"

Configure the storage with following yaml-snippet:

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

Google Cloud-Storage
--------------------

First follow this the `Google Cloud Documentation`_ to setup a System-Account and download the json-key.

.. code-block:: json

    {
        "type": "service_account",
        "project_id": "project-id",
        "private_key_id": "some_number",
        "private_key": "-----BEGIN PRIVATE KEY-----\n....
        =\n-----END PRIVATE KEY-----\n",
        "client_email": "<api-name>api@project-id.iam.gserviceaccount.com",
        "client_id": "...",
        "auth_uri": "https://accounts.google.com/o/oauth2/auth",
        "token_uri": "https://accounts.google.com/o/oauth2/token",
        "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
        "client_x509_cert_url": "https://www.googleapis.com/...<api-name>api%40project-id.iam.gserviceaccount.com"
    }

Install the dependencies:

.. code-block:: bash

    composer require "league/flysystem:^1.0" "superbalist/flysystem-google-storage:^7.1"

Dump this file to a readable folder on your machine and configure the storage with following yaml-snippet:

.. code-block:: yaml

    sulu_media:
        storage: google_cloud
        storages:
            google_cloud:
                key_file_path: '/path/to/key.json'
                bucket_name: 'sulu-bucket'
                path_prefix: 'optional path prefix'

.. _Google Cloud Documentation: https://cloud.google.com/video-intelligence/docs/common/auth#set_up_a_service_account


Azure Blob Storage
------------------

First install dependencies.

.. code-block:: bash

    composer require "league/flysystem:^1.0" "league/flysystem-azure-blob-storage:^0.1"

Configure the storage with following yaml-snippet:

.. code-block:: yaml

    sulu_media:
        storage: azure_blob
        storages:
            azure_blob:
                connection_string: 'DefaultEndpointsProtocol=https;AccountName={YOUR_ACCOUNT_NAME};AccountKey={YOUR_ACCOUNT_KEY};'
                container_name: 'container-name'
                path_prefix: 'optional path prefix'

.. note::

    **What about image formats?**

    Only the original files will be uploaded to the external storage. Image Formats / Thumbnails will still be generated
    to the local directory. This is because image formats are generated in Sulu on request, to explain in detail: when
    an image format is first request, Sulu generates the image out of the original file and stores it in the public
    directory. The webserver is here so kind of a proxy, if the image is requested again it looks into the public
    directory and can directly return the generated image instead of regenerate it. As external storages like S3,
    Google Cloud Storage or Azure Blob Storage are not supporting some kind of proxy or CDN functionality.

    If you want to store the image formats in a external service you have to use a CDN like Fastly, Cloudflare or others
    which support caching generated image formats a long time. Some hosters allow to configure CDN directly on specific URLs
    in Sulu it would be all URLs under ``/uploads/media/*`` which needs to be behind a proxy or CDN. If your used CDN
    requires a custom Domain you can use Symfony CDN feature via:

    ``{{ asset(media.thumbnail['40x40']) }}``

    and configure a CDN Domain in the Symfony configuration ``framework.assets`` configuration. If you have tested your proxy or CDN that it correctly
    caches the generated images a longer time you can disable saving the thumbnails to the local filesystem by setting
    ``sulu_media.format_cache.save_image`` to ``false`` in ``config/packages/sulu_media.yaml``, recommend a ENV variable
    to locally still use it.

    Important, never ever disable the format cache if you have not setup a CDN or Proxy, else your server always will regenerate on every
    request the image format which will mostly crash your server as image generation takes lot of resources.
