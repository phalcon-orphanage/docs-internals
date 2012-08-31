Working with Arrays
===================
Although the Zend API, and provides various functions for working with arrays, with Phalcon API we added others. Specifically helping to maintain the reference counting correctly:

One dimension Arrays
^^^^^^^^^^^^^^^^^^^^

.. code-block:: c

	//Declare the variable
	zval fruits = NULL;

	PHALCON_INIT_VAR(fruits);
	array_init(fruits);

	//Adding items to the array
	add_next_index_stringl(fruits, SL("apple"), 1);
	add_next_index_stringl(fruits, SL("orange"), 1);
	add_next_index_stringl(fruits, SL("lemon"), 1);
	add_next_index_stringl(fruits, SL("banana"), 1);

	//Get the first item in the array $fruits[0]
	PHALCON_INIT_VAR(first_item);
	phalcon_array_fetch_long(&first_item, fruits, 0, PH_NOISY_CC);

Mixing both string and number indexes:

.. code-block:: c

	//Let's create the following array using the Phalcon API
	//$fruits = array(1, null, false, "some string", 15.20, "my-index" => "another string");

	PHALCON_INIT_VAR(fruits);
	array_init(fruits);
	add_next_index_long(fruits, 1);
	add_next_index_null(fruits);
	add_next_index_bool(fruits, 0);
	add_next_index_stringl(fruits, SL("some string"), 1);
	add_next_index_double(fruits, 15.2);
	add_assoc_stringl_ex(fruits, SL("my-index")+1, SL("another string"), 1);

	//Updating an existing index $fruits[2] = "other value";
	phalcon_array_update_long_string(&fruits, 2, SL("other value"), PH_SEPARATE TSRMLS_CC);

	//Removing an existing index unset($fruits[1]);
	phalcon_array_unset_long(fruits, 1);

	//Removing an existing index unset($fruits["my-index"]);
	phalcon_array_unset_string(fruits, SL("my-index")+1);

