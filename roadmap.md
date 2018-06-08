# Tweety Roadmap

An overview of two versions of the "tweety" audio player.

Two devices are proposed, a "hacker edition" and an "artisinal production edition".  The "hacker" version is designed to be built from off-the-shelf parts with minimal fabrication and looser requirements for cost, size and energy efficiency.

The "artisinal" version is designed to meet stricter requirements as well as exhibit higher reproducibility.  As such this version requires more specialized tools, skills, fabrication and production techniques.

Where possible, free and open-source software and hardware are leveraged to reduce development time and increase quality (in both hacker and artisinal versions).  As such it is assumed that both players will be open-source hardware (this may be required to be compatible with the licensing of the components used).


## Shared

Some components can be shared between the two versions:

* Speaker
* Headphone jack
* Capacitive input pads
* Status indication

The strictly-analog components of the system including the speaker and headphone output can be shared between the two designs.  In both the speaker is directly coupled to a Class D audio amplifier and the headphone jack is fed from the same amplifier (via resistor(s) to reduce power output to a safe level).  As volume control is managed by software, no additional analog components are necessary.

Given the desire to reduce cost and mechanical complexity, simple capacitive input controls are suggested.  These consist of nothing more than exposed GPIO pins pulled high using 1M resistors and then extended to conductive pads on the outside of the device.

Some means of indicating the operational status of the device are necessary.  This *could* be strictly via audio cues, but I'd recommend providing some other form of feedback.  An inexpensive way to do this would be LED(s) tied to GPIO that can be controlled by the firmware.


## Hacker Edition

### Hardware

* Raspberry Pi Zero W
* Integrated I2S + Class D audio amplifier
* 5v power supply

Most of the heavy-lifting in the Hacker Edition is performed by the RPi0W.  A combination 3-watt Class D audio amplifier and I2S decoder (MAX98357) is used to provide audio output and amplification to drive speaker and headphones.

The Raspberry Pi is a 5 volt device and the MAX98357 can be powered from 2.5-5.5v so a single 5v power supply is sufficient.  This can be provided via a typical USB mains supply (2 amp or higher) or could be made portable using a simple "lipstick"-style battery pack commonly used to charge smartphones, etc.


### Software

* https://github.com/schollz/raspberry-pi-turnkey

The recommended O/S for the Hacker Edition is raspberry-pi-turnkey as this provides a convenient workflow for configuring the device's network with no additional hardware.

Additional custom software is required to provide music playback functionality.


## Artisanal Production Edition

### Hardware

* ESP8266
* VS1053
* Class D audio amplifier
* 3.3v power supply

The artisinal version is based around the inexpensive ESP8266.  This device provides processing power to run the player's firmware as well as WiFi connectivity and GPIO for input devices.  The ESP8266 is not powerful enough to decode audio on its own so it is coupled with the VS1053 which is a versatile hardware audio CODEC.  Due to the ESP8266's low processing power and the hardware implementation of audio decoding the artisinal version requires significantly less power at runtime compared to the hacker edition.  Furthermore, the ESP8266 has no operating system overhead and is capable of being put into very low-power sleep modes further reducing power requirements.

Since the artisinal version uses the VS1053 for decoding there's no need for the combined I2S/amplifier used in the hacker edition, so a basic analog-input Class D amplifier is sufficient.

A 3.3v supply is required and could be provided via USB and voltage step-down converter.  Additionally, on-board battery management (including remote monitoring, etc.) could be incorporated as well.

These devices require a number of additional components and as such a custom PCB will be designed to integrate everything into a single board.


### Software

* https://github.com/karawin/Ka-Radio

The software recommended for the artisinal edition is an existing open-source "webradio" implementation called Ka-Radio.  The software is designed to work with the ESP8266/VS1053 device combination and takes care of things like network configuration, playback, etc.  To be used in tweety, the software will be customized to use MUSICat media sources and receive control input via the same capacitive input mechanism as the hacker edition.


# References

* https://itstillworks.com/wire-headphone-jack-older-home-radio-2659.html
* https://www.raspberrypi.org/forums/viewtopic.php?p=587826
* https://www.sparkfun.com/datasheets/Components/SMD/vs1053.pdf
* https://cdn-shop.adafruit.com/product-files/2471/0A-ESP8266__Datasheet__EN_v4.3.pdf
