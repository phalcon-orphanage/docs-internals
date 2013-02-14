Calling Methods
===============
As seen when calling functions, in Phalcon the methods are called in the PHP userland. Thanks to this, a Phalcon
user can generate a backtrace and know exactly which components are involved in a given task. They also
allows users to replace Phalcon components by PHP classes of their own. Additionally,
like everything else we've seen, the way to make calls is familiar to PHP developers.

Instance methods
----------------

.. code-block:: c

	//Define the connection DSN
	PHALCON_INIT_VAR(dsn);
	ZVAL_STRING(dsn, "mysql:host=localhost;username=root;password=secret;dbname=some", 1);

	//Get the PDO class entry
	pdo_class_entry = zend_fetch_class(SL("PDO"), ZEND_FETCH_CLASS_AUTO TSRMLS_CC);

	//Create a PDO instance passing the dsn to the constructor
	PHALCON_INIT_VAR(pdo);
	object_init_ex(pdo, pdo_class_entry);
	PHALCON_CALL_METHOD_PARAMS_1_NORETURN(pdo, "__construct", dsn);

	//Create a SQL statement
	PHALCON_INIT_VAR(sql_statement);
	ZVAL_STRING(sql_statement, "SELECT * FROM robots", 1);

	//Call the "exec" method in the pdo object passing the sql_statement
	PHALCON_INIT_VAR(success);
	PHALCON_CALL_METHOD_PARAMS_1(success, pdo, "exec", sql_statement);

Static methods
--------------

.. code-block:: c

	PHALCON_INIT_VAR(some_variable);
	ZVAL_STRING(some_variable, "invoices_detail", 1);

	//Calling Phalcon\Text::camelize("invoices_detail")
	PHALCON_INIT_VAR(camelized);
	PHALCON_CALL_STATIC_PARAMS_1(camelized, "phalcon\\text", "camelize", some_variable);