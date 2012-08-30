Memory Management
-----------------
As you may know, the memory management in C is all manual. Within a PHP extension, we can leverage the use of the Zend Memory Manager, however management remains manual.

To help in the creation of Phalcon, we have created a memory manager that basically track every variable allocated in order to free it before exit the active method:

For example:

.. code-block:: c

	PHP_METHOD(Phalcon_Some_Component, sayHello){

		zval *greeting = NULL;

		PHALCON_MM_GROW();
		
		PHALCON_INIT_VAR(gretting);
		ZVAL_STRING(greeting, "Hello!", 1);

		PHALCON_MM_RESTORE();
	}

PHALCON_MM_GROW starts a memory frame in the memory manager, then PHALCON_INIT_VAR allocates memory for the variable greeting, before exit the method we call PHALCON_MM_RESTORE, this releases all the memory allocated from the last call to PHALCON_MM_GROW. 

By this way, we can be sure that  all the memory allocated will be freed, avoiding memory leaks. Let's pretend that we used 10-15 variables, releasing manually the memory of them can be tedious. 	

Variable Creation
-----------------
As noted above, all variables must be initialized before use. Even, it should be reset again when we change its value, for example:

.. code-block:: c

	//Declare the variable
	zval some_number = NULL;

	//Initialize the variable and assign it a string value
	PHALCON_INIT_VAR(some_number);
	ZVAL_STRING(some_number, "one hundred");

	//Reinitialize the variable and change its value to long
	PHALCON_INIT_VAR(some_number);
	ZVAL_LONG(some_number, 100);

When declaring the variables is important to initialize them to NULL. By doing this, PHALCON_INIT_VAR will know if the variable needs memory or it already have memory allocated.