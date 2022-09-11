# P2-VL53L5CX-time-of-flight-driver
Interacting with the VL53L5CX Time-of-Flight Sensor from our P2

![Project Maintenance][maintenance-shield]

[![License][license-shield]](LICENSE)

## VL53L5CX Time-of-Flight Sensor 
(*...Excerpt from [STMicroelectronics webpage](https://www.st.com/en/imaging-and-photonics-solutions/vl53l5cx.html)*)

The VL53L5CX is a state of the art, Time-of-Flight (ToF), multizone ranging sensor. It integrates a SPAD array, physical infrared filters, and diffractive optical elements (DOE) to achieve the best ranging performance in various ambient lighting conditions with a range of cover glass materials. The use of a DOE above the vertical cavity surface emitting laser (VCSEL) allows a square FoV to be projected onto the scene. The reflection of this light is focused by the receiver lens onto a SPAD array.

Unlike conventional IR sensors, the VL53L5CX allows absolute distance measurement whatever the target color and reflectance. It provides accurate ranging up to 400 cm and can work at fast speeds (60 Hz).

Multizone distance measurements are possible up to 8x8 zones with a wide 63° diagonal FoV which can be reduced by software. The VL53L5CX is able to detect different objects within the FoV.

<p align="center">
  <img src="./Images/VL53L5CX-SATEL.webp" width="200"><br>
  <B>Shown: The Satel Breakout Board package</B>
</p>

### Purchase

This sensor is available in a number of forms from a number of sources:

- [DigiKey](https://www.digikey.com/en/product-highlight/s/stmicroelectronics/vl53l5cx-time-of-flight-ranging-sensor) - Satel breakout, plus other forms
- [Mouser](https://www.mouser.com/new/stmicroelectronics/stm-vl53l5cx-tof-sensor/) - Satel breakout, plus other forms
- [Pololu](https://www.pololu.com/product/3417) - carrier board + voltage regulator
- [sparkfun](https://www.sparkfun.com/products/18642) - carrier board w/QWIIC interface


## P2 Driver for the VL53L5CX Time-of-Flight Sensor

### Features

## Current status

Latest Changes:

```
10 Sep 2022
- Full data arriving from TOF sensor correctly!
- Working on HDMI display of 4 sensors at once
6 Sep 2022
- All public methods converted, now in midst of turning on ranging...
- Code starting to take shape
5 Sep 2022
- Started porting unload and overall ranging control code
4 Sep 2022
- Firmware upload to VL53L5CX MCU is tested and working
24 Aug 2022
- Driver for i2c I/O port expander tested and working, has its own demo
20 Aug 2022
- Project started
```

## Known Issues

Things we know about that still need attention:

```
(nothing yet)
```

## Table of Contents

On this Page:

- [Driver Features](#features)
- [How to contribute](#how-to-contribute)

Additional pages:

- [PCF8575 Driver Documentation](./PCF8575.md) - this is likely to be used to select between multiple TOF sensors


## How to Contribute

This is a project supporting our P2 Development Community. Please feel free to contribute to this project. You can contribute in the following ways:

- File **Feature Requests** or **Issues** (describing things you are seeing while using our code) at the [Project Issue Tracking Page](https://github.com/ironsheep/P2-VL53L5CX-time-of-flight-driver/issues)
- Fork this repo and then add your code to it. Finally, create a Pull Request to contribute your code back to this repository for inclusion with the projects code. See [CONTRIBUTING](CONTRIBUTING.md)

---

> If you like my work and/or this has helped you in some way then feel free to help me out for a couple of :coffee:'s or :pizza: slices!
>
> [![coffee](https://www.buymeacoffee.com/assets/img/custom_images/black_img.png)](https://www.buymeacoffee.com/ironsheep) &nbsp;&nbsp; -OR- &nbsp;&nbsp; [![Patreon](./Images/patreon.png)](https://www.patreon.com/IronSheep?fan_landing=true)[Patreon.com/IronSheep](https://www.patreon.com/IronSheep?fan_landing=true)

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

### [Copyright](copyright) | [License](LICENSE)

[maintenance-shield]: https://img.shields.io/badge/maintainer-stephen%40ironsheep%2ebiz-blue.svg?style=for-the-badge

[license-shield]: https://camo.githubusercontent.com/bc04f96d911ea5f6e3b00e44fc0731ea74c8e1e9/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f69616e74726963682f746578742d646976696465722d726f772e7376673f7374796c653d666f722d7468652d6261646765
