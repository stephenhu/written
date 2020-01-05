# wireguard

## setup
1. `sudo add-apt-repository ppa:wireguard/wireguard`
1. `sudo apt-get update`
1. `sudo apt-get install wireguard`

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
