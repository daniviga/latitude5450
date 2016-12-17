### Reverting 'ACPI: Execute _PTS before system reboot'

Starting with kernel v4.8 a bug as been introduced that prevents a correct shutdown/reboot of the machine.
Bug has been fixed then in 4.9rc7 and kernel 4.9 is not be affected by the issue anymore.

The bad commit is:

```
2c85025c75dfe7ddc2bb33363a998dad59383f94
ACPI: Execute _PTS before system reboot
```

All kernels from 4.8.0 are affected at the moment. A special thanks to **Giampaolo** on *kbz* for bisecting sources down to the root cause.
An `rpmbuild` compatible patch and an updated `spec` file are provided for reverting this commit in 4.18.

On Fedora 24 and 25 is also possible to use a 4.9 kernel from rawhide:

```bash
sudo dnf install fedora-repos-rawhide
sudo dnf --enablerepo=rawhide upgrade kernel*

kernel.x86_64                         4.9.0-1.fc26                 @rawhide
kernel-core.x86_64                    4.9.0-1.fc26                 @rawhide
kernel-devel.x86_64                   4.9.0-1.fc26                 @rawhide
kernel-headers.x86_64                 4.9.0-1.fc26                 @rawhide
kernel-modules.x86_64                 4.9.0-1.fc26                 @rawhide
kernel-modules-extra.x86_64           4.9.0-1.fc26                 @rawhide
kernel-tools.x86_64                   4.9.0-1.fc26                 @rawhide
kernel-tools-libs.x86_64              4.9.0-1.fc26                 @rawhide

```
### More info

- https://bugzilla.redhat.com/show_bug.cgi?id=1393513
- https://bugzilla.kernel.org/show_bug.cgi?id=187061
- https://bugzilla.kernel.org/show_bug.cgi?id=151631
- https://patchwork.kernel.org/patch/9041141/
