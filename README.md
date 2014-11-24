## Dell Latitude E5*50 Linux support

Tested on my Dell Latitude E5450 (late 2014) with Fedora 21 64bit

### WWAN module

Dell Latitude E5*50 laptops could have a "Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card" WWAN module installed. This WWAN device is based on the Qualcomm Gobi™.

|   |   |
|---|---|
| Name | Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card |
| ID | 413c:81b1 |
| Kernel module | qcserial |

The device id of this Gobi™ module is still missing from the ```qcserial``` devices table, so I created a udev rule which loads the ```qcserial``` module, switchs the ```bConfigurationValue``` from 2 to 1** and add the new device id ```413c:81b1``` to the ```qcserial``` driver.

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

In the ```bin``` folder you can find an utility (```gobigps```) to easily start and stop the GPS.

Work is still in progress!
