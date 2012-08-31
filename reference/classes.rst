Classes
=======
As an object oriented framework, Phalcon is mostly composed of classes. The classes are organized into namespaces. The following are the steps to export a variable in the extension and also make it available for other classes inside the framework.

Registering methods and its arguments
-------------------------------------
In the dev/php_phalcon.h register in the first part of the file the pointer to the zend class entry. Let's pretend we're adding a Phalcon\\Auth to the framework:

.. code-block:: c

	extern zend_class_entry *phalcon_auth_ce;

Then, we can add the methods prototypes, it's neccesary to add all the methods that compose our class:

.. code-block:: c

	PHP_METHOD(Phalcon_Auth, __construct);
	PHP_METHOD(Phalcon_Auth, getIdentity);
	PHP_METHOD(Phalcon_Auth, auth);

Later in the same file add the information of the arguments of each method. For example, let's define the class constructor only takes two arguments and they're mandatory:

.. code-block:: c

	ZEND_BEGIN_ARG_INFO_EX(arginfo_phalcon_auth__construct, 0, 0, 2)
		ZEND_ARG_INFO(0, adapter)
		ZEND_ARG_INFO(0, options)
	ZEND_END_ARG_INFO()

Now let's attach methods to the class to which they belong:

.. code-block:: c

	PHALCON_INIT_FUNCS(phalcon_auth_method_entry){
		PHP_ME(Phalcon_Auth, __construct, arginfo_phalcon_auth__construct, ZEND_ACC_PUBLIC)
		PHP_ME(Phalcon_Auth, getIdentity, NULL, ZEND_ACC_PUBLIC)
		PHP_ME(Phalcon_Auth, auth, NULL, ZEND_ACC_PUBLIC)
		PHP_FE_END
	};

All this happens at dev/php_phalcon.h, if you check that file you will see that everything is registered right there.

Implementing a method
---------------------
Each class has its own file .c file, in the case of Phalcon\\Auth file would be dev/auth.c:

.. code-block:: c

	PHP_METHOD(Phalcon_Auth, __construct){

		zval *adapter_name, *options = NULL;

		PHALCON_MM_GROW();

		if (zend_parse_parameters(ZEND_NUM_ARGS() TSRMLS_CC, "zz", &adapter_name, &options) == FAILURE) {
			PHALCON_MM_RESTORE();
			RETURN_NULL();
		}

		PHALCON_MM_RESTORE();
	}

With the above code we create the constructor of the class Phalcon\\Auth, a method is defined with the macro PHP_METHOD, first we put the class name and then the name of the method, although Phalcon uses namespaces, class names have _ instead of \\:

.. code-block:: c

	PHP_METHOD(Phalcon_Auth, __construct){

If the method has parameters we receive them using zend_parse_parameters:

.. code-block:: c

	if (zend_parse_parameters(ZEND_NUM_ARGS() TSRMLS_CC, "zz", &adapter_name, &options) == FAILURE) {
		PHALCON_MM_RESTORE();
		RETURN_NULL();
	}

If we do not receive the correct number of parameters will result in an error message. You see, there's an argument "zz" to receive the parameters, this indicates the type of data received and the number of them. In the above example that means that the method is receiving two parameters. If it were three zval then it should be "zzz".

Then the variables are received in respective order: &adapter_name, &options

