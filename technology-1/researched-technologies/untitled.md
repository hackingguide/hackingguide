---
description: >-
  Radio-frequency identification (RFID) utilizes electromagnetic fields to
  automatically identify and track tags attached to objects.
---

# RFID

###  Possible attacks <a id="docs-internal-guid-3ff257f3-7fff-da25-d793-8eaf5bb546b7"></a>

* Bruteforce RFID UID\(Unique Identifier\)
* Card cloning
* Card Simulation
* Permanently disabling tags
* Relay attack
* Reader sniff/snoop attack
* Buffer overflows
* Malicious code injection

## Starting steps <a id="docs-internal-guid-c9c5e61f-7fff-b10a-03b0-279c76554e83"></a>

1. Try to identify what type of RFID tags are being used.
2. See what type of attacks are possible for that specific RFID tag.
3. Does The reader support multiple type of tags?
4. Check if the reader only validates by using UID \(Unique Identifier\)
   * If so it might be possible to only simulate UID to gain access.

## Tags possible for cloning to <a id="docs-internal-guid-c9c5e61f-7fff-b10a-03b0-279c76554e83"></a>

* **UID**
  * Gen1A UID write attack
* **CUID**
  * Gen2 UID write attack
  * Some ads say "write once", hinting that the card is not fused block0 from factory. I.e. supports one block0 change.
* **FUID**
  * Write Once card, it doesn't say if this is a genuine unfused card for factory or if it's a custom one.
  * Used to counter the "anti-elevator" systems. Some posts on forums talk about “broken tags” after being used on elevators. 
* **UFUID**
  * Suggest one-time card, to counter the "anti-elevator" systems.

## Tool <a id="docs-internal-guid-174f3741-7fff-b427-2e9b-787e0a050dbe"></a>

### PROXMARK

Proxmark is an RFID tool for interacting with different RFID tags.  


#### Different Proxmark devices

* [Proxmark ](https://proxmark.com/proxmark-3-hardware/proxmark-3)
* [Proxmark 3 RDV 2](https://proxmark.com/proxmark-3-hardware/proxmark-3-rdv-2)
* [Proxmark 3 RDV4](https://proxmark.com/proxmark-3-hardware/proxmark-3-rdv4)
* [Proxmark 3 EVO](https://proxmark.com/proxmark-3-hardware/proxmark-3-evo)
* [Proxmark 3 Easy](https://proxmark.com/proxmark-3-hardware/proxmark-3-easy)

#### Software

**​**[Proxmark3](https://github.com/Proxmark/proxmark3/wiki) is an open source project. Proxmark3 is the software you will need to interact with the proxmark. There is a lot of information available on how to install the software and flash your proxmark in order to update the firmware to a different version. The information on how to use the proxmark with different devices is on their[ wiki](https://github.com/Proxmark/proxmark3/wiki).

#### Commands

Basic card info commands:

* HF search - High frequency card information
* LF search - Low frequency card information

UID simulation commands:

* hf mf sim u &lt;UID&gt; - Simulating mifare classic card 
* hf 14a sim t &lt;type of card&gt; u &lt;UID&gt; - Simulating different ISO14443A RFID card 

### Written guides using RFID

{% page-ref page="../../devices-1/fontys-rfid-access-door.md" %}

{% page-ref page="../../devices-1/locker-rfid.md" %}



