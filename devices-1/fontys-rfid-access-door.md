---
description: >-
  To access some locations at Fontys you will need to use a student or teacher
  card. These cards utilize RFID to validate if you are allowed access to the
  building.
---

# Fontys RFID access door

{% hint style="info" %}
[More information about RFID on the dedicated "Technology" page.](../technology-1/researched-technologies/untitled.md)
{% endhint %}

## Attack scenarios for RFID

* Card cloning
* Card read and simulate 
* Replay attack
* Reader sniff
* Relay Attack

## Research Fontys RFID door

First we researched what type of RFID card was being used. This was done by using a proxmark3. 

By using the command: **hf search** on the proxmark3  we got the following information:

```text
[=] Checking for known tags...

 UID : ?? 69 ?? 22 ?? 3E ??
ATQA : 03 44
 SAK : 20 [1]
TYPE : NXP MIFARE DESFire 4k | DESFire EV1 2k/4k/8k | Plus 2k/4k SL3 | JCOP 31/41
MANUFACTURER : NXP Semiconductors Germany
 ATS : 06 75 77 81 02 80 02 F0
       -  TL : length is 6 bytes
       -  T0 : TA1 is present, TB1 is present, TC1 is present, FSCI is 5 (FSC = 64)
       - TA1 : different divisors are supported, DR: [2, 4, 8], DS: [2, 4, 8]
       - TB1 : SFGI = 1 (SFGT = 8192/fc), FWI = 8 (FWT = 1048576/fc)
       - TC1 : NAD is NOT supported, CID is supported
[=] Answers to magic commands: NO

[+] Valid ISO14443-A tag  found

```

It turns out to be a mifare desfire card, it is possible to get more information by using the **hf mfdes info** command on the proxmark3:

```text
-- Desfire Information --------------------------------------
-------------------------------------------------------------
  UID                :  ?? 69 ?? 22 ?? 3E ??
  Batch number       : BA 55 50 51 90
  Production date    : week 50, 2014
  -----------------------------------------------------------
  Hardware Information
      Vendor Id      : NXP Semiconductors Germany
      Type           : 0x01
      Subtype        : 0x01
      Version        : 1.0 (Desfire EV1)
      Storage size   : 0x1A (8192 bytes)
      Protocol       : 0x05 (ISO 14443-3, 14443-4)
  -----------------------------------------------------------
  Software Information
      Vendor Id      : NXP Semiconductors Germany
      Type           : 0x01
      Subtype        : 0x01
      Version        : 1.4
      storage size   : 0x1A (8192 bytes)
      Protocol       : 0x05 (ISO 14443-3, 14443-4)
-------------------------------------------------------------
 CMK - PICC, Card Master Key settings

   [0x08] Configuration changeable       : YES
   [0x04] CMK required for create/delete : YES
   [0x02] Directory list access with CMK : NO
   [0x01] CMK is changeable              : YES

   Max number of keys       : 174
   Master key Version       : 0 (0x00)
   ----------------------------------------------------------
   [0x0A] Authenticate      : NO
   [0x1A] Authenticate ISO  : NO
   [0xAA] Authenticate AES  : YES

   ----------------------------------------------------------
   Available free memory on card       : 6208 bytes
-------------------------------------------------------------

```

Now we know that the Fontys student pass is using NXP MIFARE DESFire EV1 cards. This type of card is well protected against card cloning since every data block can have their own authentication key. 

Sniff and replay attack will not be possible since it’s using AES encryption. Each time the data of the card is read it uses a different authentication response. 

### 

### UID Simulation

The card might be protected but if the reader only checks the UID\(unique identifier\) of the card. It will still be possible to gain access. This was tested by using the following proxmark3 command:

**hf 14a sim t 3 u &lt;UID&gt;**

The reader didn't give any response while holding the proxmark3 infront of it. When using a different reader we could see that the proxmark did in fact simulate the correct UID. This shows that the reader isn't checking the UID but instead a different block of data on the card.

 

## Possible attacks on Fontys 

* Creating a custom DESfire card using public read keys
* Relay Attack

### Creating a custom DESfire card using public read keys

Maybe it is possible to create our own Desfire card that is also using a free access key to read file 1 and 2. But, when checking the card we see that there are 3 more applications stored that are not using the free access key in their authentication. Depending on what the door reader is looking for it might not be possible to create a fake card for that door.

### Relay Attack 

A relay attack would require two devices that have the possibility to send data from the reader to the card and back. It would need to be executed from a distance where the attacker could get close to someone thas has a valid card. Another person would then have to be at the reader holding the other end of the device against the reader and thus transferring the data from the victims card, from a distance to open the door. This hasn’t been tested yet.

###  <a id="docs-internal-guid-3776b3d6-7fff-117b-d08a-8c8032c2b5bf"></a>

