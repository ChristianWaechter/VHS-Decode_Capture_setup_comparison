# Comparison of different capture setups used with vhs-decode

[vhs-decode](https://github.com/oyvindln/vhs-decode) is a way to decode RF captures of old video tapes. To do such captures, one needs a PC/Laptop with some kind of fast ADC, some kind of amplifier for the RF signal and a VCR that plays the actual tape and provides this raw RF signal. There are several possible hardware configurations for such a capture system. Some are already obsolete and have been replaced by better solutions. Some are alternatives without actual replacing other options. The most common capture setups shall be compared here.

***Currently work in progress and more of a brainstorming platform***

Youtube Codec requirements
https://support.google.com/youtube/answer/1722171#zippy=%2Cframe-rate%2Cvideo-codec-h%2Cbitrate%2Caudio-codec-aac-lc-or-opus-or-eclipsa-audio%2Ccontainer-mp%2Cvideo-resolution-and-aspect-ratio%2Ccolor-space

Hanover bars in test card: https://en.wikipedia.org/wiki/Hanover_bars

tl;dr, CONCLUSION
A good capture device is noticeable better than a bad one. Don't buy a bad one.

## Overview of equipment

### VCRs
Two different Video8 VCRs are available for testing, one Sony EV-S1000E and one Sony SLV-T2000. Tapes will be Video8 tapes from the year 1990 - 2000.

The Sony SLV-T2000 has a measured output impedance of 200 Ohm on its Video8 part. The Sony EV-S1000E has a hight output impedance of 1 kOhm. Therefore, more tests will be done on the EV-S1000E as the higher output impedance is more prone to disturbances caused by the low input impedance of a connected amplifier.

![Image showing the Sony EV-S1000E with the new ADA4857 amplifier connected directly to the test pin header of the head amp](setups/VCR_S1000_tap_point.jpg)
Image showing the Sony EV-S1000E with the new ADA4857 amplifier connected directly to the test pin header of the head amp (bottom left). Red and black wires connect to the +12 VDC power of the VCR (bottom right). The coax cable connects either to the Domesday Duplicator mounted in a aluminum case (top middle) or to the CX card mounted in the PC (top left).

### old AD8367 amplifier
The old [AD8367 amplifier](https://github.com/tandersn/cxadc-hw-mod/wiki/AD8367-RMS-Setup) is not perfectly suited for the amplification of the RF signal of a VCR. It has a relatively low input impedance which can not matched to the output impedance of the VCR and might overload its RF signal. This will result in poor signal quality and can be seen in the image which will look worse as for example a crosshatch pattern can get visible.


![Mounted AD8367 amplifier seen under microscope](AD8367/AD8367_board.jpg)


![Mounted AD8367 amplifier in its case](AD8367/AD8367_board_case.jpg?)

Assembly of AD8367 amplifier is as following (assembly might differ in images):
- R<sub>out,load</sub> (R5): 150 Ohm
- C<sub>in</sub> (C7) + C<sub>out</sub> (C8): 47 uF

For Amp Board  mod see: [https://github.com/tandersn/cxadc-hw-mod/wiki/AD8367:-RMS-amp-board-modification](https://github.com/tandersn/cxadc-hw-mod/wiki/AD8367bigpot-amp-board-modification)

#### Configuration for Sony EV-S1000E:
Domesday Duplicator gain of 8.5 (DIP switch set to 1000)
AD8367 gain cannot be determined due to use of potentiometer

https://discord.com/channels/665557267189334046/782578245408653313/1169038092661964871


### new ADA4857 amplifier
This variant is using this amplifier https://gitlab.com/wolfre/vhs-rf-amp-ada4857

It is configured to match the VCRs output impedance, which may differ from VCR to VCR. It is also set to a gain so that a possibly weak RF signal is amplified to a proper amplitude for the ADC.

#### Configuration for Sony EV-S1000E (1000 Ohm output impedance, V<sub>pp</sub> = ***???*** mV):
Amplifier input impedance R11 & R12 (or R21 & R22): 15 kOhm

##### Domesday Duplicator:
Amplifier gain of 6.6, R<sub>in</sub> = R13 & R<sub>f</sub> = R14 (or R<sub>in</sub> = R23 & R<sub>f</sub> = R24): R<sub>in</sub> = 100 Ohm & R<sub>f</sub> = 560 Ohm
Domesday Duplicator gain of 2.02 (DIP switch set to 1111)

##### CX Card:
Amplifier gain of 11, R<sub>in</sub> = R13 & R<sub>f</sub> = R14 (or R<sub>in</sub> = R23 & R<sub>f</sub> = R24): R<sub>in</sub> = 56 Ohm & R<sub>f</sub> = 560 Ohm


### Domesday Duplicator
One [Domesday Duplicator](https://github.com/simoninns/DomesdayDuplicator) is available for testing.

![unmodified DdD input stage](DdD/DdD_input_stage.png)

| Configuration | Switches | Gain |
| --- | --- | --- |
| 15 | 1111 | 2.02 |
| 7 | 0111 | 2.17 |
| 11 | 1011 | 2.27 |
| 13 | 1101 | 2.45 |
| 3 | 0011 | 2.54 |
| 14 | 1110 | 2.59 |
| 5 | 0101 | 2.79 |
| 6 | 0110 | 3.02 |
| 9 | 1001 | 3.04 |
| 10 | 1010 | 3.34 |
| 1 | 0001 | 3.8 |
| 12 | 1100 | 4 |
| 2 | 0010 | 4.4 |
| 4 | 0100 | 6 |
| 8 | 1000 | 8.5 |	

#### Domesday Duplicator not modified

On the unmodified version, the Domesday Duplicator is identical to the hardware assembly as provided in the git. The input impedance is 50 Ohm which will cause a high load on the RF output of the VCR when not using a additional impedance matching circuit (like the ADA4857 amplifier). Even with the maximuim gain of 8.5 of the Domesday Duplicator, the signal level was pretty low. In addition, the cable from the RF tap point to the Domesday Duplicator might be longer than a cable to an ADA4857 amplifier which can have a negative impact on the signal.
 
#### Domesday Duplicator modification

On the modified version, the input filter/impedance and gain setting will be changed to match the VCRs output impedance and signal strength. This is usually done on the [ADA4857 amplifier](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#external-amplification). But as the Domesday Duplicator already has an amplifier on board, removing that additional ADA4857 amplifier might increase the signal quality or at least remove unnecessary circuitry. Again, with this setup, the cable from the RF tap point to the Domesday Duplicator might be longer than a cable to an ADA4857 amplifier which can have a negative impact on the signal.


#### Configuration for Sony EV-S1000E (1000 Ohm output impedance, V<sub>pp</sub> = ***???*** mV):
- C<sub>in</sub> (C401): 47 uF
- R<sub>bias</sub> (R402 & R403): 33 kOhm each
- R<sub>in,load</sub> (R401): removed, not assambled
- R<sub>ADC,bias</sub> (R302): 1.5 kOhm

https://discord.com/channels/665557267189334046/782578245408653313/1345508640081313802

With a gain of 4.4 the signal showes a good signal amplitude with ~30% headroom. However, a DC offset was now visible in the captured signal. That did not allow the use of a higher gain setting, as the positive signal amplitude might have clipped otherwise. Changing R302 from 1k2 to 1k5 added more offset to the signal which was now centered again. This allowed the use of a higher gain of 6 resulting in ~20% headroom on both the positive and negative amplitudes.


#### Configuration for Sony SLV-T2000 (200 Ohm output impedance, V<sub>pp</sub> = 260 mV): TODO TODO TODO!!!

- C<sub>in</sub> (C401): 47 uF
- R<sub>bias</sub> (R402 & R403): 33 kOhm each
- R<sub>in,load</sub> (R401): removed, not assambled
- R<sub>ADC,bias</sub> (R302): 1.5 kOhm || 22 kOhm = 1.4 kOhm

Domesday Duplicator gain set to 3.02



### CX Card
Two [CX Cards](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards) are available for testing, one with the CX23883-39, one with the 25800-112. The first on is [said to be a bit better in terms of capture quality, as it causes less electrical self noise](https://github.com/happycube/cxadc-linux3/wiki/Types-Of-CX2388x-Cards#cx-white-card-old-rca-s-video), therefore this one card will be used.

See https://gitlab.com/wolfre/cx25800-11z-cxadc-rework-measurements for further measurements on the CX cards.

The following standard modifications have been done on both cards:
- [Cooling Mod](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#cooling-mod): Added a heatsink to the CX-chip
- [C31 Removal](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#c31-removal): Remove C31 to increase SNR
- [RCA to BNC Replacement](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#rca-to-bnc-replacement): More convenient connection
- [Crystal Mod 40 MHz](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#crystal-mod---5-25usd): Change the crystal so that sampling with 40 MSPS is possible

The following parameters and commands have been used for capturing with the [cxadc driver](https://github.com/happycube/cxadc-linux3)

Result of command `cxvalues`:
```
/sys/class/cxadc/cxadc0/device/parameters/sixdb 0
/sys/class/cxadc/cxadc0/device/parameters/tenbit 0
/sys/class/cxadc/cxadc0/device/parameters/audsel -1
/sys/class/cxadc/cxadc0/device/parameters/center_offset 7
/sys/class/cxadc/cxadc0/device/parameters/latency -1
/sys/class/cxadc/cxadc0/device/parameters/crystal 40000000
/sys/class/cxadc/cxadc0/device/parameters/vmux 1
/sys/class/cxadc/cxadc0/device/parameters/tenxfsc 0
/sys/class/cxadc/cxadc0/device/parameters/level 0
```

## Setups and measurements

### VCR EV-S1000E > AD8367 amplifier (old) > Domesday Duplicator (unmodified)

![Connection and configuration diagram of setup with Domesday Duplicator and AD8367 amplifier (old)](setups/DdD+AD8367_non_RMS_setup.svg)


### VCR EV-S1000E > ADA4857 amplifier (new) > Domesday Duplicator (unmodified)

![Connection and configuration diagram of setup with Domesday Duplicator and ADA4857 amplifier (new)](setups/DdD+ADA4857_setup.svg)


### VCR EV-S1000E > ADA4857 amplifier (new) > CX card (modified)

![Connection and configuration diagram of setup with CX card and ADA4857 amplifier (new)](setups/CX_card+ADA4857_setup.svg)

### VCR EV-S1000E > Domesday Duplicator (unmodified)

![Connection and configuration diagram of setup with unmodified Domesday Duplicator](setups/DdD_setup.svg)

### VCR EV-S1000E > Domesday Duplicator (modified)

![Connection and configuration diagram of setup with modified Domesday Duplicator and first VCR](setups/DdD_modified_setup.svg)

### other VCR SLV-T2000 > Domesday Duplicator (modified)

![Connection and configuration diagram of setup with modified Domesday Duplicator and second VCR](setups/DdD_modified_setup.svg)





## Comparison and Conclusion

https://discord.com/channels/665557267189334046/782578245408653313/1356418834851954738

https://support.google.com/youtube/answer/1722171

https://imgsli.com/

### AD8367 (old) vs ADA4857 (new) amplifier

### Domesday Duplicator vs CX card

### Domesday Duplicator unmodified vs modified

### Comparison of two VCRs
Compare Image quality of S1000 with T2000 with perfect matched impedance setup???
Or compare effects of wrong impedance? e.g. AD8367 with 200 Ohm input impedance might be still fine for T2000 with output impedance of 200 Ohm. But the 200 Ohm input impedance of the AD8367 will be too low for the S1000 which has a output impedance of 1000 Ohm???

### Comparison of better VCR with camcorder (worse image quality?)
