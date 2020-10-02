Color
=====

Description
-----------

Shows a text line with an attached color picker, the inserted content will be
saved as simple string.

Parameters
----------

No parameters available

Example
-------

.. code-block:: xml

    <property name="color" type="color">
        <meta>
            <title lang="en">Color</title>
        </meta>
    </property>

Twig
----

.. code-block:: twig

    <div style="background-color: {{ content.color }};">
        Block with specified background color
    </div>
