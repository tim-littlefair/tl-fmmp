Process for capturing USB traffic between FenderTone LT Desktop and Fender LT40S amplifier
==========================================================================================

* Start VM up.
* Select USB capture via USBPcap1 (not sure what difference is between 'pcap1 and 'pcap2).
* Plug USB in before powering LT40S
* Power LT40S
* Save capture as lt40s-poweron.json
* Start Fender Tone LT Desktop, press Rescan button, wait for sync to complete
* Save capture as fendertone-synced.json
* Change voices + save capture as lt40s-change-voices.json
  (includes importing non-factory voices from Fender web in auto-audition mode)
* Do backup, activate and de-activate tuner mode + save capture as backup-tuner.json
* Exit from Fender Tone Desktop LT + save capture as fendertone-exit.json

Files are in ../vbox-shared for now - may clean them up and/or filter irrelevant data 
and delete these in the future.

