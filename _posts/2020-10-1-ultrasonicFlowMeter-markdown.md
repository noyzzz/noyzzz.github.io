---
layout: post
title: Ultrasonic Flow Meter 
subtitle: Design and implementation of an ultrasonic flow meter
tags: [Project,Microcontroller, Flow_meter]
comments: true
---

### Types of Flow Meter

* **Displacement Meter**
![DP_METER](/assets/img/dpmeter.gif){: .mx-auto.d-block :}

{: .box-note}
A positive displacement meter is a type of flow meter that requires fluid to mechanically displace components in the meter in order for flow measurement. Positive displacement (PD) flow meters measure the volumetric flow rate of a moving fluid or gas by dividing the media into fixed, metered volumes (finite increments or volumes of the fluid)


* **Velocity-based Meter**

	- Electromagnetic Meter
	- **Ultrasonic Meter**

### Ultrasonic Meters
- Doppler effect meter
![DP_METER](/assets/img/dopmeter.gif){: .mx-auto.d-block :}
The Doppler Effect Ultrasonic Flow meter use reflected ultrasonic sound to measure the fluid velocity. By measuring the frequency shift between the ultrasonic frequency source, the receiver, and the fluid carrier, the relative motion are measured.
The resulting frequency shift is named the Doppler Effect.

- **Transit-time meter**
![DP_METER](/assets/img/ttmeter.gif){: .mx-auto.d-block :}

Transit time ultrasonic flow meters measure the difference in time from when an ultrasonic signal is transmitted from the first transducer until it crosses the pipe and is received by the second transducer. A comparison is made of upstream and downstream measurements.

We implemented a transit time meter using Texas Instruments chips and AVR microcontroller.

Texas Instrument chips that we used are TDC1000 and TDC7200.
TDC1000 is an analog-front-end(AFE) which is used for sending and receiving signals to the ultrasonic sensors.The signals are then sent to TDC7200 which is a time-to-digital converter.The results from TDC7200 is used by a low-power MCU to generate the flow rate.



