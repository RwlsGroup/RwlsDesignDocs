# Firmware Versioning

## Version format

Firmware releases use the common format:

```text
Major_version.minor_version.build_version
```

| Component         | Meaning                                                                                                        |
| ----------------- | -------------------------------------------------------------------------------------------------------------- |
| `Major_version` | Identifies the device hardware family/version. It distinguishes firmware built for different product hardware. |
| `minor_version` | Identifies a major software generation or significant firmware change for that hardware family.                |
| `build_version` | Identifies a build or release within the major/minor version.                                                  |

For example, `54.9.4` means hardware family 54, software generation 9, build 4. A pre-release label may be written as `54.9-RC4`; the numeric OTA package version is `54.9.4`.

## Product hardware mapping

| Product line        | Hardware major version | BLE module | Version pattern        |
| ------------------- | ---------------------: | ---------- | ---------------------- |
| 202 Dash Gauge      |                     54 | BGM220S    | `54.<minor>.<build>` |
| 201 Gauge           |                     52 | BGM220P    | `52.<minor>.<build>` |
| Additional device 1 |                    TBD | TBD        | TBD                    |
| Additional device 2 |                    TBD | TBD        | TBD                    |
| Additional device 3 |                    TBD | TBD        | TBD                    |

The three additional rows are placeholders for product lines that will be added to this documentation. Confirm the hardware version and BLE module from the approved hardware definition before assigning a version series.

## When to increment each component

- Keep `Major_version` tied to the hardware family. Use a different major value when firmware compatibility is specific to a different hardware family.
- Increment `minor_version` for a significant firmware change or new software generation while the hardware major remains the same.
- Increment `build_version` for a build or release within the same hardware and software generation.

For example, a major firmware architecture change on the 202 hardware can move from `54.8.3` to `54.9.1`. The major value remains `54` because the hardware family is unchanged; the minor value advances because the software changed significantly, and the build sequence starts at `1` for that minor release. The RW202 OTA work is an example of such a change because it introduced a new OTA flow and significant memory-layout changes.

The authoritative RW202 values are `RW202_MAJOR_VER`, `RW202_MINOR_VER`, and `RW202_RC_VER` in `dash_gauge_app/firmware/src/dash_gauge_fw_version.h`. The automated build derives the displayed pre-release and numeric OTA version from these values. See the [RW202 project and release pipeline overview](../index.md#rw202-project-details).

## Release notes by product line

Maintain a separate release-note table for each hardware family. Record the published version and the user-visible or compatibility-relevant changes. Keep implementation details in the relevant firmware documentation.

### 202 Dash Gauge — Hardware 54, BGM220S

| Version                          | Release notes                                                                                                                                                                                                                                                                                                                                                              |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `54.9.1` (RC1)                 | Introduced the MCUboot Mini OTA and recovery flow. Expanded the bootloader region and moved application start to `0x10000`; added firmware backup/check/verification, BLE security and mobile PIN unlock, QSPI DMA, watchdog support, and moved FirstBoot state to NVM user row. Signature verification and image decoding were not supported in this release candidate. |
| `54.9.2` (RC2)                 | Added sensor-count support through the `NumOfSensors` characteristic and Chassis Info. The mobile app can read and write the configured sensor count; the gauge uses the configured count in its display. Also clears Chassis Info on first boot and includes the 54.8 changes.                                                                                          |
| `54.9.3` (RC3)                 | Updated the MPLAB X build environment and CI/CD workflow. Added separate default, dev, and release configurations, build-time commit/debug metadata, and a BLE characteristic update for device-change notification.                                                                                                                                                       |
| `54.9.4` (RC4, current source) | Updated the NCP host integration for the newer BGM220S firmware, including its BLE security service and GATT database.                                                                                                                                                                                                                                                     |

### 201 Gauge — Hardware 52, BGM220P

| Version | Release notes                                                                          |
| ------- | -------------------------------------------------------------------------------------- |
| TBD     | Add the 201 release notes when the version history for this product line is available. |

### Additional device 1 — Hardware and BLE module TBD

| Version | Release notes                                                      |
| ------- | ------------------------------------------------------------------ |
| TBD     | Add release notes after the product version series is established. |

### Additional device 2 — Hardware and BLE module TBD

| Version | Release notes                                                      |
| ------- | ------------------------------------------------------------------ |
| TBD     | Add release notes after the product version series is established. |

### Additional device 3 — Hardware and BLE module TBD

| Version | Release notes                                                      |
| ------- | ------------------------------------------------------------------ |
| TBD     | Add release notes after the product version series is established. |
