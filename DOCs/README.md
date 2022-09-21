# VL53L5CX Hardware/Driver Documents
Hardware documents for the VL53L5CX TOF sensor and driver

![Project Maintenance][maintenance-shield]

[![License][license-shield]](LICENSE)


## Hardware Documents

The following documents are found in this repo (this driver is inteded to support all of these displays):

| Filename | Purpose | Description 
| --- | --- | --- | 
| [vl53l5cx.pdf](vl53l5cx.pdf) | Device: TOF Sensor | **Product data sheet**: DS13754 - Rev 8 - 38 pgs
| [vl53l5cx-satel.pdf](vl53l5cx-satel.pdf) | Breakout Board | **Data brief**: DB4506 - Rev 1 - 5 pgs
| [STMicro driver UM](um2884-use-vl53l5cx-with-ultra-lite-driver-sw.pdf) | User Manual | **User Manual** A guide to using the VL53L5CX multizone Time-of-Flight ranging sensor with wide field of view Ultra Lite Driver (ULD): UM2884 - Rev 2 - 19 pgs - Aug 2021<BR><BR>This is provided as it is a good reference for adjusting configuration of our own P2 (.spin2) driver. 
| [PCF8575.pdf](PCF8575.pdf) | Device: I2C I/O Expander | **Product data sheet**: PCF8575 Remote 16-bit I/O expander for I2C-bus - 24 pgs - 07 Apr 1997

**NOTE**: these we placed here at the time this repo was created and were used to guide the driver implementation for these devices. There may be new version at the Philips (PCF8575) and STMicroelectronics websites - *the best way to find out if there are is to do your own google search.*

Additional pages:

- [Top README](https://github.com/ironsheep/P2-VL53L5CX-time-of-flight-driver) - Return to the top-level README for this repository


---

> If you like my work and/or this has helped you in some way then feel free to help me out for a couple of :coffee:'s or :pizza: slices!
>
> [![coffee](https://www.buymeacoffee.com/assets/img/custom_images/black_img.png)](https://www.buymeacoffee.com/ironsheep) &nbsp;&nbsp; -OR- &nbsp;&nbsp; [![Patreon](../Images/patreon.png)](https://www.patreon.com/IronSheep?fan_landing=true)[Patreon.com/IronSheep](https://www.patreon.com/IronSheep?fan_landing=true)

---

## Disclaimer and Legal

> *Parallax, Propeller Spin, and the Parallax and Propeller Hat logos* are trademarks of Parallax Inc., dba Parallax Semiconductor
>
> This project is a community project not for commercial use.
>
> This project is in no way affiliated with, authorized, maintained, sponsored or endorsed by *Parallax Inc., dba Parallax Semiconductor* or any of its affiliates or subsidiaries.

---

## License

Copyright © 2022 Iron Sheep Productions, LLC. All rights reserved.

Licensed under the MIT License.

Follow these links for more information:

### [Copyright](../copyright) | [License](../LICENSE)

[maintenance-shield]: https://img.shields.io/badge/maintainer-stephen%40ironsheep%2ebiz-blue.svg?style=for-the-badge

[license-shield]: https://camo.githubusercontent.com/bc04f96d911ea5f6e3b00e44fc0731ea74c8e1e9/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f69616e74726963682f746578742d646976696465722d726f772e7376673f7374796c653d666f722d7468652d6261646765
