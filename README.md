# rpi-wifi-ap

Set up a Raspberry Pi as a wireless access point, using `hostapd` to create the wireless hotspot and `isc-dhcp-server` as DHCP server.

By default, the SSID is the hostname of the machine that runs the script, and the default password is `12345678`; it's highly recommended that you change it. The configuration of the network created by the access point is given in `defaults.conf`.

## Usage

``` bash
Usage: ./rpi-wifi-ap <start|stop>
Application that sets up a Raspberry Pi as a wireless access point.
```

## AP Customization

The access point network configuration can be customized by editing `defaults.conf` (located in `/etc/default/rpi-wifi-ap/defaults.conf`); for example, setting up a different IP address for the network, the range of addresses that the DHCP server can lease, and even SSID and passphrase.

This file is sourced in the main `rpi-wifi-ap` script, so you can use expresions that will be evaluated later on.
