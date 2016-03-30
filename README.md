## Dell Latitude E5*50 Linux support

Tested on a Dell Latitude E5450 (late 2014) with Fedora 22/23 64bit, BIOS A10.

Please note: BIOS prior to A06 have a serious issue with de-bouncing/repeating keys under Linux, see http://en.community.dell.com/support-forums/laptop/f/3518/t/19593360. Updating to [A10](http://www.dell.com/support/home/us/en/19/Drivers/DriversDetails?driverId=DHD06&fileId=3469377694&osCode=W764&productCode=latitude-e5450-laptop&languageCode=EN&categoryId=BI) is strongly reccomended.

### WWAN module

Dell Latitude E5*50 laptops could have a "Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card" WWAN module installed. This WWAN device is based on the Qualcomm Gobi™.

|   |   |
|---|---|
| Name | Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card |
| ID | 413c:81b1 |
| Kernel module | qcserial, qmi_wwan |

The device id of this Gobi™ module is still missing from the ```qcserial``` devices table, so I created a udev rule which loads the ```qcserial``` module, switchs the ```bConfigurationValue``` from 2 to 1** and adds the new device id ```413c:81b1``` to the ```qcserial``` driver. Current version of the udev rule loads the ```qmi_wwan``` modules too and adds the device id to that module to allow 4G/LTE connection through modern versions of NetworkManager/ModemManager.

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

### Smart-card reader (Fedora <= 22 only)

NOTE: this is not needed anymore, starting from Fedora 23, since `pcsc-lite-ccid-1.4.19` is now part of the stable release.

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

## Related links
- http://www.0xf8.org/2015/06/dell-wireless-5809e-support-in-opensuse-13-2/
- http://www.0xf8.org/2015/07/dell-wireless-5809e-support-in-linux-a-followup/
- https://sigquit.wordpress.com/2015/02/09/dell-branded-sierra-wireless-3g4g-modem-not-online/
