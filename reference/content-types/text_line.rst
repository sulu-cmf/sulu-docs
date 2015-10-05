Text line
=========

Description
-----------

Shows a simple text line, the inserted content will be saved as simple string.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - headline
      - boolean
      - If true the height and font size of the text line get increased.

Example
-------

.. code-block:: xml

    <property name="title" type="text_line">
        <meta>
            <title lang="en">Title</title>
        </meta>
        <params>
            <param name="headline" value="true"/>
        </params>
    </property>
