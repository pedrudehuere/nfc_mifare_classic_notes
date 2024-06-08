---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Hacking my laundry cards

Here I describe how I hacked my laundry card, the resulting software can be found here:\
[https://github.com/pedrudehuere/laundry\_card\_topup](https://github.com/pedrudehuere/laundry\_card\_topup)

## Hardware setup

TODO

## Read the card's content

My laundry card is a MIFARE Classic 1K, fortunately MIFARE Classic cards use the [Crypto-1](https://en.wikipedia.org/wiki/Crypto-1) encryption which has been broken in 2009, there are several tools which allow to extract the secret keys from those cards.

The repository of [nfc-tools github project](https://github.com/nfc-tools) contains a lot of interesting things

## Analyse the content of a card

### Card with a credit of 0.-

This is an example of the content of the card that has a credit of 0.- (not very useful for our case), this card has been used until its credit was depleted.

{% code fullWidth="true" %}
```

                                                                                Access bits
---- Sector 0
                                                                                R  W  I  D
00000000: 24ad 8e3c 3b88 0400 c808 0020 0000 0020  $..<;...... ...      # Data  AB B
00000010: 970f 0200 0000 0000 0000 0000 0000 0000  ................     # Data  AB B
00000020: 0000 0000 0000 0000 0000 0000 0000 0200  ................     # Data  AB B

                                                                                RA WA Rab Wab RB WB
00000030: a0a1 a2a3 a4a5 7877 8800 ffff ffff ffff  ......xw........     # Keys     B  AB  B      B
																		
---- Sector 1
                                                                                R  W  I  D
00000040: e903 aa02 0000 0000 0000 0000 0000 0001  ................     # Data  AB
00000050: 0000 0000 e520 0000 0000 0000 0000 0000  ..... ..........     # Data  AB B
00000060: 0000 0000 ffff ffff 0000 0000 00ff 00ff  ................     # Value AB B  B  AB      ---+
                                                                                                    |
                                                                                RA WA Rab Wab RB WB |
00000070: f134 99e6 dddd a1e7 8500 d13f a084 2e9a  .4.........?....     # Keys        AB  B         |
                                                                                                    + Same access bits
---- Sector 2                                                                                       |
                                                                                R  W  I  D          |
00000080: 0000 0000 ffff ffff 0000 0000 00ff 00ff  ................     # Value AB B  B  AB      ---+
00000090: 0000 0000 0000 0000 0000 0000 0000 0000  ................     # Data  AB AB AB AB
000000a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................     # Data  AB AB AB AB

                                                                                RA WA Rab Wab RB WB
000000b0: f134 99e6 dddd e697 8100 d13f a084 2e9a  .4.........?....     # Keys        AB  B

---- Sectors 3-15 - Unused
000000c0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000d0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000e0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000f0: 8e1e bc1b a61e ff07 8000 ffff ffff ffff  ................
00000100: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000110: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000120: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000130: ba33 3d2a def0 ff07 8000 ffff ffff ffff  .3=*............
00000140: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000150: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000160: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000170: 8e75 032b 2c68 ff07 8000 ffff ffff ffff  .u.+,h..........
00000180: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000190: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001b0: 56c4 436f 8205 ff07 8000 ffff ffff ffff  V.Co............
000001c0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001d0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001e0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001f0: c8e0 31a4 1f4e ff07 8000 ffff ffff ffff  ..1..N..........
00000200: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000210: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000220: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000230: ec6a 5e7f d480 ff07 8000 ffff ffff ffff  .j^.............
00000240: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000250: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000260: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000270: 9a06 6e51 8ac7 ff07 8000 ffff ffff ffff  ..nQ............
00000280: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000290: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000002a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000002b0: c2eb c191 6a20 ff07 8000 ffff ffff ffff  ....j ..........
000002c0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000002d0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000002e0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000002f0: 7c35 42d3 fdda ff07 8000 ffff ffff ffff  |5B.............
00000300: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000310: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000320: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000330: 798b a51e dca8 ff07 8000 ffff ffff ffff  y...............
00000340: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000350: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000360: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000370: 1fd1 9ccb 8445 ff07 8000 ffff ffff ffff  .....E..........
00000380: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000390: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000003a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000003b0: 391b f7e7 47b5 ff07 8000 ffff ffff ffff  9...G...........
00000380: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000390: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000003a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000003b0: 391b f7e7 47b5 ff07 8000 ffff ffff ffff  9...G...........

```
{% endcode %}

#### Takeaways from card content

* The first 4 Bytes contain the card ID `0x24ad8e3c`
* The laundry's card reader displays this ID when inserting the card, it displays it as `1,015,983,396` which is the card ID interpreted as an int32, little-endian
* Blocks at `0x60` and `0x80` are value blocks (according to the access bits), the value on this type of blocks can be changed with increment and decrement MIFARE commands (no idea how this is better than just write the data to them). This might indicate that the credit is stored in these blocks
* Since the credit on the card is 0.-, there's not much to analyse further

### Card with a credit of 3.90

Here's the content of a card with a credit of 3.90. Only the first 3 sectors are interesting (the access bits are omitted since they are the same as in the above card)

{% code fullWidth="false" %}
```
---- Sector 0
                                                                   
00000000: c45c eced 9988 0400 c817 0020 0000 0021  .\......... ...!
00000010: 970f 0200 0000 0000 0000 0000 0000 0000  ................
00000020: 0000 0000 0000 0000 0000 0000 0000 0200  ................
                                                                   
00000030: a0a1 a2a3 a4a5 7877 8800 ffff ffff ffff  ......xw........

---- Sector 1
                                                                   
00000040: e903 aa02 0000 0000 0000 0000 0000 0001  ................
00000050: 0000 0000 e520 0000 0000 0000 0000 0000  ..... ..........
00000060: 8601 0000 79fe ffff 8601 0000 00ff 00ff  ....y...........
                                                                   
00000070: 4a31 b5cf 9fea a1e7 8500 97a4 e7fe 952c  J1.............,

---- Sector 2
                                                                   
00000080: 8601 0000 79fe ffff 8601 0000 00ff 00ff  ....y...........
00000090: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
                                                                   
000000b0: 4a31 b5cf 9fea e697 8100 97a4 e7fe 952c  J1.............,
```
{% endcode %}

This is the difference of the dump of the above card (first item) and the card with a credit of 3.90 (`#comments` added manually). I possess several of these cards, the diffs are all similar to this one.

```diff
1c1  # ID (first 4 Bytes) + manufacturer data (read-only)
< 00000000: 24ad 8e3c 3b88 0400 c808 0020 0000 0020  $..<;...... ... 
---
> 00000000: c45c eced 9988 0400 c817 0020 0000 0021  .\......... ...!

7,9c7,9  # The card credit (at 0x60 and 0x80) and the key of sector 1 (0x70)
< 00000060: 0000 0000 ffff ffff 0000 0000 00ff 00ff  ................
< 00000070: f134 99e6 dddd a1e7 8500 d13f a084 2e9a  .4.........?....
< 00000080: 0000 0000 ffff ffff 0000 0000 00ff 00ff  ................
---
> 00000060: 8601 0000 79fe ffff 8601 0000 00ff 00ff  ....y...........
> 00000070: 4a31 b5cf 9fea a1e7 8500 97a4 e7fe 952c  J1.............,
> 00000080: 8601 0000 79fe ffff 8601 0000 00ff 00ff  ....y...........

12c12  # The key of sector 2
< 000000b0: f134 99e6 dddd e697 8100 d13f a084 2e9a  .4.........?....
---
> 000000b0: 4a31 b5cf 9fea e697 8100 97a4 e7fe 952c  J1.............,
```

#### What is the same

* All the access bits
* All keys except for sectors 1 and 2, this means that we can tell `miLazyCreacker` that we already know some keys by creating the file `mfc_<cardID>_foundKeys.txt` before launching it, this way we greatly reduce the keys to be cracked (and thus the time to crack a card)

#### What differs between cards

* The card ID and the manufacturer data in block 0x00, this is to be expected
* Keys (both A and B) for sectors 1 and 2, fair enough
* Content of sectors 1 and 2, OK, the credit must be contained in these sectors

### The card's credit

This is the interesting part, since we want to modify the card's credit (for research purposes of course) the question is: "Where and how is the credit written on the card?" and "Are there other values that depend on the credit amount (like CRC, write counters, etc.)?".

We have seen the differences between cards with a different credit and we know the credit in the cards (the one with 0.- credit is not very interesting in this case).

Knowing that the credit  is 3.90 we can search for the place where it is written. Let's assume that the credit is written as an amount of cents (this allows to use integer numbers) as 32 bit little-endian (because this is how the ID of the card is interpreted) and we get `0000 8601`, and there it is, in the blocks at `0x60` and `0x80`.

```
Credit = 3.90 
       => 390 cents
       => 0x0186
       => 8601 0000 (32 bit little-endian)
 
```

The value immediately after the amount is `0xffffffff - credit`

```
0xffffffff - 0x186 = 0xfffffe79
                   => 79fe ffff (32 bit little-endian)
```

In fact, the blocks containing the card credit are formatted as [value blocks](broken-reference)

Let's recap what we know about the card content so far (ASCII representation omitted since useless), `USAC` = Unknown but same on all cards:

{% code fullWidth="true" %}
```
---- Sector 0
                    BCC (one Byte)
           card ID   | manufacturer data (read-only)
          |-------| |||-------------------------|
00000000: c45c eced 9988 0400 c817 0020 0000 0021
00000010: 970f 0200 0000 0000 0000 0000 0000 0000  # USAC
00000020: 0000 0000 0000 0000 0000 0000 0000 0200  # USAC
00000030: a0a1 a2a3 a4a5 7877 8800 ffff ffff ffff  # keys and access

---- Sector 1

00000040: e903 aa02 0000 0000 0000 0000 0000 0001  # USAC
00000050: 0000 0000 e520 0000 0000 0000 0000 0000  # USAC


          credit    ~credit   credit    address, not used?
           |         |         |         |
          |-------| |-------| |-------| |-------|
00000060: 8601 0000 79fe ffff 8601 0000 00ff 00ff
                                                 
00000070: 4a31 b5cf 9fea a1e7 8500 97a4 e7fe 952c  # keys and access bits

---- Sector 2
                                                 
00000080: 8601 0000 79fe ffff 8601 0000 00ff 00ff  # Same as block at 0x60
00000090: 0000 0000 0000 0000 0000 0000 0000 0000
000000a0: 0000 0000 0000 0000 0000 0000 0000 0000
                                                 
000000b0: 4a31 b5cf 9fea e697 8100 97a4 e7fe 952c  # keys and access bits
```
{% endcode %}

## Why is the credit written twice?

I've read that it is recommended to keep data duplicated in two different sectors to handle problems that can arise with write interruptions  (if the user withdraws the card during a write operation), not sure what the implications of this are though...

## Modify the credit on a card

It looks like we have all the information we need in order to "legitimately" (at least from the point of view of the laundry card machine) change the credit on a card, just change the credit by incrementing blocks `0x60` and `0x80`, easy, right?
