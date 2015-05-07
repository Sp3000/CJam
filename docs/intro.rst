Intro to CJam
=============

Getting started
---------------

There are currently two ways to run CJam code.

The easy way is to simply use the `online CJam interpreter <http://cjam.aditsu.net/>`_. This will be good for most purposes, but is not recommended if you need code to run quickly or the code generates copious amounts of output.

Alternatively, if you have Java, you can download the JAR from `SourceForge <http://sourceforge.net/projects/cjam/files/>`_ and run like ::

    java -jar cjam-someversion.jar file.cjam
    
You can also launch an interactive shell with ::

    java -cp cjam-someversion.jar net.aditsu.cjam.Shell

    
Hello, World!
-------------

Let's begin with the traditional beginner program -- something that outputs ``Hello, World!`` (`permalink <http://cjam.aditsu.net/#code=%22Hello%2C%20World!%22>`_). ::

    "Hello, World!"
    
Simple, isn't it?

CJam's main source of memory is a stack which can hold values. You can push elements onto the stack and modify the stack with operators. At the end of the program, the contents of the stack are automatically printed.

The above program simply pushes the string ``"Hello, World!"`` onto the stack, the program ends, and the contents of the stack are printed.


CJam's data types
-----------------

