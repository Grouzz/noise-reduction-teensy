# Real-Time Noise Reduction on Teensy 4.0

A real-time embedded audio noise-reduction system running on a **Teensy 4.0** with the Teensy Audio Shield. The project captures microphone audio, performs spectral denoising using an **STFT → Wiener filter → inverse STFT** pipeline, and outputs the cleaned signal to headphones in real time.

---

## Project Overview

In noisy environments, a microphone signal is often polluted by relatively stationary background noise such as ventilation, electronic hiss, room hum, or continuous ambient sound. These disturbances reduce speech intelligibility and can make communication systems harder to use.

This project implements an embedded spectral denoiser. It first learns the ambient noise profile during a short silence phase, then attenuates frequency bins whose signal-to-noise ratio is low. The processing runs directly on the microcontroller using the ARM CMSIS DSP FFT routines.

**Main idea:**

```text
Microphone input
    ↓
Hann window
    ↓
Real FFT / STFT
    ↓
Spectral Wiener gain
    ↓
Inverse FFT
    ↓
Overlap-add reconstruction
    ↓
Headphone output
```

---

## Hardware Requirements

The project is designed for the following hardware setup:

| Component | Purpose |
|---|---|
| Teensy 4.0 | Main microcontroller running the DSP algorithm |
| Teensy Audio Shield with SGTL5000 codec | Audio input/output interface |
| Microphone | Captures the noisy audio signal |
| Headphones or amplified output | Plays the processed audio |
| Push button | Controls bypass and noise-learning mode |
| Potentiometer | Controls denoising aggressiveness |
| Jumper wires / breadboard | Wiring and prototyping |

---

## Software Requirements

Install the following tools before compiling and uploading the sketch:

- Arduino IDE or PlatformIO.
- Teensyduino / Teensy board support.
- Teensy Audio Library.
- ARM CMSIS DSP support, available through the Teensy environment.

The sketch uses these libraries:

```cpp
#include <Audio.h>
#include <Wire.h>
#include <SPI.h>
#include <arm_math.h>
```

---
