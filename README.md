
# Analog Clap Switch and Counter Circuit

This repository contains the design, simulation, and hardware implementation of a sound-activated counting system. The circuit detects acoustic events (such as hand claps), converts them into clean digital pulses, and sequentially displays the count on a 7-segment display. 

This project was developed as the final project for the EEE307 Analog Electronics Laboratory course at Izmir Katip Celebi University.

## System Architecture & Working Principle

The project relies purely on analog electronics and mixed-signal logic without the use of any microcontrollers (like Arduino). It consists of three main functional blocks:

### 1. Signal Sensing & Amplification (MAX4466)
Clapping generates short-duration, high-amplitude pressure variations. An electret microphone captures these sounds, but the raw electrical output is only in the millivolt range (approx. 6 mV). The **MAX4466** audio preamplifier is used to step up this low-level analog signal to several hundred millivolts, providing enough voltage to exceed the threshold required for the next logic stage.

### 2. Signal Conditioning & Debouncing (NE555 Timer)
A common problem with acoustic triggers is that a single clap can produce echoes and noisy signal spikes, which would normally cause a digital counter to count multiple times for one clap. 

To solve this, the amplified audio signal is fed into the trigger pin of an **NE555 timer**, configured in **monostable multivibrator** mode. The NE555 acts as a "debouncer". Using a 10 kΩ resistor and a 47 µF capacitor, it generates a single, stable digital pulse lasting approximately 0.52 seconds ($T = 1.1 \times R \times C$). This effectively ignores the echoes and ensures that one clap equals exactly one count.

### 3. Counting & Displaying (CD4026)
The clean, half-second pulse from the NE555 serves as the clock input for the **CD4026** IC. The CD4026 is a specialized chip that functions as both a decade counter and a 7-segment display driver. It internally counts the pulses and directly drives a common-cathode 7-segment display without needing any additional decoding circuitry.

## Components Used
* 1x NE555 Timer IC
* 1x CD4026 Decade Counter / 7-Segment Driver
* 1x MAX4466 Microphone Preamplifier Module
* 1x 7-Segment Display (Common Cathode)
* Passive Components: 10kΩ, 2.2kΩ, 330Ω, 100Ω resistors, 47µF capacitor
* 5V DC Power Supply
<img width="605" height="454" alt="Resim2" src="https://github.com/user-attachments/assets/2e8249c9-f3b9-4fa9-86d7-aa75fc8491c8" />

## Circuit Schematic
<img width="605" height="428" alt="Resim1" src="https://github.com/user-attachments/assets/6a3d6057-f6bc-4dce-a240-316fe92571ba" />


## Simulation
Before physical implementation on a breadboard, the circuit behavior—especially the monostable pulse generation of the NE555 and the clocking of the CD4026—was fully simulated and verified using Proteus.

## Limitations & Future Improvements
While the circuit accurately counts claps, it relies purely on amplitude-based triggering. Therefore, it may also register other sharp impulsive sounds (like tapping on a hard surface). Implementing frequency-selective filtering or automatic gain control could further isolate human claps from general background noise.
