# dbus

## notes

* polkit < 0.106 doesn't understand .rules files (they switched to using javascript syntax).  ubuntu 20.04 has polkit 0.105 by default which uses .pkla and .conf files instead
* need to add .pkla file to /etc/polkit-1/localauthority/50-local.d

contents of 10-piguard0.pkla:
```
[Allow all without authentication for members of sudo group]
Identity=unix-user:hu
Action=org.freedesktop.systemd1.manage-units;org.freedesktop.login1.power_off;org.freedesktop.login1.reboot
ResultAny=yes
ResultInactive=yes
ResultActive=yes
```

## useful commands

* `pkaction`
* `pkaction --version`
* `busctl call org.freedesktop.login1 /org/freedesktop/login1 org.freedesktop.login1.Manager Reboot b no`
* `busctl call org.freedesktop.systemd1 /org/freedesktop/systemd1 org.freedesktop.systemd1.Manager StartUnit ss "dnsmasq.service" "replace"`
* `busctl introspect org.freedesktop.login1 /org/freedesktop/login1`
* `busctl introspect org.freedesktop.systemd1 /org/freedesktop/systemd1`
* `busctl introspect org.freedesktop.DBus /org/freedesktop/DBus`
* ` busctl call org.freedesktop.systemd1 /org/freedesktop/systemd1 org.freedesktop.systemd1.Manager GetUnit s wg-quick@wg0.service`
* ` busctl get-property org.freedesktop.systemd1 /org/freedesktop/systemd1/unit/wg_2dquick_40wg0_2eservice org.freedesktop.systemd1.Unit ActiveState`

```
busctl call org.freedesktop.systemd1 /org/freedesktop/systemd1 org.freedesktop.systemd1.Manager GetUnit s wg-quick@wg0.service
o "/org/freedesktop/systemd1/unit/wg_2dquick_40wg0_2eservice"
```

## references

* https://trstringer.com/python-systemd-dbus/
* https://www.freedesktop.org/wiki/Software/systemd/dbus/
* https://stackoverflow.com/questions/31415665/using-gdbus-to-start-a-systemd-service
* https://pi3g.com/2020/08/01/enabling-and-disabling-a-systemd-service-in-python-using-dbus/#:~:text=disable%20a%20service%3A%20import%20dbus%20system_bus%20%3D%20dbus.SystemBus,manager%20%3D%20dbus.Interface%20%28systemd1%2C%20%E2%80%98org.freedesktop.systemd1.Manager%E2%80%99%29%20manager.DisableUnitFiles%20%28%5B%E2%80%98picockpit-client.service%E2%80%99%5D%2C%20False%29
* https://stackoverflow.com/questions/32332128/how-to-use-polkit-with-python-to-run-systemd-d-bus-commands-from-non-root-user
* https://venam.nixers.net/blog/unix/2020/07/06/dbus-polkit.html
* https://wiki.archlinux.org/title/Polkit
* https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-security-policykit.html
* https://unix.stackexchange.com/questions/606452/allowing-user-to-run-systemctl-systemd-services-without-password/606476#606476
* https://electronproton.com/busctl-command-examples/
* https://prog.world/monitoring-systemd-services-in-real-time-with-chronograf/
* https://zignar.net/2014/09/08/getting-started-with-dbus-python-systemd/
* https://stackoverflow.com/questions/43499880/how-to-extract-service-state-via-systemd-dbus-api
* https://www.programcreek.com/python/example/1862/dbus.Interface