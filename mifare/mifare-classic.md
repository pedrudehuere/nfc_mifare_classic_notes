# MIFARE Classic

MIFARE Classic cards' memory is organized in sectors which are composed of blocks, sector size varies, block size is always 16 Bytes.

## Memory layout

MIFARE Classic cards' memory is organized in sectors which are organized in blocks

### Sectors

* Each sector has either 4 (MIFARE Classic 1K and 4K) or 16 blocks (Only MIFARE Classic 4K)
* First sector contains the manufacturer block

### Blocks

* A block is made of 16 Bytes

#### Manufacturer block

* First block of first sector
* Read-only
* Contains ID and manufacturer data
* Generally the ID is retrieved at card activation

#### Sector trailer block

**Last** block of each sector, contains keys (A and B) as well as access conditions for the other blocks in the sector (see [#access-bits](mifare-classic.md#access-bits "mention"))

```
+-----------------------------+--------------+----+-----------------------------+
|  0 |  1 |  2 |  3 |  4 |  5 |  6 |  7 |  8 |  9 | 10 | 11 | 12 | 13 | 14 | 15 |
+-----------------------------+--------------+----+-----------------------------+
|            Key A            | Access Conditions |            Key B            |
|          (6 bytes)          |     (4 bytes)     |          (6 bytes)          |
+-----------------------------+--------------+----+-----------------------------+
```

#### Data block

General purpose read/write block

#### Value block

* Data block with additional capabilities
* Typically used for "credit" applications
* Additional special commands for R/W: `CmdWriteValueBlock`, `CmdReadValueBlock`, `CmdIncrementValueBlock`, `CmdDecrementValueBlock`

&#x20;Value blocks must be initialized with a write operation to formatted in a special way:

<div align="left">

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Value block formatting</p></figcaption></figure>

</div>

Where value is the "credit value" and address can be used for backup purposes (this is up to the application creator)

An example with a value of 390 would be (numbers stored as int32, little-endian: LSB first)

```
 value     ~value    value    address
|-------| |-------| |-------| |-------|
8601 0000 79fe ffff 8601 0000 00ff 00ff
```

### MIFARE Classic 1K

* 16 sectors with 4 blocks each
* Total: 1024 Bytes

<div align="left">

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>MIFARE Classic 1K memory layout</p></figcaption></figure>

</div>

### MIFARE Classic 4K

* 40 sectors in total
* First 32 sectors: 4 blocks each(same as MIFARE Classic 1K)
* Last 8 sectors: 16 blocks each
* Total: 1536 Bytes

<div align="left">

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>MIFARE Classic 4K memory layout</p></figcaption></figure>

</div>

## Manufacturer block

### Card ID

A card ID can be 4 or 7 Bytes long

#### 4 Bytes NUID

* Considered non-unique since only 4 Bytes

<div align="left">

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>4 Bytes ID</p></figcaption></figure>

</div>

#### 7 Bytes UUID

<div align="left">

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>7 Bytes ID</p></figcaption></figure>

</div>

### BCC

The Block Check Character is a 1 Byte XOR checksum on the Bytes composing the ID, it is respectively Byte 4 and 7 on MIFARE 1K and 4K cards (not depicted in the screenshots above)

## Access bits

{% hint style="danger" %}
With each memory access the internal logic verifies the format of the access conditions. If it detects a format violation the whole sector is irreversibly blocked
{% endhint %}

<div align="left">

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Access bits</p></figcaption></figure>

</div>

