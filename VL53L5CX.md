# P2 VL53L5CX Object - Time-of-Flight Sensor
Interacting with the VL53L5CX Time-of-Flight Sensor from our P2

![Project Maintenance][maintenance-shield]

[![License][license-shield]](LICENSE)

## VL53L5CX Time-of-Flight Sensor 
(*...Excerpt from [STMicroelectronics webpage](https://www.st.com/en/imaging-and-photonics-solutions/vl53l5cx.html)*)

The VL53L5CX is a state of the art, Time-of-Flight (ToF), multizone ranging sensor. It integrates a SPAD array, physical infrared filters, and diffractive optical elements (DOE) to achieve the best ranging performance in various ambient lighting conditions with a range of cover glass materials. The use of a DOE above the vertical cavity surface emitting laser (VCSEL) allows a square FoV to be projected onto the scene. The reflection of this light is focused by the receiver lens onto a SPAD array.

Unlike conventional IR sensors, the VL53L5CX allows absolute distance measurement whatever the target color and reflectance. It provides accurate ranging up to 400 cm and can work at fast speeds (60 Hz).

Multizone distance measurements are possible up to 8x8 zones with a wide 63° diagonal FoV which can be reduced by software. The VL53L5CX is able to detect different objects within the FoV.

<p align="center">
  <img src="./Images/VL53L5CX-SATEL.webp" width="600"><br>
  <B>Shown: The Satel Breakout Board package</B>
</p>

### Purchase

This sensor is available in a number of forms from a number of sources:

- [DigiKey](https://www.digikey.com/en/product-highlight/s/stmicroelectronics/vl53l5cx-time-of-flight-ranging-sensor) - Satel breakout, plus other forms
- [Mouser](https://www.mouser.com/new/stmicroelectronics/stm-vl53l5cx-tof-sensor/) - Satel breakout, plus other forms
- [Pololu](https://www.pololu.com/product/3417) - carrier board + voltage regulator
- [sparkfun](https://www.sparkfun.com/products/18642) - carrier board w/QWIIC interface



## Table of Contents

On this Page:

- [The vl53l5cx Object](#the-vl53l5cx-object-spin2)
- [Use the vl53l5cx object in your own Project](#using-the-vl53l5cx-object-in-your-own-project)

Additional pages:

- [VL53L5CX part](./DOCs/vl53l5cx.pdf) datasheet
- [VL53L5CX Satel board](./DOCs/vl53l5cx-satel.pdf) datasheet
- [VL53L5CX driver](./DOCs/um2884-use-vl53l5cx-with-ultra-lite-driver-sw.pdf) windows driver notes but mostly useful with this driver/object too
- [GOAL - TOF Sensor: w/180° Field of View](./DOCs/Designs/README.md) - how I make this wide field-of-view sensor?
- [PCF8575 Object Documentation](./PCF8575.md) - this is used to select between multiple TOF sensors

## Current status

Latest Changes:

```
10 Sep 2022
- Ranging working
- Full data arriving from TOF sensor correctly!
6 Sep 2022
- All public methods converted, now in midst of turning on ranging...
- Code starting to take shape
5 Sep 2022
- Started porting unload and overall ranging control code
4 Sep 2022
- Firmware upload to VL53L5CX MCU is tested and working
20 Aug 2022
- Project started
```

## Known Issues

Things we know about that still need attention:

```
- Optional features not yet implemented:
 -- xtalk calibration
 -- motion indicator
 -- detection thresholds

```
## The VL53L5CX object (.spin2)

### Features

This driver object provides 

### Timings

This driver has been measured using a logic analyzer and is currently achieving the following:

| Activity | Description |
| --- | --- |
| TBA

### Driver Interface

The programming interface to this driver is as follows:

| Method Name | Description |
| --- | --- |
| | **-- General Setup --** 
| start(pinINT, pinRST, pinSDA, pinSCL, pinLPN, pinPWREN) : bDevicePresent | Start the device running
| stop() | Stop our i2c bus use and float all pins
| isAlive() : eDvcStatus, bIsAlive | Returns {bIsAlive} when {eDvcStatus} == `VL53L5CX_STATUS_OK`</br>{bIsAlive} contains T/F (where T means the device was found on the i2c bus)</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` - if there was an I2C communications error
| setI2CAddress(i2cAddress) : eDvcStatus |Reconfigure device to respond to new i2c device address</br> Returns `VL53L5CX_STATUS_OK` if the sensor's address was correctly changed or `VL53L5CX_STATUS_ERROR` otherwise.
| getI2CAddress() : nDvcI2CAddr |Return the current i2c device address (or `DVC_NOT_FOUND` if no device found on i2c bus)
| | **-- Alternate Muli-Sensor Setup, when using driver COG --**</br>Use these instead of start()
| setPinsAndID(pSDA, pSCL, nID) : ok |Record PINs for later Deferred Start [startUsingPins()] for use by driver cog
| startUsingPins() : bDevicePresent | Deferred Start for use by driver cog</br>MUST be preceed by setPins()
| activateSensor() : bDevicePresent |  Deferred Start for use by driver cog   MUST be preceed by setPins() and startUsingPins()
| | **-- Configuration --** 
| setRangingFrequencyHz(newFrequency8) : eDvcStatus |  Configure sensor for a new ranging frequency in Hz {newFrequency8}</br>NOTE1: legal range in Hz varies by resolution: [4x4: 1-60 Hz, 8x8: 1-15Hz]</br> NOTE2: must be called after setRangingMode()</br>Returns `VL53L5CX_STATUS_OK` if the sensor's sampling frequency was changed.</br> Alternatively returns {eDvcStatus}:</br>`VL53L5C_STATUS_INVALID_PARAM` - then curr resolution is yet set or Hz value if out of range for resolution</br>`VL53L5CX_STATUS_ERROR` - if there was an I2C communications error
| getRangingFrequencyHz() : eDvcStatus, frequencyHz8 | Returns the current ranging frequency {frequencyHz8} in Hz when {eDvcStatus} == `VL53L5CX_STATUS_OK`</br>Ranging frequency corresponds to the time between each measurement.</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` - if there was an I2C communications error
| setRangingMode(eRangingMode) : eDvcStatus |  Configure sensor ranging mode: [`VL53L5CX_RANG_MODE_CONTINUOUS` or `VL53L5CX_RANG_MODE_AUTONOMOUS`]</br>Returns {eDvcStatus} of `VL53L5CX_STATUS_OK` if the sensor's ranging mode was changed.</br>Alternatively returns {eDvcStatus}:</br>`VL53L5C_STATUS_INVALID_PARAM` if not one of two expected values</br>`VL53L5CX_STATUS_ERROR` - if there was an I2C communications error
| getRangingMode() : eDvcStatus, eRangingMode |  Return the sensors' current ranging mode when {eDvcStatus} is `VL53L5CX_STATUS_OK` {eRangingMode} will be set to [`VL53L5CX_RANG_MODE_CONTINUOUS` or `VL53L5CX_RANG_MODE_AUTONOMOUS`]</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| setResolution(eResolution) : eDvcStatus |  Set the sensors' resolution to [`VL53L5CX_RESOLUTION_4X4`, or `VL53L5CX_RESOLUTION_8X8`]</br>Returns {eDvcStatus} of `VL53L5CX_STATUS_OK` if the sensor's ranging resolution was changed</br>Alternatively returns {eDvcStatus}:</br> `VL53L5C_STATUS_INVALID_PARAM` if not one of two expected values</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| getResolution() : eDvcStatus, eResolution |  Return the sensors' currently configured ranging resolution when {eDvcStatus} is `VL53L5CX_STATUS_OK`</br>{nResolution8} will be set to [`VL53L5CX_RESOLUTION_4X4`, or `VL53L5CX_RESOLUTION_8X8`]</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_BAD_DATA` if an unexpected value was read from the sensor</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| setPowerMode(ePowerMode) : eDvcStatus |  Set the sensors' power-mode to [`VL53L5CX_POWER_MODE_SLEEP`, or `VL53L5CX_POWER_MODE_WAKEUP`]</br>Returns {eDvcStatus} of `VL53L5CX_STATUS_OK` if the sensor's power-mode was changed</br>Set the sensor in Low Power mode, for example if the sensor is not used during a long time.</br>The value `VL53L5CX_POWER_MODE_SLEEP` can be used to enable the low power mode.</br>To restart the sensor, use the value `VL53L5CX_POWER_MODE_WAKEUP`.</br>NOTE: Please ensure that the device is not streaming before calling the function.</br>Alternatively returns {eDvcStatus}:</br>`VL53L5C_STATUS_INVALID_PARAM` if not one of two expected values</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| getPowerMode() : eDvcStatus, ePowerMode |  Return the sensors' current power-mode when {eDvcStatus} is `VL53L5CX_STATUS_OK`</br>{ePowerMode} will be set to [`VL53L5CX_POWER_MODE_WAKEUP` or `VL53L5CX_POWER_MODE_SLEEP`]</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| setIntegrationTime(timeMsec32) : eDvcStatus |  Sets a new integration time in ms. [2ms <= {timeMsec32} <= 1000ms]</br>Returns {eDvcStatus} of `VL53L5CX_STATUS_OK` if the sensor's integration time was changed</br>NOTE: Integration time must be computed to be lower than the ranging period, for the selected resolution.</br>NOTE: that this setting has NO IMPACT on ranging mode continous.</br>Alternatively returns {eDvcStatus}:</br>`VL53L5C_STATUS_INVALID_PARAM` if not within the supported range</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| getIntegrationTime() : eDvcStatus, timeMsec32 | Return the sensors' currently configured integration time (in ms) when {eDvcStatus} is `VL53L5CX_STATUS_OK`</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| setSharpenerPercent(percent8) : eDvcStatus |  Sets a new sharpener value in percent. [0 <= {percent8} <= 99] - where 0 means disabled</br>Returns {eDvcStatus} of `VL53L5CX_STATUS_OK` if the sensor's sharpener value was changed.</br>Change to blur more or less zones depending upon the application</br>Alternatively returns {eDvcStatus}:</br> `VL53L5C_STATUS_INVALID_PARAM` if not within the supported range</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| getSharpenerPercent() : eDvcStatus, percent8 | Return the sensors' currently configured sharpener (in percent) when {eDvcStatus} is `VL53L5CX_STATUS_OK` value returned {percent8} will be [0 (disabled) to 99] percent.</br>NOTE: Sharpener can be changed to blur more or less zones depending of the application.</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| setTargetOrder(eTargetOrder) : eDvcStatus |  Set the sensors' target order to [`VL53L5CX_TGT_ORDER_CLOSEST`, or `VL53L5CX_TGT_ORDER_STRONGEST`]</br>Returns {eDvcStatus} of `VL53L5CX_STATUS_OK` if the sensor's target order was changed</br>NOTE: By default, the sensor is configured with the strongest output.</br>Alternatively returns {eDvcStatus}:</br>`VL53L5C_STATUS_INVALID_PARAM` if not within the supported range</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| getTargetOrder() : eDvcStatus, eTargetOrder |  Return the sensors' currently configured target order when {eDvcStatus} is `VL53L5CX_STATUS_OK`</br>{eTargetOrder} will be set to [`VL53L5CX_TGT_ORDER_CLOSEST` or `VL53L5CX_TGT_ORDER_STRONGEST`]</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_BAD_DATA` if an unexpected value was read from the sensor</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| | **-- Ranging Control --** 
| startRanging() : eDvcStatus |  Starts a ranging session.</br>NOTE: When the sensor is streaming, user cannot change settings 'on-the-fly'.</br>Returns `VL53L5CX_STATUS_OK` if the start ranging command was acknowledged by the sensor or `VL53L5CX_STATUS_ERROR` otherwise.
| stopRanging() : eDvcStatus | Stops the ranging session. It must be used when the sensor is streaming, after calling startRanging().</br>Returns `VL53L5CX_STATUS_OK` if the stop ranging command was acknowledged by the sensor or `VL53L5CX_STATUS_ERROR` otherwise.
| isRanging() : bSensorRanging | Return T/F where T means the sensor is actively ranging
| | **-- Handling Ranging Data --** 
| didInterrupt() : bPinState | Return interpreted value of Interrupt pin where 1 = TRUE (did interrupt), 0 = FALSE (did not)
| isDataReadyToUnload() : eDvcStatus, bDataReady | Check sensor by polling i2c. Return T/F (where T means data is ready) when {eDvcStatus} is `VL53L5CX_STATUS_OK`</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| unloadRangingData() : eDvcStatus, bDataReturned |  Retrieve ranging data from the sensor.</br>Return T/F (where T means data was retrieved) when {eDvcStatus} is `VL53L5CX_STATUS_OK`</br>Alternatively returns {eDvcStatus}:</br>`VL53L5CX_STATUS_ERROR` if there was an i2c i/o error
| targetBufferPointers() : pTgtStatus8, pDistanceMM16 |  Return pointers to internal data arrays</br>that is: </br>`@DistanceMM[0] (WORD[])` and </br>`@TargetStatus[0] (BYTE[])`
| sampleDistanceMM(index) : nValue16 | --TBA--
| sampleReflectance(index) : nValue8 | --TBA--
| sampleTargetStatus(index) : nValue8 | --TBA--
| sampleNbTgtDetected(index) : nValue8 | --TBA--

**NOTE:** *We're stopping here with adding method details. We are still working on how best to do unloads and how best to make the data avaialable.  We'll add more here as we are satisfyed the new interface.*

### Files

This driver has its own top-level demo. The files involved are:

| Filename | Description |
| --- | --- |
| demo_vl53l5cx.spin2 | Top level demo file |
| isp_vl53l5cx.spin2 | The P2 driver |
| isp_i2c.spin2 | Underlying i2c driver  tuned for 1Mbit xfers |


### Demo Configuration

As written the demo assigned the following pins to communicate with the board. 

| P2 PIN | Satel Board Connector | Purpose | Direction
| --- | ---| ---| ---|
| 16 | INT | interrupt | In
| 17 | I2C_RST | i2c I/F reset | In
| 18 | SDA | data| In/Out
| 19 | SCL | clock| In
| 20 | LPn | comms enable | In
| 21 | PWREN | power enable | In

**NOTE:** *The LPn pin has special use when using more than 1 TOF sensor in a single system. It is used in this case to select each device individually  so that a new I2C address can be assigned to it.*

Of course you can adjust these assignments. Adjust the follwing constants within the file `demo_vl53l5cx.spin2` to your liking:

```
    PIN_HDMI_BASE   = 8

    PIN_TOF_INT     = 16
    PIN_TOF_RST     = 17
    PIN_TOF_SDA     = 18
    PIN_TOF_SCL     = 19
    PIN_TOF_LPN     = 20
    PIN_TOF_PWREN   = 21
```

## Using the VL53L5CX object in your own project

Include the object in your project:

```
OBJ { our sensors }

    tofSensor   : "isp_vl53l5cx"                      ' our TOF Sensor
```

Start the object:

```
    ' find and configure our pcf8575 device
    bDevicePresent := tofSensor.start(PIN_TOF_INT, PIN_TOF_RST, PIN_TOF_SDA, PIN_TOF_SCL, PIN_TOF_LPN, PIN_TOF_PWREN)  ' i2c @ 1Mz
```

See if it is present on the I2C bus:

```
        addrFound := tofSensor.getI2CAddress()
        if(addrFound <> tofSensor.DVC_NOT_FOUND)
            ... ' its there!
```

Now interact with the device to your hearts' content:

For example:

```
        tofSensor.setResolution(tofSensor.`VL53L5CX_RESOLUTION_8X8`)

        tofSensor.startRanging()

        ' test for data ready
         eDvcStatus, bDataReturned := tofSensor.isDataReadyToUnload()
            if bDataReturned
              ....
              
        ' unlod the data
        eDvcStatus, bDataUnloaded := tofSensor.unloadRangingData()     ' load distance data
        if bDataUnloaded
           ...
           
        ' when done
        tofSensor.stopRanging() ' if you don't, you'll have to power off the sensor before you can control again...
        
        tofSensor.stop()     ' to release the pins in use
```

That's all there is. It's pretty simple to use...


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
