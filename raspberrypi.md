# building product w/ raspberry pi

one of my recent side projects was to build a digital scoreboard for sporting events.  i decided to use a raspberry pi model 3b connected
to an led monitor as the solution.  i had a lot of fun building this project, but there were lots of tradeoffs and choices so
here are my experiences for anyone interested in building similar devices.  by the way, the terms of the raspberry pi are extremely
product-friendly, i.e. you can sell products based on raspberry pi without paying any royalties/license fees, the raspberry pi
foundation merely suggests that you mention their product on your pages or product documentation, and this isn't even a hard 
requirement.

## scoreboard product

the scoreboard should leverage a commodity 4k tv for display, the bigger the better, a smartphone as a client controller, and some sort of service on the backend to manage state and persist data.  i initially planned on using an apple tv to wirelessly forward my mobile client's data to the tv, but found that a mobile app makes a bad backend service because a) a phone call or other 
distraction could potentially disrupt the service  b) a device that's frequently wading through different ip addresses or 
potentially using cellular networks would make it difficult for clients to connect consistently  c) apple tv would add to the cost and keep you stuck on apple's stack  d) you wouldn't want all your facebook and other private sms messages to pop up on the tv.  so the end decision was to employ a low-cost, low-powered, small form factor server that could connect to any large 4k tv, the raspberry pi fits perfectly into this category.

the backend needs to support multiple client connections (long term) with frequent reads and writes, e.g. start clock, stop clock, increase away team points by 2, call timeout, increase total fouls, etc; there's typically one person controlling a clock, a different person managing the score.  i originally thought about hosting the backend in the cloud, but for my digital scoreboard, i need millisecond accuracy for the game clocks, the lag over wan would be a non-starter, otherwise the clock would look jittery jumping all over the place, e.g. from 4.6s to suddenly 5.2s, lan's latency is tolerable.  on top of the service, i need a desktop environment that can display a browser, my scoreboard will basically be a webpage that can be easily customized.  the device also needs to handle storage and playback of various media like high resolution video, audio, and photos.

to control the scoreboard service, i need a smartphone app, iphone and android being the obvious platforms to support so
the backend has to support both of these.

## hardware stack

i chose raspberry pi because i had heard a lot of great things about it, it's quite affordable and performance is decent, plus i
always wanted to play around with one of these.  the raspberry pi 3 model b is based on an arm (v7) quad core 1.2ghz broadcom bcm2837 
64-bit processor for low power consumption, includes hdmi output, 1g ram, onboard wifi router, the ability to use microsd cards for 
storage, is quite expandable with 4k cameras, sensors, etc, and the size and form factor are just about right, like the size of a non-4k 
apple tv device.  for my digital scoreboard, this could hang on the wall or ceiling of an indoor basketball gym so size and 
energy efficieny are important.

to note, the raspberry pi 3 model b+ was just recently released, i have not gotten my hands on one yet, but everything seems to be a 
bump up, wifi, bluetooth, cpu (quad core broadcom bcm2837b0, cortex-a53 (v8) 64-bit soc @ 1.4ghz), even ethernet.  i would recommend the 
3 model b+ over the 3 model b since the price is relatively the same.

there are many other singleboard devices like raspberry pi, like the asus tinker board, orange pi, beagleboard, etc, and i think i heard 
or read from some review that the raspberry pi is the lower end of the bunch in terms of performance (and price), especially i heard the 
storage controller is a shared bus or some such which limits the throughput, you'll have to look this up on your own, but thus far, for
my needs, things seem to work well, note that i have not done extensive performance testing or the like yet.

## software stack

### operating system

in terms of operating systems, i've always liked using linux and the recommended system on raspberry pi is raspbian, this is 
a debian based linux distribution, but its purpose is specifically to create a desktop environment for education, 
programming, or general use, there were lots of applications like mathematica and libre ofice that just weren't needed, the size required 
for install was over 4gb and the default window manager is something called pixel which stands for _pi improved xwindows 
environment lightweight_ which is based on [lxde](http://lxde.org).  note that there's also a window-less version of raspbian 
called raspbian lite, the total download size for this is around 325mb which is quite acceptable, from there you can add the 
packages that you need.

i tried ubuntu-mate which required 8g storage, though also debian based, but this seemed much more familiar to me, 
it was running 16.04 LTS, mate (gnome 2) and the menus and everything seemed fairly familiar.

ubuntu core is built for iot devices which raspberry could be construed as, but ubuntu core leverages docker which is just overkill for this project, i'd need additional memory for the docker processes, and there is this whole thing about forcing you to create 
docker images and putting them up on some ubuntu snap service which requires registration that just turned me off entirely.  i never 
measured how much more resources i would need to run docker, but with 1g ram, it just didn't seem necessary, i'm not trying to deploy thousands or even hundreds of web applications, my stack is fairly opinionated, at most a couple processes, and they don't need runtimes to be installed like python or ruby, so this is just unnecessary cruft.  the ubuntu core itself is quite minimalized, but the deep, ingrained docker dependency is a non-starter for me.  i like the concept of a minimalized linux with just enough to run your workload, but i also need to load a window manager, xorg, browser, etc, so it's not only a headless setup. 

fedora atomic is very similar to ubuntu core--in addition to docker, k8 is built in as well and seems to 
serve as a way to build up kubernetes clusters.

microsoft has windows iot, but just the thought of having to develop on windows and the potential licensing costs turned me off.

there are other distributions, like [diet pi](https://dietpi.com/), which is actually a tool to help you build a customized linux for arm, but just 
too much work at this point, i just wanted to get my scoreboard up and running, maybe an exercise for later.  and [armbian](https://www.armbian.com/) deserves special mention, this is another minimalized linux for arm based on debian.  the other way to tighten up the install is to install something like ubuntu-mate and then work backwards and remove what you don't like, i think this method is like taking a step backwards because you're installing and then un-installing.

the operating system image needs to be installed onto a microsd card, for me, my main development platform is a macbook pro 
(usb-c) which doesn't have a microsd reader so i had to go purchase a usb to microsd dongle which is about the size of a usb thumb drive, it also happened to have ports for memory stick, sd cards, and other card formats, i guess these are usually referred to as a multi-card reader.

# TODO: screenshot of multi-card reader

to get os images onto the raspberry pi, initially i used the dd command line tool, but i found a gui tool that works 
perfectly across multiple platforms (linux, mac, windows) called [etcher](https://etcher.io/).  i highly recommend this 
tool, i think they also have a hardware device that can burn os images onto multiple microsd cards in parallel, you could 
build a mini-factory to ship large amounts of raspberry pi devices in this manner.

the base ubuntu-mate image required a few additional packages to be installed:

1.  `sudo apt-get install chromium-browser`
1.  `sudo apt-get install openssh-server`

in the end, i used raspbian-lite with lxde, the interface is actually very similar to mate (gnome 3) and seems to be built for lightweightness.

1.  `sudo apt-get install xinit xserver-org`
1.  `sudo apt-get install lxde-core`
1.  `sudo apt-get install lightdm`
1.  `sudo apt-get install chromium-browser`

# TODO: raspi-config

once you have everything tweaked to your liking, operating system plus all the application code and configuration, you can 
save an image of the entire system and create an image that you can then use etcher to copy onto other microsd cards.

# TODO: dd command

### backend server

i had learned golang around the time i started this project and decided to create a websocket server to manage the 
clock/scoreboard, this would allow clients to update and receive data asynchronously.  one really nice thing about golang is 
that you can compile and copy a binary directly to the target platform, you don't really need to install a golang compiler 
(or runtime) onto the device which would save me space and complexity (think upgrades to golang, having to have source code 
on the device itself, git pull's, etc), golang binaries are all inclusive, all dependencies linked into the single binary which makes the binaries larger, but obviously during runtime, it dynamically links in only the necessary pieces.  all persistent data is stored to an sqlite file, i didn't want to have another 
database process running as my data requirements are not large, probably a few thousand to at most a million rows over 5 or 
so simple tables, so an embedded storage system was preferred.

there also needed to be a web server to serve up static pages (html, css, js), i just bundled this together with the golang 
websocket server.

and last, but not least, this backend server needs to be started automatically everytime the device is started or if the 
process crashes.  i leveraged systemd to start the process automatically and restart if the process goes down.  here's my 
systemd configuration file from /etc/systemd/system/mboard.service

```
[Unit]
Description=madsportslab mboard daemon
After=network-online.target

[Service]
Type=simple
WorkingDirectory=/home/mboard/bin
ExecStart=/home/mboard/bin/mboard-go -mode 1 -database /home/mboard/data/mboard.db
Restart=always

[Install]
WantedBy=multi-user.target
```

you need to run a few commands to get this loaded by systemd

1. `sudo systemctl enable mboard`
1. `sudo systemctl start mboard`
1. `sudo systemctl status mboard`

### browser application startup

the goal of the device is to spin up linux really quickly, automatically login so that there's no prompting for 
username/password, start up chrome browser in full screen mode, and point at the very first page of the scoreboard.

to accomplish this, i had to first enable automatic login for a linux user, this can be done from the ubuntu user and 
password manager gui. TODO:

the next step is to leverage linux [autostart](https://www.freedesktop.org/wiki/Specifications/autostart-spec/) to start the 
chromium browser.

i added the file /home/mboard/.config/autostart/mboard.desktop

```
[Desktop Entry]
Type=Application
Exec=/usr/bin/chromium-browser --noerrdialogs --disable-session-crashed-bubble --disable-infobars --incognito --password-store=basic --kiosk http://127.0.0.1:8000/setup
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=mboard
```

### wifi router enablement

in some basketball courts, there's a lack of broadband internet or cellular network, i.e. no wan access, so i had to accommodate for 
these types of environments.  ubuntu comes with the ability to make your linux server into a wireless router with dhcp, in this manner, 
if there's no wan or lan network, you could connect to the ssid of the device on your phone and control the device in this manner.

for those environments with broadband and wifi available, the task becomes of connecting to the right ssid with the right credentials 
and security protocols.  so how does this happen when you don't have a keyboard and mouse connected to the raspberry pi device?

### upgrades

again i have to accommodate for environments (courts) where there's no wan access, in this case, you still need to be able to upgrade 
the device.  if the cellular network is not even available then there has to be a manual way to update the device, i.e. you could
download an update when you are on the network, cache it on your phone, or somehow connect with the raspberry pi using some other backup 
method like a thumb drive or laptop, in either case, the update binary is downloaded ahead of time in a place where there's wan access. 
thumb drives are no good if the raspberry pi is installed way up on the ceiling or some unreachable area, but if the raspberry pi is 
accessible, a thumb drive is a pretty good way of updating given that thumb drives are abundant and especially given that the raspberry 
pi provides 4 usb ports.

TODO: automating upgrades

### media playback

initially i was embedding video tags in my static html pages that would access the local media files on the raspberry pi device, i then 
found out that hardware acceleration is not enabled for the ubuntu-mate build of chromium that means that high resolution video will be 
extremely choppy and unviewable on raspberry pi's.  what i did find out is that there's a natively compiled package as part of raspbian 
that's also ported to ubuntu-mate called omxplayer, it's a command line tool which is perfect for my case as i could call this from my 
static page/websocket server.

omxplayer has a way to pipe commands asynchronously such as pausing or stopping the video using linux [d-bus](https://www.freedesktop.org/wiki/Software/dbus/).  i didn't leverage this yet as i basically fork a process from golang to run omxplayer.  each time a video is played is a forked process, but i think d-bus is definitely the cleaner method moving forward, just a time trade off.

### desktop background image

`sudo gsettings set org.gnome.desktop.background picture-uri "file://home/mboard/splash.001.png"`

### boot screen

instead of looking at 4 raspberries plus a bunch of log information from dmesg, perhaps you'd like a custom startup progress 
indicator or something with your device's logo.  ubuntu leverages a bootloader called grub, modify this file to show the 
splashscreen for ubuntu:

`sudo vi /etc/default/grub`

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"


### wake on lan

in terms of power, the raspberry pi is relatively energy efficient, but there will be scenarios where you'd want to turn off 
or suspend the raspberry pi remotely without having to use ssh ideally. 

### logging and debugability

### build, packaging

naturally on ubuntu apt-get is the best way to package up your code.  i didn't want to deal with hosting binaries on ubuntu mirrors so 
i'm distributing .deb package files.  i've actually never gone through the process of hosting debian packages on the ubuntu mirror, and 
i'm sure it's not terribly difficult, but at the moment i have a local build script that assumes all the updated source files are 
available locally then compiles the golang application, stores it into a .deb binary.  you can actually cross-compile golang, so for 
example if i need an arm based golang binary, i can still compile on an x86 by setting GOOS and GOARCH environment variables.

`GOOS=linux GOARCH=arm go build`

there's a nice summary from [digital ocean](https://www.digitalocean.com/community/tutorials/how-to-build-go-executables-for-multiple-platforms-on-ubuntu-16-04) on how to cross compile golang applications for your platform.

### final stack

![Alt text](https://github.com/stephenhu/written/blob/master/mboard-stack.001.png)

### conclusion

several things still need tightening up, the linux distribution still has a lot of unused packages that could be cleaned up, this could 
save storage space, but also reduce your surface area for attacks.

## references

* https://hackaday.com/2019/07/10/raspberry-pi-4-benchmarks-processor-and-network-performance-makes-it-a-real-desktop-contender/#:~:text=While%20wired%20connections%20are%20always%20better%2C%20the%20Raspberry,20%20and%2030%20Mbps%20in%20the%20same%20environment.