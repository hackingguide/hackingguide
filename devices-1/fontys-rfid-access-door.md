---
description: >-
  To access some location at Fontys you need to use a student or teacher card.
  These cards are using RFID to see if you are allowed to gain access to the
  building.
---

# Fontys RFID access door

## Attack scenarios for RFID

* Card cloning
* Card read and simulate 
* Replay attack
* Reader sniff
* Relay Attack

## Research Fontys RFID door

First we researched what type of RFID card was being used. This was done using a proxmark3. 

using command hf search on the proxmark3  we got the following information

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

Now it is a mifare desfire card it is possible to see more information using the hf mfdes info command on the proxmark3:

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

Now we know that the fontys student pass is using NXP MIFARE DESFire EV1 cards. This card is very strong against card cloning since every data block can have there own authentication key.  

Sniff and replay attack not possible since it is using AES encryption method. Each time the data of the card is read it using different authentication response. 

### UID Simulation

The card might be protected but when the reader would only check the UID\(unique identifier\) of the card. It would then still be possible to get access. This was tested by using the proxmark3 command: 

**hf 14a sim t 3 u &lt;UID&gt;**

The reader didn't show any response when holding the proxmark3 infront of the reader. When using a different reader we could see that the proxmark did simulate the correct UID. This shows us that the reader isn't checking the UID but different block of data in the card. 

 

## Possible attacks on Fontys 

* Creating a custom DESfire card using public read keys
* Relay Attack

### Creating a custom DESfire card using public read keys

Maybe it is possible to create our own Desfire card that is also using a free access key to read file 1 and 2. But when looking on the card there are 3 more applications stored that are not using the free access key in there authentication. Depending on where the door reader is looking for it might not be possible to to make a fake card for that door

### Relay Attack 

Relay attack would contain 2 devices that have the possibility send the data from the reader to the card and back. From a distance where the attacker would need to get close to someone with student or teacher card. Then an other person would need to be at the reader holding the other end of the device against the reader and transferring the data from the student/teacher card from a bigger distance to open the door. This isn't tested.

###  <a id="docs-internal-guid-3776b3d6-7fff-117b-d08a-8c8032c2b5bf"></a>

