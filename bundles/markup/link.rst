sulu-link
=========

The sulu-link tag allows to link to pages and other entities in the application by their id.
This id of the tag will be validated and replaced by a proper anchor tag when a response is generated.

In a basic installation, the tag supports 2 different providers: ``page`` (default) and ``media``.
Additional providers can be implemented by :doc:`registering a service with the sulu.link.provider tag<../../cookbook/link-provider>`.

Example
-------

.. code-block:: html

    <sulu-link href="123-123-123" />
    <sulu-link href="123-123-123" title="test-title" />
    <sulu-link href="123-123-123" target="_blank">Link Text</sulu-link>
    <sulu-link href="123-123-123#test-anchor">Anchor Example</sulu-link>
    <sulu-link provider="page" href="123-123-123" target="_blank">Link Text</sulu-link>
    <sulu-link provider="media" href="1" title="test-title"/>
    <sulu-link provider="media" href="1">Link Text</sulu-link>

**Results into:**

.. code-block:: html

    <a href="http://example.com/test" title="Page Title">Page Title</a>
    <a href="http://example.com/test" title="test-title">Page Title</a>
    <a href="http://example.com/test" title="Page Title" target="_blank">Link Text</a>
    <a href="http://example.com/test#test-anchor" title="Page Title">Anchor Example</a>
    <a href="http://example.com/test" title="Page Title" target="_blank">Link Text</a>
    <a href="http://example.com/media/1/download/image.jpg?v=1" title="test-title">Media Title</a>
    <a href="http://example.com/media/1/download/image.jpg?v=1" title="Media Title">Link Text</a>

How the tag is processed
------------------------

The ``<sulu-link>`` tag is processed after the response content is generated and
before it is sent to the client. A parser detects all links and loads the paths of all
linked pages from the PHPCR in a *single* query.

If a twig function would be used to generate the links, each call would trigger a
request to the PHPCR which would worsen the performance.
