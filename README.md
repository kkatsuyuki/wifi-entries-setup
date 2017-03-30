
# addWifi

`addWifi` is the shell-script program which adds the new SSID entry 
to `wpa_supplicant.conf` using `wpa_passphrase`. 
To my knowledge interactive program `wpa_cli` attached with `wpa_supplicant` can update 
the file, but it refreshes already registed entries.

## Requirements

It requires the following wifi confuguration program.

-   [wpa\_supplicant](https://w1.fi/wpa_supplicant/)

## Install

At first download and modify the path of `wpa_supplicant.conf` in `addWifi`.

    $ git clone https://github.com/kkatsuyuki/addWifi.git
    $ cd addWifi
    
In `addWifi` edit as the following.

    CONFIG_FILE=/to/yourpath/wpa_supplicant.conf
Then move this executable `addWifi` file to the directory included in `$PATH` (usually `/usr/bin/`).

    $ cp addWifi /usr/bin/

## Usage

You can use this script very easy. Get the root privilege and execute `addWIfi` 
with SSID and Passphrase(Optional) arguments. 

    Usage: addWifi [-h] SSID [Passphrase]

IF Passphrase is not given, the entry is registered as the open network.
If `addWIfi` is executed with `-h` option or no arguments, the help 
message is appeared.
The following is the example to add the wireless
network whose SSID and passphrase are "foo" and "foobarbaz", respectively.

    # addWifi foo foobarbaz
    Protected Wifi network
    SSID foo was registered and you can connect.

## How to connect the wifi after registered

The followings are refered to [Wireless network configuration](https://wiki.archlinux.org/index.php/Wireless_network_configuration) on ArchWiki. 

If necessary, make the wifi interface enabled at first.

    # ip link set <interface> up
    
Then get the authentification and IP address from the wireless network.

    # wpa_supplicant -B -i <interface> -c /to/yourpath/wpa_supplicant.conf
    # dhcpcd -b <interface>
