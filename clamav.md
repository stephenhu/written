# clamav

1. `xcode-select --install`

## install pcre2
1. `https://ftp.pcre.org/pub/pcre/pcre2-10.34.tar.gz`
1. `tar zxvvf pcre2-10.34.tar.gz`
1. `cd pcre2-10.34`
1. `make`
1. `sudo make install`

## install openssl
1. `tar zxvvf OpenSSL_1_0_2u.tar.gz`
1. `./Configure darwin64-x86_64-cc`
1. `make`
1. `sudo make install`

## install clamav
1. `./configure`
1. `make -j2`
1. `make test`

## configure

1. `cd /usr/local/share`
1. `mkdir clamav`
1. `freshclam`

modify /usr/local/etc/clamd.conf

make sure `LocalSocket /tmp/clamd.socket` and `LocalSocketMode 660` are uncommented

## usage

`sudo clamscan -r /` recursively perform one time scan

## notes

* clamd is the daemon
* freshclam updates the database definitions for viruses
* clamdscan connects to clamd to scan
* clamscan, different from clamdscan, is a one-time scanner, does not require clamd


# this is for homebrew
`./configure --with-openssl=/usr/local/Cellar/openssl/1.0.2l --with-zlib=/usr/local/Cellar/zlib/1.2.11/ --with-zlib=/usr/local/Cellar/zlib/1.2.11 --with-libjson=yes --enable-check`

* http://www.gctv.ne.jp/~yokota/clamav/
* https://github.com/openssl/openssl
* http://www.gctv.ne.jp/~yokota/clamav/#openssl
* https://www.clamav.net/documents/installation-on-macos-mac-os-x#Users-and-on-user-privileges
* https://www.clamav.net/documents/scanning#clamscan
* https://www.devguerrilla.com/notes/2014/09/linux-speeding-up-clamav-with-clamd-on-rhel/
