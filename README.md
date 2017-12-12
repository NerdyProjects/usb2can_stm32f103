# USB2CAN adapter reverse engineering

This is information about a "cheap" USB2CAN Adapter from China.
It is based on a USB-Serial bridge (CP2102), a STM32F103 microcontroller and a NXP A1050 CAN transceiver.
It came to me for approx. 15€. (See https://de.aliexpress.com/item/Free-shipping-CAN-Bus-Analyzer-USB-to-CAN-USB-CAN-debugger-adapter-communication-converter/32489958299.html for example).

I actually suggest NOT buying this device. Use a solution like [CandleLight](https://github.com/HubertD/candleLight) as that is a much more flexible approach, using a controller with USB and CAN in one (where even firmware with different USB implementations is available).

## Hardware
### Power supply
The device is USB powered via the regulator inside the CP2102.
It is therefore able to provide ~75 mA to the application at 3.3V.

### USB to serial
The USB interface is a CP2102 USB to serial bridge which allows 300bps to 1Mbps.
The serial lines RXD and TXD are connected to the microcontroller.

### Microcontroller
The STM32F103C8T6 microcontroller is used.
Pinout:
* PD0/PD1 (OSC_IN/OSC_OUT) to 12 MHz crystal
* NRST Open but fed via unplaced capacitor to GND
* PA0 feeds low side of red LED
* PA2/PA3 serial communication
* PA13 to J4.1 SWDIO
* PA14 to J4.2 SWCLK
* PB8/PB9 to CAN interface
* PB7 to /RST of CP2102 (RST I/O of CP2102 -  will be low and can be driven low to reset)
* PB6 to /SUSPEND of CP2102 (CP2102 drives low when USB suspend is entered)
* BOOT0 to GND


Programming connector:

* J4: SWDIO SWCLK RST\_NC 3,3V GND


## Software
The microcontroller is protected against readout, so I could not obtain the firmware via SWD.
