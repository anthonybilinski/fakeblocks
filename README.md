## fakeblocks
### the faster fakeblock checker
fakeblocks leverages [badblocks](https://git.kernel.org/pub/scm/fs/ext2/e2fsprogs.git/tree/misc/badblocks.c) and specializes at testing potentially fake storage instead of naturally occuring bad blocks. Fake storage is often found in low cost SD cards and USB storage bought online. The storage is sold as a large size and even shows up as a large size when inserted, but then loses data written to it.

fakeblocks is easy to use and automatically tests small portions of the device using random data before doing a full disk check, often finding that the disk is invalid much faster than badblocks. If all initial small tests pass, badblocks is then run to verify every bit of the device is working correctly.
