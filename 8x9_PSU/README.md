#How to Provide Power to Your IR829 and IR809

When I received my [IR829](http://www.cisco.com/c/en/us/td/docs/routers/access/800/829/hardware/install/guide/829hwinst/pview.html) I opened the box with the expectation of
taking it out, plugging it and starting to work with it. BUT, it was
not shipped with a Power Supply Unit (PSU) or adaptor, so I was somewhat
disappointed instead.

If that happens to you, then what I have written here may help you.

By the time my [IR809](http://www.cisco.com/c/en/us/products/collateral/routers/809-industrial-router/datasheet-c78-734980.html) arrived, I was mentally prepared.

##What the IR829 is Supposed to do for Power

The IR829, as originally conceived, was designed to go into
vehicles. To make that simple, it comes with an on-board power adapter
suitable for the 12v-24v DC power supply that is standard in
vehicles. This makes perfect sense for the original intended use.

![Molex DC power port - Front Panel IR829](images/molex_dc_in_front_panel.png)

Note that "GND" is ground, i.e. power out -ve, and that "BAT" is battery, which
is the power in +ve. IGN is the ignition connector. This is explained,
sort of,
[here](http://www.cisco.com/c/en/us/td/docs/routers/access/800/829/hardware/install/guide/829hwinst/pview.html#pgfId-1077228),
but with reference to a non-existent "DC Power section".

Outside of the vehicle environment, though, 12v-24v, 5A, 60-120W power
sources are not that common.

##The Alternatives

Cisco has twodent part numbers for suitable PSUs: PWR-60W-AC, PWR-60W-AC-V2, and
PWR-125W-AC=. The former supplies 60W, which is sufficient for the
IR829, and the latter supplies 125W, which is sufficient for Power
over Ethernet (PoE) also.

This is what the PWR-60W-AC= looks like:

![PWR-60W-AC=](images/PWR-60W-AC=.png)

Note that the plug at the end is *not* a Molex. We will get to that momentarily.

The PWR-60W-AC-V2 looks much the same as the  PWR-60W-AC, but it comes
with a Molex plug fitted already. That plu, though, does not click
into the power port on the IR829, so some plastic trimming is
required, which is illustrated below.

They both connect to 100-240V outlets common in North
America or EMEA. Note that the PSU ships *without* a cable to connect
to a wall socket. This is what you will need:

![Wall Power Cable](images/wall_power_cable.png)

##Some Hacking Required to Use the Pre-Fitted Molex on the PWR-60W-AC-V2

Sometimes hacking really is, in the physical sense, hacking. The
picture below illustrates what I did to get the pre-fitted Molex plug
on the PWR-60W-AC-V2 to click into the IR829. Shown is the tool, a plug
with some plastic cut off, and a plug without the plastic cut off.

![Adjusted Molex Plug](images/adjusted_molex_plug.jpeg)

#DIY Required to Fit the Provided Molex to the PWR-60W-AC for the IR829

In the box with the IR829 I did find a four hole Molex plug and a strip of four
metal pieces designed to clamp around cables and insert into the Molex
plug. These are intended to be used to connect power cables in a
vehicle environment to plug into the IR829. With a little work, they
can be adapted for use with a AC/DC PSU also.

What I did
was cut off the plug that the PSU came with, and used the metal pieces
in the IR829 box to connect the inner and outer cables into the Molex
plug, which also came in the IR829 box.

That looks like this (note
that the white cable that looks like it is going into the top of the Molex
slipped when I took the photo. It actually goes into the hole on the
right of the bare metal ground cable):

![The Hack](images/the_hack.png)

That, when fully pushed together, can be plugged in like this:

![Plugged](images/plugged.png)

And the lights come on like this:

![Lights](images/lights.png)

Once you are happy that it is all connected properly, tape it up like
this (or perhaps even tidier):

![Taped](images/taped.png)

#DIY Required to Fit the Provided Molex to the PWR-60W-AC for the IR809

The details for the IR809 are similar, in the sense that one has to
strip off the plug and use the supplied, black four hole, plug to
connect the PSU to the IR829. Note that the screws to hold the wires
in place are very small, I needed to use my specialist electronics
screwdriver set for those screws. Note, also, that the plug itself is
screwed into the body of the IR829 to hold it in place.

The wiring directions are
[here](http://www.cisco.com/c/en/us/td/docs/routers/access/800/809/hardware/install/guide/809hwinst/pview.html#pgfId-1154042).

This is what it looks like:

![IR809 Power](images/IR809_power.jpeg)
