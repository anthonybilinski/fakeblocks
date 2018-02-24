## fakeblocks
### the faster fake storage checker
fakeblocks specializes at testing potentially fake storage instead of naturally damaged storage. Fake storage is often found in low cost SD cards and USB storage bought online. The storage is sold as a large size and even shows up as a large size when inserted, but then loses data written to it.

fakeblocks is easy to use and automatically tests small portions of the device using random data before doing a full disk check, often finding that the disk is invalid much faster than badblocks. If all initial small tests pass, badblocks is then run to verify every bit of the device is working correctly.

### usage
```sudo ./fakeblocks /dev/<device>```
### performance
__best case__: You test __1MB instead of 2TB__ of a very slow 500GB SD card. This is not uncommon for cheap large capacity storage bought online. This saves you days of test time.

__worst case__: You test 11GB extra ontop of your 2TB badblocks test (4-pass on a 500GB drive), for a __0.55% overhead__.

### reliability
fakeblocks tests a very small portion of the storage in a way that still catches most fake storage. This is not completely reliable for fake storage, and is not at all reliable for naturally damaged storage, which is why fakeblocks falls back to badblocks if fakeblocks doesn't find any problems. fakeblocks is no less reliable than badblocks because of this.

### sample failure
```
sudo ./fakeblocks /dev/mmcblk1
******************************** WARNING ********************************
THIS TEST WILL DESTROY ALL DATA ON /dev/mmcblk1! ONLY RUN THIS ON EMPTY DEVICES
IF YOU ARE ABSOLUTELY SURE /dev/mmcblk1 HAS NOTHING YOU WANT ON IT, TYPE "please destroy all data on /dev/mmcblk1"
please destroy all data on /dev/mmcblk1
Your disk reports itself as 500GiB
writing 1MiB of random data to /dev/mmcblk1 with a random offset of 181MiB...
write complete
checking the random data on the device is what we wrote...
Comparison failed. This device has bad blocks :(
Go get yourself a refund and leave some bad feedback.
```
### sample pass
```
sudo ./fakeblocks /dev/sda
******************************** WARNING ********************************
THIS TEST WILL DESTROY ALL DATA ON /dev/sda! ONLY RUN THIS ON EMPTY DEVICES
IF YOU ARE ABSOLUTELY SURE /dev/sda HAS NOTHING YOU WANT ON IT, TYPE "please destroy all data on /dev/sda"
please destroy all data on /dev/sda
Your disk reports itself as 1GiB
writing 1MiB of random data to /dev/sda with a random offset of 1382MiB...
write complete
checking the random data on the device is what we wrote...
test successful, continuing
writing 10MiB of random data to /dev/sda with a random offset of 1539MiB...
write complete
checking the random data on the device is what we wrote...
test successful, continuing
writing 100MiB of random data to /dev/sda with a random offset of 316MiB...
write complete
checking the random data on the device is what we wrote...
test successful, continuing
Your disk doesn't look fake. Calling badblocks to verify every bit of the drive. This will take a long time.
Checking for bad blocks in read-write mode
From block 0 to 2016255
Testing with pattern 0xaa: done                                                 
Reading and comparing: done                                                 
Testing with pattern 0x55: done                                                 
Reading and comparing: done                                                 
Testing with pattern 0xff: done                                                 
Reading and comparing: done                                                 
Testing with pattern 0x00: done                                                 
Reading and comparing: done                                                 
Pass completed, 0 bad blocks found. (0/0/0 errors)
Your disk is valid and safe to use. You must now add a partition table and a filesystems to use it.
```
