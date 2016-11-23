
# addWifi

`addWifi` is the shell-script program which adds the new SSID entry 
to `wpa_supplicant.conf` using `wpa_passphrase`. 
To my knowledge interactive program `wpa_cli` attached with `wpa_supplicant` can update 
the file, but it refreshes already registed entries.

## Requirements

It requires the following wifi confuguration program.

-   [wpa\_supplicant](https://w1.fi/wpa_supplicant/)

## Install

Download and move this executable `addWifi` file to the directory included in `$PATH`.

    $ git clone https://github.com/kkatsuyuki/addWifi.git
    $ cd addWifi
    $ cp addWifi /to/yourpath/

## Usage

You can use this script very easy. Get the root privilege and execute `addWIfi`, then 
input the SSID 
and its passphrase you want to access. The following is the example to add the wireless
network whose SSID and passphrase are "hoge" and "hugaHuga", respectively.

    # addWifi
    Input wifi parameters as the following format
    SSID passphrase:
    > hoge hugaHuga
    SSID hoge was registered.

## Modify

Maybe you should add the command to connect the wifi network. In the case of 
me I use the wireless interface `wlp2s0`, so I uncomment the following command in 
`addWifi`.

    ⋮
    # systemctl restart network-wireless@wlp2s0.service
    ⋮

The following is `network-wireless@.service`.

    [Unit]
    Description=Wireless network connectivity (%i)
    Wants=network.target
    Before=network.target
    
    [Service]
    Type=oneshot
    RemainAfterExit=yes
    
    ExecStart=/usr/bin/ip link set dev %i up
    ExecStart=/usr/bin/wpa_supplicant -B -i %i -c /etc/wpa_supplicant/wpa_supplicant.conf
    # ExecStart=/usr/bin/dhcpcd -t 0 %i
    ExecStart=/usr/bin/dhcpcd -p -b %i
    
    ExecStop=/usr/bin/ip link set dev %i down
    
    [Install]
    WantedBy=multi-user.target

Writing the above code I refered to [Wireless network configuration](https://wiki.archlinux.org/index.php/Wireless_network_configuration) on ArchWiki. 
If you don't use the other network managers (Netctl, NetworkManager, &#x2026;)
except above wpa\_supplicant,
you can use this code by modifying the wireless interface `wlp2s0`.
