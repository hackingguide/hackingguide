---
description: >-
  This product is a locker that uses different type of RFID tags for closing and
  opening a personal locker.
---

# Locker RFID

{% hint style="info" %}
[More information about RFID on the dedicated "Technology" page.](../technology-1/researched-technologies/untitled.md)
{% endhint %}

### Attack scenarios  <a id="docs-internal-guid-3ff257f3-7fff-da25-d793-8eaf5bb546b7"></a>

* Bruteforce RFID UID 
* Card cloning
* Card read and simulate 
* Card write sector 0 \(dos\)

### Real life scenario  <a id="docs-internal-guid-6a79a191-7fff-0996-04ca-bf7b26f938f4"></a>

Card skimming by placing a reader next or on top of the existing reader. To do a **card cloning or card simulate attack.**

A **Bruteforce attack** by trying multiple UID'S on a specific locker. This is not ideal because you have to press a button each time  before you can unlock it. So this is not realistic.

**Dos attack** by rewriting sector 0 on users RFID cards. Only possible with writeable cards.

### Steps

Steps for  a card cloning/Simulate attack.

1. Use card 1 to lock locker
2. Scan card 1 with a rfid reader to get the UID.
3. Write UID of card 1 to card 2
4. Use card 2 to open locker or simulate UID of card 1 to open locker.

#### Proxmark easy Steps

Steps for  a card cloning/Simulate attack using a proxmark.

1. Use card 1 to lock locker
2. Scan card 1 with proxmark  using command **hf search** to get UID and type of card

   ![](../.gitbook/assets/afbeelding%20%286%29.png)

3. Check default keys of Mifare classic card using **hf mf chk \*1 ? \(Go to step 9 for Simulate UID\)**                                                 ![](../.gitbook/assets/afbeelding%20%285%29.png)  **** 

4. Dumping keys for each sector to dumpkeys.bin using command **hf mf nested 1 0 A ffffffffffff d**  

   \*\*\*\*![](../.gitbook/assets/afbeelding%20%282%29.png)\*\*\*\*

5. Create dump file **hf mf dump**   ![](../.gitbook/assets/afbeelding%20%2812%29.png)
6. Get card 2 change UID with command **hf mf csetuid 795f17ad** ![](../.gitbook/assets/afbeelding%20%287%29.png) 
7. Restore data of card 1 to card 2 using command **hf mf restore 1** ![](../.gitbook/assets/afbeelding%20%283%29.png) 
8. Use card 2 to open locker.
9. Simulate card 1 using command **hf 14a sim t 1 u 795f17ad** ![](../.gitbook/assets/afbeelding%20%281%29.png) 
10. Use proxmark to open locker

### DEMO

{% embed url="https://drive.google.com/open?id=1ARkWg0eN\_rc2suxeIY9FSc--V38q1PVN" %}



#### Tools used

* Proxmark easy for read/write and simulate of RFID tags
  * Software and firmware used from [proxmark3 GitHub](https://github.com/Proxmark/proxmark3)
* RFID card 1 M1 S50 13,56 MHZ
* RFID card 2 UID 13,56 MHZ \(clone card\)

