Tutorial #2: Basic array manipulations
======================================

Now that we've had a quick introduction, we'll be taking a look at the core features which make CJam such an expressive language, starting with the ever-versatile array.

Wrapping and unwrapping
-----------------------

We've seen that arrays can be created with ``[`` and ``]``, which are actually operators. Additionally, there is an ``a`` command, which wraps the top element of the stack into an array (`permalink <http://cjam.aditsu.net/#code=2%20a%20p>`__). ::

    2 a    ->    [2]

Arrays can be unwrapped using ``~``, pushing its elements onto the stack (`permalink <http://cjam.aditsu.net/#code=%22another%20element%22%20%5B1%202%203%5D%20ed%20~%20ed>`__): ::

    [1 2 3] ~    ->    1 2 3

Note that this doesn't work for strings, i.e. arrays consisting of only characters, for which ``~`` is eval. If dumping a string's characters to the stack is desired, then an empty for-each loop ``{}/`` can be used instead (we'll look more closely at blocks in a later tutorial).

Concatenation
-------------

Two arrays can be concatenated with ``+`` (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%5D%20%5B8%205%207%5D%20%2B%20p%0A%22house%22%20%22boat%22%20%20%2B%20p>`__): ::

    [1 4 2] [8 5 7] +    ->    [1 4 2 8 5 7]
    "house" "boat"  +    ->    "houseboat"

Concatenating a non-array element is equivalent to concatenating a one-element array, negating the need to use ``a`` (`permalink <http://cjam.aditsu.net/#code=%5B1%202%203%5D%204%20%20%2B%20p%0A%5B1%202%203%5D%204a%20%2B%20p%0A4%20%5B1%202%203%5D%20%20%2B%20p%0A4a%20%5B1%202%203%5D%20%2B%20p>`__): ::

    [1 2 3] 4  +    ->    [1 2 3 4]
    [1 2 3] 4a +    ->    [1 2 3 4]
    4 [1 2 3]  +    ->    [4 1 2 3]
    4a [1 2 3] +    ->    [4 1 2 3]

However, it is important to remember than strings are arrays, or else there might be some unexpected results (`permalink <http://cjam.aditsu.net/#code=%5B1%202%203%5D%20%22four%22%20%20%2B%20p%0A%5B1%202%203%5D%20%22four%22a%20%2B%20p>`__): ::

    [1 2 3] "four"  +    ->    [1 2 3 'f 'o 'u 'r]
    [1 2 3] "four"a +    ->    [1 2 3 "four"]

Repetition
----------

Given an integer ``n`` and an array, ``*`` repeats the array ``n`` times (`permalink <http://cjam.aditsu.net/#code=%5B1%202%203%5D%205%20*%20p%0A%22ha%22%20%20%20%202%20*%20p>`__): ::

    [1 2 3] 5 *    ->    [1 2 3 1 2 3 1 2 3 1 2 3 1 2 3]
    "ha"    2 *    ->    "haha"

Array length and ranges
-----------------------

Recall that ``,`` on an array gives the length of an array, so ::

    ["this" "array" "has" "five" "elements"] ,

results in ``5`` (`permalink <http://cjam.aditsu.net/#code=%5B%22this%22%20%22array%22%20%22has%22%20%22five%22%20%22elements%22%5D%20%2C>`__). ``,`` on an integer *creates* an array, namely it is a range operator which produces ``[0 1 ... n-1]``: (`permalink <http://cjam.aditsu.net/#code=5%2C%20p>`__)::

    5,    ->    [0 1 2 3 4]

``,`` also acts as a range operator on characters, where it creates a string instead. For instance, ``'a,`` results in the first 97 ASCII characters, including the initial unprintables (`permalink <http://cjam.aditsu.net/#code='a%2C>`__).

Getting and setting
-------------------

Getting an element at a specific index of an array can be done with ``=``. A nice feature is that out-of-bounds indices wrap around. (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%20%20%20%20%200%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20%203%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20%205%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20%206%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20-2%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20-1000%20%3D%20%20p>`__) ::

    [1 4 2 8 5 7]     0 =      ->   1
    [1 4 2 8 5 7]     5 =      ->   7
    [1 4 2 8 5 7]     6 =      ->   1
    [1 4 2 8 5 7]    -2 =      ->   5
    [1 4 2 8 5 7] -1000 =      ->   2

``=`` can also take a block, returning the first element for which the given condition is true. For example, the following returns the first positive integer in the array (`permalink <http://cjam.aditsu.net/#code=%5B0%20-1%202%20-3%200%204%5D%20%7B0%3E%7D%20%3D>`__): ::

    [0 -1 2 -3 0 4] {0>} =    ->    2

Setting a position in an array is done with ``t``, with the index followed by the element (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%200%20-3%20t%20p>`__): ::
    
	[1 4 2 8 5 7]  0 -3 t      ->   [-3 4 2 8 5 7]
	
Similarly to ``=``, ``t`` also wraps around for out-of-bounds indices. However, unlike ``=``, ``t`` doesn't take a block.

Slices
------

Array slicing is done with ``>`` and ``<`` (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%202%20%3C%20p%0A%5B1%204%202%208%205%207%5D%202%20%3E%20p%0A%5B1%204%202%208%205%207%5D%20-3%20%3C%20p%0A%5B1%204%202%208%205%207%5D%20-3%20%3E%20p>`__): ::

    [1 4 2 8 5 7]  2 <     ->    [1 4]
    [1 4 2 8 5 7]  2 >     ->    [2 8 5 7]
    [1 4 2 8 5 7] -3 <     ->    [1 4 2]
    [1 4 2 8 5 7] -3 >     ->    [8 5 7]

For example, one way of getting the lowercase letters is ``'{,97>`` (`permalink <http://cjam.aditsu.net/#code='%7B%2C97%3E>`__).

Getting every nth element can be done with ``%`` (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%20%202%20%25%20p%0A%5B1%204%202%208%205%207%5D%20%203%20%25%20p%0A%5B1%204%202%208%205%207%5D%20%20W%20%25%20p%20%20%20%20e%23%20%20W%20is%20-1%0A%5B1%204%202%208%205%207%5D%20-2%20%25%20p>`__): ::

    [1 4 2 8 5 7]  2 %    ->    [1 2 5]
    [1 4 2 8 5 7]  3 %    ->    [1 8]
    [1 4 2 8 5 7]  W %    ->    [7 5 8 2 4 1]
    [1 4 2 8 5 7] -2 %    ->    [7 8 4]

For negative numbers the step is backwards, so ``W%`` is a common idiom for reversing an array, as ``W`` is preinitialised to -1.


Uncons
------

Sometimes you want to just pop from an array, removing the first or last element while leaving the rest of the array intact. This can be done with ``(`` and ``)`` respectively (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%20(%20ed%20%3B%3B%0A%5B1%204%202%208%205%207%5D%20)%20ed%20%3B%3B>`__): ::

    [1 4 2 8 5 7]  (    ->    [4 2 8 5 7] 1
    [1 4 2 8 5 7]  )    ->    [1 4 2 8 5] 7
	
Sort
----

Sorting can be done with ``$`` (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%20%24%20p>`__): ::

    [1 4 2 8 5 7]  $    ->    [1 2 4 5 7 8]
	
If two elements aren't comparable, an error is sensibly thrown.

``$`` can optionally take an additional block argument which determines what key to sort by. For example, ``$`` on its own sorts strings by ASCII values, while ``{el} $`` sorts strings by their lowercase counterparts, giving a case-insensitive search. Compare (`permalink <http://cjam.aditsu.net/#code=%5B%22Bee%22%20%22candy%22%20%22Cake%22%20%22apple%22%5D%20%20%20%20%20%20%24%20p%0A%5B%22Bee%22%20%22candy%22%20%22Cake%22%20%22apple%22%5D%20%7Bel%7D%20%24%20p>`__): ::

    ["Bee" "candy" "Cake" "apple"]      $   ->   ["Bee" "Cake" "apple" "candy"]
    ["Bee" "candy" "Cake" "apple"] {el} $   ->   ["apple" "Bee" "Cake" "candy"]

``el`` and ``eu`` convert strings to lowercase and uppercase respectively.

Another example is ``{3-z} $``, which sorts an array of numbers by the key ``|n-3|``, with ``z`` being the absolute value operator for numbers (`permalink <http://cjam.aditsu.net/#code=%5B-2%204%202%206%209%201%203%20-1%5D%20%7B3-z%7D%20%24%20p>`__): ::

    [-2 4 2 6 9 1 3 -1] {3-z} $ p    ->    [3 4 2 1 6 -1 -2 9]

Note how 4 comes before 2 in the resulting array, even though they are both the same distance from 3. CJam uses Java's ``Collections.sort``, which is stable and `does not reorder equal elements <http://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#sort(java.util.List,%20java.util.Comparator)>`__.

Find index
----------

If both top elements of the stack are arrays, ``#`` returns the first index of a subarray within another, or ``-1`` if the subarray is not found (`permalink <http://cjam.aditsu.net/#code=%5B1%200%201%201%200%200%201%200%201%201%200%5D%20%5B1%201%200%5D%20%23%20p%0A%5B1%200%201%201%200%200%201%200%201%201%200%5D%20%5B0%201%200%5D%20%23%20p%0A%5B1%200%201%201%200%200%201%200%201%201%200%5D%20%5B0%200%200%5D%20%23%20p>`__): ::

    [1 0 1 1 0 0 1 0 1 1 0] [1 1 0] #    ->    2
    [1 0 1 1 0 0 1 0 1 1 0] [0 1 0] #    ->    5
    [1 0 1 1 0 0 1 0 1 1 0] [0 0 0] #    ->   -1

It is important to remember that strings are arrays too. For instance, the second example below won't work because ``["fish"]`` is not a subarray of ``"one fish two fish" = ['o 'n 'e ...]`` (`permalink <http://cjam.aditsu.net/#code=%22one%20fish%20two%20fish%22%20%22fish%22%20%20%23%20p%0A%22one%20fish%20two%20fish%22%20%22fish%22a%20%23%20p%0A%0A%5B%22one%22%20%22fish%22%20%22two%22%20%22fish%22%5D%20%22fish%22%20%20%23%20p%0A%5B%22one%22%20%22fish%22%20%22two%22%20%22fish%22%5D%20%22fish%22a%20%23%20p>`__). ::

    "one fish two fish" "fish"  #    ->    4
    "one fish two fish" "fish"a #    ->    -1

    ["one" "fish" "two" "fish"] "fish"  #    ->    -1
    ["one" "fish" "two" "fish"] "fish"a #    ->    1

If one of the elements is an array but the other is a number/character, ``#`` returns the first index of the number/character in the array, or ``-1`` if not found (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%201%20%23%20p%0A%5B1%204%202%208%205%207%5D%205%20%23%20p%0A%5B1%204%202%208%205%207%5D%209%20%23%20p%0A%22mhsjmdkmgc%22%20'j%20%23%20p%0A%22mhsjmdkmgc%22%20'q%20%23%20p>`__): ::

    [1 4 2 8 5 7] 1 #    ->    0
    [1 4 2 8 5 7] 5 #    ->    4
    [1 4 2 8 5 7] 9 #    ->    -1
    "mhsjmdkmgc" 'j #    ->    3
    "mhsjmdkmgc" 'q #    ->    -1
	
Like ``=`` and ``$``, ``#`` can also take a block, returning the index of the first element which satisfies the given condition. For example, ``{0>} #`` gives the index of the first positive element (`permalink <http://cjam.aditsu.net/#code=%5B-3%20-2%200%204%209%20-3%200%202%5D%20%7B0%3E%7D%20%23>`__): ::

    [-3 -2 0 4 9 -3 0 2] {0>} #    ->    3
	
Split
-----

``/`` splits an array based on another. The most common use for this is to split a string, e.g. ``S/`` splits by spaces and ``N/`` splits by newlines (`permalink <http://cjam.aditsu.net/#code=%22one%20fish%20two%20fish%22%20%20%20%20%20%20%20S%20%2F%20p%20%20%20e%23%20S%20is%20space%2C%20or%20%22%20%22%0A%22one%20fish%20two%20fish%22%20%20%22fish%22%20%2F%20p%0A%22one%20fish%20two%20fish%22%20%22uh-oh%22%20%2F%20p>`__): ::

    "one fish two fish"       S /    ->    ["one" "fish" "two" "fish"]
    "one fish two fish"  "fish" /    ->    ["one " " two " ""]
    "one fish two fish" "uh-oh" /    ->    ["one fish two fish"]

In fact, ``/`` works for any two general arrays (`permalink <http://cjam.aditsu.net/#code=%5B1%204%20%22cake%22%203%20%22blue%22%204%20%22cake%22%202%5D%20%5B4%20%22cake%22%5D%20%2F%20p>`__): ::

    [1 4 "cake" 3 "blue" 4 "cake" 2] [4 "cake"] /    ->    [[1] [3 "blue"] [2]]

Similarly, ``%`` can also be used for splitting, but removes empty chunks in the resulting array (`permalink <http://cjam.aditsu.net/#code=%22one%20fish%20two%20fish%22%20%22fish%22%20%25%20p>`__): ::

    "one fish two fish" "fish" % p    ->    ["one " " two "]
    
``/`` also works if one element is an array and the other is a number. In this situation the array is split into chunks of size equal to the number, except possibly the last chunk if there aren't enough elements (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%202%2F%20p%0A%5B1%204%202%208%205%207%5D%203%2F%20p%0A%5B1%204%202%208%205%207%5D%204%2F%20p>`__): ::

    [1 4 2 8 5 7] 2/    ->    [[1 4] [2 8] [5 7]]
    [1 4 2 8 5 7] 3/    ->    [[1 4 2] [8 5 7]]
    [1 4 2 8 5 7] 4/    ->    [[1 4 2 8] [5 7]]

Join
----

``*`` takes two arrays – the second array is riffled between the elements of the first. For example, ``", " *`` joins an array of values with commas (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%20%22%2C%20%22%20*>`__): ::

    [1 4 2 8 5 7] ", " *    ->    1, 4, 2, 8, 5, 7
	
The actual resulting array looks like this: ::

    [1 ', '  4 ', '  2 ', '  8 ', '  5 ', '  7]

We can join with any array, e.g. (`permalink <http://cjam.aditsu.net/#code=%5B1%207%202%209%5D%20%5B%22a%22%200%5D%20*%20p>`__)::

    [1 7 2 9] ["a" 0] *    ->    [1 "a" 0 7 "a" 0 2 "a" 0 9]

Joining can be combined with splitting to give an idiom for search-and-replace (`permalink <http://cjam.aditsu.net/#code=%2212313132131231%22%20%2213%22%2F%20%22..%22*%20p%20%20e%23%20Replace%20%2213%22%20with%20%22..%22%0A%22a%20%20%20b%20c%20%20%20%20d%20%20%22%20S%25%20S*%20p%20%20%20%20%20%20%20%20e%23%20Collapse%20runs%20of%20spaces>`__): ::

    Replace "13" with "..":
    "12313132131231" "13"/ ".."*    ->    "123....2..1231"
	
    Collapse runs of spaces:
    "a   b c    d  " S% S* p        ->    "a b c d"


Set operations
--------------

CJam arrays can actually be used as sets. Like sorting, CJam set operations are stable, preserving element order.

The set operations are: ::

    &    Set AND, or intersection
    |    Set OR, or union
    ^    Set XOR, or symmetric difference
    -    Set minus, or relative complement
	
For instance (`permalink <http://cjam.aditsu.net/#code=%5B3%201%204%201%205%209%5D%20%5B2%206%205%203%5D%20%26%20p%0A%5B3%201%204%201%205%209%5D%20%5B2%206%205%203%5D%20%7C%20p%0A%5B3%201%204%201%205%209%5D%20%5B2%206%205%203%5D%20%5E%20p%0A%5B3%201%204%201%205%209%5D%20%5B2%206%205%203%5D%20-%20p>`__): ::

    [3 1 4 1 5 9] [2 6 5 3] &    ->    [3 5]
    [3 1 4 1 5 9] [2 6 5 3] |    ->    [3 1 4 5 9 2 6]
    [3 1 4 1 5 9] [2 6 5 3] ^    ->    [1 4 9 2 6]
    [3 1 4 1 5 9] [2 6 5 3] -    ->    [1 4 1 9]

A common idiom for removing duplicates from an array is to simply do ``L|``, finding the union of an array with the empty list.

Example program: Name sorter
----------------------------

Suppose we have a list of properly capitalised names, e.g. ::

    John Doe
    Jane Smith
    John Smith
    Tommy Atkins
    Jane Doe
    Foo Bar

Let's say we want to sort this list by last name, with ties broken by first name.

First we need to read in the input with ``q`` and split by newlines with ``N/``. This gives an array of strings: ::

    ["John Doe" "Jane Smith" "John Smith" "Tommy Atkins" "Jane Doe" "Foo Bar"]

Sorting normally is clearly not sufficient here, so we'll need to sort with a block like ``{}$``. To include a tie breaker in our sort, we will use an array as the key. Since last names take priority, the key array will simply be the last name followed by the first name.

Suppose we're in the situation where we're looking at a name, say ``"John Doe"``. To get the key we just split by spaces with ``S/`` to get ``["John" "Doe"]``, then reverse the array with ``W%`` to give the sorting key ``["Doe" "John"]``.

Putting this together, after ``{S/W%}$`` we get ::

    ["Tommy Atkins" "Foo Bar" "Jane Doe" "John Doe" "Jane Smith" "John Smith"]

All that remains is to join with newlines ``N*`` to get the final list: ::

    Tommy Atkins
    Foo Bar
    Jane Doe
    John Doe
    Jane Smith
    John Smith

And the final program is (`permalink <http://cjam.aditsu.net/#code=qN%2F%7BS%2FW%25%7D%24N*&input=John%20Doe%0AJane%20Smith%0AJohn%20Smith%0ATommy%20Atkins%0AJane%20Doe%0AFoo%20Bar>`__)::

    qN/{S/W%}$N*

Now what if we have middle names, like ``John B. Doe``? Our current program would still work, but our sorting key would prioritise middle names before first names. If we want to prioritise first names *before* middle names, then we would need to change the key a little.

Splitting ``"John B. Doe"`` by spaces gives ``["John" "B." "Doe"]``, but the desired priority order is ``["Doe" "John" "B."]``. We simply need to move the last name to the front, and to do so we start by popping the last name off: ::

    ["John" "B." "Doe"] )      ->    ["John" "B."] "Doe"

Then we swap ``\`` and concatenate ``+``, right? Almost... ::

    ["John" "B." "Doe"] )\+    ->    ['D 'o 'e "John" "B."]

Because ``"Doe"`` is a string, i.e. an array of characters, concatenating the two arrays dumps the *characters* of ``"Doe"`` at the front, rather than adding on the string as a whole piece. To fix this, we just need to add an ``a`` to wrap the last name in an array, giving ``)a\+`` (`permalink <http://cjam.aditsu.net/#code=%5B%22John%22%20%22B.%22%20%22Doe%22%5D%20)a%5C%2B%20p>`__): ::

    ["John" "B." "Doe"] )a\+   ->    ["Doe" "John" "B."]
	
Using this new key gives the program (`permalink <http://cjam.aditsu.net/#code=qN%2F%7BS%2F%29a%5C%2B%7D%24N*&input=John%20B.%20Doe%0AJane%20Louise%20Smith%0AJohn%20Smith%0AJohn%20A.%20R.%20Doe%0ATommy%20Atkins%0AJane%20Smith%0AJane%20Doe%0AFoo%20Bar%0AJohn%20A.%20Doe>`__)::

    qN/{S/)a\+}$N*

