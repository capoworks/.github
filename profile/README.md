# Capoworks

Open-source engine management firmware, derived from **[FOME](https://github.com/FOME-Tech/fome-fw)** and the original **[rusEFI](https://github.com/rusefi/rusefi)** project.

Capoworks develops and integrates improvements to fuel modelling, ignition scheduling, idle control, sensor support, and configuration integrity.

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

## Compatibility and Validation

The feature set has been validated on a **Rotax V990**, as reported by the project owner. Hardware support remains specific to the board, wiring, trigger arrangement, and feature configuration.

Experimental ignition strategies retain their experimental status. Firmware updates may require calibration migration: use the matching INI and follow the [wiki](https://capoworks.github.io/capoworks-ecu-docs/).

## Origins and Acknowledgements

**Capoworks derives from [FOME](https://github.com/FOME-Tech/fome-fw), which derives from [rusEFI](https://github.com/rusefi/rusefi).**

We thank both projects’ developers, contributors, and communities for the engineering, knowledge, and tools that made Capoworks possible. Our work includes original changes and adapted upstream contributions, with provenance preserved in Git history.

Documentation derives from the [FOME Wiki](https://github.com/FOME-Tech/wiki) under **[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)**. Firmware and dependencies retain their respective licenses and notices.

Capoworks is independent and is not officially endorsed or supported by [FOME](https://github.com/FOME-Tech/fome-fw) or [rusEFI](https://github.com/rusefi/rusefi).
