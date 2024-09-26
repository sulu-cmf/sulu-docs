Imagine adapter
===============

The rendering engine for media is able to use different imagine adapters. By default the bundle detects the installed
adapters and uses the most mature implementation by default.

Available adapters are:

* `gd`_
* `imagick_extension`_
* `vips_extension`_

.. note::

    You may require to install on your operation system ``libjpeg-dev``, ``libpng-dev``, ``libwebp-dev``, ``libavif-dev``, ...
    or other libraries to support the range of image mime types you want to support. Linux distributions like
    Alpine may are not shipping them in the base image.

GD
--

The simplest adapter is GD and it has no external dependencies besides the PHP extension.

**Docker:**

To use the GD extension, you need to install it with docker-php-ext-install. However, GD requires Linux libraries to function properly. Depending on the type of file you want to generate (such as JPG, PNG, or WebP), you also need to install the corresponding libraries.

..code-block:: bash

    RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libwebp-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
        && docker-php-ext-install -j$(nproc) gd \

Imagick
-------

This extension uses imagemagick to render the images.

To install it use following commands:

**Mac:**

.. code-block:: bash

    brew install imagemagick
    pecl install imagick

**Linux:**

.. code-block:: bash

    apt-get install libmagickwand-dev
    apt-get install php8.3-imagick

VIPS
----

This extension uses vips to render the images.

To install it use following commands:

**Mac:**

.. code-block:: bash

    brew install pkg-config
    brew install vips
    pecl install vips
    composer require rokka/imagine-vips

**Linux:**

.. code-block:: bash

    apt-get install libvips-dev
    pecl install vips
    composer require rokka/imagine-vips

.. _gd: http://php.net/manual/en/book.image.php
.. _imagick_extension: http://php.net/manual/en/book.imagick.php
.. _vips_extension: https://github.com/libvips/php-vips-ext
