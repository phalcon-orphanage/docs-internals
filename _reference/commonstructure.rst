Common Structures
=================

-Zvals:- Most variables used in the framework are Zvals. Each value within a PHP application is a zval. The Zvals are polymorphic structures, ie a zval can have any value (string, long, double, array, etc.). Inspecting some code you will see that most variables are declared Zvals. PHP has scalar data types (string, null, bool, long and double) and non-scalar data types (arrays and objects). There is a way to assign a value the zval according to the data type:

