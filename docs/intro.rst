Intro to CJam
=============

Getting started
---------------

There are currently two ways to run CJam code.

The easy way is to simply use the `online CJam interpreter <http://cjam.aditsu.net/>`__. This will be good for most purposes, but is not recommended if you need code to run quickly or the code generates copious amounts of output.

Alternatively, if you have Java, you can download the JAR from `SourceForge <http://sourceforge.net/projects/cjam/files/>`__ and run like ::

    java -jar cjam-someversion.jar file.cjam
    
You can also launch an interactive shell with ::

    java -cp cjam-someversion.jar net.aditsu.cjam.Shell

    
Hello, World!
-------------

CJam's main source of memory is a stack which can hold values. You can push elements onto the stack and modify the stack with operators. At the end of the program, the contents of the stack are automatically printed.

``Hello, World!`` is very easy in CJam -- we can just push a string onto the stack and let automatic printing do its job (`permalink <http://cjam.aditsu.net/#code=%22Hello%2C%20World!%22>`__). ::

    "Hello, World!"

Simple!
    
Numerical calculations in CJam
------------------------------

To perform calculations in CJam, we can push integers onto the stack and apply operators to them. For example, the program (`permalink <http://cjam.aditsu.net/#code=7%208%20%2B>`__) ::

    7 8 +
    
results in the output ::

    15
    
by first pushing ``7`` and ``8`` onto the stack, then performing the ``+`` operator, which adds two integers.

Another example is ``4 2 3 * 7 - +`` (`permalink <http://cjam.aditsu.net/#code=4%202%203%20*%207%20-%20%2B>`__), which results in ``3``. Here is a trace: ::

    Instruction       What happens           Stack
    -----------       ------------           -----
    4                 Push 4                 [4]
    2                 Push 2                 [4 2]
    3                 Push 3                 [4 2 3]
    *                 Multiply top two       [4 6]
    7                 Push 7                 [4 6 7]
    -                 Subtract top two       [4 -1]
    +                 Add top two            [3]
    
In addition to integers, CJam also has doubles. For example, the program ``1.3 2.6 +`` (`permalink <http://cjam.aditsu.net/#code=1.3%202.6%20%2B>`__) results in ``3.9000000000000004``, which is close enough to the expected ``3.9`` (silly `floating point numbers! <https://en.wikipedia.org/wiki/Floating_point#Accuracy_problems>`__).

Here are examples of operators which do numerical calculations: ::

    + - * /         Plus, minus, times, and divide respectively
    ( )             Decrement and increment by one respectively
    %               Modulo
    #               Exponentiate
    < = >           Less than, equal to and greater than respectively
    & | ^ ~         Bitwise AND, OR, XOR and NOT respectively (for integers)

Note that division between two integers in CJam results in integer division, which keeps the integer part of the result. For example, ``5 2 /`` (`permalink <http://cjam.aditsu.net/#code=5%202%20%2F>`__) results in ``2``, rather than ``2.5``. To get floating point division, one of the arguments needs to be a double. For instance, both ``5.0 2 /`` and ``5 2. /`` (`permalink <http://cjam.aditsu.net/#code=5%202.%20%2F>`__) result in ``2.5``. Alternatively, one can convert a number to a double using the ``d`` operator.

Blocks and variables
--------------------

Block is a data type in CJam which represents a code block. They can be executed with the ``~`` operator, e.g. the program (`permalink <http://cjam.aditsu.net/#code=6%20%7B7*%7D%20~>`__) ::

    6 {7*} ~
    
gives the output ::

    42

You may notice that ``~`` executes a block when used on blocks, but performs bitwise not when used on integers. CJam operators are very heavily overloaded, with the same operator doing different things depending on what is at the top of the stack.

Blocks in CJam are first class objects. They can be assigned to variables, allowing them to act as functions. CJam has 26 variables, one for each uppercase letter, and a variable is assigned to via ``:<variable>``. For example, the program (`permalink <http://cjam.aditsu.net/#code=%7B7*%7D%3AF%206%20F>`__) ::

    {7*}:F 6 F

results in ::

    {7*}42
    
Note how the block is also in the output. When you assign something to a variable, it is not popped from the stack, so here the block gets outputted along with the ``42`` when the stack is automatically printed.