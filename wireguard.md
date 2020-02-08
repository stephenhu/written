# wireguard

## setup
1. `sudo add-apt-repository ppa:wireguard/wireguard`
1. `sudo apt-get update`
1. `sudo apt-get install wireguard`

## generate keys
1. `wg genkey > wg0.conf` # /etc/wireguard
1. `wg pubkey < example.key > example.key.pub`

## server config

Create the following file: `/etc/wireguard/wg0.conf`

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

## client config

depends on the client, i use the mac os x wireguard gui and android wireguard client, will refrain from adding screenshots.

* use the vpn server's `public key` in the peer section.
* use the address:port for the peer section variable `endpoint`
* use 0.0.0.0/0, ::/0 for `AllowedIPs`

## start/stop
1. `wg-quick up wg0`  # if wg0.conf is in /etc/wireguard/wg0.conf
1. `wg-quick down wg0`

## additional useful commands
* `sudo modprobe wireguard` # used to invoke kernel module, reboot should also load automatically
* `lsmod | grep wireguard`
* `$ sudo wg set wg0 peer CLIENT_PUB_KEY allowed-ips CLIENT_VPN_IP/32`

## references

* https://securityespresso.org/tutorials/2019/03/22/vpn-server-using-wireguard-on-ubuntu/
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