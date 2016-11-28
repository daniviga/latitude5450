### Reverting 'ACPI: Execute _PTS before system reboot'

Starting with kernel v4.8 a bug as been introduced that prevents a correct shutdown/reboot of the machine.
Bug has been fixed then in 4.9rc7 and kernel 4.9.0 should not be affected by the issue anymore.

The bad commit is:

```
2c85025c75dfe7ddc2bb33363a998dad59383f94
ACPI: Execute _PTS before system reboot
```

All kernels from 4.8.0 are affected at the moment. A special thanks to **Giampaolo** on *kbz* for bisecting sources down to the root cause.
An `rpmbuild` compatible patch and an updated `spec` file are provided for reverting this commit. Binary *rpms* are available for [F24](https://daniele.vigano.me/files/5e366c91-41ac-4e43-be8e-15139c0463b1/f24/) and [F25](https://daniele.vigano.me/files/5e366c91-41ac-4e43-be8e-15139c0463b1/f25/).

### More info

- https://bugzilla.redhat.com/show_bug.cgi?id=1393513
- https://bugzilla.kernel.org/show_bug.cgi?id=187061
- https://bugzilla.kernel.org/show_bug.cgi?id=151631
- https://patchwork.kernel.org/patch/9041141/
