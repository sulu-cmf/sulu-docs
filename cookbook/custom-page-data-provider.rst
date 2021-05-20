Filter pages by a property using a custom SmartContent DataProvider
===================================================================

The ``smart_content`` content type allows for the configuration of a ``provider`` param, which is used for gathering
the items that are passed to the twig template. As described in :doc:`/reference/content-types/smart_content`, Sulu
comes with a few included providers and also allows to implement new ones to load custom entities.

In some cases, it might be useful to register an additional provider, which only returns pages including a property
with a specific value. This is possible by registering a new instance of the ``PageDataProvider`` included in Sulu
that uses a custom impementation of the ``QueryBuilder``. In the following example a custom ``AuthorPageDataProvider``
is implemented, which filters the pages for a specific author:

**1. Implement a custom** ``QueryBuilder`` **class that adds conditions**

.. code-block:: php

    <?php
    // src/SmartContent/AuthorPageQueryBuilder.php

    namespace App\SmartContent;

    use Sulu\Bundle\ContactBundle\Entity\ContactRepositoryInterface;
    use Sulu\Component\Content\Compat\StructureManagerInterface;
    use Sulu\Component\Content\Extension\ExtensionManagerInterface;
    use Sulu\Component\Content\SmartContent\QueryBuilder;
    use Sulu\Component\PHPCR\SessionManager\SessionManagerInterface;

    class AuthorPageQueryBuilder extends QueryBuilder
    {
        /**
         * @var ContactRepositoryInterface
         */
        private $contactRepository;

        public function __construct(
            StructureManagerInterface $structureManager,
            ExtensionManagerInterface $extensionManager,
            SessionManagerInterface $sessionManager,
            $languageNamespace,
            ContactRepositoryInterface $contactRepository
        )
        {
            parent::__construct($structureManager, $extensionManager, $sessionManager, $languageNamespace);

            $this->contactRepository = $contactRepository;
        }

        protected function buildWhere($webspaceKey, $locale)
        {
            // reuse existing functionality of the default QueryBuilder implementation
            $sql2Where = explode(' AND ', parent::buildWhere($webspaceKey, $locale));

            // Find authors by firstName and lastName
            $authors = $this->contactRepository->findBy(['firstName' => 'Adam', 'lastName' => 'Ministrator']);
            // Get the first author
            $author = reset($authors);

            // add custom condition to check the author of the page to the query
            $sql2Where[] = "(page.[i18n:" . $locale . "-author] = " . $author->getId() . ")";

            // optional: add custom condition to check the value of a specific property of the page to the query
            // $sql2Where[] = "(page.[i18n:" . $locale . "-propertyName] = 'propertyValue')";

            return implode(' AND ', $sql2Where);
        }
    }

**2. Register** ``PageDataProvider`` **service that uses custom** ``QueryBuilder`` **implementation**

.. code-block:: yaml

    # config/services.yaml
    services:
        app.smart_content.author_page_query_builder:
            class: App\SmartContent\AuthorPageQueryBuilder
            arguments:
                - '@sulu.content.structure_manager'
                - '@sulu_page.extension.manager'
                - '@sulu.phpcr.session'
                - '%sulu.content.language.namespace%'
                - '@sulu.repository.contact'

        app.smart_content.data_provider.author_pages:
            class: Sulu\Component\Content\SmartContent\PageDataProvider
            arguments:
                - '@app.smart_content.author_page_query_builder'
                - '@sulu.content.query_executor'
                - '@sulu_document_manager.document_manager'
                - '@sulu_page.smart_content.data_provider.content.proxy_factory'
                - '@sulu_document_manager.default_session'
                - '@sulu_page.reference_store.content'
                - '%sulu_document_manager.show_drafts%'
            tags:
                - { name: 'sulu.smart_content.data_provider', alias: 'author_pages' }

**3. Use registered** ``author_pages`` **DataProvider in the template**

.. code-block:: xml

    <property name="authorPages" type="smart_content">
        <meta>
            <title lang="en">Author Pages</title>
        </meta>

        <params>
            <param name="provider" value="author_pages"/>
        </params>
    </property>
