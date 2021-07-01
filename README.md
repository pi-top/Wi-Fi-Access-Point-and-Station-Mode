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

### Settings

By default, the access point's SSID is the hostname of the machine that runs the script. If the machine is a Raspberry Pi, the default passphrase is based in the device's serial number; otherwise it defaults to `12345678`. This allows for repeatable network credentials between sessions, making it easy for clients to connect to their local device even if the OS has been reset. The configuration of the network created by the access point is read from `defaults.conf`.

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

Even though the wireless interface is enabled, you may still need to configure it for it to work with `wpa_supplicant`. For example, in Raspberry Pi OS and derivatives you'll need to select your country during onboarding or set it manually using raspi-config.

## AP Customization

The access point network configuration can be customized by editing `defaults.conf` (located in `/etc/default/wifi-ap-sta/defaults.conf`); for example, setting up a different IP address for the network, the range of addresses that the DHCP server can lease, and the network SSID and passphrase.

This file is sourced in the main `wifi-ap-sta` script, so you can use expresions that will be evaluated later on.

## How It Works

This application makes use of ``hostapd`` and ``isc-dhcp-server`` to work.

``hostapd`` (`host` `a`ccess `p`oint `d`aemon) is a user space daemon software that can enable a network interface card to act as an access point and authentication server.

``isc-dhcp-server`` (`I`nternet `S`ystems `C`onsortium's `-` `D`ynamic `H`ost `C`onfiguration `P`rotocol `-` `server`) is a network service that enables host computers to be automatically assigned settings from a server as opposed to manually configuring each network host.

### Constraints

This application is specifically developed to work on Raspberry Pi OS and derivatives, with some additional functionality to work with pi-topOS's DHCP server that is used for connecting to a pi-top [4] via the display cable (or connecting to a Pi via USB-OTG from the power connector).

As such, `systemd` and other core configuration is relied upon being present and largely unmodified. Unexpected behaviour may come from modifying the system state or running in a different environment.
