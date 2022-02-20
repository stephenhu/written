# debian

## setup

* `sudo apt-get install dpkg-dev devscripts`

## directory structure

debian
  /DEBIAN
    changelog
    compat
    control
    copyright
    postinst
    postrm
    preinst
    prerm
    rules
## useful commands

* `dpkg-deb --build debian`

## references

* https://debian-handbook.info/browse/stable/debian-packaging.html
* https://www.debian.org/doc/manuals/maint-guide/index.en.html
* https://www.baeldung.com/linux/create-debian-package#:~:text=How%20to%20Create%20a%20Simple%20Debian%20Package%201,4%20Verify%20the%20Package.%20...%205%20Conclusion.%20
* https://help.launchpad.net/Packaging/PPA
