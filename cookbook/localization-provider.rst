How to add localizations with the localization provider?
========================================================

If you are creating a bundle that has its own localizations they should be registered in the Sulu system.
Otherwise you won't be able to use Sulu's security features, for example.

It's possible to add locales by simple defining a service with the LocalizationProvider and pass your custom
locales as arguments.

Example
-------

.. code-block:: xml

    <service id="sulu_product.localization_provider" class="Sulu\Component\Localization\Provider\LocalizationProvider">
        <argument>%sulu_product.locales%</argument>

        <tag name="sulu.localization_provider"/>
    </service>
