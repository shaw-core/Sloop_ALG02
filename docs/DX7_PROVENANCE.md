# DX7 engine provenance

`firmware/src/eng_dx7.c` is a C adaptation for the FM-1. It uses the 32 algorithm routing flags, envelope model, pitch-envelope tables and LFO rate/delay/waveform model from [Google's Music Synthesizer for Android](https://github.com/google/music-synthesizer-for-android), specifically `app/src/main/jni/fm_core.cc`, `env.cc`, `dx7note.cc`, `patch.cc`, `pitchenv.cc` and `lfo.cc`.

Copyright 2012–2013 Google Inc. These portions are licensed under Apache License 2.0, retained in [Apache-2.0.txt](Apache-2.0.txt). The surrounding SLOOP/Felucca firmware remains GPL-3.0; Apache-2.0 is compatible with GPL-3.0.

Changes: conversion to C and integer DSP; FM-1's 32-sample control blocks, sine and exponential tables; two voices per track; operator gain interpolation; packed 128-byte patch access; approximate key scaling and velocity scaling; bounded oscillator increments and output; integration with SLOOP's voice allocation, mixer, bank and project storage. Factory voices in this extension are authored for the project; external Yamaha ROM banks are not bundled in firmware.

This is a DX7-format-compatible six-operator engine, not a bit-exact Yamaha chip or Dexed emulator. It supports all 32 routings, operator ratio/fixed modes, coarse/fine/detune, four-rate/four-level envelopes, pitch envelope, transpose, LFO wave/speed/delay, PM/AM, feedback and operator masks. Keyboard/velocity scaling and some modulation depths are approximations. Extremely high oscillator frequencies are bounded, so high fixed-frequency or high-ratio voices may differ from hardware. Released voices are stopped after 120 seconds at most.

SysEx import is independently implemented from the published Yamaha DX7 layouts, cross-checked against the MSFA `patch.cc` and Dexed [sysex-format.txt](https://github.com/asb2m10/dexed/blob/master/Documentation/sysex-format.txt). Only validated single-voice (155 data bytes) and bank (4096 data bytes, 32 packed voices) messages are accepted; incoming files are parsed in the browser and never forwarded verbatim to the hardware.
