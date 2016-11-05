---
layout: post
title: "CAEN DPP-PSD"
date: 2016-07-24 13:32:09 +0800
comments: true
tags: [Digitizer]
---
<!-- CAEN_DPPPSD.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 日 7月 24 13:32:09 2016 (+0800)
;; Last-Updated: 五 11月  4 20:50:02 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 25
;; URL: http://wuhongyi.cn -->


This user manual is intended to describe the Digital Pulse Processing for Pulse Shape Discrimination firmware (DPP-PSD) running on the 720 series digitizers with the EP1C20 FPGA and DT5790 (12 bit, 250MS/s), on the 725 series (14 bit,250 MS/s), the 730 series (14 bit, 500 MS/s), and on the 751 series digitizers (10 bit, 1 GS/s; DES mode not supported).

<!-- more -->

The main functionalities of a digitizer running DPP-PSD firmware are listed below, where any parameter can be programmed by the provided software:

- Auto selection of the events with a digital leading edge discrimination or a digital constant fraction discrimination (725 and 730 series only);
- Input signal baseline (pedestal) calculation and pedestal subtraction for energy calculation;
- Single gate integration for the energy spectra calculation;
- Double integration of the prompt and delayed charge for Pulse Shape Discrimination

The slow and fast components are used by the algorithm to compute the variable used for the PSD, using the following function:

$$PSD = \frac{Q_{Long} - Q_{Short}}{Q_{Long}}$$

![Plot of typical Gamma-Neutron waveforms](/img/Digitizer/CAENDPPPSD_GammaNeutron_11.png)

## Principle of Operation

The figure below shows the functional block diagram of the DPP-PSD firmware:

![Functional Block Diagram of the DPP-PSD](/img/Digitizer/CAENDPPPSD_FunctionalBlockDiagram_21.png)

The aim of the DPP-PSD firmware is to calculate the two charges Q short and Q long , performing a double gate integration of the input pulse. The ratio between the charge of the tail (slow component) and the total charge gives the PSD parameter used for the gamma-neutron discrimination:

$$PSD = \frac{Q_{LONG} - Q_{SHORT}}{Q_{LONG}}$$

![Long and short gate graphic position with respect to a couple of input pulses. The blue pulse has a longer tail than the red one](/img/Digitizer/CAENDPPPSD_LongAndShortGate_22.png)

The main operations of the DPP-PSD firmware can be summarized as follows:

- receive an input signal directly from the detector and digitize it continuously. It is possible to adjust the dynamic range with a programmable DC offset to exploit the full dynamics of the digitizer;
- the algorithm continuously calculates the baseline of the input signal by averaging the samples belonging to a moving window of programmable size. The baseline is subtracted from the input signal,giving input_sub= input – baseline (Digital Baseline Restorer);
- the input_sub value is compared with the value of the trigger threshold and the event is selected as soon as the input_sub signal crosses the threshold. In case of 725 and 730 series, the digital CFD(Constant Fraction Discriminator) zero crossing can be used for a better timing information. Once the event is selected a local trigger is generated (refer to Sect. DPP-PSD trigger management for further details);
- at the trigger fire, the signal is delayed by a programmable number of samples (corresponding to the “pre-trigger” value in ns).to be able to integrate the pulse before the trigger (“Gate Offset”). The gates for charge integration are then generated and are received by the charge accumulator before the signal. While the gates are active the baseline remains frozen to the last averaged value and its value is used as charge integration reference. Fig. 2.3 summarizes all the DPP-PSD parameters;
- for the whole duration of a programmable “trigger hold-off” value, other trigger signals are inhibited. It is recommended to set a trigger hold-off value compatible with the signal width. The baseline remains frozen for the whole trigger hold-off duration. For 751 series the baseline remains frozen for a longer time;
- the trigger enables the event building, that includes the waveforms (i.e. the raw samples) of the input, the trigger time stamp, the baseline, and the charge integrated within the gates. After that the system gets ready for a new event;
- the event data is saved into a memory buffer. The Control Software automatically optimize both the number of events inside the buffer, and the number of total buffers that the memory is divided in. The user can also choose to set these parameters by hand. If the buffer contains only one event, that buffer becomes immediately available for the readout and the acquisition continues into another buffer. If more events are written in one buffer, only when the buffer is complete those events become available for the readout;
- the software can then plot the signal waveforms for debugging and parameters adjustment, as well as plot energy spectrum of $Q_{long}$ and timing distribution, the 2-D scatter plot of the PSD parametervs $Q_{long}$ energy is also available;
- finally output files (list and waveform) can be generated in different formats suitable for external spectroscopy analysis software tools. Energy, time, and 2D spectra are not managed onboard but they can be generated and saved by the DPP-PSD Control Software.

![Diagram summarizing the DPP-PSD parameters. The trigger fires as soon as the signal crosses the threshold value. Long Gate, Short Gate, Gate Offset, Pre-Trigger, Trigger Hold-Off, and Record Length are also shown for one acquisition window](/img/Digitizer/CAENDPPPSD_DiagramSummarizingTheDPPPSDParameters_23.png)

Further on the above list there are additional features that can be performed by the DPP-PSD firmware, listed below:

- automatically discard events according to a programmable PSD threshold (see section **Online PSD selection**);
- detect pile-up conditions (See Appendix A and B for 751 and 720 families respectively; not yet available for 725 and 730 series);
- implement coincidences between channels of the same board as well as among channels of different boards through the LVDS I/O connectors (VME only), external trigger, and external modules.

## Baseline

The baseline calculation is an important feature of the DPP-PSD firmware, since its value is used as a reference value for the charge integration of the input pulses. Moreover, most of the DPP parameters are related to the baseline value.This paragraph describes in detail how the baseline calculation works.

The user can choose to set a fixed value for the baseline, or to let the DPP firmware calculate it. In the first case the user must set the baseline value in LSB units, where **1 LSB = 0.48 mV** for 720 series (DT5790), **1 LSB = 0.97 mV** for 751 series,and **1 LSB = 0.12 mV** for 725 and 730 series with 2 Vpp input range, or **0.03 mV** for 725 and 730 series with 0.5 Vpp input range.

The firmware can dynamically evaluate the baseline as the mean value of N points inside a moving time window. The user can choose the N value among *8, 32, and 128* for 720 series (DT5790); *8, 16, 32, 64, 128, 256, and 512* for 751 series, and *16, 64, 256, 1024* for 725 and 730 series. The baseline is then frozen from few clocks before the gates start up to the end of the maximum value between the long gate and the trigger hold-off. For 751 series the freeze lasts some trigger clocks more than that maximum value. After that the baseline restarts again its calculation considering in the mean value also the points before the freeze. This allows to have almost no dead-time due to the baseline calculation.


shows how the baseline calculation and freeze work. The trigger threshold dynamically follows the variation of the baseline.

![Baseline calculation as managed by the DPP-PSD algorithm](/img/Digitizer/CAENDPPPSD_BaselineCalculation_24.png)

## CFD implementation for 725 and 730 series

The input sample is split into two path: the first performs the delay in steps of the sampling clock (2 ns step), the second performs the attenuation. Possible choices of attenuation are: 25%, 50%, 75%, and 100% (i.e. no attenuation) with respect to the input amplitude. The CFD signal is referred to the mid-scale of the dynamics, i.e. channel 8192.

To enable the CFD discrimination the user must:

- set to 1 bit[6] of register 0x1n80.

Then it is possible to set the values of the delay and of the fraction, writing register 0x1n3C “CFD Settings”:

- bits[9:8] correspond to the four options of the fraction:
	- 00: fraction = 25%;
	- 01: fraction = 50%;
	- 10: fraction = 75%;
	- 11: fraction = 100%;
- bits[7:0] correspond to the CFD delay value.

A typical signal from CFD is shown in **Fig. 2.7**, where the red points are the digital samples. The Negative Zero Crossing (NZC) and the Positive Zero Crossing (PZC) are the samples before and after the zero crossing. The NZC corresponds to the Coarse Time Stamp ($T_{coarse}$ ), that is the trigger time stamp as evaluated by the standard PSD algorithm.

For 725 and 730 series the algorithm can also evaluate the value of the Fine Time Stamp T fine (see *Fig. 2.7*), as the interpolation of the PZC and the NZC according to the formula:

$$T_{fine} = \frac{8192-NZC}{PZC-NZC}\cdot 2 ns$$

The “Interpolated Zero Crossing” (ZC) then corresponds to the sum of the Coarse Time Stamp and the Fine Time Stamp.

$$ZC = T_{coarse} + T_{fine}$$

![A typical CFD signal.](/img/Digitizer/CAENDPPPSD_ATypicalCFDSignal_27.png)

The user can choose which information from the CFD algorithm is saved in the event structure for the readout.

In addition it is also possible to reserve 16 bit for the time stamp extension. The extra 16 bit must be read as the most significant bits of the time stamp. The extended time stamp is therefore expressed as a 31+16 = 47 bit number.

Both the CFD and the time stamp extension are written in an additional word to the event format, which is called “EXTRAS” word (see section **Channel Aggregate Data Format for 725 and 730 series**). The EXTRAS format varies according to the user settings. The specific registers that must be enabled are:

- bit[6] of register 0x1n80 set to 1 to enable the evet selection via CFD;
- bit[17] of register 0x8000 set to 1 to enable the EXTRAS word recording;
- bits[10:8] of register 0x1n84 (where n is the channel number) allows the user to select the specific information saved into the EXTRAS word. 

![Summary of the options for the EXTRAS word write in the 725 and 730 series](/img/Digitizer/CAENDPPPSD_SummaryOfTheOptionsForTheEXTRASWordWrite_21.png)

Where the “Baseline” corresponds to the baseline value multiplied by 4, and “Flags” are:

- bit[15]: Trigger Lost (the first event after a trigger lost has this flag set);
- bit[14]: Over-range (the first event after a saturation has this flag set).

Considering that the trigger time tag (“TTT”) is a 31 bit number (refer to the word “TRIGGER TIME TAG” of section **Channel Aggregate Data Format for 725 and 730 series**) and that EXTRAS is a 32 bit word, the final time stamp can be equal to one of the possibilities reported in **Tab. 2.1**, according to the selected choice.

![Summary of the CFD options for 725 and 730 series, where “Time Stamp” is the timing information of a single event](/img/Digitizer/CAENDPPPSD_TimeStamp_22_1.png)
![Summary of the CFD options for 725 and 730 series, where “Time Stamp” is the timing information of a single event](/img/Digitizer/CAENDPPPSD_TimeStamp_22_2.png)


----

## Memory Organization

Each channel has a fixed amount of RAM memory to save the events. The memory is divided into a programmable number of buffers (also called “aggregates”), where each buffer contains a programmable number of events. For the 725 and 730 families each buffer is shared between two channels, i.e. channel 0 and channel 1, channel 2 and channel 3, etc. The event format is programmable as well. The board registers involved are the following:

- “Aggregate Organization” (Nb), address 0x800C: defines the total number of buffers (i.e. aggregates) in which the memory is divided ( num..buffers = $2^{Nb}$ ).
- “Number of Events per Aggregate” (Ne), address 0x1n34: defines the number of events contained in one aggregate. The maximum allowed value is 1023.
- “Record Length” (Ns), address 0x1n20: defines the number of samples of the waveform, if enabled (Ns = 8 * CUST.SIZE for 720 and DT5790 series, and Ns = 12 * CUST.SIZE for 751 series, where CUST.SIZE is the value written in the register).
- “Board Configuration”, address 0x8000: defines the acquisition mode and the event data format.

According to the programmed event format, an event can contain a certain number of samples of the waveform, one trigger time stamp, the two charges $Q_{short}$ and $Q_{long}$ , and the Baseline/Extras information.


### 720 (DT5790), 725 and 730 series

The following section will describe the structure of the memory organization of 720 (DT5790), 725 and 730 series that are quite similar each other. Differences will be explicitly pointed out.

The physical memory inside the board is made of memory locations, each of 128-bit (16B).

In terms of location occupancy:  
Trigger Time Stamp = 1 location;Waveform (if enabled) = 1 location every 8 samples;Charges (Q L and Q S ) and Baseline (BSL) = 1 location.

**Fig. 4.1** and **Fig. 4.2** show the data format as saved into the physical memory for 720 (DT5790), 725 and 730 series,respectively. The structure is the same apart for the Trigger Time Tag, that has one bit less in the 725 and 730 format. Since two channels share the same buffer one bit is reserved to store the channel number, where 0 corresponds to the odd channel of the couple, and 1 to the even channel.

**Note: Fig. 4.1** and **Fig. 4.2** refers to the event storage into the physical memory of the board. Data are then organized in a different format for the event readout. The event readout format is shown in the **Event Data Format** section.

![Data organization into the Internal Memory of x720 digitizer and DT5790 Baseline data is the baseline value frozen at the trigger fire](/img/Digitizer/CAENDPPPSD_DataOrganization720Baseline_41.png)
![Data organization into the Internal Memory of x725 and x730 digitizer Baseline data is the baseline value frozen at the trigger fire](/img/Digitizer/CAENDPPPSD_DataOrganization730Baseline_42.png)

As previously said, the “Record Length” and the “Board Configuration” settings determine the event size; the user must calculate the number of event per buffer (Ne) and the number of buffers ( $2^{Nb}$ ) accordingly. When the board runs in List Mode, the event memory contains only two locations, one for the Trigger Time Tag and one for the Charge and Baseline. Therefore it is very small and it is suggested to use a big value for Ne to make the buffer size as big as at least a few KB. Small buffer size results in low readout bandwidth. The only drawback of setting high values for Ne is that the events are not available for the readout until the buffer is complete; hence there is some latency between the arrival of a trigger and the readout of the relevant event data. Conversely, when the board runs in Oscilloscope Mode, especially when the record length is large, it is more convenient to keep Ne low (typically 1).

Example1: suppose that the mixed mode is enabled and Ns is set to 400 samples:  
event size (in locations) = 1(Time\_Stamp) + Ns/8(Waveform) + 1(Charge\_Baseline) = 52 loc.  
Suppose to set Ne = 60 (number of events per buffer), hence:  
buffer\_size (in locations) = 52 * 60 = 3120 loc.

Supposing that the memory board is made of 128k loc./ch, the number of buffers will be:  
128k/3120 = 42 (buffers).

This value corresponds to the maximum number of buffers that the memory can contain. However, since the programmable value must be a power of two, the user has to choose the closest number smaller than 42 which can be represented as a power of two, that is $2^5 = 32$ (i.e. Nb = 5 has to be written in the “Aggregate Organization” register).

Example2: suppose that the mixed mode is enabled and Ns is set to 24 samples:  
Event size (in locations) = 1(Time\_Stamp) + Ns/8(Waveform) + 1(Charge\_Baseline) = 5 loc.  
Having a small event size, is it convenient to divide the memory into few buffers of bigger size to store a large amount of events.  
Suppose to have set Nb = 3, so that the number of buffers is 8.  
Supposing that the board memory option is made of 64k locations, each buffer consists in 64k/8 = 8k locations and so the resulting number of event per aggregate should be:  
Ne = 8k/5 = 1639.

**IMPORTANT: in this case, the real number of events stored per aggregate is 1023, due to the register length constraint already mentioned.**

## Event Data Format

When the data readout is performed by the Control Software, the data format has the following encoding. Those who need to write their own acquisition software must take care of the following sections.


### Channel Aggregate Data Format for 720 (DT5790) series

……


### Channel Aggregate Data Format for 725 and 730 series

The Channel Aggregate is composed by the set of Ne events, where Ne is the programmable number of events contained in one aggregate (see the previous section). The structure of the Channel Aggregate of two events (EVENT 0 and EVENT 1) for 725 and 730 series is shown in **Fig. 4.5**, where:

![Channel Aggregate Data Format scheme for 725 and 730 series](/img/Digitizer/CAENDPPPSD_ChannelAggregateDataFormat730_45.png)

- DT: Dual trace enabled flag (1 = enabled, 0 = disabled)
- EQ: Charge enabled flag, must be 1
- ET: Time Tag enabled flag, must be 1
- EE: Extras enabled flag
- ES: Waveform (samples) enabled flag
- EX: Extras option enabled flag:**Note: to enable the “EXTRAS” word set bit[17] of register 0x8000 (refer to Sect. CFD implementation for 725 and 730 series for more details).**
	- 000 = the word “EXTRAS” will be read as:
		- [31:16] = extended time stamp: those 16 bits can be added (left) to the Time Stamp representation, which becomes a 31+16=47 bit number;
		- [15:0] = the baseline value multiplied by 4.
	- 001 = = the word “EXTRAS” will be read as:
		- [31:16] = extended time stamp: those 16 bits can be added (left) to the Time Stamp representation, which becomes a 31+16=47 bit number;
		- [15:0] = flags, where bit[15] = Trigger Lost (the first event after a trigger lost has this flag set), and bit[14] = Over-range (the first event after a saturation has this flag set);
	- 010 = the word “EXTRAS” will be read as:
		- [31:16] = extended time stamp – the trigger time stamp becomes a 31+16=47 bit number;
		- [15:10] = flags, where bit[15] = Trigger Lost (the first event after a trigger lost has this flag set), and bit[14] = Over-range (the first event after a saturation has this flag set);
		- [9:0] = fine time stamp (T fine – see the definition in Sect. CFD implementation for 725 and 730 series).
	- 101 = the word “EXTRAS” will be read as:
		- [31:16] = CFD positive zero crossing PZC;
		- [15:0] = CFD negative zero crossing NZC.
- AP: Analog Probe selection. For 725 and 730 series possible selections are:
	- If DT = 0:
		- 00 = “Input”;
		- 01 = “CFD”;
	- If DT = 1:
		- 00 = “Input” and “Baseline”;
		- 01 = “CFD” and “Baseline”;
		- 10 = “Input” and “CFD”.
- DP1: Digital Virtual Probe 1 selection among:
	- 000 = “Long Gate”;
	- 001 = “Over Threshold”, digital signal that is 1 when the input signal is over the requested threshold;
	- 010 = “Shaped TRG”, logic signal generated by a channel in correspondence with its local self- trigger. It is used to propagate the trigger to the other channels of the board and to other external boards, as well as to feed the coincidence trigger logic ;
	- 011 = “TRG Val. Acceptance Win.”, logic signal corresponding to the time window where the coincidence validation is accepted. The validation enables the event dump into the memory;
	- 100 = “Pile Up“, logic pulse set to 1 when a pile up event occurred (not implemented);
	- 101 = “Coincidence“, logic pulse set to 1 when a coincidence occurred;
	- 110 = reserved;
	- 111 = “Trigger”.
- DP2: Digital Virtual Probe 2 selection among:
	- 000 = “Short Gate”;
	- 001 = “Over Threshold”, digital signal that is 1 when the input signal is over the requested threshold;
	- 010 = “TRG Validation”, digital signal that is 1 when a coincidence validation signal comes from the mother board FPGA;
	- 011 = “TRG HoldOff”, logic signal generated by a channel in correspondence with its local self- trigger. Other triggers are inhibited for the overall Trigger Hold-Off duration;
	- 100 = “Pile Up“, logic pulse set to 1 when a pile up event occurred (to be implemented);
	- 101 = “Coincidence“, logic pulse set to 1 when a coincidence occurred;
	- 110 = reserved;
	- 111 = “Trigger”.
- CH: since two consecutive channels share the same buffer, the CH flag identifies if the even channel or the odd channel participated to the event (0 for even, 1 for odd).
- $DPi_m$ (i=1, 2; m=0, 1, ...,n-1): Digital Virtual Probe value i for sample m
	- $DP1_m$ is the value of the probe written in DP1 flag
	- $DP2_m$ is the value of the probe written in DP2 flag

- $S_{m’} (m’=0, 2, 4, ..., n-2): Even Samples of the analog probe (whose flag is stored in AP) at time t=m’.
- $S_{m’’} (m’’=1, 3, 5, ..., n-1): Odd Samples of the analog probe (whose flag is stored in AP) at time t=m’’.
	- If DT=1, S m’’ corresponds to the second analog probe at time t=m’’- 1.
	- For example if DT= 1, and the analog probe selection is “Input” – “Baseline”, S m’ will corresponds to the “Input”
sample at time m’, while S m’’ will corresponds to the “Baseline” at the same time m’ (= m’’-1).
- $Q_{short/long} : integrated charge value in the short/long gate

**Note: when the “Dual Trace” option is enabled half of the samples are used to store the baseline. Therefore only the remaining half samples are used for the input waveform. In the plot visualization each input sample is duplicated to keep the same granularity. Those who need to acquire waveforms with full resolution should disable the dual trace option. In the DPP-PSD Control Software select the “NONE” option for “Analog Probe 2”.**

### Channel Aggregate Data Format for 751 series

……

## Board Aggregate Data Format

For each readout request (occurring when at least one channel has available data to be read) the “interface FPGA (ROC)” reads one aggregate from each enabled channel memory. No more than one aggregate per channel is read each time. The sample of Channel Aggregates is the Board Aggregate. If one channel has no data, that channel does not come into the Board Aggregate.

The data format when all 8 channels of a VME have available data is as shown in **Fig. 4.7** for 720 (DT5790) series, in **Fig. 4.8** for 725 and 730 series, and in **Fig. 4.9** for 751 series:

![Board Aggregate Data Format scheme for 725 and 730 series](/img/Digitizer/CAENDPPPSD_BoardAggregateDataFormat730_48.png)

- BOARD AGGREGATE SIZE: total size of the aggregate
- BOARD ID: corresponds to the GEO address of the board. In case of VX boards this number is automatically set for each board. In case of VME boards this value is by default = 0 for all boards. It is possible to set the Board ID through register 0xEF08. The GEO address is quite useful in case of concatenate BLT (CBLT) read.
- BF: Board Fail flag. This bit is set to “1” as a consequence of a hardware problem, as for example the PLL unlock. The user can investigate the problem checking the Board Failure Status register 0x8178, or contacting CAEN support.**Notes: BF bit is meaningful only for ROC FPGA firmware revision greater than 4.5. It is reserved for previous releases.**
- PATTERN: is the value read from the LVDS I/O (VME only);
- CHANNEL MASK: corresponds to those channels participating to the Board Aggregate;
- DUAL CHANNEL MASK: corresponds to the couple of channels participating to the Board Aggregate (725 and 730 only);
- BOARD AGGREGATE COUNTER: counts the board aggregate. It increase with the increase of board aggregates;
- BOARD AGGREGATE TIME TAG: is the time of creation of the aggregate (this does not corresponds to any physical quantity).

## Data Block

The readout of the digitizer is done using the Block Transfer (BLT); for each transfer, the board gives a certain number of Board Aggregates, consisting in the Data Block. The maximum number of aggregates that can be transferred in a BLT is defined by the READOUT\_BTL\_AGGREGATE_NUMBER. In the final readout each Board Aggregate comes successively. In case of n Board Aggregates, the Data Block is as in **Fig. 4.10**.

![Data Block scheme](/img/Digitizer/CAENDPPPSD_DataBlockScheme_410.png)

----

Note: The algorithm is internally designed to work with negative pulses. When the analog input pulse is positive, it is necessary to invert it before the DPP algorithms are applied. The option PULSE POLARITY = “Positive” enables the internal inversion of the pulse polarity. Please notice that the inversion is applied to the DPP algorithms only, while it doesn’t affect the waveform recording (both plots and output files keep the original pulse polarity).





<!-- CAEN_DPPPSD.md ends here -->
