Common Structures
-----------------

**Zvals:**
Most variables used in the framework are Zvals. Each value within a PHP application is a zval. The Zvals are polymorphic structures, ie a zval can have any value (string, long, double, array, etc.). Inspecting some code you will see that most variables are declared Zvals. PHP has scalar data types (string, null, bool, long and double) and non-scalar data types (arrays and objects). There is a way to assign a value the zval according to the data type:

.. code-block:: c
	
	PHALCON_INIT_VAR(name);
	ZVAL_STRING(name, "Sonny", 1);

	PHALCON_INIT_VAR(number);
	ZVAL_LONG(number, 12000);

	PHALCON_INIT_VAR(price);
	ZVAL_DOUBLE(price, 15.50);

	PHALCON_INIT_VAR(nothing);
	ZVAL_NULL(nothing);

	PHALCON_INIT_VAR(is_alive);
	ZVAL_BOOL(is_alive, false);	

**Zend Class Entries:**
Another common structure we used is the zend class entry. That structure help us to describe a class, its name, method, default properties, etc.

.. code-block:: c
	
	//Get the class entry
	class_entry = Z_OBJCE_P(this_ptr);

	//Print the class name
	fprintf(stdout, "%s", class_entry->name);
