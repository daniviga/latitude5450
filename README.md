## Dell Latitude E5*50 Linux support

Tested on my Dell Latitude E5450 (late 2014) with Fedora 21 64bit

### WWAN module

The Dell Latitude E5*50 could have a "Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card" WWAN module. It's based on the Qualcomm Gobi™.

|   |   |
|---|---|
| Name | Dell Wireless 5809e Gobi™ 4G LTE Mobile Broadband Card |
| ID | 413c:81b1 |
| Kernel module | qcserial |

I created a udev rule which loads the ```qcserial``` module, switchs the ```bConfigurationValue``` from 2 to 1 and add the new device id ```413c:81b1``` to the ```qcserial``` driver.
