Variables
=========

Creation
--------
Unlike PHP, each variable in C must be declared at the beginning of the function in which we are working. Also, as noted above, all variables must be initialized before use. Even, it should be reset again when we change its value, for example:

.. code-block:: c

	//Declare the variable
	zval *some_number = NULL;

	//Initialize the variable and assign it a string value
	PHALCON_INIT_VAR(some_number);
	ZVAL_STRING(some_number, "one hundred");

	//Reinitialize the variable and change its value to long
	PHALCON_INIT_VAR(some_number);
	ZVAL_LONG(some_number, 100);

When declaring the variables is important to initialize them to NULL. By doing this, PHALCON_INIT_VAR will know if the variable needs memory or it already have memory allocated.

Copy-on-Write
-------------
All the zval variables that we use in Phalcon are pointers. Each pointer points to its value in memory. Two pointers may eventually point to the same value in memory:

.. code-block:: c

	zval *a = NULL, *b = NULL;

	PHALCON_INIT_VAR(a);
	ZVAL_LONG(a, 1500);

	b = a;

	//Print both zvals print the same value
	zend_print_zval(a, 1); // 1500
	zend_print_zval(b, 1); // 1500

Changing the value of any of the two variables will change both pointers because their value are the same:

.. code-block:: c

	ZVAL_LONG(b, -10);

	//Print both zvals print the same value
	zend_print_zval(a, 1); // -10
	zend_print_zval(b, 1); // -10

Now, imagine the following code PHP:

.. code-block:: php

	<?php

	$a = 1500;
	$b = $a;

	echo $a, $b; // 1500 1500

This is practically the same as we did before, however, let's change the value of $b:

.. code-block:: php

	<?php

	$a = 1500;
	$b = $a;

	echo $a, $b; // 1500 1500

	$b = -10;

	echo $a, $b; // 1500 -10

To our eyes, PHP has done what we did in C, but in reality there has been an internal process called copy-on-write.  Pretend that our main memory is this:

.. code-block:: php

    // $a = "hello"
                +---------+----+
                |   0x1   | RC |
                +---------+----+
    zval *a --> | "hello" |  1 |
                +---------+----+

The variable $a is pointing to a virtual memory address 0x1, also that memory have a reference counting of 1. It means that only one variable
it's pointing to that memory address.

.. code-block:: php

    // $b = $a
                +---------+----+
                |   0x1   | RC |
                +---------+----+
    zval *a --> | "hello" |  2 |
                +---------+----+
    zval *b ---------^

Now, $b is equal to $a, now both variables are pointing to the same memory address 0x1. The reference counting is now 2, because two variables
are pointing to the same memory slot. As you can see PHP is saving memory. Although the variables have different names, they point to the same value in memory so that we are not unnecessarily doubling its value.

.. code-block:: php

    // $b = 18
                +---------+----+  +---------+----+
                |   0x1   | RC |  |   0x2   | RC |
                +---------+----+  +---------+----+
    zval *a --> | "hello" |  1 |  |    18   |  1 |
                +---------+----+  +---------+----+
    zval *b ----------------------------^

We're changing the variable $b, to avoid changing the value of $a. PHP performs an internal process called "separation". In this process, PHP allocates memory for $b and reduces the reference count in $a to indicate that $b is not pointing anymore to $a.

Let's see how to write the above process using Zend API:

.. code-block:: c

    zval *a, *b;

    ALLOC_INIT_ZVAL(a);
    ZVAL_STRING(a, "hello", 1);

    //b = a, so we increase the reference counting in a
    b = a;
    Z_ADDREF_P(a);

    //Changing the value of b
    Z_DELREF_P(b);

    ALLOC_INIT_ZVAL(b);
    ZVAL_LONG(b, 18);

It isn't a very complicated process, however properly maintain reference counts is another low-level task that can take us away from our true needs. In a large codebase like the Phalcon one, being aware of this can take a long time and wear us down.

With Phalcon API we should not worry about this:

.. code-block:: c

    zval *a = NULL, *b = NULL;

    PHALCON_INIT_VAR(a);
    ZVAL_STRING(a, "hello", 1);

    PHALCON_CPY_WRT(b, a);

    PHALCON_INIT_VAR(b);
    ZVAL_LONG(b, 18);

Copying variables using PHALCON_CPY_WRT, we leave the task to Phalcon API of caring about the reference counting without worrying us about it.