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

Two arrays can be concatenated with ``+`` (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%5D%20%5B8%205%207%5D%20%2B%20p>`__): ::

    [1 4 2] [8 5 7] +    ->    [1 4 2 8 5 7]

Ranges
------

Recall that ``,`` on an array gives the length of an array, so ::

    ["this" "array" "has" "five" "elements"] ,

results in ``5`` (`permalink <http://cjam.aditsu.net/#code=%5B%22this%22%20%22array%22%20%22has%22%20%22five%22%20%22elements%22%5D%20%2C>`__). ``,`` on an integer *creates* an array, namely it is a range operator which produces ``[0 1 ... n-1]``: (`permalink <http://cjam.aditsu.net/#code=5%2C%20p>`__)::

    5,    ->    [0 1 2 3 4]

``,`` also acts as a range operator on characters, where it creates a string instead. For instance, ``'a,`` results in the first 97 ASCII characters, including the initial unprintables (`permalink <http://cjam.aditsu.net/#code='a%2C>`__).

Getting and setting
-------------------

Getting an element from an array can be done with ``=``. A nice feature is that out of bounds indices wrap around. (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%20%20%20%20%200%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20%203%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20%205%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20%206%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20%20%20%20-2%20%3D%20%20p%0A%5B1%204%202%208%205%207%5D%20-1000%20%3D%20%20p>`__) ::

    [1 4 2 8 5 7]     0 =      ->   1
    [1 4 2 8 5 7]     5 =      ->   7
    [1 4 2 8 5 7]     6 =      ->   1
    [1 4 2 8 5 7]    -2 =      ->   5
    [1 4 2 8 5 7] -1000 =      ->   2

Setting a position in an array is done with ``t``, with the index followed by the element (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%200%20-3%20t%20p>`__): ::
    
	[1 4 2 8 5 7]  0 -3 t      ->   [-3 4 2 8 5 7]
	
Similarly to ``=``, ``t`` also wraps around for out of bounds indices.

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

``$`` can optionally take an additional block argument, which determines what key to sort by. For example, ``$`` on its own sorts strings by ASCII values, while ``{el} $`` sorts strings by their lowercase counterparts, giving a case-insensitive search. Compare (`permalink <http://cjam.aditsu.net/#code=%5B%22Bee%22%20%22candy%22%20%22Cake%22%20%22apple%22%5D%20%20%20%20%20%20%24%20p%0A%5B%22Bee%22%20%22candy%22%20%22Cake%22%20%22apple%22%5D%20%7Bel%7D%20%24%20p>`__): ::

    ["Bee" "candy" "Cake" "apple"]      $   ->   ["Bee" "Cake" "apple" "candy"]
    ["Bee" "candy" "Cake" "apple"] {el} $   ->   ["apple" "Bee" "Cake" "candy"]

``el`` and ``eu`` convert strings to lowercase and uppercase respectively.

Another example is ``{3-z} $``, which sorts an array of numbers by the key ``|n-3|``, with ``z`` being the absolute value operator for numbers (`permalink <http://cjam.aditsu.net/#code=%5B-2%204%202%206%209%201%203%20-1%5D%20%7B3-z%7D%20%24%20p>`__): ::

    [-2 4 2 6 9 1 3 -1] {3-z} $ p    ->    [3 4 2 1 6 -1 -2 9]

Note how 4 comes before 2 in the resulting array, even though they are both the same distance from 3. CJam uses Java's ``Collections.sort``, which is stable and `does not reorder equal elements <http://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#sort(java.util.List,%20java.util.Comparator)>`__.

Find index
----------

``#`` finds the index of an element in an array, returning ``-1`` if the element is not found (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%201%20%23%20p%0A%5B1%204%202%208%205%207%5D%205%20%23%20p%0A%5B1%204%202%208%205%207%5D%209%20%23%20p>`__): ::

    [1 4 2 8 5 7] 1 #    ->    0
    [1 4 2 8 5 7] 5 #    ->    4
    [1 4 2 8 5 7] 9 #    ->    -1
	
Like ``$``, ``#`` can also take a block, returning the index of the first element which satisfies the given condition. For example, ``{0>} #`` gives the index of the first positive element (`permalink <http://cjam.aditsu.net/#code=%5B-3%20-2%200%204%209%20-3%200%202%5D%20%7B0%3E%7D%20%23>`__): ::

    [-3 -2 0 4 9 -3 0 2] {0>} #    ->    3
	
Split
-----

``/`` splits an array based on another. The most common use for this is to split a string, e.g. ``S/`` splits by spaces and ``N/`` splits by newlines (`permalink <http://cjam.aditsu.net/#code=%22one%20fish%20two%20fish%22%20%20%20%20%20%20%20S%20%2F%20p%20%20%20e%23%20S%20is%20space%2C%20or%20%22%20%22%0A%22one%20fish%20two%20fish%22%20%20%22fish%22%20%2F%20p%0A%22one%20fish%20two%20fish%22%20%22uh-oh%22%20%2F%20p>`__): ::

    "one fish two fish"       S /    ->    ["one" "fish" "two" "fish"]
    "one fish two fish"  "fish" /    ->    ["one " " two " ""]
    "one fish two fish" "uh-oh" /    ->    ["one fish two fish"]

But ``/`` actually works for any two arrays (`permalink <http://cjam.aditsu.net/#code=%5B1%204%20%22cake%22%203%20%22blue%22%204%20%22cake%22%202%5D%20%5B4%20%22cake%22%5D%20%2F%20p>`__): ::

    [1 4 "cake" 3 "blue" 4 "cake" 2] [4 "cake"] /    ->    [[1] [3 "blue"] [2]]

``%`` is also for splitting, but removes empty chunks in the resulting array (`permalink <http://cjam.aditsu.net/#code=%22one%20fish%20two%20fish%22%20%22fish%22%20%25%20p>`__): ::

    "one fish two fish" "fish" % p    ->    ["one " " two "]

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

Note 

Set operations
--------------

CJam arrays can actually be used as sets. Like sorting, CJam set operations are stable, keeping elements in order.

The set operations are: ::

    &    Set AND, or intersection
    |    Set OR, or union
    ^    Set XOR, or symmetric difference
    -    Set minus, or relative complement
	
For instance (`permalink <http://cjam.aditsu.net/#code=%5B1%204%202%208%205%207%5D%20%5B1%207%202%209%5D%20%26%20p%0A%5B1%204%202%208%205%207%5D%20%5B1%207%202%209%5D%20%7C%20p%0A%5B1%204%202%208%205%207%5D%20%5B1%207%202%209%5D%20%5E%20p%0A%5B1%204%202%208%205%207%5D%20%5B1%207%202%209%5D%20-%20p>`__): ::

    [1 4 2 8 5 7] [1 7 2 9] &    ->    [1 2 7]
    [1 4 2 8 5 7] [1 7 2 9] |    ->    [1 4 2 8 5 7 9]
    [1 4 2 8 5 7] [1 7 2 9] ^    ->    [4 8 5 9]
    [1 4 2 8 5 7] [1 7 2 9] -    ->    [4 8 5]

A common idiom for removing duplicates from an array is to simply do ``_&``, intersecting the array with itself.