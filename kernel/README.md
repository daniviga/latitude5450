### Reverting 'ACPI: Execute _PTS before system reboot'

Starting with kernel v4.8.0 a bug was introduced that prevented a correct shutdown/reboot of the machine.
Bug has been fixed in 4.9rc7 and then backported to 4.8; the kernel tree 4.9 and kernels >= 4.8.15 are not be affected by the issue anymore.

The bad commit was:

```
2c85025c75dfe7ddc2bb33363a998dad59383f94
ACPI: Execute _PTS before system reboot
```

Kernels from 4.8.0 to 4.8.14 are affected, please upgrade to 4.8.15 or to 4.9.

A special thanks to **Giampaolo** on *kbz* for bisecting sources down to the root cause.

### More info

- https://bugzilla.redhat.com/show_bug.cgi?id=1393513
- https://bugzilla.kernel.org/show_bug.cgi?id=187061
- https://bugzilla.kernel.org/show_bug.cgi?id=151631
- https://patchwork.kernel.org/patch/9041141/
- https://www.kernel.org/pub/linux/kernel/v4.x/ChangeLog-4.8.15
