# fpga-synthesizer
This will contain my efforts for creating a digital synthesizer on a FPGA. It will serve as a notebook for learning the basics of audio processing.


## The basics

### How is sound even generated?
- Analog sound is AC voltage / current flowing wires. (What about stereo vs mono?
- El-guitar pickups are transducers, they transform vibrating magnetic fields (made from the strings) into oscialltion electronic field (or just voltages)
- Microphones are transducers turning oscillating air pressure (sound) into oscillating voltage
- Speakers are also transducers which turn oscillating voltages (and maybe currents?) into oscillating air pressure which can be detected by our ears.
- Our goal is to generate those oscillating voltages which can be inputted to a loudspeaker (with active amplification)
- Our FPGA is not capable of generating an oscillating output signal.

#### Using a PWM
- By using a PWM we can represent a varying voltage by varying the duty cycle.
- For this we need a filter with analog output which can translate the PWM signal into a voltage signal

#### Using a DAC [http://nic.vajn.icu/PDF/STMicro/ARM/STM32_DAC_audio.pdf]
- A DAC is a component which makes a analog voltage based on a digital input. Its often found as peripherals on MCUs.
- They can often be configured together with a timer module to do a conversion on a interrupt which can be scheduled regularily
- So by lining up writing a series of numbers to a register in the DAC module you can generate an output waveform which can be connected to a speaker.
- Normally you have a waveform table stored in RAM and either a CPU or DMA writing those values to the DAC on certain interval
- This is equivalent with using a Audio Codec (which is duplex DAC and ADC). 
- It is normal for the Audio Codecs to support a I2S interface to the FPGA over a PMOD interface
- 

#### I2S https://en.wikipedia.org/wiki/I%C2%B2S
- Inter IC Sound communication. A protocol used to transfer sound information
- It synchronous with a clock line, channel select line and data line. 
- You can send stereo sound by toggeling the channel select line.
- The clock frequency needs to match the sampling frequency. 
- The bit precision is not specified.
- My FPGA circuit must implement a I2S master which transmits values from a wavetable to generate the wanted sounds


## Synthesizer intro



## Components
- element14 project: https://www.element14.com/community/docs/DOC-91958/l/episode-388-fpga-midi-music-synthesizer


### MIDI controller
- I need a simple MIDI keyboard that will transmit MIDI messages upon keypress.
- USB or 5 pin DIN (I have a USB based keyboard but to use this I need a special USB driver. Could ofc use the RPi to parse MIDI commands and forward them over a UART interface to the FPGA
- Better use the 5 pin DIN directyl

### USB to FPGA
- I need a way of translating the MIDI USB messages. IceSugar has a microUSB port but does not seem to have a USB PHY?
- Also seems to be quite difficult to implement MIDI USB stack. I could check out the Teensy MCUs
- https://www.pjrc.com/store/teensylc.html Can implement MIDI USB protocol and send MIDI commands directly over UART or SPI (we can try both) our design should in anyway be independent of that interface.


### FPGA
- I am thinking to use the Lattice ICE40 and the IceSugar board. ICE40 has a completely open source tool chain
- I can either design a complete circuit for the whole app (MIDI decoding, audio generation, I2S output). Or I could use a soft CPU to help
- I will use Chisel HDL for the circuit design.

### Audio generation


## BoM
- IceSugar FPGA board containing a Lattic ICE40
- MAudio MIDI keyboard/controller with a USB interface
- Audio Codec Pmod
- Some active (amplified speakers)
