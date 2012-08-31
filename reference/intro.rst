General Considerations
======================
The way the code is written is not the usual, if you compare the code of Phalcon with other C extensions you'll see a huge difference, this unusual way of write the extension has several objectives:

* Provide a code that is closer to be understood by developers PHP
* Create objects and components that act as close as possible to PHP objects
* Avoid dealing with low-level issues, whenever possible

When writing code in Phalcon try to follow these guidelines:

* Writing the code in a PHP style, this will help to PHP developers to introspect easily understanding the internals of PHP objects. `Reflection <http://en.wikipedia.org/wiki/Reflection_%28computer_programming%29>`_
* Help the Phalcon user to obtain more human backtraces.
* `Don't repeat yourself <http://en.wikipedia.org/wiki/Don%27t_repeat_yourself>`_, if PHP has a functionality already developed, why do not simply use it?

