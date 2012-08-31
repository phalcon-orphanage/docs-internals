Objects Manipulation
====================
Phalcon is a pure object oriented OO framework for PHP. In this chapter, we explain how to make most common operations on objects using the Phalcon API.

Creation/Instantiation
----------------------
Instantiate objects of the framework classes is easy:

.. code-block:: c

	//Instantiate a object from the Phalcon\Mvc\Router\Route class entry
	PHALCON_INIT_VAR(route);
	object_init_ex(route, phalcon_mvc_router_route_ce);

	//Calling the constructor and passing a pattern as parameter
	PHALCON_INIT_VAR(pattern);
	ZVAL_STRING(pattern, "#^/([a-zA-Z0-9\\_]+)[/]{0,1}$#", 1);
	PHALCON_CALL_METHOD_PARAMS_1_NORETURN(route, "__construct", pattern, PH_CHECK);

The above code is the same as doing in PHP:

.. code-block:: php

	<?php $route = new Phalcon\Mvc\Router\Route("#^/([a-zA-Z0-9\\_]+)[/]{0,1}$#");

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

Reading Properties
------------------

