README.md
=========

for Tim Littlefair's scripts for parsing Wireshark and Android captures of 
traffic between the following device/application pairs:
* Fender Mustang LT40S/Fender Tone Desktop 1.5 running inside a VirtualBox Windows 10 VM
* Fender Mustang Micro Plus/Fender Tone running on Android 8.1 Nexus 7X.



Files in this folder include:
* documents about how to capture USB and Bluetooth traffic between the two device/application 
  pairs mentioned above
* source code for rendering some of the content of the traffic recorded into more 
  understandable, formats, using the 'tshark' utility to convert wireshark format dumps to text 
  reports, and python to process the text reports further
* snippets of the (hopefully) more understandable output of the analysis source code

This folder does not include any samples of the raw capture data processed by these scripts,
due to concern about leaking serial numbers, Bluetooth addresses etc of devices belonging to 
the author.  


Copyright Status
----------------
Author: Tim Littlefair (https://github.com/tim-littlefair)

To the extent possible, the intent of the author Tim Littlefair 
is that this document and the other documents and scripts it describes should become part of the public domain.

The intent in sharing the documents, scripts and snippets is to encourage and assist @spod and other 
contributors to his repository and/or other repositories containing code for interoperation
with Fender's Mustang modelling amplifier products.

The scripts and documents in this folder are contributed to @spod's fmmp github repository 
by Tim Littlefair.

@spod is welcome to modify them and continue to use them freely, but these files 
were originally generated as part of a GitHub repository by Tim Littlefair, 
which is presently private, but will hopefully be made public at some time in the near future.
When Tim Littlefair's repository becomes public the scripts and documents in this directory 
(but not the current file README.md) will form part of that repository, and the definitive versions will 
be maintained in that repository by Tim Littlefair. 

@spod's permission to modify these files includes permission to amend or remove this 
paragraph and/or Tim Littlefair's copyright notices from the scripts if he chooses to do so 
(note that amendment or removal of the notices does not cause the copyright status of the file 
content to change).

Capturing USB traffic to and from Fender Mustang LT40S (+other LT-series products)
----------------------------------------------------------------------------------
The control application provided by Fender for these devices is called Fender Tone Desktop, 
with versions available for Windows 10 and macOS available from links on this page: 

https://support.fender.com/en-us/knowledgebase/article/KA-02086

I wrote a very short reminder to myself how to do Wireshark captures using a VirtualBox
Windows 10 virtual machine some time ago.  Unfortunately I no longer have access to that
VM, so it's missing some details (e.g. whether to install Wireshark in the VM or the host).

[./virtual-box-capture-process.md](./virtual-box-capture-process.md)

I have since done captures in macOS without a VM - they were relatively easy, but I did 
need to discover the name of the network interface which USB traffic flows over (I think
this might have been a virtual ethernet connection automatically established over the 
USB).

The captures can be saved in Wireshark's (current) native format with a '.pcapng' extension,
or Wireshark can be used to interactively filter them and export in other formats including 
JSON.  When I originally did these captures, I exported them in native format and in JSON 
and then looked at the JSON, but the parse_captures.py script in this directory use the 
command line tool 'tshark' provided by the Wireshark developers to extract various views
from the .pcapng format.

Capturing Bluetooth traffic to and from Fender Mustang Micro Plus
-----------------------------------------------------------------
The control application provided by Fender for these devices is called Fender Tone, available for Android and iOS:

https://play.google.com/store/apps/details?id=com.fender.tone

https://apps.apple.com/au/app/fender-tone/id1174113426

On a Nexus 5X running Android 8.1 (with Android Developer Mode enabled, obviously), I was able to 
capture Bluetooth traffic between Mustang Micro Plus by following the directions here:

https://source.android.com/docs/core/connect/bluetooth/verifying_debugging#debugging-with-logs

Once the steps above have been done to capture logs on the phone, they can be pulled off to 
a workstation using the adb 'bugreport' command.  

Issuing the command 'adb bugreport' on a workstation/laptop with a USB debugging 
connection to the device will cause the capture of a file with a name like 
'bugreport-bullhead-OPM7.181205.001-2024-12-24-17-35-25.zip'.

Within that zip file there should be a subdirectory 'FS/data/misc/bluetooth/logs' containing two 
files, called 'btsnoop_hci.log.last' and 'btsnoop_hci.log'.  The file ending .log should contain 
a few minutes worth of the most recent Bluetooth traffic, the one ending .log.last should contain 
earlier traffic going back considerably further (possibly to last time device was powered on or Bluetooth
was turned on).  These files are in a binary format, but also are automatically recognized by Wireshark/tshark.

Why I'm interested in this
--------------------------

These scripts have been produced as part of an exercise to work on software interoperable 
with the Fender devices which I own which provide an alternative to the control software provided
by Fender.  This work will remain under wraps until I have a proof of concept I'm happy to let other
people see and play with.  For the moment I'll use the codename 'craig' to refer to this project if 
I need to.

The itches I want to scratch for craig, for both the LT40S and Micro Plus are:

* For both devices, it is hard to change presets quickly, especially 
  if their index numbers are a long way apart.
  I'm hoping I can build something which offers a view of up to 
  12 favourite presets in a grid, from which the player can switch
  to any item with a single action.

* I'm a strong believer in configuration management, and for being
  able to do a factory reset on any device I own and then restore 
  it to a preferred known state quickly.  Both control apps allow
  states to be backed up into Fender's servers, but it feels like I can 
  only have one backed up state per device (maybe I can have more
  if I register for separate accounts with different emails).  Having
  a capability to keep the saved state library on my own storage
  appeals strongly to me.


Given these interests, I'm only really interested in a small subset of 
the different classes of messages exchanged between the Mustang and
its control app, particularly:

* messages involved in the startup process, enabling the control 
  app to know firmware version, serial number etc.
* messages which are used by the control application to activate
  a different preset
* messages which allow preset states to be backed up and restored


The subdirectories 
[./snippets/LT40S](./snippets/LT40S) 
and 
[./snippets/MMP](./snippets/MMP) 
contain some examples of messages related to these classes.  I may add some
commentary on these examples in a future version of this document.




  















