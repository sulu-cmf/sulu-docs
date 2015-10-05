Layout
======

With the layout-hook the three-column-layout (navigation, content, sidebar) can be manipulated

Lets see an example:

.. code-block:: javascript

       layout: {
           navigation: {
               collapsed: true
           },
           content: {
               width: 'fixed',
               topSpace: false,
               leftSpace: false,
               rightSpace: true
           },
           sidebar: {
               width: 'max',
               url: '/admin/widget-groups/my-widget-group'
           }
       }

This layout-property just needs to be placed in the return block of the javascript-component
and the AdminBundle does the rest.

List of all available options:
------------------------------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - navigation
      - Object
      - contains properties affecting the navigation-column
    * - navigation.collapsed
      - Boolean
      - If true the navigation transitions to collapsed state
    * - navigation.hidden
      - Boolean
      - If true the navigation gets hidden
    * - content
      - Object
      - contains properties affecting the content-column
    * - content.width
      - String
      - ``fixed` to keep the column at a fixed width or ``max`` to make the column take the maximum of the available space
    * - content.leftSpace
      - Boolean
      - If false the content-column has no padding on the left
    * - content.rightSpace
      - Boolean
      - If false the content-column has no padding on the right
    * - content.topSpace
      - Boolean
      - If false the content-column has no padding on the top
    * - sidebar'
      - Object|Boolean
      - Contains properties affecting the sidebar-column. If false the sidebar gets hidden.
    * - sidebar.width
      - String
      - ``fixed`` to keep the column at a fixed width or ``max`` to make the column take the maximum of the available space
    * - sidebar.url
      - String
      - Url from which markup gets fetched and placed inside the sidebar
    * - extendExisting
      - Boolean
      - If true the passed configurations for navigation, column and sidebar won't be merged with the defaults. So the existing layout stays the same and only the explicitly passed properties take effect.

.. note::
    Either the sidebar-column or the content-column must have a ``fixed`` width. If the width
    of both columns is of type ``max`` the behaviour is not specified.

The defaults of the layout-hook are:

.. code-block:: javascript

    layout: {
        extendExisting: false,
        navigation: {
            collapsed: false,
            hidden: false
        },
        content: {
            width: 'fixed',
            leftSpace: true,
            rightSpace: true,
            topSpace: true
        },
        sidebar: false
    }

To get the default layout just write ``layout: {}``.
