Common Structures
-----------------

To start explaining Phalcon, is neccesary to understand a couple of low level structures that are commonly used in the framework:

Zvals
^^^^^
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

This is the internal structure of a zval:

.. code-block:: c

	typedef union _zvalue_value {
		long lval;                  /* long value */
		double dval;                /* double value */
		struct {
			char *val;
			int len;
		} str;
		HashTable *ht;              /* hash table value */
		zend_object_value obj;
	} zvalue_value;

	struct _zval_struct {
		/* Variable information */
		zvalue_value value;     /* value */
		zend_uint refcount__gc;
		zend_uchar type;    /* active type */
		zend_uchar is_ref__gc;
	};

Most of the time you will not have to deal directly accessing the internal structure of a zval. Instead the Zend API offers a comfortable syntax for altering its value or get information from it.

Previously we saw how to change their values ​​change according to the type, let's see how to read their values​​:

.. code-block:: c

	long number = Z_LVAL_P(number_var);

	char *str = Z_STRVAL_P(string_var);

	int bool_value = Z_BVAL_P(bool_var);

If we want to know the data type of a zval:

.. code-block:: c

	int type = Z_TYPE_P(some_variable);
	if (type == IS_STRING) {
		//Is string!
	}

Zend Class Entries
^^^^^^^^^^^^^^^^^^
Another common structure we used is the zend class entry. Every class in the C internal world has its own zend class entry.
That structure help us to describe a class, its name, method, default properties, etc.

.. code-block:: c

	//Get the class entry
	class_entry = Z_OBJCE_P(this_ptr);

	//Print the class name
	fprintf(stdout, "%s", class_entry->name);

