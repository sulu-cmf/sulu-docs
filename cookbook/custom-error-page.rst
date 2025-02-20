Custom error page
=================

With Sulu it is very easy to customize the error pages for your website users.
You can define a template for each HTTP status code.

Configuration
-------------

The following code block from the webspace configuration file shows a default
configuration for the exception templates. If you want to add your own exception
for example 404 you can simply add it to the list. You can specify that for
each theme.

.. code-block:: xml

    <templates>
        <template type="error">error/error</template>
        <template type="error-404">error/error-404</template>
    </templates>

The `ErrorController` uses the status code of the response to determine
which template is responsible for the exception. If no special template is
defined it uses the template without an error code.

Twig Template
-------------

In the twig template you can use your website base template to reuse your
style.

.. code-block:: html

	{% extends "base.html.twig" %}

	{% block title %}Error {{ status_code }}{% endblock %}

	{% block content %}
	    <h1>Error {{ status_code }}</h1>
	    <p>{{ status_text }}</p>

	    <p>{{ exception.message }}</p>
	{% endblock %}

.. warning::

    Be careful which variables you use in your `base.html.twig`. If you use variables
    which are not defined in the error template, the error page cannot be rendered.

The following variables are available inside the exception template.

+---------------------------------+------------------------------------------------------------------+
| Name                            | Description                                                      |
+=================================+==================================================================+
| `status_code`                   | http-status-code                                                 |
+---------------------------------+------------------------------------------------------------------+
| `status_text`                   | http-status-code message                                         |
+---------------------------------+------------------------------------------------------------------+
| `exception`                     | complete exception object                                        |
+---------------------------------+------------------------------------------------------------------+
| `urls`                          | localized urls to start page (e.g. for language-switcher)        |
+---------------------------------+------------------------------------------------------------------+
| `request.webspaceKey`           | key of the current webspace                                      |
+---------------------------------+------------------------------------------------------------------+
| `request.defaultLocale`         | default locale of current portal                                 |
+---------------------------------+------------------------------------------------------------------+
| `request.locale`                | current locale                                                   |
+---------------------------------+------------------------------------------------------------------+
| `request.portalUrl`             | url of current portal                                            |
+---------------------------------+------------------------------------------------------------------+
| `request.resourceLocatorPrefix` | prefix for resourcelocators of current portal                    |
+---------------------------------+------------------------------------------------------------------+
| `request.resourcelocator`       | current resourcelocator                                          |
+---------------------------------+------------------------------------------------------------------+
| `request.get`                   | array of get parameter                                           |
+---------------------------------+------------------------------------------------------------------+
| `request.post`                  | array of post parameter                                          |
+---------------------------------+------------------------------------------------------------------+
| `request.analyticsKey`          | analytics key of current webspace                                |
+---------------------------------+------------------------------------------------------------------+

Test it
-------

To test your error pages you can use the following routes:

.. code-block:: bash

    {portal-prefix}/_error/{statusCode}

.. note::

    If you are not sure about your portal configuration you can get the routes with this
    `bin/websiteconsole debug:router | grep _error` command

Examples:

.. code-block:: bash

    example.org/ch._twig_error_test       ANY    ANY    example.org /ch/_error/{code}.{_format}
    example.org/en._twig_error_test       ANY    ANY    example.org /en/_error/{code}.{_format}
    example.org/fr._twig_error_test       ANY    ANY    example.org /fr/_error/{code}.{_format}
    example.org/de._twig_error_test       ANY    ANY    example.org /de/_error/{code}.{_format}
    example.org._twig_error_test          ANY    ANY    example.org /_error/{code}.{_format}
