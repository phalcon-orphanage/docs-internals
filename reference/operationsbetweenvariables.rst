Operations between Variables
----------------------------
PHP is a dynamic language, we can do almost any operation between two variables, regardless of type. Sometimes we do not know exactly the type of data that have the variables, using the Zend API we can do operations between them seamlessly:

.. code-block:: c

	//First variable is string but it's a numeric string
	PHALCON_INIT_VAR(first_var);
	ZVAL_STRING(first_var, "100.10", 1);

	//Second variable is long
	PHALCON_INIT_VAR(second_var);
	ZVAL_LONG(second_var, 150);

	//add_function will make the necessary conversions to produce the addition
	PHALCON_INIT_VAR(result);
	add_function(result, first_var, second_var);
