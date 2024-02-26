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

`hostapd` configuration:
```
ctrl_interface=/var/run/hostapd
ctrl_interface_group=0
hw_mode=g
channel=6
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_pairwise=TKIP
rsn_pairwise=CCMP
country_code=GB
```

### Overriding default behaviour

Simply redefine the parameter you want to change in `/etc/default/wifi-ap-sta`, which is sourced by `wifi-ap-sta`.

This file is sourced in the main `wifi-ap-sta` script, so you can evaluate bash expressions to create dynamic results.

### Steps

#### Start

* Enable wireless interface via `rfkill` if necessary
* Create a virtual network interface to be used by the access point
* Patch system network configuration files
* Start access point service through `hostapd`
* Restart networking services

#### Stop

* Stop `hostapd` access point service
* Restore patched networking configuration files
* Remove virtual network interface
* Restart networking services

## Station Mode: Things To Note

Even though the wireless interface is enabled, you may still need to configure it for it to work with `wpa_supplicant`. For example, in Raspberry Pi OS and derivatives you'll need to select your country during onboarding or set it manually using `raspi-config`.

## How It Works

On bookworm, this application creates an access-point connection using the ``NetworkManager``. On older distributions, this application makes use of ``hostapd`` and ``isc-dhcp-server`` to do this.

``hostapd`` (`host` `a`ccess `p`oint `d`aemon) is a user space daemon software that can enable a network interface card to act as an access point and authentication server.

``isc-dhcp-server`` (`I`nternet `S`ystems `C`onsortium's `-` `D`ynamic `H`ost `C`onfiguration `P`rotocol `-` `server`) is a network service that enables host computers to be automatically assigned settings from a server as opposed to manually configuring each network host.

### Constraints

This application is specifically developed to work on Raspberry Pi OS and derivatives, with some additional functionality to work with pi-topOS's DHCP server that is used for connecting to a pi-top [4] via the display cable (or connecting to a Pi via USB-OTG from the power connector).

As such, `systemd` and other core configuration is relied upon being present and largely unmodified. Unexpected behaviour may come from modifying the system state or running in a different environment.
