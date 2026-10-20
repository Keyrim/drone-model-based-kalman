# drone-model-based-kalman

Flight controller firmware for the fc2 board (STM32F405), 2021-2022. Kalman
filter (CMSIS-DSP) on a state-space model of the angular rates, matrices
derived in `MatLab/main.m` from the inertia matrix, centre of mass and motor
positions exported from Fusion. Complementary filter for the angles, task
manager with duration monitoring. Flies in angle mode and glove mode.

Hardware (flight controller, power and sensors boards, schematic PDFs):
[drone-fc2-hw](https://github.com/Keyrim/drone-fc2-hw), split out of this repo on
2026-10-20.
