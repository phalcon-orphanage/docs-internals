Memory Management
=================
As you may know, the memory management in C is all manual. Within a PHP extension, all memory is managed by
Zend Memory Manager, however, the management remains manual.

Manual memory management is a powerful tool that C offers. But, as PHP developers we are not used to this
sort of thing. We like to just leave this task to the language without worrying about that.

Phalcon MM
----------
To help in the creation of Phalcon, we have created the Phalcon Memory Manager (Phalcon MM) that basically
tracks every variable allocated in order to free it before exit the active method:

For example:

.. code-block:: c

	PHP_METHOD(Phalcon_Some_Component, sayHello){

		zval *greeting = NULL;

		//Add a memory frame
		PHALCON_MM_GROW();

		PHALCON_INIT_VAR(greeting);
		ZVAL_STRING(greeting, "Hello!", 1);

		//Release all the allocated memory
		PHALCON_MM_RESTORE();
	}

PHALCON_MM_GROW starts a memory frame in the memory manager, then PHALCON_INIT_VAR allocates memory for the
variable greeting, before exit the method we call PHALCON_MM_RESTORE, this releases all the memory allocated
from the last call to PHALCON_MM_GROW.

By this way, we can be sure that  all the memory allocated will be freed, avoiding memory leaks. Let's pretend
that we used 10-15 variables, releasing manually the memory of them can be tedious.

Others Kinds of Allocations
---------------------------
PHALCON_INIT_VAR only allocates memory for zvals, if we want to request memory for other structures then we must use
the functions that provides the Zend API. One very important thing to know is that an extension in C must never
use the standard functions malloc and free. The Zend API has its own functions that make the memory access safer.

.. code-block:: c

	char *some_word = (char *) emalloc(sizeof(char *) * 6);
	memcpy(some_word, "Hello", 5);
	some_word[5] = 0;
	efree(some_word);

In short, emalloc allocates memory and efree frees it. If you forgot to make the efree you will get a memory leak.

