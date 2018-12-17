# Running Stepmania 5 with SMX

This guide walks you through building a stepmania 5 rig and using it with your Stempaniax All-in-one (AIO) or Dedicab machine.  All of these instructions were tested with an AIO but should work similarly with a dedi.

This does NOT modify the SMX machine itself, besides unplugging a few things then later plugging them back in.  It is not recommended that you try to modify your SMX hardware (it simply isn't necessary).

## Hardware

First order of business is the sm5 rig itself.  I ordered my machine from Qotom, I got [this one](https://www.amazon.com/gp/product/B014113X02).

It cost just under $300 and had a bunch of stuff I didn't need, but importantly, had:
* Wifi
* 8G of ram
* 128G SSD
* multiple USB ports
* quad core CPU, intel graphics HD, hdmi port capable of audio output

Looking around, you can probably get similar for as little as ~$200

I can tell you that the machine is not as snappy as I would like when I'm compiling or working on the command prompt, but it loads thousands of songs ok and when I play a song, the load average remains under 1.0 and it does not stutter or have any problems.  If you are worried, just buy a real gaming machine and use that instead.  The advantage of this machine is it is cheap(ish) and has no moving parts.

Finally, you are going to want to get some cabling and stuff to make it easy to swap between SMX and SM5.  Here you have two options.

* You could get an HDMI/USB KVM [like this](https://www.amazon.com/IOGEAR-2-Port-Switch-Cables-GCS62HU/dp/B004YCUDMU/)
* You can just get extra cables and extensions and adapters and plug/unplug stuff

I recommend the later - it is cheaper and will not introduce compatibility or latency problems.

    # Here is a rough wiring diagram for my AIO:

    --------
    |      |
    |      | E
    |      |
    |      | D
    |      | C
    --------
     F A B

    A: OTG USB - lets you plug in a thumb drive I guess?  Keyboards don't work when plugged into this, not sure if it is the port or other magic...
    B: Player 1 and 2 SMX pads
    C: touchscreen HID (works like a mouse device)
    D: ? (TBD)
    E: HDMI out to monitor
    F: Audio Out (3.5mm)

    # Here is a rough wiring diagram for my Qotom device:

    --------
    |      |
    |      |
    |      |
    |      |
    |      |
    --------
     W X Y

     W: USB port
     X: HDMI port
     Y: Audio Out (3.5mm)
     (power, wifi, audio in, network, etc not shown)


To work with an SM5 machine, we will want to unplug B, C, and E and plug them in to our rig instead.  Additionally, we will want a keyboard which we can plug into either device.  Therefore, I recommend buying a very small 3-port unpowered USB hub, plugging B and C and a keyboard into it.  Then, you can use a set of extension cables to make a single "move everything at once bundle".  I have a small kiddo so I don't want any wires in reach, so I will keep the SM5 box on a shelf mounted to the wall behind the AIO, so I am assuming 6ft will be sufficient.  Here is my parts list:

* 3x USB extension cable [I got 2 of these](https://www.amazon.com/AmazonBasics-Extension-Cable-Male-Female/dp/B014RWATK2) (~$20)
* 1x 3-port (or more) mini unpowered [usb hub](https://www.amazon.com/Sabrent-4-Port-Individual-Switches-HB-UM43/dp/B00JX1ZS5O) (~$10)
* 2x HDMI cable [here's a super cheap 2-pack](https://www.amazon.com/Rankie-High-Speed-Supports-Ethernet-Return/dp/B00Z07XQ4A) (~$12)
* 1x HDMI [extension cable](https://www.amazon.com/Aurum-Ultra-Extension-Ethernet-Supports/dp/B00Y1F3PRA) (~$9)
* 3x 3.5mm audio cable [I got one of these](https://www.amazon.com/AmazonBasics-3-5mm-Stereo-Audio-Cable/dp/B079L8FTZN) (~$14)

Alternate stuff you might be interested to learn exists:
* male/female audio [extension cable](https://www.amazon.com/UGREEN-Extension-Adapter-Compatible-Smartphones/dp/B00LM4ON2E) (~$7 for 2)
* female/female audio [adapter](https://www.amazon.com/gp/product/B01C9U3XI4) (~$5 for 3)
* female/female hdmi [adapter](https://www.amazon.com/RELPER-LiNESO-HCH009-Extension-Connector-Converter/dp/B01MXOND77) (~$8 for 4)
* Cable Management velcro [cable ties](https://www.amazon.com/Monoprice-106457-Hook-Fastening-Cable/dp/B004AFUJZC) (~$8 for 50)

Optionally, you can get two female/female adapters and 3x of the male/male cables for HDMI/audio.  They work the same way, comes down to preference.

So here is our completed wiring diagram using my suggestions above:

    # (X_Port means the given port, X_Cable means the cable that started out plugged into that port):

    B_Cable, C_Cable, Keyboard -> USB Hub
    USB Hub (USB-A Male) -> usb extension cable -> DEV bundle out (USB-A Male)
    E_Cable (HDMI Male) -> HDMI extension cable -> DEV bundle out (HDMI Male)
    F_Cable (3.5mm Male) -> audio extension cable -> DEV bundle out (3.5mm Audio Male)

    B_Port (USB-A Female) -> usb extension cable -> SMX bundle out (USB-A Female)
    E_Port (HDMI Female) -> HDMI extension cable -> SMX bundle out (HDMI Female)
    F_Port (3.5mm Female) -> audio extension cable -> SMX bundle out (3.5mm Female)

    W_Port (USB-A Female) -> usb extension cable -> SM5 bundle out (USB-A Female)
    X_Port (HDMI Female) -> HDMI Extension Cable -> SM5 bundle out (HDMI Female
    Y_Port (3.5mm Female) -> audio cable (male male) -> SM5 bundle out (3.5mm Audio Female

Using the velcro cable ties, you end up with 3 bundles of 3 wires each (DEV, SMX, and SM5) and you can plug either SMX or SM5 into DEV.  Switching these three cables is how you swap between SMX and SM5.  Doing so probably doesn't even require a reboot (TBD: confirm this).

## Initial Setup of the Qotom Device

Even though they kindly put Debian on it as per my request, the drive was imaged in a way that left most of its space unused.  You should probably plan to reinstall your OS of choice.  These instructions will use Debian because it is the light and the truth.

Additionally, it was Debian i686, but lscpu showed that the Intel Celeron J1900 was capable of 32-bit and 64-bit op modes, which I believe means you should install the amd64 platform.
As i686 ages, more and more packages may have trouble on 32-bit.

Produce a USB install media.  Fetch a netinst CD image from here:
https://www.debian.org/CD/netinst/

using a thumb drive on which you WANT TO DESTROY ALL DATA, use `dd` to copy the iso to the thumb drive.  This makes it bootable.

Determine which device the thumbdrive is, using 'lsblk'.  Whichever device is the proper size to be your thumb drive (i.e. 16GB or 32GB or whatever), may have more than one device listed.  If you see "/dev/sda" and "/dev/sda1", you should use "/dev/sda".  If you get it wrong, however, you will overwrite your hard drive.  Try not to do that.  You can also see where various devices are mounted using `cat /etc/mtab` to make sure the device that is your hard drive is not the one you are about to overwrite.

    sudo umount /dev/sda1
    sudo dd if=/home/cmyers/Downloads/debian-9.2.1-amd64-netinst.iso of=/dev/sda

Plug this thumb drive into your device, along with a keyboard and hdmi monitor, and power it up.  You should see a Debian install menu.  Selet a normal install (not graphical).

Select English for language

Select United States for Country

Select American English for keymap

If you have a network adapter that requires non-free firmware, like me, you will be prompted here to load it from removable media.  The easiest thing is to have a second thumb drive you can put it on.  An explanation for how to prepare that is here: https://wiki.debian.org/Firmware#Firmware_during_the_installation

If you have multiple network adapters, you'll be asked to choose one.

If it is a wifi adapter, you will be asked to choose a network and if necessary, to enter auth creds.

My Qotom device was unable to connect to my wifi network until I installed the antennae that came with it, so be sure to do that.

Enter a hostname.  I used "sm.cmyers.org"

Enter a root password.  Create a user account and a password for that.  I created my account to be "cmyers".

Select your timezone (mine is "Pacific")

Select a Partitioning method.  I recommend "Guided - use entire disk and set up LVM".  I normally use encryption but we will not use it here because we don't want to have to type in a password every time the host restarts.

Select the hard disk (the large one, not the thumb drive).

Since this is one big disk, and we are unlikely to reinstall the base system without backing up the home dir, I elected to go with all files in one big partition.  This ensures that we don't make the root partition too big or too small and waste space.

Select "Yes" to write the changes to disk - it asks you like 3 times, just keep saying yes.

It will now install the base system.

Select an appropriate country to configure a nearby mirror

No proxy configuration should be needed, leave that blank.

I selected "no" for participating in the anonymous package survey, because this is a weird machine that shouldn't inform any decisions.

Next comes the important part.  It asks you what additional packages should be installed.  I uncheck everything except for SSH server and standard system utilities.  This is because I will request the packages I want myself, let's not install any irrelevant cruft.

Finally, it will install the boot loader (and ask you if you want to install it to the master boot record - yes, you do - pick the same device you installed on before probably).

You can now remove the install and/or firmware thumb drives and reboot into your new system.

After the reboot, you should see a text console asking you to log in.  Do so using the user account you created.

# Initial OS System Configuration

Now that you have a squeaky clean debian linux system, we will install and/or compile the various bits we need for it to become a stepmania 5 box (and/or openitg box).

Debian does not install sudo by default.  You are going to want that and a few other things, so temporarily become root.

Before we can install anything, we need to set up wireless.  You would normally use network-manager to do this, or nmcli, but we are going to use a simpler approach so it requires less fiddling and "just works".

run "ip a" to figure out your wifi interface.  Mine was "wlp2s0".

you can scan for available networks to ensure yours shows up:

    ip link set wlp2s0 up
    iwlist scan

Since my network (called "cmyers") was in this list, I can proceed.  I need to generate the wpa passphrase for my network.

    wpa_passphrase cmyers <your password>
    # this generates output like this:
    network={
            ssid="cmyers"
            #psk="your password"
            psk=<some long terrible thing>
    }

    copy the "psk" for use later.


    vi /etc/network/interfaces
    # (add to the bottom of the file, leave the loopback stuff alone)

    auto wlp2s0
    iface wlp2s0 inet dhcp
        wpa-ssid cmyers
        wpa-psk <that long terrible thing from before, no quotes>

if you instead have an open network, you could do this:

    auto wlp2s0
    iface wlp2s0 inet dhcp
        wireless-essid some-open-network

More details for various network types here: https://wiki.debian.org/WiFi/HowToUse

Now save that file and run:

    ifup wlp2s0

You should see dhcp happening as well as wpa-supplicant running.  Then your network should work.  yay!

with wifi working, you can now install your creature comforts.  Mine are:

    apt-get update
    apt-get install vim htop less zsh tmux sudo git

then, add your user to the sudo group to give your user sudo access, and you can be done logging in as root

    adduser cmyers sudo

You will have to log out all the way, then back in as your user, for that to take effect.

Now is an excellent time to turn capslock into ctrl always and forever, if you aren't a cretin.

Add the line `XKBOPTIONS="ctrl:nocaps"` to /etc/default/keyboard, then run `sudo dpkg-reconfigure -phigh console-setup`.

Finally, you can install xorg so you can run gui apps like stepmania.  We won't need a window manager just yet.

    sudo apt-get install xorg

Lets get some sound happening!

    sudo apt-get install alsa-utils

Now run alsamixer and set the sound properly:

    alsamixer

My machine had the sound muted by default, so sound might be working but appear broken.  Fiddle with the mixer until you can hear sound.  You can run `speaker-test` to generate sound if you have no better way (I personally ran mpg123 playing some streaming music service I subscribe to).

The SMX machine uses the line out, so we will do that also, but just FYI if you want sound to go through HDMI for some reason (say, you are reading this guide but actually setting up independent pads with a TV), then you will want to do the following:

    $ aplay -l
    **** List of PLAYBACK Hardware Devices ****
    card 0: PCH [HDA Intel PCH], device 0: ALC662 rev3 Analog [ALC662 rev3 Analog]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 0: PCH [HDA Intel PCH], device 1: ALC662 rev3 Digital [ALC662 rev3 Digital]
      Subdevices: 1/1
      Subdevice #0: subdevice #0
    card 0: PCH [HDA Intel PCH], device 3: HDMI 0 [HDMI 0]
      Subdevices: 1/1
      Subdevice #0: subdevice #0

    # you can see from here, the HDMI card is card 0 device 3
    # write the following to ~/.asoundrc
    defaults.pcm.card 0
    defaults.pcm.device 3
    defaults.ctl.card 0

	# then you can test again and it should come out over HDMI:
	$ speaker-test

Fix screen blanking - ensure the screen doesn't blank anymore.  See: https://unix.stackexchange.com/questions/8056/disable-screen-blanking-on-text-console

    $ sudo /bin/bash -c 'echo -ne "\033[9;0]" >> /etc/issue'

## Building Stepmania 5

    mkdir projects
    cd projects
    git clone https://github.com/stepmania/stepmania.git
    cd stepmania

    # check out a version that Simply Love supports - as of this writing, 5.0.12 or 5.1 alpha (but current tip of master did not work for me)
    git checkout v5.0.12

    # initialize the submodules
    git submodule init
    git submodule update

    # the provided list of SM5 dependencies:
    sudo apt-get install mesa-common-dev libglu1-mesa-dev libglew1.5-dev libxtst-dev libxrandr-dev libpng12-dev libjpeg-dev zlib1g-dev libbz2-dev libogg-dev libvorbis-dev libc6-dev yasm libasound-dev libpulse-dev libjack-dev binutils-dev libgtk2.0-dev libmad0-dev libjack0 libudev-dev libva-dev
    # my self-discovered sm5 dependencies - mostly the same, some differences, safe to do both
    sudo apt-get install autoconf cmake gcc g++ libmad0-dev libvorbis-dev libbz2-dev libpcre3-dev zlib1g-dev libjpeg-dev libgl1-mesa-glx libgl1-mesa-dev libglew-dev nasm libva-dev libxrandr-dev libxinerama-dev libasound2-dev libudev-dev
    # turn off minimaid for now, see: https://github.com/stepmania/stepmania/issues/1352#issuecomment-269593979
    cmake -G 'Unix Makefiles' -DWITH_MINIMAID=OFF
    make -j8
    # this takes about 15 minutes on my box


    # get simply love theme
    cd ~/projects
    git clone https://github.com/dguzek/Simply-Love-SM5.git
    mkdir -p "$HOME/.stepmania-5.0/Simply Love"
    cp -r Simply-Love-SM5/* "$HOME/.stepmania-5.0/Simply Love"

    # you can activate the theme in the menus, or by doing this:
    # To find in the menu, do Options -> Display Options -> Appearance Options -> Theme
    sed -i 's/Theme=default/Theme=Simply Love/g' "$HOME"/.stepmania-5.0/Save/Preferences.ini


## Insert Songs

Copy all your songs into `$HOME/.stepmania-5.0/Songs`.

## First time running

You will want to go into options -> graphics options and set the resolution and other settings




====
My hardware uses a realtek RTL8192CE wifi device which sadly requires a "nonfree" driver.
