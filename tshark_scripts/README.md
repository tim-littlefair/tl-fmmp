# README.md

for scripts originally written by GitHub user @tim-littlefair for parsing Wireshark and 
Android captures of traffic between the following device/application pairs:
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


## Copyright Status

Author: Tim Littlefair\
https://github.com/tim-littlefair\
https://www.linkedin.com/in/tim-littlefair/

To the extent possible, the intent of the author Tim Littlefair
is that this document and the other documents and scripts it describes should become part of the public domain.

The intent in sharing the documents, scripts and snippets in the current folder and its subfolders is to encourage 
and assist GitHub user @spod and other contributors to his 'fmmp' github project (https://github.com/spod/fmmp) 
and/or other free software projects containing code for interoperation with Fender's Mustang modelling amplifier products.

The scripts and documents in this folder are contributed to @spod's fmmp github repository 
by @tim-littlefair.

@spod is welcome to modify these files and continue to use them freely, but these files 
were originally generated as part of a GitHub repository by @tim-littlefair, 
which is presently private, but will hopefully be made public at some time in the near future.
When @tim-littlefair's repository becomes public the scripts and documents in this directory 
(but not the current file README.md) will form part of that repository, and the definitive versions will 
be maintained by @tim-littlefair in his then-public repository.

@spod's permission to modify these files includes permission to amend or remove this 
paragraph and/or @tim-littlefair's copyright notices from the scripts if he chooses to do so 
(note that amendment or removal of the notices does not cause the copyright status of the file 
content to change).

## Capturing USB traffic to and from Fender Mustang LT40S (+other LT-series products)

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

## Capturing Bluetooth traffic to and from Fender Mustang Micro Plus

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

## Capturing USB or Bluetooth traffic to and from other Fender devices

I believe that the processes used here to capture and analyze USB traffic to and from the Mustang LT40S 
device are likely to work for other devices with LT prefix in their model names, including Mustang LT25, LT50,
and the Bass amp marketed as Rumble LT25.  As all of these devices are interoperable with the latest version
of the Fender Tone desktop control application, it is likely that the protocols for these devices have some
similarities with the LT25 protocol, but as the LT40S is more recently released than the other amps in 
this range, it is possible there are some significant differences due to evolution of the vendor's 
code base for device firmware.

I also believe that it is likely that the high end GT- and GTX- range devices, which interoperate 
with the same Fender Tone iOS and Android apps as the Mustang Micro Plus, are likely to use similar 
(not necessarily identical) protocols to the MMP, so the capture/analysis script mode intended for 
MMP for these should work for these as well with a little tweaking.

For older ranges of combo-style modelling amps under the Mustang brand (broadly, products released 
up to around 2015/2016), there is an existing GitHub project [https://github.com/offa/plug]('plug') 
which already supports (some or all?) of these on Linux. The Fender-supplied control application 
for these older devices was called 'Fender Plug' (for Windows and macOS/MacOS/OS-X), but Fender 
no longer seem to have a latest version of this available for download - there are probably sources 
on the web the original installers can downloaded from, but knowing which sources
can be relied upon to deliver a malware-free product is not something I'm prepared to guess on.

Finally, there is the Fender Mustang Micro (without 'Plus' suffix) which has been available for 
a couple of years.  This is similar in form factor to the FMMP, but is designed to be controlled 
entirely through buttons on the device itself and does not interoperate with any control application
at all, mobile or desktop.  I used to have one of these before purchasing the FMMP and passing the 
older model on to someone else, based on my investigations when I had it I do not believe that this
device offers any kind of control application interface either over Bluetooth or USB, and the 
probability of integrating anything external with it is close to zero.


## Why I'm interested in this

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

## Comments on the snippets

The subdirectories 
[./snippets/LT40S](./snippets/LT40S) 
and 
[./snippets/MMP](./snippets/MMP) 
contain some examples of messages related to the message classes mentioned in the 
section above.  

There is also a subdirectory 
[./snippets/LT40S-tl-plug-fork](./snippets/LT40S-tl-plug-fork)
which contains pretty-printed renderings of some JSON documents captured using 
[https://github.com/tim-littlefair/plug/tree/LT40S-support-1019](this fork of the 
older 'plug' control application for Linux mentioned earlier on this page).
The files in this subdirectory are intended as an easier-to-read alternate view
of the JSON which appears at the end of some of the LT40S messages captured using 
the scripts in this project/subdirectory.

For example:
* [./snippets/LT40S-tl-plug-fork/preset01.json](./snippets/LT40S-tl-plug-fork/preset01.json) 
  contains the same content as the 
  JSON which appears from bytes 0x0009 to 0x088a of  
  [./snippets/LT40S/B-fender_tone_requests_all_presets_during_startup/03-report-01.hexdump](./snippets/LT40S/B-fender_tone_requests_all_presets_during_startup/03-report-01.hexdump)
* [./snippets/LT40S-tl-plug-fork/preset02.json](./snippets/LT40S-tl-plug-fork/preset02.json) 
  contains the same content as the 
  JSON which appears from bytes 0x0009 to 0x097f of  
  [./snippets/LT40S/B-fender_tone_requests_all_presets_during_startup/04-report-01.hexdump](./snippets/LT40S/B-fender_tone_requests_all_presets_during_startup/04-report-01.hexdump)
* [./snippets/LT40S-tl-plug-fork/preset60.json](./snippets/LT40S-tl-plug-fork/preset60.json) 
  contains the same content as the 
  JSON which appears from bytes 0x0009 to 0x085c of  
  [./snippets/LT40S/B-fender_tone_requests_all_presets_during_startup/63-report-01.hexdump](./snippets/LT40S/B-fender_tone_requests_all_presets_during_startup/63-report-01.hexdump)  

  Unfortunately, when I generated the 'easier to read' pretty-printed JSON for the presetXX.json 
  files, the parameters I chose caused the JSON keys at each level to be sorted lexicographically
  making it harder to do a comparison between the .hexdump and .json files because the order
  of keys within the JSON is different.  I do believe that these pairs of files are likely to be 
  exactly equivalent in content though.

Having looked at the snippets (and the older JSON captured during work on the plug fork), 
I have a few observations:

### Application/Device Dialog

In both cases, the conversation protocol between the control applications and the device
uses unsolicited transmission in both directions.  The device never seems to spontaneously
send a message to the application, but a single message from the application to the device
can give to zero, one or more unsolicited messages being delivered in response.

In the naming convention for the dumped package data, I have chosen to call messages from 
the control application to the device 'command' and messages from the device to the 
control application 'report'.

### Packet framing and message reassembly

Both devices use a framing mechanism for reassembling limited-length packets into messages
which can exceed the packet length indicator bytes are used:
* 0x35 signifies the last packet in a message (which may also be the first)
* 0x33 signifies the first packet in a message made up of multiple packets
* 0x34 signifies a packet which is neither the first nor the last in a multi-packet message.

The format of the packet header which contains these indicators differs between the LT40S/USB
and MMP/Bluetooth transports:
* in LT40S/USB, every packet is 64 bytes long, and contains a packet header which is either 
  2 or 3 bytes bytes long.
  * For packets sent from the control application to the device, the last/first/middle indicator 
    is byte 0, byte 1 contains the length of the packet not including the 2 byte header.
  * For packets sent from the device to the control application, byte 0 is always 0, byte 1 is
    the last/first/middle indicator, and byte 2 contains the length of the packet not including 
    the 3 byte header
  * For first and middle packets from the device to the control app, the packet length is always 
    0x3d (i.e. 64 bytes - the 3 bytes consumed by the header).  For the last packet, the packet 
    length tells the receiver to ignore bytes beyond that offset, even though the packet will 
    always be exactly 64 bytes long.
  * I haven't found an example of a multi-packet message from the control app to the device, but
    I would expect that in first/middle packets of such a message, the packet length will be 0x3e
    (i.e. 64 byts - the 2 bytes consumed by the header).
  * In the Wireshark dump, the packet data appears in elements with the tag 'usbhid.data'.
* in MMP/Bluetooth, the packets vary in length up to a maximum of 120 bytes, and contains a 
  similar 3 byte packet header
  * Whether the packet is from control application to device or in the opposite direction, byte 0
    contains the last/first/middle indicator, byte 1 is always zero, and byte 2 contains the 
    length of the packet not including the 3 byte header.
  * For first and middle packets, the packet length is always 0x75 (i.e. 120 bytes - the three bytes 
    consumed by the header).  For last packet, the length redundantly agrees with the position of 
    the end of the packet.
  * In the Wireshark dump, the packet data appears in elements with the tag 'btatt.value'.  The direction 
    of packet flow is not evident from the packet content, but there are a number of other 
    tags in the dump which indicate it - the most appropriate one to rely on is probably 
    'hci_h4.direction'.

Note that the data covering the packet framing notes above has been trimmed off and thrown away 
by the time the script exports the NN-command-... and NN-report-nn..., but small samples of packet-level 
data which allow the patterns described above to be seen are captured in the following files:
* [snippets/LT40S/packet_sample.csv](snippets/LT40S/packet_sample.csv)
* [snippets/MMP/packet_sample.csv](snippets/MMP/packet_sample.csv)

These CSV files are extracts for longer files generated by the parse_captures.py script for any given
.pcapng or .zip file processed.

### Use of protobuf

While 
[work in @brentmaxwell's LtAmp project](https://github.com/brentmaxwell/LtAmp/blob/main/Docs/Protocol.md)
and @spod's work in this project show that Fender's device firmware and control 
applications use make some use of Google's protobuf framework, I'm not convinced 
that protobuf is being used consistently across the whole protocol for either 
device, and it doesn't look to me like there is a single consistent protobuf model 
of the protocol underlying both the Bluetooth and USB control applications evolving 
in anything like a backward-compatible manner which would allow the latest model to 
be used for older device firmware versions.  

Note that @brentmaxwell's file
[FenderMessageLT.proto](https://github.com/brentmaxwell/LtAmp/blob/main/Schema/protobuf/FenderMessageLT.proto)
includes an enumeration which appears to cover the tags used at the top-level to identify 
message types in the LT protocol - I can't see anything which gives a comparable enumeration 
in the .proto files captured by @spod, so knowing which .proto to use to decode a specific 
message type is likely to be a manual task, and maybe harder than working out the function 
of the message by looking at its structure and content.  Most messages are either quite 
short (i.e. <= 10 bytes long) or contain a long JSON component (compressed or not) wrapped
in a message which is short once the JSON is pulled out.

So far, I've chosen not to use any of the protobuf material from either source in my analysis, 
although I am occasionally running 'protoc --decode_raw' on subranges of the exported .bin 
renderings of messages to see if that provides any insight into the message structure.

### How does the control app retrieve the preset state of the device?

With the LT40S/USB protocol, the desktop Fender Tone control app sends a sequence of 60 
commands during startup, and the amp responds to each of these with a report containing 
a full JSON description of a single preset (identified by an index in the command).  There
is also one more command with a different opcode which receives a similar report in 
response, I think that this is getting the current active preset (possibly including 
unsaved state associated with that preset).

With the MMP/Bluetooth protocol, there is a single command sent by the mobile Fender Tone
control app during startup for which a report is received in response containing only the 
only of the 100 presets stored in the device.  When a preset is made active through the 
control app it requests the compressed JSON description of that preset.

### Use of lz4 compression on JSON preset data

@spod's efforts reverse engineering the contents of a Fender Tone APK download show that
the LZ4 library is present, and I think that in the MMP protocol, LZ4 is probably being
used to compress JSON preset data (possibly as a layer of obfuscation rather than in 
order to reduce the on-air transmission load).   

Comparing the following messages:

* [the LT40S message 03-report-01.hexdump](./snippets/LT40S/B-fender_tone_requests_all_presets_during_startup/04-report-01.hexdump); and
* [the MMP message 15-report-02.hexdump](./snippets/MMP/C-fender_tone_requests_compressed_preset_when_preset_activated/15-report-02.hexdump)

it is clear that much of the MMP message is in a similar JSON format to the LT40S one, but 
there are some sections which are replaced by sequences of non-printable data which does decode
cleanly as either ASCII or UTF-8.  
From reading material on the lz4 algorithm, I suspect that lz4 has been used with an external 
dictionary of byte sequences to replace, and this accounts for the non-UTF-8
passages.  Hopefully it will be possible to reconstitute the dictionary and recover parseable 
JSON relatively easily.














