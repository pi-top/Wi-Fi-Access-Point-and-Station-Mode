# Wi-Fi Access Point and Station Mode

> Easily start and stop using your device as a wireless access point. Specifically designed to work with the host device's onboard Wi-Fi interface (wlan0).
>
> Built for Raspberry Pi, but should work with other Debian-based systems running with compatible Wi-Fi hardware.

## Usage

``` bash
Usage:

    wifi-ap-sta {start,stop,status}

where:
    start  : use device as wireless access point.
    stop   : stop access point mode and restore previous configuration.
    status : display access point state and information.
```

## Default Behaviour

By default, the access point's SSID is the hostname of the machine that runs the script combined with the serial number of the device. The passphrase is an 8-digit random number.

General AP mode/DHCP server configuration:
```
AP_SSID=$(hostname)
AP_IFACE=ap0
AP_MAC=99:88:77:66:55:44
WIFI_IFACE=wlan0
STATIC_IP_PREFIX=192.168.90
IFACE_IP=${STATIC_IP_PREFIX}.1
SUBNET_IP=${STATIC_IP_PREFIX}.0
SUBNET_MASK=255.255.255.0
DHCP_RANGE_START=10
DHCP_RANGE_END=50
```

### Overriding default behaviour

Simply redefine the parameter you want to change in `/etc/default/wifi-ap-sta`, which is sourced by `wifi-ap-sta`.

This file is sourced in the main `wifi-ap-sta` script, so you can evaluate bash expressions to create dynamic results.

### Steps

#### Start

* Enable wireless interface via `rfkill` if necessary
* Create a virtual network interface to be used by the access point

* Create and start a new connection using `nmcli`

#### Stop

* Stop and remove connection

Also:
* Remove virtual network interface
* Restart networking services

## Station Mode: Things To Note

Even though the wireless interface is enabled, you may still need to configure it for it to work with `wpa_supplicant`. For example, in Raspberry Pi OS and derivatives you'll need to select your country during onboarding or set it manually using `raspi-config`.

## How It Works

This application creates an access-point connection using ``NetworkManager``.

### Constraints

This application is specifically developed to work on Raspberry Pi OS and derivatives, with some additional functionality to work with pi-topOS's DHCP server that is used for connecting to a pi-top [4] via the display cable (or connecting to a Pi via USB-OTG from the power connector).

As such, `systemd` and other core configuration is relied upon being present and largely unmodified. Unexpected behaviour may come from modifying the system state or running in a different environment.
