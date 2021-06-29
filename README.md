# Wi-Fi Access Point and Station Mode

> Easily start and stop using your Raspberry Pi as a wireless access point.
> Specifically designed to work with the host device's onboard Wi-Fi interface (wlan0).
> Built for Raspberry Pi, but should work with other Debian-based systems running with compatible Wi-Fi hardware.

## Usage

``` bash
Usage:
    wifi-ap-sta {start,stop,status}
where:
    start  : use Raspberry Pi as wireless access point.
    stop   : stop access point mode and restore previous configuration.
    status : display access point state and information.
```

## Default Behaviour

By default, the SSID is the hostname of the machine that runs the script, and the default password is set based on the Raspberry Pi serial number. The configuration of the network created by the access point is read from `defaults.conf`.

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
