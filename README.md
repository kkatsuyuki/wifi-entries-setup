
# addWifi

`addWifi` is the shell-script program which adds the new Wi-Fi entry 
to assigned configuration file (ex. `wpa_supplicant.conf`) 
using `wpa_passphrase`. 
To my knowledge interactive program `wpa_cli` attached with `wpa_supplicant` 
can update the file, but it refreshes already registed entries.

## Requirements

It requires the following Wi-Fi confuguration program.

-   [wpa\_supplicant](https://w1.fi/wpa_supplicant/)

## Install

Download and modify the path of the configuration file
(default: `/etc/wpa_supplicant/wpa_supplicant.conf`) 
in `addWifi`.

    $ git clone https://github.com/kkatsuyuki/addWifi.git
    $ cd addWifi
    
In `addWifi` edit `CONFIG_FILE` to adjust your environment.

    CONFIG_FILE=/to/yourpath/wpa_supplicant.conf
Then move this executable `addWifi` file to the directory included in `$PATH` (usually `/usr/bin/`).

    $ cp addWifi /usr/bin/

## Usage

Get the root privilege and execute `addWifi` 
with SSID and Passphrase(Optional) arguments. 

    Usage: addWifi [-h] SSID [Passphrase]

IF Passphrase is not given, the entry is registered as the open network.
If `addWifi` is executed with `-h` option or no arguments, the help 
message is appeared.
The following is the example to add the wireless
network whose SSID and passphrase are "foo" and "foobarbaz", respectively.

    # addWifi foo foobarbaz
    Protected Wifi network
    SSID foo was registered and you can connect.

## How to connect the Wi-Fi after registered

The followings are refered to [Wireless network configuration](https://wiki.archlinux.org/index.php/Wireless_network_configuration) on ArchWiki. 

If necessary, make the Wi-Fi interface enabled at first.

    # ip link set <interface> up
    
Then get the authentification and IP address from the wireless network.

    # wpa_supplicant -B -i <interface> -c /to/yourpath/wpa_supplicant.conf
    # dhcpcd -b <interface>
