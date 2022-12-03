# ir

*  /boot/overlays/README



## setup

* `sudo apt-get update`
* `sudo apt-get upgrade`
* `sudo apt-get install ir-keytable`

1. GND -> PIN 6 (ground)
1. VCC -> PIN 1 (3.3V power)
1. DAT -> PIN 11 (GPIO 17)
1. UAT -> PIN 4 (GPIO 14)

_seems GPIO 17 is the only PIN that works for both receiver and trasnmitter_

`/boot/config.txt`

```
dtoverlay=gpio-ir,gpio_pin=17
dtoverlay=gpio-ir-tx,gpio_pin=18
```

## commands

* `sudo ir-ctl -d /dev/lirc1 -r`
* `ir-ctl -d /dev/lirc1 --send=file.txt`
* `sudo ir-keytable`
* `ir-keytable -p all -t # enables all protocols`
* `sudo lsmod | grep gpio`
* `raspi-gpio get`

## references

* https://blog.gordonturner.com/2020/05/31/raspberry-pi-ir-receiver/

* https://blog.gordonturner.com/2020/06/10/raspberry-pi-ir-transmitter/
* https://www.raspberry-pi-geek.com/Archive/2015/10/Raspberry-Pi-IR-remote
* https://pinout.xyz/pinout/pin12_gpio18
* https://peppe8o.com/setup-raspberry-pi-infrared-remote-from-terminal/
* http://www.technoblogy.com/show?24A9
* https://www.mess.org/2019/09/03/Support-for-keymap-with-raw-IR-and-sending-key-from-keymap/
* https://forum.libreelec.tv/thread/12111-kernel-module-gpio-ir-sending-ir-codes/
* https://www.etechnophiles.com/raspberry-pi-4-gpio-pinout-specifications-and-schematic/
