Capoworks-ecu builds on [FOME](https://github.com/FOME-Tech/fome-fw) and [rusEFI](https://github.com/rusefi/rusefi), with a direction focused on **new features, experimental control strategies, and application-specific development**.

The project provides a space to explore alternative fuel models, ignition scheduling, signal processing, and hardware integration, giving developers and tuners room to investigate ideas and adapt the firmware to specific engine requirements.

**Experimentation is part of the development process:** features can evolve through testing, calibration, and practical experience. Experimental options are identified explicitly, with their operating requirements, limitations, and validation status documented so users can make informed decisions when adopting them.

**[Firmware & Source Code](https://github.com/capoworks/capoworks-ecu) · [Wiki & Documentation](https://capoworks.github.io/capoworks-ecu-docs/)**

## Tune Independently. Blend Freely.

Capoworks separates **Speed Density and Alpha-N calibration** from the choice of how to blend them.

Each model has its own calibration and can be tuned manually or through autotuning. Once both models are calibrated, an RPM-based table with TPS or MAP as its second axis determines their contribution.

**Changing the blend does not require retuning either model.** You can adjust where and how the models work together while preserving their individual calibrations.

Blending combines calculated air masses, with common corrections applied once. The default configuration uses **100% Speed Density**.

Fuel-control features also include:

- Separate **16×16 per-cylinder trim maps** for Speed Density and Alpha-N.
- Independent load-coordinate selection for control functions and protections.
- VE Analyze sample qualification to exclude unsuitable operating conditions.

## Ignition and Idle Control

Optional cycle-averaged RPM feedback supports idle-air and idle ignition correction.

Two experimental ignition strategies provide additional timing options:

- **Cycle Profile** uses tooth-history information to calculate ignition-event timing.
- **Time Budget** plans coil charging and spark against a specific TDC occurrence.

Both default to **Legacy**. Their use depends on supported trigger geometry, cam synchronization, and ignition configuration; full requirements are documented in the wiki.

Event-lifecycle handling and per-output software dwell limits improve control across stops, failed starts, and restarts.

## Workshop, Sensors, and Hardware

- **Service / Workshop flood clear:** maintains injection cut during cranking and temporary transitions into running RPM, releasing it when the accelerator is released.
- **Hella OPS+T support:** combined oil pressure and temperature acquisition, with fault and timeout diagnostics.
- **Board-enforced TunerStudio settings:** protects parameters fixed by the hardware configuration.
- **Configuration integrity:** ordered flash access and verified backups.
- **DC/ETB actuator handling:** inhibits outputs during configuration burns and invalid control conditions.
- **Hardware and logging improvements:** Core8 error-LED routing, configurable MC33816 SPI, independent USB mass storage, and corrected MLG data offsets.

## Supported Hardware and Processors

Capoworks inherits board configurations and processor-port work from [FOME](https://github.com/FOME-Tech/fome-fw) and [rusEFI](https://github.com/rusefi/rusefi). Listed configurations require the exact board revision and matching build; supported features and validation vary.

| Hardware group | Board configurations and sources |
| --- | --- |
| Core8 / Core48 | [Core8 config](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/core8), [Core48 config](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/core48) |
| Atlas / Polygonus | [Atlas config](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/atlas), [Atlas hardware guide](https://wiki.fome.tech/Hardware/Brain-Boards/Atlas/FOME-Atlas/), [Polygonus Universal112 base](https://github.com/FOME-Tech/Polygonus-Universal-112). Atlas is a modular processor brain module that pairs with a base board; Polygonus Universal112 is one such base board. Each assembly needs its matching pinout and build. |
| Proteus | [Proteus F4/F7/H7 configs](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/proteus), [hardware guide](https://wiki.fome.tech/Intro-Start-Here/Hardware-Running-FOME/) |
| [microRusEFI](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/microrusefi) | [F4/F7 configs](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/microrusefi), [hardware guide](https://wiki.fome.tech/Intro-Start-Here/Hardware-Running-FOME/) |
| Hellen / AlphaX | [Selected Hellen and AlphaX configurations](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/hellen), not all generations |
| Frankenso | [Legacy Frankenso config](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/frankenso) |
| Discovery / Nucleo | [F407 Discovery](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/f407-discovery), [F429 Nucleo](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/nucleo_f429), [F767 Nucleo](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/nucleo_f767), and [H743 Nucleo](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/nucleo_h743) development targets |

| Processor family | Capoworks target class | Concrete target examples |
| --- | --- | --- |
| STM32F4 | Arm Cortex-M4 | F407 ([Discovery](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/f407-discovery)), F429 ([Nucleo](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/nucleo_f429), [Proteus F4](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/proteus)) |
| STM32F7 | Arm Cortex-M7 | F767 ([Nucleo](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/nucleo_f767)) |
| STM32H7 | Arm Cortex-M7 | H743 ([Atlas](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/atlas), [Nucleo](https://github.com/capoworks/capoworks-ecu/tree/master/firmware/config/boards/nucleo_h743)) |

These targets follow the [firmware processor mapping](https://github.com/capoworks/capoworks-ecu/blob/master/firmware/rusefi.mk) and do not imply support for every chip, memory size, package, or pinout in a family.

### Custom Hardware and Future Ports

Custom carrier boards and OEM harness adapters are welcome development opportunities, together with tailored I/O, driver, and CAN support or additional MCU variants and families. Each needs board integration or HAL/driver porting, followed by appropriate bench and engine validation.

Newer upstream [rusEFI](https://github.com/rusefi/rusefi) designs need individual assessment. Discovery and Nucleo development boards need appropriate power protection, signal conditioning, and actuator drivers. To discuss requirements early, open a [GitHub issue](https://github.com/capoworks/capoworks-ecu/issues/new) with the MCU, schematics, I/O counts, connectors, and peripheral requirements.

## Compatibility and Validation

The feature set has been validated on a **Rotax V990**, as reported by the project owner. Hardware support remains specific to the board, wiring, trigger arrangement, and feature configuration.

Experimental ignition strategies retain their experimental status. Firmware updates may require calibration migration: use the matching INI and follow the [wiki](https://capoworks.github.io/capoworks-ecu-docs/).

## Origins and Acknowledgements

**Capoworks derives from [FOME](https://github.com/FOME-Tech/fome-fw), which derives from [rusEFI](https://github.com/rusefi/rusefi).**

We thank both projects’ developers, contributors, and communities for the engineering, knowledge, and tools that made Capoworks possible. Our work includes original changes and adapted upstream contributions, with provenance preserved in Git history.

Documentation derives from the [FOME Wiki](https://github.com/FOME-Tech/wiki) under **[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)**. Firmware and dependencies retain their respective licenses and notices.

Capoworks is independent and is not officially endorsed or supported by [FOME](https://github.com/FOME-Tech/fome-fw) or [rusEFI](https://github.com/rusefi/rusefi).
