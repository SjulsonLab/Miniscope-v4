# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Miniscope-v4 is an open-source miniature fluorescence microscope for neural imaging in freely behaving animals. This is primarily a **hardware design project** encompassing PCB design, embedded firmware, optical simulation, and 3D mechanical design. The upstream project is [Aharoni-Lab/Miniscope-v4](https://github.com/Aharoni-Lab/Miniscope-v4); this fork is maintained by SjulsonLab.

Key documentation: [Miniscope V4 Wiki](https://github.com/Aharoni-Lab/Miniscope-v4/wiki)

## Repository Structure

- **`Miniscope-v4-MCU-Firmware/`** — ATmega328 firmware (C, Atmel Studio project). Handles I2C slave communication, SPI passthrough, LED excitation control, and ETL (electrowetting tunable lens) focus adjustment. Compiled output: `Compiled Firmware/Miniscope-v4-MCU.hex`
- **`Development Files/Individual PCBs/`** — Six modular KiCad PCB designs: IMU, LED-ETL-Driver, Ser-Pow (serializer/power), PYTHON480 (image sensor), FlexPC, and MCU
- **`Miniscope-v4-Rigid-Flex/`** — KiCad rigid-flex PCB design (integrated board variant)
- **`Miniscope-v4-VDD-PIX-FC/`** — Power delivery and imaging PCB (KiCad)
- **`Miniscope-v4-Body-Parts/`** — 3D CAD models (STEP, STL, Fusion 360) for printed/machined components
- **`Miniscope-v4-Holder/`** — Stereotaxic holder assembly CAD files
- **`Miniscope-v4-Zemax/`** — Zemax optical system simulations for emission path
- **`Miniscope-v4-Denoising-Notebook/`** — Jupyter notebook for horizontal noise removal in V4 recordings

## Firmware Architecture

The MCU firmware (`Miniscope-v4-MCU-Firmware/`) targets an ATmega328 at 1MHz and acts as an I2C slave (address `0x10` by default). Key design details:

- **Protocol**: Host sends I2C commands; MCU interprets register-based commands to control LED brightness (via GPIO), ETL focus (via PWM/SPI DAC), and gain settings
- **Firmware version**: `0x11`, Product ID: `0x40`
- **SPI passthrough**: MCU can relay SPI transactions to configure external ICs
- **Programming**: ISP interface (no soldering required)

## Development Tools

- **PCB Design**: KiCad (`.kicad_pcb`, `.sch`, `.pro` files)
- **Firmware**: Atmel Studio (`.atsln`, `.cproj` project files), AVR-GCC toolchain
- **Optical Design**: Zemax OpticStudio (`.zmx` files)
- **3D CAD**: Fusion 360 (`.f3d`), with exported STEP and STL files
- **Data Processing**: Python/Jupyter

## Git Workflow

- **`master`** is the main/stable branch
- **`IRIG-dev`** is the active development branch
- Upstream contributions come via pull requests
- No CI/CD or automated tests — this is a hardware project
