# wireguard

## setup
1. `sudo apt-get install resolvconf` # seems this isn't installed by default
1. `sudo add-apt-repository ppa:wireguard/wireguard` # newest ubuntu includes wireguard, no need for this step
1. `sudo apt-get update`
1. `sudo apt-get install wireguard`
1. `sudo modprobe wireguard` # used to invoke kernel module, reboot should also load automatically
1. `lsmod | grep wireguard`
1. `cd /etc/wireguard`
1. `umask 077`
1. `wg genkey | sudo tee server_private_key | wg pubkey | sudo tee server_public_key`

## server config

Create the following file: `/etc/wireguard/wg0.conf`

_make certain you replace eth0 with the right nic for your machine_

```
[Interface]
Address = 10.10.0.1/24
Address = fd86:ea04:1111::1/64
SaveConfig = true
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE; ip6tables -A FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE; ip6tables -D FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
ListenPort = 9023
PrivateKey = <Server Private Key>

[Peer]
PublicKey = <Client #1 Public Key>
AllowedIPs = 10.10.0.2/32, fd86:ea04:1111::2/128

[Peer]
PublicKey = <Client #2 Public Key>
AllowedIPs = 10.10.0.3/32, fd86:ea04:1111::3/128
```

### start/stop
1. `wg-quick up wg0`  # /etc/wireguard/wg0.conf
1. `wg-quick down wg0`

## client config

_if you want to use the same computer as both a server and client, you'll need
to create 2 different conf files wg0.conf and wg0c.conf (client) for example._

1. `wg genkey | sudo tee client_private_key | wg pubkey | sudo tee client_public_key`

use a separate file from the server config:

```
[Interface]
Address = 10.10.0.5/32 # this should correspond to the address the server will give you
PrivateKey = <client private key>
DNS = 1.1.1.1  # this is google's, can try using your own dns like unbounded

[Peer]
PublicKey = <server public key>
Endpoint = getsdone.xyz:9095
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 21
```

depends on the client, i use the mac os x wireguard gui and android wireguard client, will refrain from adding screenshots.

* use the vpn server's `public key` in the peer section.
* use the address:port for the peer section variable `endpoint`
* use 0.0.0.0/0, ::/0 for `AllowedIPs`

## enable systemctl
* `sudo systemctl enable wg-quick@wg0`

### start/stop
1. `wg-quick up wg0c`  # /etc/wireguard/wg0c.conf
1. `wg-quick down wg0c`

## additional useful commands
* `$ sudo wg set wg0 peer CLIENT_PUB_KEY allowed-ips CLIENT_VPN_IP/32`

## OUTDATED: generate keys
_DO NOT USE THESE COMMANDS FOR NOW, I DON'T THINK THESE ARE THE RIGHT COMMANDS, BUT KEEPING JUST IN CASE_
1. `wg genkey > wg0.conf` # /etc/wireguard
1. `wg pubkey < example.key > example.key.pub`

## references

* https://securityespresso.org/tutorials/2019/03/22/vpn-server-using-wireguard-on-ubuntu/  # top article
* https://github.com/pirate/wireguard-docs
* https://golb.hplar.ch/2018/10/wireguard-on-amazon-lightsail.html
* https://www.ckn.io/blog/2017/11/14/wireguard-vpn-typical-setup/
* https://research.kudelskisecurity.com/2017/06/07/installing-wireguard-the-modern-vpn/
* https://freedif.org/unbound-your-own-dns-server
* https://www.digitalocean.com/community/tutorials/how-to-create-a-point-to-point-vpn-with-wireguard-on-ubuntu-16-04
* https://www.linode.com/docs/networking/vpn/set-up-wireguard-vpn-on-ubuntu/
* https://anthonyspiteri.net/quick-post-installing-wireguard-client-on-macos/
* https://www.wireguard.com/#ready-for-containers
* https://github.com/pirate/wireguard-docs#Peer
* https://github.com/cmulk/wireguard-docker
* https://github.com/ironhalik/docker-wireguard
* https://maximilianehlers.com/blog/docker-in-wireguard/
* https://routing-bits.com/2009/03/19/converting-ipv4-to-ipv6/
* https://www.cyberciti.biz/faq/ubuntu-20-04-set-up-wireguard-vpn-server/
* https://websiteforstudents.com/how-to-install-wireguard-vpn-server-on-ubuntu-18-04-20-04/
* https://github.com/StreisandEffect/streisand/issues/1434
* https://jawher.me/wireguard-ansible-systemd-ubuntu/
* https://openwrt.org/docs/guide-user/services/vpn/wireguard/automated
