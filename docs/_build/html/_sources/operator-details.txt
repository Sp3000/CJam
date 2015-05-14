Operator details
================

Basic arithmetic
----------------

``x:Num y:Num #`` -- power/exponentiation
-----------------------------------------

Finds ``x`` raised to the power of ``y``. ::

    5 3 #   ->  125
    2 .5 #  ->  1.4142135623730951
    0 0 #   ->  1

Logical operations
------------------

``Any !`` -- boolean "not"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Return 0 if the element is truthy, otherwise 1. ::

    5 !      ->  0
    "abc" !  ->  0
    [1 0] !  ->  0
    0 !      ->  1
    "" !     ->  1
  
+--------------------------------------------------------------------+  
| **Tip:** ``!!`` can be used to give the truthiness of an element.  |
+--------------------------------------------------------------------+

