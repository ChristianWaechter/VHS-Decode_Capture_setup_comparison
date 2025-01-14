# Comparison of different capture setups used with vhs-decode

[vhs-decode](https://github.com/oyvindln/vhs-decode) is a way to decode RF captures of old video tapes. To do such captures, one needs a PC/Laptop with some kind of fast ADC, some kind of amplifier for the RF signal and a VCR that plays the actual tape and provides this raw RF signal. There are several possible hardware configurations for such a capture system. Some are already obsolete and have been replaced by better solutions. Some are alternatives without actual replacing other options. The most common capture setups shall be compared here.

***Currently work in progress and more of a brainstorming platform***

## Overview of equipment

### VCRs
Two different Video8 VCRs are available for testing, one Sony EV-S1000E and one Sony SLV-T2000. Tapes will be Video8 tapes from the year 1990 - 2000.

The Sony SLV-T2000 has a measured output impedance of 200 Ohm on its Video8 part. The Sony EV-S1000E has a hight output impedance of 1 kOhm. Therefore, more tests will be done on the EV-S1000E as the higher output impedance is more prone to disturbances caused by the low input impedance of a connected amplifier.

### old AD8367 RMS amplifier
The old [AD8367 RMS amplifier](https://github.com/tandersn/cxadc-hw-mod/wiki/AD8367-RMS-Setup) is not perfectly suited for the amplification of the RF signal of a VCR. It has a relatively low input impedance which can not matched to the output impedance of the VCR and might overload its RF signal. This will result in poor signal quality and can be seen in the image which will look worse as for example a crosshatch pattern can get visible.

[Mounted AD8367 amplifier in its case](AD8367/AD8367_board.jpg)

https://discord.com/channels/665557267189334046/782578245408653313/1169038092661964871

Configuration of AD8367 amplifier is as following:
- R_out,load (R5): 150 Ohm (2 x 330 Ohm parallel)
- Cin (C7) + Cout (C6 or C8): 1 uF


### new ADA4857 RMS amplifier
This variant is using this amplifier https://gitlab.com/wolfre/vhs-rf-amp-ada4857

It is configured to match the VCRs output impedance, which may differ from VCR to VCR. It is also set to a gain so that a possibly weak RF signal is amplified to a proper amplitude for the ADC.

### Domesday Duplicator
One [Domesday Duplicator](https://github.com/simoninns/DomesdayDuplicator) is available for testing.

On the unmodified version, the Domesday Duplicator is identical to the hardware assembly as provided in the git. The input impedance is 50 Ohm which will cause a high load on the RF output of the VCR when not using a additional impedance matching circuit (like the ADA4857 amplifier).

On the modified version, the input filter/impedance and gain setting will be changed to match the VCRs output impedance and signal strength. This is usually done on the [ADA4857 amplifier](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#external-amplification). But as the Domesday Duplicator already has an amplifier on board, removing that additional ADA4857 amplifier might increase the signal quality.

### CX Card
Two [CX Cards](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards) are available for testing, one with the CXxxxxxxxxxx

See https://gitlab.com/wolfre/cx25800-11z-cxadc-rework-measurements for further measurements on the CX cards.

The following standard modifications have been done on both cards:
- [Cooling Mod](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#cooling-mod): Added a heatsink to the CX-chip
- [C31 Removal](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#c31-removal): Remove C31 to increase SNR
- [RCA to BNC Replacement](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#rca-to-bnc-replacement): More convenient connection
- [Crystal Mod 40 MHz](https://github.com/oyvindln/vhs-decode/wiki/CX-Cards#crystal-mod---5-25usd): Change the crystal so that sampling with 40 MSPS is possible


## Setups and measurements

### VCR EV-S1000E > AD8367 RMS amplifier (old) > Domesday Duplicator (unmodified)



### VCR EV-S1000E > ADA4857 amplifier (new) > Domesday Duplicator (unmodified)



### VCR EV-S1000E > ADA4857 amplifier (new) > CX card (modified)



### VCR EV-S1000E > Domesday Duplicator (unmodified)


### VCR EV-S1000E > Domesday Duplicator (modified)


### other VCR SLV-T2000 > Domesday Duplicator (modified)






## Comparison and Conclusion

https://imgsli.com/

### AD8367 (old) vs ADA4857 (new) amplifier

### Domesday Duplicator vs CX card

### Domesday Duplicator unmodified vs modified

### Comparison of two VCRs
Compare Image quality of S1000 with T2000 with perfect matched impedance setup???
Or compare effects of wrong impedance? e.g. AD8367 with 200 Ohm input impedance might be still fine for T2000 with output impedance of 200 Ohm. But the 200 Ohm input impedance of the AD8367 will be too low for the S1000 which has a output impedance of 1000 Ohm???

### Comparison of better VCR with camcorder (worse image quality?)
