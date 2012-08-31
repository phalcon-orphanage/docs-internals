Objects Manipulation
====================
Phalcon is pure object oriented OO framework.

Creation/Instantiation
----------------------
Instantiate objects of the framework classes is easy:

.. code-block:: c

	PHALCON_INIT_VAR(route);
	object_init_ex(route, phalcon_mvc_router_route_ce);

	PHALCON_INIT_VAR(pattern);
	ZVAL_STRING(pattern, "#^/([a-zA-Z0-9\\_]+)[/]{0,1}$#", 1);
	PHALCON_CALL_METHOD_PARAMS_1_NORETURN(route, "__construct", pattern, PH_CHECK);

The above code is the same as doing in PHP:

.. code-block:: php

	$route = new Phalcon\Mvc\Router\Route("#^/([a-zA-Z0-9\\_]+)[/]{0,1}$#");

Moreover, if the class is not Phalcon objects must then initialized as follows:

.. code-block:: php

	zend_class_entry *reflection_ce;

	//Obtain the ReflectionClass class entry, this will also call autoloaders
	reflection_ce = zend_fetch_class(SL("ReflectionClass"), ZEND_FETCH_CLASS_AUTO TSRMLS_CC);

	//Instantiate the Reflection object
	PHALCON_INIT_VAR(reflection);
	object_init_ex(reflection, reflection_ce);

	//Pass a class name as constructor's parameter
	PHALCON_CALL_METHOD_PARAMS_1_NORETURN(reflection, "__construct", class_name, PH_CHECK);
