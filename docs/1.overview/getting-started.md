# Getting Started

## Choose a project

Start by identifying the product line. The product family has two main lines:

| Product line  | User interface                                     | Product characteristics                |
| ------------- | -------------------------------------------------- | -------------------------------------- |
| **201** | Seven-segment display with mechanical push buttons | Earlier gauge design.                  |
| **202** | Color display with touch input                     | More compact and updated gauge design. |

After selecting the product line, choose the firmware project that matches its MCU and Bluetooth hardware. This repository contains firmware for the **202** line. The [central project overview](../index.md) describes the RW202 SAMD51 project and related BGM220 and PIC24F/BGM220P projects.

## Get source and submodules

The repository uses Git submodules for shared code and BLE dependencies. The `.gitmodules` file currently uses HTTPS URLs, so HTTPS with Git Credential Manager is the simplest option for most developers. The main repository and private submodule repositories require authorized GitHub access.

### Clone with HTTPS

Run these commands in a terminal. Sign in through Git Credential Manager when prompted; do not put a personal access token directly in the clone URL.

```sh
git clone https://github.com/RwlsGroup/RW_202_UI_FW.git
cd RW_202_UI_FW
git submodule sync --recursive
git submodule update --init --recursive
git submodule status --recursive
```

The `sync` command applies the URLs from `.gitmodules`. The `update` command fetches and checks out the exact submodule revisions recorded by this repository. In the status output, each submodule should have a commit hash and should not begin with `-` (not initialized) or `+` (checked-out revision differs from the recorded revision).

If you already have a checkout, run the last three Git commands from the repository root to initialize or refresh submodules.

<!-- ### Clone with SSH

If your GitHub SSH key is already registered and has access to the organization, you can clone the main repository over SSH:

```sh
git clone git@github.com:RwlsGroup/RW_202_UI_FW.git
cd RW_202_UI_FW
git submodule sync --recursive
git submodule update --init --recursive
```

The submodule URLs in `.gitmodules` are HTTPS, so SSH access to the main repository alone does not authenticate the private submodules. Configure HTTPS credentials for those repositories or use an approved Git URL rewrite to SSH.

GitHub Actions uses its configured repository and submodule credentials; local Git credentials are not used by the workflow. -->

## Build the RW202 firmware in MPLAB X IDE

Use the project-compatible MPLAB X IDE version. **MPLAB X IDE v6.35** is the version used by the current project build environment.

1. Open the bootloader project: `samd51_bootload_from_qspi/firmware/samd51_bootload_from_qspi.X/`.
2. Open the application project: `dash_gauge_app/firmware/right_weigh_202_gauge_ui_fw.X/`.
3. Select the intended build configuration in each project. Use matching configurations for the bootloader and application.
4. Build the bootloader first.
5. Build the application second. Its post-build step consumes the outputs from both projects.

### Check the application build configuration

Before building the application, verify these settings in MPLAB X project properties:

- Enable generation of the application `.elf` file. The build should also generate the map and HEX files needed by post-build processing.
- The **default** application configuration is prepared with the Git commit ID and the **Execute script after build** command that runs the post-build packaging script. That script creates the OTA package and invokes the image-combination script.
- The **production** application configuration does not currently include those local commit-ID and post-build command settings. If you build locally with `production`, configure and verify them first. The automated workflow supplies the required build metadata and runs the packaging steps itself.

The post-build process depends on the build utilities and signing configuration described in the project build documentation. Do not publish private signing material.

## Build outputs

  The curents scripts in folder scripts are used for build final output **`file.hex = bootload.bin + app.bin + checksum`**. The application post-build process also creates a versioned **`app.bin`** file for OTA updates.

## Program the device

- Program Microchip MCU firmware with a compatible Microchip programmer, such as **PICkit 5**. , and software MPLAB IPE.
- Program Silicon Labs BGM220 Bluetooth-module firmware with a compatible **SEGGER Flasher PRO** setup.
- Confirm the target MCU, board revision, module variant, image, and programming procedure before flashing.

## Provision Chassis Info

After first programming a gauge, use the corresponding **printer application** to write its Chassis Info included: serial number, hardware version, firmware version, number of sensors ... 

For device with external flash: If a device must be returned to a completely unprovisioned state, program the approved `reset_chassis_info` image to clear the external flash data on products that use external flash. This repository includes `PCB-0-54-x-ResetChassisInfoFw-v1-1.hex`; verify the image is approved for the target hardware before use.

## Connect with the mobile application

The Right Weigh mobile application supports **Android and iOS**. Use it to connect to and add a device, read device parameters, view user instructions, and start an OTA firmware update. Keep the phone near the gauge and follow the app's prompts until the update completes.

[play.google.com/store/apps/details?id=com.motus.rightweigh&amp;pcampaignid=web_share](https://play.google.com/store/apps/details?id=com.motus.rightweigh&pcampaignid=web_share)

[apps.apple.com/us/app/right-weigh-load-scales/id985400243](https://apps.apple.com/us/app/right-weigh-load-scales/id985400243)

For shared build, protocol, and manufacturing practices, see the [firmware](../3.firmware/1.firmware.md), [mobile integration](../4.mobile-application/mobile-application.md), and [supporting software](../6.supporting-software/supporting-software.md) guides.
