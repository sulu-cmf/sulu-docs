Content Architecture
====================

Our content is heavily structured. As mentioned in :doc:`structure`, we rely on
the Symfony CMF on top of PHPCR. On this page we'll get into the Sulu Content
Architecture.


Sulu Instance
-------------

At first, you create a Sulu Instance. A Sulu instance can be seen as a **single
installation** or one pool of data. Its real live mapping could be a company or
an organization.


Webspace
--------

On your instance you'll define webspaces. Webspaces could represent your brands
and corporations. A landing page could be a single webspace.


Languages
---------

Once you created your webpsaces, you could define languages. Pages could
then be translated or mapped to another language as **shadow-** or
**ghost-pages**.


Page
----

As already heard in :doc:`backend-template` pages are configured in templates.
They are created in webspaces and represent an entry in a certain menu. These
pages are contained within your webspace. Once you got your
:doc:`setup <../getting-started>` right, you can start exploring the Sulu default
structure in the backend.


Content-Type
------------

A template is built up of several content types. A blog post, for example, could
consist of the following content types:

* Single line text (*Title*)
* Multi line text (*Infobox*)
* Richtext (*Content*)
* Date (*Event-Date*)
* List of Tags (*Tags*)

A detailed list of all the content types is covered in
:doc:`../templates`.


Assets
------

Assets are media files like images and downloads. They are shared through all
the Websites on the instance.


Snippets
--------

Snippets are very similar to assets. They are small pieces of content that could
be shown on several pages in several webspaces.


Contacts
--------

Contacts represent personal information. They are also used to manage Sulu
Users itself.


Whatever you want
-----------------

Sulu is very extensible. You got existing content you want to integrate through
DBAL? Do it. An API that delivers content? Integrate it. Other Symfony bundles
you've already coded. Integrate them.

A more detailed documentation could be found in the section
:doc:`../content-architecture`.

Now you are really deep into the concepts of Sulu. So the next step is to get
started. :doc:`../getting-started`
