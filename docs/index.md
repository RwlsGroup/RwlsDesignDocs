# Embedded Gauge Documentation

This page is the central overview for the related firmware projects. It records project-specific facts and comparisons. The topic guides linked below are shared across projects and describe common engineering practices.

## Projects

| Project                             | MCU    | Bluetooth component | Product role                                                     |
| ----------------------------------- | ------ | ------------------- | ---------------------------------------------------------------- |
| **RW202 SAMD51 Gauge**        | SAMD51 | BGM220S module      | Right Weigh 202 truck scale gauge; source is in this repository. |
| **PIC24F Gauge with BGM220P** | PIC24F | BGM220P module      | Related gauge platform with a similar product role to RW202.     |

`✓` means confirmed from the current source or project information. `—` means not applicable. `TBD` means the capability or implementation must be confirmed against that project's source, specification, or release owner.

## Hardware comparison

| Hardware capability                   | RWLS202                       | RWLS 201                             |
| ------------------------------------- | ----------------------------- | ------------------------------------ |
| Host MCU                              | SAMD51J19A                    | PIC24FJ256                           |
| BLE component                         | BGM220S                       | BGM220P                              |
| Display                               | LCD color display             | LCD 7 Segment display                |
| Brightness                            | ✓                            | X                                    |
| Input                                 | touch on screen               | 6 buttons                            |
| Weighing inputs                       | 4 Air sensors                 | 4 Air sensor and/or vibration sensor |
| Daisy chain                           | X                             | ✓                                   |
| RS232                                 | Need solder                   | ✓                                   |
| External memory                       | Flash 2MB                     | X                                    |
| Board revision and electrical details | See RW202 hardware references | TBD                                  |

## Firmware comparison

| Firmware capability          | RWLS 202         | RWLS 201         |
| ---------------------------- | ----------------- | ---------------- |
| Gauge host application       | ✓                | ✓               |
| BLE module firmware          | BGM220S           | BGM220P          |
| Host bootloader & app        | ✓ SAMD51         | ✓ PIC24F        |
| Product OTA and recovery     | ✓                | X                |
| Automated build pipeline     | ✓ GitHub Actions | X                |
| Prog Reset for clear chassis | Need clear        | No need to clear |

## Mobile application comparison

| Mobile capability                     | RW202 SAMD51 | PIC24F with BGM220P |
| ------------------------------------- | ------------ | ------------------- |
| BLE communication                     | ✓           | ✓                  |
| Product setup and diagnostics         | ✓           | ✓                  |
| Mobile-initiated product OTA          | ✓           | X                   |
| App and firmware compatibility matrix | ✓           | ✓                  |

## Web distribution comparison

| Distribution capability                             | RWLS 202 | RWLS 201 |
| --------------------------------------------------- | -------- | -------- |
| Firmware package available through a web/cloud path | ✓       | X        |
| Mobile app discovers or selects a product image     | ✓       | X        |
| Separate module firmware distribution               | ✓       | X        |

## Supporting software comparison

| Tooling area                         | RWLS 202                                                | RWLS 201                                                |
| ------------------------------------ | ------------------------------------------------------- | ------------------------------------------------------- |
| MCU IDE and toolchain                | MPLAB X / XC32                                          | MPLABX /XC16                                            |
| BLE module IDE and SDK               | BGM220S SDK2025                                         | BGM220P                                                 |
| Automated build environment          | GitHub Actions and Docker                               | TBD                                                     |
| Factory programming and provisioning | pickit 5 + SDCard<br />Flasher pro<br />printer for 202 | Pickit 5 + SDCard<br />Flasher Pro<br />printer for 201 |

## RW202 project details

### Version

The current source version is **54.9-RC4**; the numeric OTA image version is **54.9.4**. Version values are defined by `RW202_MAJOR_VER`, `RW202_MINOR_VER`, and `RW202_RC_VER` in `dash_gauge_app/firmware/src/dash_gauge_fw_version.h`. Check the [GitHub Releases page](https://github.com/RwlsGroup/RW_202_UI_FW/releases) for published packages.

### Build and release pipeline

See at [pipeline](1.overview/operating-model.md)

## Shared topic guides

- [Operating model](1.overview/operating-model.md)
- [Versioning practices](1.overview/versioning.md)
- [Getting started](1.overview/getting-started.md)
- [Hardware design notes](2.hardware/hardware.md)
- [Firmware architecture](3.firmware/1.firmware.md)
- [Build and release practices](3.firmware/1.firmware.md#shared-build-and-release-practices)
- [Mobile integration](4.mobile-application/mobile-application.md)
- [Web distribution](5.web/web.md)
- [Supporting software and manufacturing](6.supporting-software/supporting-software.md)

## Documentation conventions

- Keep all project-specific facts, matrices, and exceptions on this page.
- Keep reusable concepts, procedures, and design guidance in the topic pages.
- Use `assets/images/shared/` for media used across products and `assets/images/<project-id>/` for product-specific media.
- Use relative links so the documentation remains portable as a standalone repository or Git submodule.
