Calling Functions
-----------------
Calling functions is another common action we do when create programs in PHP. Although many functions may be called directly using its internal function pointer in C, others can simply being called using the PHP userland. For example:

.. code-block:: c

	//$length = strlen(some_variable);

	PHALCON_INIT_VAR(length);
	PHALCON_CALL_FUNC_PARAMS_1(length, "strlen", some_variable);

The macro PHALCON_CALL_FUNC_PARAMS_1 calls functions that requires 1 parameter returning a value. There are another macros to call functions in the PHP userland:

.. code-block:: c

	//Calling substr() with its 3 arguments returning the substring into part
	PHALCON_INIT_VAR(part);
	PHALCON_CALL_FUNC_PARAMS_3(part, "substr", some_variable, start, length);

	//Calling ob_start(), this function does not return anything
	PHALCON_CALL_FUNC_NORETURN("ob_start");
	
	//Closing a file with fclose
	PHALCON_CALL_FUNC_PARAMS_1_NORETURN("fclose", file_handler);

As mentioned above, PHP already has many things going, the fact that an extension in C, does not mean we going to reinvent the wheel all over again develop. Also worth saying that we also try to avoid using very low-level features of C, if PHP already has its own version. PHP functions help us get a behavior similar to that programming in the PHP would. In this way we avoid possible errors and as result the framework works as if it were PHP. 

The following code opens a file in C and write something on it. Its functionality is limited because it only works on local files:

.. code-block:: c

	FILE * pFile;
	pFile = fopen ("myfile.txt","w");
	if (pFile != NULL) {
		fputs ("fopen example",pFile);
		fclose (pFile);
	}

Now write the same code using the PHP userland:

.. code-block:: c

	PHALCON_INIT_VAR(mode);
	ZVAL_STRING(mode, "w", 1);
	
	PHALCON_INIT_VAR(file_handler);
	PHALCON_CALL_FUNC_PARAMS_2(file_handler, "fopen", file_path, mode);

	if (PHALCON_IS_NOT_FALSE(file_handler)) {

		PHALCON_INIT_VAR(text);
		ZVAL_STRING(text, "fopen example", 1);

		PHALCON_CALL_FUNC_PARAMS_2_NORETURN("fputs", file_handler, text);

		PHALCON_CALL_FUNC_PARAMS_1_NORETURN("fclose", file_handler);
	}	

Although both codes perform the same task, the former is more powerful as it could open a PHP stream, a file in a URL or a local file.We can also write the same code using the PHP API, without losing functionality. However the above code is more familiar if we are primarily PHP developers.

The same code in PHP:

.. code-block:: c

	$fp = fopen($file_path, "w");
	if($fp){			
		fputs($fp, "fopen example");
		fclose($fp);
	}
