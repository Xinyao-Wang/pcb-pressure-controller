# PCB Pressure Controller

This repository contains the PCB design for a pressure control system used to drive and measure pneumatic artificial muscles (PAMs). The board interfaces with multiple pressure sensors and solenoid valves and was validated in hardware experiments.

## System Overview

The PCB performs two main functions:

1. **Pressure sensing** for multiple PAMs using digital pressure sensors over I2C.
2. **Valve actuation** using solenoid valves driven through transistor arrays.

The system was designed to work with a BeagleBone Black as the host controller.

## Pressure Sensing (I2C)

- **Sensor:** Honeywell 150PG2A3  
  - Range: 0–150 psi  
  - Interface: I2C (address 0x28)
- **Challenge:** All sensors share the same fixed I2C address.
- **Solution:** TCA9548A I2C multiplexer
  - Supports up to 8 channels
  - Configured at address `0x70`
  - Allows multiple identical sensors on a single I2C bus :contentReference[oaicite:0]{index=0}

**Electrical details:**
- I2C pull-up resistors: 1.8 kΩ on the main bus
- Sensor-side pull-ups: 1 kΩ
- Multiplexer connected directly to the BeagleBone Black I2C interface

## Valve Control

- **Actuators:** 5 V solenoid valves
- **Driver:** ULN2803A Darlington transistor array
  - Used to sink valve current beyond GPIO capability
  - Protects the controller from inductive loads :contentReference[oaicite:1]{index=1}

### Manual Control and Indicators
- Push-button switches for manual valve actuation
- Per-valve LED indicators showing activation state
- Additional LEDs indicating:
  - Valve power (VCC)
  - Transistor supply power

The valve drive stage is adaptable to valves with different voltage requirements by modifying the supply rail.

## Power

- Valve and transistor power supplied at 5 V
- 5 V rail provided by the BeagleBone Black in the current setup
- Power indicators included for debugging and bring-up

## Repository Contents

