Matrix SDK
========

Matrix SDK provides components
needed to get started on RapidSilicon's Matrix device and it's open source
evaluation boards.


Currently, the following boards are supported:

-  `Quickfeather Development Kit 
   <https://www.quicklogic.com/products/eos-s3/quickfeather-development-kit/>`__
-  `SparkFun Thing Plus
   <https://www.sparkfun.com/products/17273/>`__

Getting started on Matrix Evaluation Board
-------------------------------------

The easiest way to get started on is to build
and run example application projects included in this SDK on a
Matrix Evaluation Board.

| Clone this Matrix SDK repository using
| ``git clone --recursive https://github.com/RapidSilicon/Matrix-SDK``

Install the items listed in Pre-requisites section below.

Pre-requisites
--------------

Toolchain
~~~~~~~~~

-  Firmware

   1. Download tarball according to the system configuration from:
      https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads

      Current stable version tested with is ``9-2020-q2-update``

   2. Extract the tarball to a preferred path(/BASEPATH/TO/TOOCHAIN/)

      ``sudo tar xvjf gcc-arm-none-eabi-your-version.tar.bz2 -C /BASEPATH/TO/TOOCHAIN/``

      The usual preferred path is for example ``/usr/share``

      ``sudo tar xvjf gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2 -C /usr/share/``

   3. Add the /BASEPATH/TO/TOOCHAIN/gcc-arm-none-eabi-your-version/bin/
      to PATH (only for current terminal session)

      ``export PATH=/BASEPATH/TO/TOOCHAIN/gcc-arm-none-eabi-your-version/bin/:$PATH``

      For the preferred path of ``/usr/share`` and current tested stable
      version ``9-2020-q2-update`` for example:

      ``export PATH=/usr/share/gcc-arm-none-eabi-9-2020-q2-update/bin/:$PATH``

   4. If the path settings need to be permanent, it can be added to the
      ``~/.bashrc`` or ``~/.bash_profile.``

      Examples and illustrations are for example here:
      https://stackabuse.com/how-to-permanently-set-path-in-linux/

Utilities
~~~~~~~~~
-  Programming Language: `Python 3.7
   <https://www.python.org/downloads/release/python-379/>`__
   or equivalent version

-  Flash Tool: TinyFPGA Programmer 

   Refer to `TinyFPGA
   programmer <https://github.com/RapidSilicon/TinyFPGA-Programmer-Application>`__
   for installation instructions.

-  Serial Communication tool such as: `putty <https://putty.org/>`__

   ::

      sudo apt-get install putty -y

-  Build System: `GNU make
   3.8.1 <https://sourceforge.net/projects/gnuwin32/files/make/3.81/>`__
   or equivalent

Hardware
~~~~~~~~

-  Matrix Evaluation Board
-  A micro USB cable
-  [Optional] A serial-to-USB cable
-  [Optional] `J-Link Debug
   probe <https://www.segger.com/products/debug-probes/j-link/>`__

Baremetal Example
-----------------

The qf_baremetal app tests qf_baremetalsetup which sets up the power
domains and clocks without using the S3X_CLK_XXX management, or the
power management schemes. As a result the code is smaller and simpler,
however all of the responsibility is on the user to get the right power
domains enabled, and the clocks set correctly.

.. _lesson-1a-m4-only--qt_helloworldsw:

Lesson #1a: M4 only – qt_helloworldsw
-------------------------------------

This section describes how to build and run the qt_helloworldsw project.

1.  Navigate to qt_helloworldsw build folder and run make

    ::

       cd Matrix-SDK/qt_apps/qt_helloworldsw/GCC_Project
       make 

2.  | Reset QuickFeather board and press ‘user button’ while blue LED is
      flashing.
    | Should switch to mode where green LED is breathing.
    | If green LED not breathing, press reset again and ‘user button’
      within 5 seconds of releasing reset (while blue LED is still
      flashing)

3.  With green LED breathing, program qf_helloworldsw app into
    QuickFeather:

    ::

       qfprog --port /dev/ttyXX --m4app output/bin/qt_helloworldsw.bin

    replace /dev/ttyXX with the actual device path.

4.  | After programming has completed, reset the QuickFeather board and
      do not press the user button.
    | Blue LED should flash for 5 sec and then load the m4app and run
      it.

5.  Run PuTTY or some other terminal emulator and attach to the
    QuickFeather (NOTE: the port name will most probably be different
    than the port name used for programming).

6.  | You should see a banner that says:
    | |qt_helloworldsw banner|

7.  The prompt ‘[0]’ indicates that you are level 0 in the CLI menus
    system. Type ``diag red`` and you should see the red LED on
    QuickFeather light up

8.  | Type ``help`` and you should see:
    | |qt_helloworld CLI Help|

    Which lists what is in the top-level CLI menu:

    1. diag is a user defined sub-menu
    2. the others are there by default

9.  | Type ``diag`` to enter the diag sub-menu:
    | You should see
    | |qt_helloworld CLI diag|

    Where the [1] diag indicates that you are in a 1st level submenu
    called diag

10. | Type ``help`` to get help for this menu and you should see:
    | |qt_helloworld CLI diag sub-menu|

11. You can try these by typing red (should turn the red led off), green
    and so forth. Note that if you are level 0, you can access submenu
    elements by typing ``submenuname submenu action``, which is what we
    did earlier when we typed ``diag red``

.. _lesson-1b-m4-only--modify-qt_helloworldsw:

Lesson #1b: M4 only – modify qt_helloworldsw
--------------------------------------------

1. | Using the editor of your choice, edit
     ``Matrix-SDK/qt_apps/qt_helloworldsw/src/main.c``. Change the line
   | ``dbg_str(“\n\nHello world !!\n\n”)``
   | to say something else. Save the changes

2. Now naviagte to qt_helloworldsw build folder and run make.

   ::

      cd Matrix-SDK/qt_apps/qt_helloworldsw/GCC_Project  
      make

3. Reset QuickFeather board and press ‘user button’ while blue LED is
   flashing.

   1. Should switch to mode where green LED is breathing
   2. If green LED not breathing, press reset again and ‘user button’
      within 5 seconds of releasing reset

4. With green LED breathing, program the updated qf_helloworldsw app
   into QuickFeather:
   ``qfprog --port /dev/ttyXX --m4app output/bin/qt_helloworldsw.bin``

5. After programming has completed, reset the QuickFeather board and do
   not press the user button.

   1. Blue LED should flash for 5 sec and then load the m4app and run it

6. Run PuTTY or some other terminal emulator and attach to the
   QuickFeather (NOTE: the port name will most probably be different
   than the port name used for programming).

7. You should see a banner and then your changed message.

