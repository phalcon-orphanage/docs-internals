Compilation on Windows
======================
Follow these instructions to build Phalcon in your Windows system. This guide is tested on Windows XP and Windows 7.

The Process Idea
---------------------

Compiling Phalcon is performed as a part of PHP compilation. This is done in order to ensure, that Phalcon is built with exactly the same defines and options as PHP.

Before you Begin
----------------

Building Phalcon on Windows will require four things

* A properly set up build environment, including a compiler with the right SDK's and some binary tools used by the build system
* Prebuilt libraries and headers for third party libraries that PHP uses in the correct location
* The PHP source
* The Phalcon source

The Build Environment
---------------------

This is the hardest part of the PHP windows build system to set up and will take up a lot of space on your hard drive - you need to have several GB of space free.
Requirements

- Microsoft Visual C++, PHP officially supports building with Visual C++ 6.0 or with Visual C++ 9 (also known as Visual C++ 2008 just to be confusing). You can use the Express versions as well. MinGW and other compilers are NOT supported or even known to work. For more information and how to get the compiler see the supported versions.
- The correct Windows SDK or Platform SDK to match your compiler. see this page for the supported versions
- Various tools, see http://windows.php.net/downloads/php-sdk/ for binary versions of them

Setup Quick 'n' easy
--------------------

- get visual studio 2008 (no matter what version - express, pro or others; all should work) and install it. Download: http://www.microsoft.com/visualstudio/en-us/products/2008-editions/express
- get and install windows sdk 6.1. Download http://www.microsoft.com/en-us/download/details.aspx?id=11310
- get a php 5.3 snapshot (do not extract yet!) Download: http://windows.php.net/download/
- get Phalcon source code: https://github.com/phalcon/cphalcon
- create the folder “c:\\php-sdk“
- unpack the binary-tools.zip archive (http://windows.php.net/downloads/php-sdk/) into this directory, there should be one sub-directory called “bin” and one called “script“
- open the “windows sdk 6.1 shell” (it’s available from the start menu group) and execute the following commands in it:

.. code-block:: bash

    setenv /x86 /xp /release

    cd c:\php-sdk\

    bin\phpsdk_setvars.bat

    bin\phpsdk_buildtree.bat php53dev


- now extract the snapshot from 3) to C:\\php-sdk\\php53dev\\vc9\\x86 with your favourite unpacker (winrar should handle it) so that the following directory gets created: C:\\php-sdk\\php53dev\\vc9\\x86\\php5.3-xyz

- in the same directory (C:\\php-sdk\\php53dev\\vc9\\x86) there is a “deps” folder, extract any of your required libraries inside that folder (see http://wiki.php.net/internals/windows/libs) but make sure their top-level contains /include and /lib (some of them have an extra directory level in there)

- put prepared Phalcon source code to C:\\php-sdk\\php53dev\\vc9\\x86\\php5.3-xyz\\ext\\phalcon folder. The prepared Phalcon source is located inside the “build\\\ *[type]*\ ” folder of the Phalcon repository, where *type* can be “safe”, “32bits” or “64bits”. For more information on how to chose proper type or regenerate that source code (which is needed for development versions of Phalcon) read the file “build\\README.md” in the Phalcon repository.

- run in the windows-sdk-shell:

.. code-block:: bash

	cd C:\php-sdk\php53dev\vc9\x86\php5.3-xyz

	buildconf

to get an overview of the compiling flags:

.. code-block:: bash

	configure --help

create your configure command:

.. code-block:: bash

	configure --disable-all --enable-cli --enable-phalcon=shared

	nmake

The php_phalcon.dll after the compilation is located at: C:\\php-sdk\\php53dev\\vc9\\x86\\php5.3-xyz\\Release_TS\\

This guide is based on this another guide: https://wiki.php.net/internals/windows/stepbystepbuild