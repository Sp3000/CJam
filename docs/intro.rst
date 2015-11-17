Tutorial #1: Introduction to CJam
=================================

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

To perform calculations in CJam, we can push integers onto the stack and apply operators to them. For example, the program ::

    7 8 +
    
results in the output (`permalink <http://cjam.aditsu.net/#code=7%208%20%2B>`__) ::

    15
    
by first pushing ``7`` and ``8`` onto the stack, then performing the ``+`` operator, which adds two integers.

Another example is ``4 2 3 * 7 - +``, which results in ``3`` (`permalink <http://cjam.aditsu.net/#code=4%202%203%20*%207%20-%20%2B>`__). Here is a trace: ::

    Instruction       Function               Stack
    -----------       ------------           -----
    4                 Push 4                 [4]
    2                 Push 2                 [4 2]
    3                 Push 3                 [4 2 3]
    *                 Multiply top two       [4 6]
    7                 Push 7                 [4 6 7]
    -                 Subtract top two       [4 -1]
    +                 Add top two            [3]
    
For debugging, you can insert the two-char operator ``ed`` to print the current stack. For example, ``4 2 3 * 7 ed - +`` (`permalink <http://cjam.aditsu.net/#code=4%202%203%20*%207%20ed%20-%20%2B>`__) prints the state of the stack after the ``7`` is pushed.
    
In addition to integers, CJam also has doubles. For example, the program ``1.3 2.6 +`` results in ``3.9000000000000004`` (`permalink <http://cjam.aditsu.net/#code=1.3%202.6%20%2B>`__), which is close enough to the expected ``3.9`` (silly `floating point numbers <https://en.wikipedia.org/wiki/Floating_point#Accuracy_problems>`__!).

Here are examples of operators which do numerical calculations: ::

    + - * /         Add, subtract, multiply and divide respectively
    ( )             Decrement and increment by one respectively
    %               Modulo
    #               Exponentiate/power
    & | ^ ~         Bitwise AND, OR, XOR and NOT respectively (for integers)

Note that division between two integers in CJam results in integer division, which keeps the integer part of the result. For example, ``5 2 /`` results in ``2``, rather than ``2.5`` (`permalink <http://cjam.aditsu.net/#code=5%202%20%2F>`__). To get floating point division, one of the arguments needs to be a double, for instance both ``5.0 2 /`` and ``5 2. /`` result in ``2.5`` (`permalink <http://cjam.aditsu.net/#code=5%202.%20%2F>`__).

Blocks and variables
--------------------

Block is a data type in CJam which represents a code block. They can be executed with the ``~`` operator, e.g. the program ::

    6 {7*} ~
    
gives the output (`permalink <http://cjam.aditsu.net/#code=6%20%7B7*%7D%20~>`__) ::

    42

You may notice that ``~`` executes a block when used on blocks, but performs bitwise not when used on integers. CJam operators are very heavily overloaded, with the same operator doing different things depending on what is at the top of the stack.

Blocks in CJam are first class objects. They can be assigned to variables, allowing them to act as functions. CJam has 26 variables, one for each uppercase letter, and a variable is assigned to via ``:<variable>``. For example, the program ::

    {7*}:F 6 F

results in (`permalink <http://cjam.aditsu.net/#code=%7B7*%7D%3AF%206%20F>`__) ::

    {7*}42
    
Note how the block is also in the output. When you assign something to a variable, it is not popped from the stack, so here the block gets outputted along with the ``42`` when the stack is automatically printed.

Of course, you can also assign numbers and strings to variables, which just get pushed onto the stack when "used". For instance, ::

    "HOUSE":A "BOAT":B A B B B A
    
gives the output (`permalink <http://cjam.aditsu.net/#code=%22HOUSE%22%3AA%20%22BOAT%22%3AB%20A%20B%20B%20B%20A>`__) ::

    HOUSEBOATHOUSEBOATBOATBOATHOUSE

Each of CJam's variables is preinitialised with a value, the full list of which can be found `here <http://sourceforge.net/p/cjam/wiki/Variables/>`__.

Arrays and characters
---------------------

An array is just a collection of items, for instance ::

    [1 2 3 "foo"]
    
is an array of four things -- three integers and a string. You can get the length of an array with the ``,`` operator, so ``[1 2 3 "foo"],`` would result in ``4`` (`permalink <http://cjam.aditsu.net/#code=%5B1%202%203%20%22foo%22%5D%2C>`__).

Funnily enough, there is no special literal for general arrays -- although arrays can be formed by surrounding the items with square brackets ``[]``, in actuality ``[`` and ``]`` are both *operators* which start and end an array respectively. This lets you do things like ::

    1 2 3 "foo"]
    
which also creates an array of the same four items, but instead works because the ``]`` here wraps the *whole stack* into an array.

In CJam, strings are actually a special case of arrays -- CJam strings are just arrays of characters. Aside from integers, doubles, blocks and arrays, the character is CJam's fifth and final data type. Character literals take the form ``'<character>``, i.e. a single quote/apostrophe followed by the character. For example ``5 '$`` prints ``5$`` (`permalink <http://cjam.aditsu.net/#code=5%20'%24>`__).

There is no escape character for characters, so ``''`` is actually just the single quote character. There *is*, however, an escape character for strings -- the backslash ``\``, like most major languages. But unlike most languages, backslashes only escape double quotes and other backslashes in CJam, with every other character preserved as-is. In particular, newlines are perfectly okay in strings, e.g. ::

    "This is a double quote: \"
    Backslash followed by 'n' is not special: \n"
    
results in (`permalink <http://cjam.aditsu.net/#code=%22This%20is%20a%20double%20quote%3A%20%5C%22%0ABackslash%20followed%20by%20'n'%20is%20not%20special%3A%20%5Cn%22>`__) ::

    This is a double quote: "
    Backslash followed by 'n' is not special: \n

Converting types
----------------

Now that we've seen examples of each CJam data type, here is a summary of operators which convert between types: ::

    `       String representation
    c       Convert to character
    d       Convert to double
    i       Convert to integer
    s       Convert to string
    ~       Evaluate string/block

Revisiting the ``5 2 /`` example from earlier, if we already had 5 and 2 on the stack and wanted float division, this means that we can perform ``d`` to convert the 2 to a double before dividing, i.e. (`permalink <http://cjam.aditsu.net/#code=5%202%20d%20%2F>`__) ::

    5 2 d /    ->    2.5

Anothere note is that backtick ````` and ``s`` differ primarily in how arrays are turned into strings. For example, ``[1 2 3] ``` results in the string ``"[1 2 3]"`` while ``[1 2 3] s`` results in ``"123"``.

Input and output
----------------

In CJam, there are three operators for getting input via STDIN: ::

    l       Read line
    q       Read all input
    r       Read token (whitespace-separated)
    
For instance, ::

    l i 2 *

is a simple program which doubles an input integer (`permalink <http://cjam.aditsu.net/#code=l%20i%202%20*&input=6>`__). ``ri2*`` or ``qi2*`` would work just as well here (whitespace is typically unnecessary except between numeric literals).

There is also a fourth operator, ``ea``, which pushes arguments from command-line onto the stack as an array. This feature is only available in the offline interpreter.

As for output, aside from the automatic printing upon program termination, CJam also has two specific operators for output: ::

    o       Print value
    p       Print string representation and newline
    
Stack manipulation
------------------

Much of CJam's power as a golfing language comes from its stack manipulation operators, which reduce the need to assign to variables. Here are some of these operators ::

    Operator      Function                     Stack after [1 2 3 4]
    --------      --------                     ---------------------
    <n> $         Copy top nth from stack      [1 2 3 4 3] (for 1$)
                                               [1 2 3 4 2] (for 2$)
    ;             Pop and discard              [1 2 3]
    \             Swap top two                 [1 2 4 3]
    @             Rotate top three             [1 3 4 2]
    _             Duplicate                    [1 2 3 4 4]

Note that ``$`` starts counting from ``0``, so ``0$`` is the same as ``_``. It also allows for negative numbers, e.g. ``-1$`` copies the bottom of the stack.

The easy way to remember which way ``@`` rotates is that since ``\`` moves the second element from the top to the top, it's more useful for ``@`` to move the third element from the top to the top.

Example program: Distance calculator
------------------------------------

Let's take a look at how we might write a program which takes in 4 numbers ``x1 y1 x2 y2`` and outputs the Euclidean distance between ``(x1, y1)`` and ``(x2, y2)``, i.e. ::

    sqrt((x1 - x2)^2 + (y1 - y2)^2)
    
For example, the input ``3 7 4 5`` would output ``sqrt(5) ~ 2.23``.

First, we need to read in and convert the input, for which we can use ``l~``. ``~`` evaluates the whole string, leaving the stack like ::

    [x1 y1 x2 y2]

We can then move ``y1`` to the top with ``@``, bringing the ``y`` s together, after which we subtract with ``-`` and square with ``_*`` (duplicate and multiply). This gives: ::

    [x1 x2 (y2-y1)^2]

As a side comment, although it appears it could, ``2#`` (power of 2) can't be used in place of ``_*`` here due to the preceding ``-``, which together is parsed as ``-2 #`` (power of -2). Adding a space in between like ``- 2#`` would work, but it is better to use the ``m`` operator, which is designed to act as subtraction when followed by a numeric literal (i.e. ``m2#`` can be used instead of ``-_*``).

Moving on, we can move the top of the stack to the bottom by rotating twice with ``@@``, giving: ::

    [(y2-y1)^2 x1 x2]
    
Then we subtract and square again with ``-_*``: ::

    [(y2-y1)^2 (x1-x2)^2]
    
Finally, we add and square root with ``+.5#``, thus leaving the final stack as ::

    [((y2-y1)^2 + (x1-x2)^2)^.5]
    
Altogether, this gives the program (`permalink <http://cjam.aditsu.net/#code=l~%40-_*%40%40-_*%2B.5%23&input=3%207%204%205>`__) ::

    l~@-_*@@-_*+.5#
    
Considering we only used the basic operators here, this is already a fairly short program. However, CJam actually has a builtin hypotenuse function ``mh``, which is another two-char operator like ``ed`` (``e`` is for *extended* operators, while ``m`` is for *mathematical* operators).  Using this, we can make the program even shorter with (`permalink <http://cjam.aditsu.net/#code=l~%40-%40%40-mh&input=3%207%204%205>`__)::

    l~@-@@-mh

    