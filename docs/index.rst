.. _olimex_esp32gateway:

Olimex Esp32 Gateway
====================

The Esp32 Gateway device is one of the development board created by Olimex to evaluate the ESP-WROOM-32 module. Based on the `ESP32 microcontroller <https://espressif.com/en/products/hardware/esp32/overview>`_, Olimex ESP32 Gateway features 100Mb Ethernet Interface, Bluetooth LE, WiFi.

.. figure:: /custom/img/olimex_esp32gateway.jpg
   :align: center
   :figwidth: 70% 
   :alt: Olimex Esp32 Gateway

Pin Mapping
***********

.. figure:: /custom/img/Olimex_ESP32_gateway_pin_comm.png
   :align: center
   :figwidth: 100% 
   :alt: Olimex Esp32 Gateway

Official reference for Olimex Esp32 Gateway can be found `here <https://www.olimex.com/Products/IoT/ESP32-GATEWAY/open-source-hardware>`_.

Flash Layout
************

The internal flash of the ESP32 module is organized in a single flash area with pages of 4096 bytes each. The flash starts at address 0x00000, but many areas are reserved for Esp32 IDF SDK and Zerynth VM. In particular:

=============  ============  =========================
Start address  Size          Content
=============  ============  =========================
  0x00009000      16Kb         Esp32 NVS area
  0x0000D000       8Kb         Esp32 OTA data
  0x0000F000       4Kb         Esp32 PHY data
  0x00010000       1Mb         Zerynth VM
  0x00110000       1Mb         Zerynth VM (FOTA)
  0x00210000     512Kb         Zerynth Bytecode
  0x00290000     512Kb         Zerynth Bytecode (FOTA)
  0x00310000     512Kb         Free for user storage
  0x00390000     448Kb         Reserved
=============  ============  =========================

Device Summary
**************

* Microcontroller: Tensilica 32-bit Single-/Dual-core CPU Xtensa LX6
* Operating Voltage: 3.3V
* Input Voltage: 2.7-5.5V
* Digital I/O Pins (DIO): 34
* Analog Input Pins (ADC): 6
* UARTs: 2
* SPIs: 1
* I2Cs: 1
* Flash Memory: 4 MB 
* SRAM: 520 KB
* Clock Speed: 240 Mhz
* Wi-Fi: IEEE 802.11 b/g/n/e/i:

    * Integrated TR switch, balun, LNA, power amplifier and matching network
    * WEP or WPA/WPA2 authentication, or open networks 

Power
*****

Power to the Olimex Esp32 Gateway is supplied via the on-board USB Micro B connector or directly via the “VIN” pin. The power source is selected automatically.

The device can operate on an external supply of 2.7 V to 5.5 V. If using more than 5.5 V, the voltage regulator may overheat and damage the device.

Connect, Register, Virtualize and Program
*****************************************

The Olimex Esp32 Gateway comes with a serial-to-usb chip on board that allows programming and opening the UART of the ESP32 module. The CH340 USB to UART chip is also connected to the boot pins of the module, allowing for a seamless virtualization of the device. 

.. note:: Drivers for the CH340 Module can be downloaded `here <https://www.olimex.com/Products/IoT/ESP32-GATEWAY/open-source-hardware>`_  in "Software" section and are needed for **Windows and Mac platforms**. In Linux systems, the Olimex Esp32 Gateway should work out of the box.

.. note:: **For Linux Platform**: to allow the access to serial ports the user needs read/write access to the serial device file. Adding the user to the group, that owns this file, gives the required read/write access:
        
        * **Ubuntu** distribution --> dialout group
        * **Arch Linux** distribution --> uucp group


Once connected on a USB port, if drivers have been correctly installed, the Olimex Esp32 Gateway device is recognized by Zerynth Studio. The next steps are:

* **Select** the Olimex Esp32 Gateway on the **Device Management Toolbar** (disambiguate if necessary);
* **Register** the device by clicking the "Z" button from the Zerynth Studio;
* **Create** a Virtual Machine for the device by clicking the "Z" button for the second time;
* **Virtualize** the device by clicking the "Z" button for the third time.

.. note:: No user intervention on the device is required for registration and virtualization process

After virtualization, the Olimex Esp32 Gateway is ready to be programmed and the  Zerynth scripts **uploaded**. Just **Select** the virtualized device from the "Device Management Toolbar" and **click** the dedicated "upload" button of Zerynth Studio.

.. note:: No user intervention on the device is required for the uplink process.

Firmware Over the Air update (FOTA)
***********************************

The Firmware Over the Air feature allows to update the device firmware at runtime. Zerynth FOTA in the Olimex Esp32 Gateway device is available for bytecode and VM.

Flash Layout is shown in table below:

=============  ============  ============================
Start address  Size          Content
=============  ============  ============================
  0x00010000       1Mb         Zerynth VM (slot 0)
  0x00110000       1Mb         Zerynth VM (slot 1)
  0x00210000     512Kb         Zerynth Bytecode (slot 0)
  0x00290000     512Kb         Zerynth Bytecode (slot 1)
=============  ============  ============================

For Esp32 based devices, the FOTA process is implemented mostly by using the provided system calls in the IDF framework. The selection of the next VM to be run is therefore a duty of the Espressif bootloader; the bootloader however, does not provide a failsafe mechanism to revert to the previous VM in case the currently selected one fails to start. At the moment this lack of a safety feature can not be circumvented, unless by changing the bootloader. As soon as Espressif relases a new IDF with such feature, we will release updated VMs. 

Secure Firmware
***************

Secure Firmware feature allows to detect and recover from malfunctions and, when supported, to protect the running firmware (e.g. disabling the external access to flash or assigning protected RAM memory to critical parts of the system).

This feature is strongly platform dependent; more information at :ref:`Secure Firmware - ESP32 section<sfw-esp32>`.

Missing features
****************

Not all IDF features have been included in the Esp32 based VMs. In particular the following are missing but will be added in the near future:

    * BLE support
    * Touch detection support 