## Dell Latitude E5*50 Linux support

Tested on a Dell Latitude E5450 (late 2014) with Fedora 21/22 64bit, BIOS A07.

Please note: BIOS prior to A06 have a serious issue with de-bouncing/repeating keys under Linux, see http://en.community.dell.com/support-forums/laptop/f/3518/t/19593360. Updating to [A07](http://www.dell.com/support/home/us/en/19/Drivers/DriversDetails?driverId=DHD06&fileId=3469377694&osCode=W764&productCode=latitude-e5450-laptop&languageCode=EN&categoryId=BI) is strongly reccomended.

### Shutdown/reboot issues with kernel >= 4.8

See kernel/README.md

### WWAN module

Dell Latitude E5*50 laptops could have a "Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card" WWAN module installed. This WWAN device is based on the Qualcomm Gobi™.

|   |   |
|---|---|
| Name | Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card |
| ID | 413c:81b1 |
| Kernel module | qcserial |
| Tested kernels  | 3.17, 3.16, 3.19, 4.0 |

The device id of this Gobi™ module is still missing from the ```qcserial``` devices table, so I created a udev rule which loads the ```qcserial``` module, switchs the ```bConfigurationValue``` from 2 to 1** and adds the new device id ```413c:81b1``` to the ```qcserial``` driver.

** By default the device is loaded in the ```cdc_mbim``` mode (see https://www.kernel.org/doc/Documentation/networking/cdc_mbim.txt) instead in the classic serial modem mode.

#### GPS ####

To start the GPS

```bash
echo "\$GPS_START" > /dev/ttyUSB0
```

To stop the GPS

```bash
echo "\$GPS_STOP" > /dev/ttyUSB0
```

In the ```bin``` folder you can find an utility (```gobigps```) to easily start and stop the GPS. To be able to use this utility as normal user (more in general, to use the GPS) the user must be member of the ```dialout``` group.

You can check if the GPS is running with a simple ```cat```

```bash
cat /dev/ttyUSB0
```

or using _QGIS_.

### Smart-card reader

The Broadcom 5880v5 used in this Latitude (id `0a5c:5804`) is supported by `pcsc-lite-ccid` starting from release 1.4.19. Fedora 21 and 22 provide version 1.4.18; `pcsc-lite-ccid-1.4.19`, backported from rawhide, can be downloaded from the following repo:

https://copr.fedoraproject.org/coprs/dani/ccid-drivers-backport/

Using DNF:
```bash
sudo dnf copr enable dani/ccid-drivers-backport
sudo dnf install pcsc-lite-ccid
```

Using YUM:
```bash
wget -O- https://copr.fedoraproject.org/coprs/dani/ccid-drivers-backport/repo/fedora-21/dani-ccid-drivers-backport-fedora-21.repo | sudo tee /etc/yum.repos.d/dani-ccid-drivers-backport-fedora-21.repo
sudo yum install pcsc-lite-ccid
```

