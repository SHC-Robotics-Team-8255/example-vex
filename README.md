#Example Program for Vex#

Do not edit and use this program.  Copy it into a new folder and make
that a new program instead.

##Making Your Own Program##

In the repo directory make a new directory for your code and then copy the example into it as a starting point.  If you wanted your program to be called testvex, you would:

   mkdir testvex
   cd testvex
   cp -Rv ../example-vex .

You would then edit the Makefile and make two important changes.
Change the name of your  project from the default of "output" to a new name.  Change this:

```
# Define project name here
ifeq ($(PROJECT),)
PROJECT  = output
endif
```

to instead be:

```
# Define project name here
ifeq ($(PROJECT),)
PROJECT  = testvex
endif
```

using the name of your directory as the name instead of output.  This
will embed that name into the program allowing you to see it on the robot using the shell (described later).

The other change you need to make is to use the right serial port.  You will need to dete\
ct what serial port device your computer assignes.  Run this command:

    ls -al /dev/tty* >a
    (plug in the USB programmer connected through the remote control to the robot)
    ls -al /dev/tty* >b
    diff a b

You will see something like this:

    5a6
    > crw-rw-rw-  1 root      wheel   18,  18 Nov 24 12:39 /dev/tty.usbmodem14161
    55c56

The name of the serial port is the /dev/tty.usbmodem14161 part.  Now
go edit the Makefile to make that be the defined serial port.  Change this:

```
SERIAL_PORT = /dev/ttyACM0
```

into the port you detected:

```
SERIAL_PORT = /dev/tty.usbmodem14161
```

Test that it works by connecting your terminal to the robot:

     make connect

You may see some garbage letters or nothing for a moment.  Hit the "Enter" key a few time\
s and you should get a prompt that looks like this:

    ch>

##Create a Repository##

Go to github.com and create a repository.  In the terminal in your new
directory with your edited code follow the instructions on the github
new repo page to get your code into the repo.

###Remember to make a git ignore file##

Copy the file from the example folder... it likely got missed in your
copy above:

     cp ../example-vex/.gitignore .
     git add .gitigore
     make clean
     git commit -am"added an ignore file"
     git push origin master

This will keep the binary files out of your source repo if you forget
to do a 'make clean' before commiting.

##Editing Code###

The only file you need to edit for code is vexuser.c.  You can use any
editory you like.  Common ones are nano, emacs, sublime, xcode, etc.  Make
sure you don't use a word processor.  Code files are simple text only.

##Using Git for Source Control##

After you have edited your code you want to make sure you take a
snapshot of it so that you can undo it if you need to.  This is called
a commit.  Do this:

     cp ../example-vex/.gitignore .
     git add *
     make clean
     git commit -am"<put some description here>"
     git push origin master

Your code will be saved in the local repo and stored in github.com.
You now have a backup and a history!

##Compiling##

To compile just type:

   make

##Installing Code on Robot##

To install:

   make install

You should see something like this:

```
make install
/usr/local/bin/cortexflash -X -w bin/output.hex -g 0x0 /dev/tty.usbmodem14161
VEX cortex flash loader
Working directory /Users/gherlein/herlein/src/vex/vexilla/src/example-vex

Using Parser : Intel HEX
Send system status request
Status AA 55 21 0A 04 19 04 19 51 EB 03 10 00 03 
Connection       : USB Tether
Joystick firmware: 4.25
Master firmware  : 4.25
Joystick battery : 4.78V
Cortex battery   : 13.86V
Backup battery   : 0.18V

Send bootloader start command
Version      : 0x22
Option 1     : 0x00
Option 2     : 0x00
Device ID    : 0x0414 (High-density)
RAM          : 64KiB  (512b reserved by bootloader)
Flash        : 384KiB (sector size: 2x2048)
Option RAM   : 15b
System RAM   : 2KiB

40536 bytes to transfer
0....10....20....30....40....50....60....70....80....90....100
Transfer time   7.70 seconds, data rate  5267 bytes/sec

Starting execution at address 0x08000000... 
done.

make: *** [install] Error 1
```

You can ignore that "[install] Error 1" - it's not real.

##Shell Commands on the Robot##

You can inspect the robot by connecting to it using the shell (see
above).

You can see what commands are available by typing 'help' but the command you want to use \
to inspect the robot is 'info' which will give you a lot of good data:

```
ch>
ch>
ch> info
Kernel:       2.6.2
Compiler:     GCC 5.4.1 20160919 (release) [ARM/embedded-5-branch revision 240496]
Architecture: ARMv7-M
Core Variant: Cortex-M3
Port Info:    Advanced kernel mode
Platform:     STM32F10x Performance Line High Density
Board:        VEX CORTEX
ConVEX:       1.0.4
Build time:   Nov 24 2016 - 20:23:19
Project:      testvex
Git Hash:     4fe44de756b66c709344878c0adac0b1cec89600
```

The parts that will be the most useful are the Build time (when the program was compiled)\
, the Project (the name you gave it in the Makefile) and the Git hash.

The git hash is very valuable since it can be used to verify that the program running on \
the robot is the code in your source folder.  You can cross-check like this:  in your cod\
e directory in a terminal type this:

    git log

You will see something like this:

```
commit 4fe44de756b66c709344878c0adac0b1cec89600
Author: Greg Herlein <gherlein@herlein.com>
Date:   Thu Nov 24 12:22:36 2016 -0800

    fixed git hash

```

You can compare the commit hash (4fe44de756b66c709344878c0adac0b1cec89600) to the hash in\
 the info command.  If they are the same then you know what code is running.  If they are\
 not then you either did not load that code into the robot or you forgot to do a git comm\
it before compiling.  Either way you know you need to fix it.  You are no longer unsure w\
hat code is running on the robot.

