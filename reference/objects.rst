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

Moreover, if is not a Phalcon class then objects must then initialized as follows:

.. code-block:: c

	zend_class_entry *reflection_ce;

	//Obtain the ReflectionClass class entry, this will also call autoloaders
	reflection_ce = zend_fetch_class(SL("ReflectionClass"), ZEND_FETCH_CLASS_AUTO TSRMLS_CC);

	//Instantiate the Reflection object
	PHALCON_INIT_VAR(reflection);
	object_init_ex(reflection, reflection_ce);

	//Pass a class name as constructor's parameter
	PHALCON_CALL_METHOD_PARAMS_1_NORETURN(reflection, "__construct", class_name, PH_CHECK);

Reading/Writing Properties
--------------------------
Writing scalar values:

.. code-block:: c

	//Create a stdClass object
	PHALCON_INIT_VAR(employee);
	object_init(employee);

	// $employee->name = "Sonny"
	phalcon_update_property_string(employee, SL("name"), "Sonny" TSRMLS_CC);

	// $employee->age = 23
	phalcon_update_property_long(employee, SL("age"), 23 TSRMLS_CC);

	//Read the "name" property $name = $employee->name
	PHALCON_INIT_VAR(name);
	phalcon_read_property(&name, employee, SL("name"), PH_NOISY_CC);

Assigning other zvals to properties:

.. code-block:: c

	PHALCON_INIT_VAR(language);
	ZVAL_STRING(language, "English", 1);

	PHALCON_INIT_VAR(employee);
	object_init(employee);

	// $employee->language = $language
	phalcon_update_property_zval(employee, SL("language"), language TSRMLS_CC);

Reading/Writing dynamical properties:

.. code-block:: c

	PHALCON_INIT_VAR(language);
	ZVAL_STRING(language, "English", 1);

	PHALCON_INIT_VAR(property);
	ZVAL_STRING(property, "language", 1);

	PHALCON_INIT_VAR(employee);
	object_init(employee);

	// $employee->$property = $language
	phalcon_update_property_zval_zval(employee, property, language TSRMLS_CC);

	// $user_language = $employee->$property
	PHALCON_INIT_VAR(user_language);
	phalcon_read_property_zval(&user_language, employee, property, PH_NOISY_CC);
