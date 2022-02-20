# pi guard

pi guard is a wifi router built on top of raspberry pi, the portable allows you to take this with you wherever you go.

## requirements

* raspberry pi 4 model b (recommended)
* wireguard server
* ethernet wan access

## tldr

1.  setup raspbian lite (tested with oct )
1.  install/setup dnsmasq
1.  install/setup hostapd
1.  install/setup wireguard


### raspbian

1. `sudo apt-get update`
1. `sudo apt-get upgrade`
1. `sudo apt-get install -y netfilter-persistent iptables-persistent`

### dnsmasq

dnsmasq is used to provide dhcp server capabilities which assigns ip addresses to wifi clients.  it also provides recursive dns capabilities, but is not leveraged.

1.  `sudo apt-get install dnsmasq`
1.  `sudo vi /etc/dnsmasq.conf`

```
interface=wlan0
dhcp-range=192.168.8.2,192.168.8.20,255.255.255.0,24h
domain=raspberry # local wireless DNS domain
address=/piguard.raspberry
```

1.  `sudo vi /etc/dhcpcd.conf`

```
interface wlan0
    static ip_address=192.168.8.1/24
    nohook wpa_supplicant
    denyinterfaces wlan0
```

### sysctl

1.  enable packet forwarding by adding this line from `/etc/sysctl.d/routed-ap.conf`

```
net.ipv4.ip_forward=1
```

`on the latest jan 22 2022 release of raspbian, the file is /etc/sysctl.d/99-sysctl.conf`


### hostapd

1.  `sudo apt-get install hostapd`
1.  `sudo vi /etc/hostapd/hostapd.conf`
1.  `sudo systemctl unmask hostapd`
1.  `sudo systemctl enable hostapd`

```
country_code=CN
interface=wlan0
driver=nl80211
ssid=piguard
hw_mode=g
channel=6
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=piwireguard
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
wpa_pairwise=CCMP

```

### country code

1.  add these 3 lines to `/etc/wpa_supplicant/wpa_supplicant.conf`

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CN
```

### routing rules

1. `sudo iptables -t nat -A POSTROUTING -o wg0 -j MASQUERADE`
1. `sudo iptables -A FORWARD -i wg0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT`
1. `sudo iptables -A FORWARD -i wlan0 -o wg0 -j ACCEPT`
1. `sudo netfilter-persistent save`
1. `sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE` # use this if you don't need wireguard

### wireguard

1. `systemctl enable wg-quick@wg0`
1.  `chown -R root:root /etc/wireguard`
1.  `chmod -R og-rwx /etc/wireguard/*`

### errata

* i tested several raspberry pi 3 model b's with varying results, wifi is extremely unstable and seems to disconnect often, there have been no issues with raspberry pi 4.
* using `raspi-config`, i couldn't get the wifi country set, there were repeated errors about this, so you can change this in the wpa_supplicant file

### todo

* administrative console
* need a way to turn on/off vpn (p2)
* dashboard/monitoring
* support 802.11a (p0)
* rules based routing (p0)

### default ssh account

`pi/piguard08`

### default ssid account

`piguard/piwireguard`

### references

* `https://www.raspberrypi.com/documentation/computers/configuration.html#setting-up-a-routed-wireless-access-point`
* `https://zhuanlan.zhihu.com/p/163827788`
* `https://github.com/raspberrypi/firmware/issues/1359`
* `https://shawnliu.me/post/smart-and-transparent-routing-with-policy-based-routing/`
* `https://philipdeljanov.com/posts/2019/03/21/setting-up-a-wireguard-vpn/`