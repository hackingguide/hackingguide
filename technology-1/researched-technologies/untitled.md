---
description: >-
  Radio-frequency identification (RFID) uses electromagnetic fields to
  automatically identify and track tags attached to objects.
---

# RFID

###  Possible attacks <a id="docs-internal-guid-3ff257f3-7fff-da25-d793-8eaf5bb546b7"></a>

* Bruteforce RFID UID\(Unique IDentifier\) 
* Card cloning
* Card Simulation
* Permanently disabling tags
* Relay attack
* Reader sniff/snoop attack
* Buffer overflows
* Malicious code injection

## Starting steps <a id="docs-internal-guid-c9c5e61f-7fff-b10a-03b0-279c76554e83"></a>

1. Try to identify what for type of RFID tags are being used
2. See what type of attacks are possible for that RFID tag
3. Does The reader support multiple type of tags?
4. Check if the reader only validate by using UID \(Unique IDentifier\) 
   1. If so it might be possible to only simulate UID to gain access

## Tags possible for cloning to <a id="docs-internal-guid-c9c5e61f-7fff-b10a-03b0-279c76554e83"></a>

* **UID**
  * Gen1A UID write attack
* **CUID**
  * Gen2 UID write attack
  * Some ads says "write once" hinting that the card is not fused block0 from factory.  ie support one block0 change.
* **FUID**
  * Write Once card, it doesn't say if this is a unfused genuine card for factory or if its a custom one.
  * Used to counter the "anti-elevator" systems. Some posts on forum suggests broken tags after used on elevators. 
* **UFUID**
  * Suggest one-time card, to counter the "anti-elevator" systems.

## Tool <a id="docs-internal-guid-174f3741-7fff-b427-2e9b-787e0a050dbe"></a>

### PROXMARK

Proxmark is an RFID tool for interacting with different RFID tags.  


#### Different Proxmark devices

* [Proxmark 3](https://proxmark.com/proxmark-3-hardware/proxmark-3)
* [Proxmark 3 RDV4](https://proxmark.com/proxmark-3-hardware/proxmark-3-rdv4)
* [Proxmark 3 EVO](https://proxmark.com/proxmark-3-hardware/proxmark-3-evo)
* [Proxmark 3 RDV 2](https://proxmark.com/proxmark-3-hardware/proxmark-3-rdv-2)
* [Proxmark 3 Easy](https://proxmark.com/proxmark-3-hardware/proxmark-3-easy)

#### Software

[Proxmark3 ](https://github.com/Proxmark/proxmark3/wiki)is open source project. Proxmark3 is the software you will need to interact with proxmark. There is a lot of information on how to install the software and flash your proxmark to updated it to the firmware to a different version.  The information on how to use the proxmark  with different devices is on there [wiki](https://github.com/Proxmark/proxmark3/wiki). 

#### Commands

Basic card info commands:

* HF search - High frequestion card information
* LF search - Low frequestion card information

UID simulation commands:

* hf mf sim u &lt;UID&gt; - Simulating mifare classic card 
* hf 14a sim t &lt;type of card&gt; u &lt;UID&gt; - Simulating different ISO14443A RFID card 

