.. _nrf700x_wifi:

nRF700X Wi-Fi driver
####################

.. contents::
   :local:
   :depth: 2

This driver implements the Wi-Fi® protocol for the nRF700X FullMAC series of devices.
FullMAC devices implement the Wi-Fi protocol in the chipset.
The driver configures the chipset and transfers the frames to and from the device to the networking stack.

nRF700X is a companion IC and can be used with any Nordic Semiconductor System-on-Chips (SoCs) such as the nRF53 and nRF91 Series SoCs.

You can enable the driver by using the :kconfig:option:`CONFIG_WIFI_NRF700X` Kconfig option.

Architecture
*************

The following figure illustrates the architecture of the nRF700X Wi-Fi driver.

.. figure:: /images/nrf700x_wifi_driver.svg
   :alt: nRF700X Wi-Fi driver block diagram
   :align: center
   :figclass: align-center

   nRF700X Wi-Fi driver architecture overview

Design overview
***************

The nRF700X Wi-Fi driver follows an OS agnostic design, and the driver implementation is split into OS agnostic and OS (Zephyr) specific code.
The OS agnostic code is located in the :file:`drivers/wifi/nrf700x/osal/` folder and the OS specific code is located in the :file:`drivers/wifi/nrf700x/zephyr/` folder.

The driver supports two modes of operations:

Wi-Fi mode
^^^^^^^^^^
In this mode, the driver is designed to be used with the Zephyr networking stack.
It is implemented as a network interface driver.

The driver supports the following IEEE 802.11 features:

* Wi-Fi 6 (802.11ax) support
* WPA3/WPA2 personal security
* IEEE 802.11 Power Save modes
* Scan only mode
* IEEE 802.11 Station (STA) mode

The following features are in the driver code but not yet supported:

* IEEE 802.11 AP mode (Soft AP)
* Wi-Fi Direct mode

Except for scan only mode, the driver uses host access point daemon (hostapd) to implement AP Media Access Control (MAC) Sublayer Management Entity (AP MLME) and wpa_supplicant to implement 802.1X supplicant.

Radio test mode
^^^^^^^^^^^^^^^
The nRF700X Wi-Fi driver supports Radio Test mode, which you can use to test the RF performance of the nRF700X device.
This is a build time option that you can enable using the :kconfig:option:`CONFIG_NRF700X_RADIO_TEST` Kconfig option.

For more details about using this driver in Radio Test mode, see :ref:`wifi_radio_test`.

Driver to nRF700X communication
*******************************

The driver communicates with the nRF700X device using the QSPI/SPI interface.
The driver uses the QSPI/SPI interface to send commands to the nRF700X device, and to transfer the data to and from the device.
The nRF7002 DK uses QSPI whereas the nRF7002 EK uses SPI.

To connect the nRF7002 EK to the SoC, the ``nrf7002ek`` shield is required.


API documentation
*****************

After the nrf700x driver has been initialized, the application will see it as an Ethernet interface.
To use the Ethernet interface, the application can use `Zephyr Network APIs`_.

Low-level API documentation
***************************

The nRF700X Wi-Fi driver provides a low-level API for use cases where the application needs to access the nRF700X device directly.
This is typically intended for customers who want to use the nRF700X device in a different platform than Zephyr.

.. toctree::
   :maxdepth: 1
   :caption: Modules:
   :glob:

   nrf700x/*
