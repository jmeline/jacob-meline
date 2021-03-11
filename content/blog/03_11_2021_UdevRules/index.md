---
title: Udev rules in Archlinux
date: "2021-03-11"
description: "How to write udev rules to disable an input device?"
---

## Problem

I have an ASRock B550 motherboard and for some reason the LED Controller gets registered as a joystick which causes games like Hollow Knight to immediately crash.

## Udev to the rescue!

1) take a look at which device is causing you problems

Using udevadm info with attribute walk, I can look through children components until I find the name I am looking for. I take note of what attributes I can refer to later in my script to ignore them.

```
# udevadm info --attribute-walk /dev/input/js0

  looking at parent device '/devices/pci0000:00/0000:00:01.2/0000:02:00.0/usb1/1-10/1-10:1.0/0003:26CE:01A2.0007/input/input11':
    KERNELS=="input11"
    SUBSYSTEMS=="input"
    DRIVERS==""
    ATTRS{capabilities/abs}=="100000101ff"
    ATTRS{capabilities/ev}=="1b"
    ATTRS{capabilities/ff}=="0"
    ATTRS{capabilities/key}=="40000001000000 1200000000 c00000000000000 800000000 40000010cc00 10168000000000 0"
    ATTRS{capabilities/led}=="0"
    ATTRS{capabilities/msc}=="10"
    ATTRS{capabilities/rel}=="0"
    ATTRS{capabilities/snd}=="0"
    ATTRS{capabilities/sw}=="0"
    ATTRS{id/bustype}=="0003"
    ATTRS{id/product}=="01a2"
    ATTRS{id/vendor}=="26ce"
    ATTRS{id/version}=="0110"
    ATTRS{inhibited}=="0"
    ATTRS{name}=="ASRock LED Controller"
    ATTRS{phys}=="usb-0000:02:00.0-10/input0"
    ATTRS{power/async}=="disabled"
    ATTRS{power/control}=="auto"
    ATTRS{power/runtime_active_kids}=="0"
    ATTRS{power/runtime_active_time}=="0"
    ATTRS{power/runtime_enabled}=="disabled"
    ATTRS{power/runtime_status}=="unsupported"
    ATTRS{power/runtime_suspended_time}=="0"
    ATTRS{power/runtime_usage}=="0"
    ATTRS{properties}=="0"
    ATTRS{uniq}=="A02019100900"
```

Here we can see that udev has registered the AsRock LED Controller as an input. We can tell by looking at what subsystem it is using.

2) write a udev rule to disable it or in other words, remove all permissions to run it.


```
   $ touch /etc/udev/rules.d/98-bye-bye-ASRock-LED-Controller.rules
```

Open it up in your favorite editor. Note that even though the attributes say ```ATTRS{id/vendor}``` it is really ```ATTRS{idVendor}```

```
SUBSYSTEM=="input", ATTRS{idVendor}=="26ce", ATTRS{idProduct}=="01a2", KERNEL=="js[0-9]*", MODE="0000", ENV{ID_INPUT_JOYSTICK}=""

SUBSYSTEM=="input", ATTRS{idVendor}=="26ce", ATTRS{idProduct}=="01a2", KERNEL=="event*", MODE="0000", ENV{ID_INPUT_JOYSTICK}=""
```

I could have used any of the attributes listed above. I found that the vendor + product id combination is pretty unique and is a good reliable way of finding the right device.

Once you have created a rule. Be sure to to reload the rules files by running:

```
   $ sudo udevadm control --reload
```
Since this isn't something I can easily pull out because it is on the motherboard, I could restart. Or I could trigger udevadm again using the following command:

```
   $ sudo udevadm trigger
```

This is simply how I handled the wrong device.
