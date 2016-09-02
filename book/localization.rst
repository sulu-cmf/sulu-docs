Adding localizations
====================

Sulu is built for companies with an international focus,
translating pages into multiple different languages is a very important task
for a content editor using Sulu. Sulu also considers the different variations
of a language among different countries. The combination of these two factors
is called a localization.

Configuring webspace localizations
----------------------------------

Localizations for the content are configured in the webspaces, as already
described in :doc:`webspaces`. Adding another localization is as easy as
adding another ``localization`` tag to the webspace configuration file.
Localizations can also be nested, which has no impact on the representation in
all the dropdowns, but it will help the system to find better fallbacks.

So a good example using english and german as a language might look something
like the following fragment.

.. code-block:: xml

    <localizations>
        <localization language="en">
            <localization language="en" country="us"/>
            <localization language="en" country="gb"/>
        </localization>
        <localization language="de">
            <localization language="de" country="de"/>
            <localization language="de" country="at"/>
            <localization language="de" country="ch"/>
        </localization>
    </localizations>

With this configuration the system will contain seven different content
localizations: ``en``, ``en-us``, ``en-gb``, ``de``, ``de-de``, ``de-at``,
``de-ch``, whereby ``en-us`` and ``en-gb`` are falling back to ``en``, and
``de-de``, ``de-at`` and ``de-ch`` are falling back to ``de``.

Adding custom localizations
---------------------------

There is another possibility for adding non webspace related localizations. More details can be found in
:doc:`../cookbook/localization-provider`

Usage of localizations
----------------------

For the developer the only touching points with localizations are the
configuration and the eventual use of a language switcher on the homepage.
For the language switcher the ``urls`` variable delivered to the twig template
can be used, which contains an associative array with the localization code
being the code and the value the URL to the current page in this language.

The template itself does not have to be adapted for usage with multiple
localizations. The twig variables are the same for every language, only the
content is different, and this is handled by Sulu for the developer.

The nesting of the localizations is very important for the column navigation of
the content. In case there is a ghost page - which means that the page is not
translated into the current localization - this tree will be used to determine
the "closest" language available.

