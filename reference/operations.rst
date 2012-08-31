Operations between Variables
============================
PHP is a dynamic language, we can do almost any operation between two variables, regardless of type. Sometimes we do not know exactly the type of data that have the variables, using the Zend API we can do operations between them seamlessly:

.. code-block:: c

	//First variable is string but it's a numeric string
	PHALCON_INIT_VAR(first_var);
	ZVAL_STRING(first_var, "100.10", 1);

	//Second variable is long
	PHALCON_INIT_VAR(second_var);
	ZVAL_LONG(second_var, 150);

	//add_function will make the necessary conversions to produce the addition
	PHALCON_INIT_VAR(result);
	add_function(result, first_var, second_var);

Concatenation
-------------
Concatenation is one of the most common operations we do in PHP. However using the Zend API can be tedious when concatenate many values​​, for example:

.. code-block:: c

	// The following concatention using just Zend API:
	//
	// $month = "July"; $day = 1;
	// $today = "Today is ".$month." ".$day;

	PHALCON_INIT_VAR(month);
	ZVAL_STRING(month, "2012", 1);

	PHALCON_INIT_VAR(day);
	ZVAL_LONG(day, 1);

	PHALCON_INIT_VAR(today_is);
	ZVAL_STRING(today_is, "Today is", 1);

	PHALCON_INIT_VAR(first_part);
	concat_function(first_part, today_is, month);

	PHALCON_INIT_VAR(space);
	ZVAL_STRING(space, " ", 1);

	PHALCON_INIT_VAR(second_part);
	concat_function(second_part, space, day);

	PHALCON_INIT_VAR(today);
	concat_function(today, first_part, second_part);

Another way to do that is use sprintf, in this case, you need to be completely sure that the variables have all string types:

.. code-block:: c

	char *final_string;
	zval *final;

	PHALCON_INIT_VAR(month);
	ZVAL_STRING(month, "2012", 1);

	PHALCON_INIT_VAR(day);
	ZVAL_STRING(day, "1", 1);

	final_string = emalloc(sizeof(char)*(Z_STRLEN_P(month)+Z_STRLEN_P(day)+12)));
	sprintf(final_string, "Today is %s %s", Z_STRVAL_P(month), Z_STRVAL_P(day));

	PHALCON_INIT_VAR(final);
	ZVAL_STRING(final, final_string, 0);

It's short, but if some of your variables aren't string you will get a segmentation fault or an unexpected behavior.

To help to solve this problem, we have created a set of macros to concatenate zvals and strings:

.. code-block:: c

	PHALCON_INIT_VAR(month);
	ZVAL_STRING(month, "2012", 1);

	PHALCON_INIT_VAR(day);
	ZVAL_STRING(day, "1", 1);

	PHALCON_INIT_VAR(today);
	PHALCON_CONCAT_SVSV(today, "Today is", month, " ", day);

Other examples:

.. code-block:: c

	PHALCON_CONCAT_VV(result, month, day); //July1
	PHALCON_CONCAT_VSV(result, month, ", ", day); //July, 1
	PHALCON_CONCAT_SVSV(result, "Today is", month, " ", day); //July 1
	PHALCON_CONCAT_SVSVSV(result, "Today is", month, " ", day, ", ", year); //July 1, 2012

S=String and V=Zval, just put the S and V to get the right concatenation macro. Easy, no?

