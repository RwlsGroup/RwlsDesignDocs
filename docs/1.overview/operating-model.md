# Shared Operating Model

Embedded gauge products commonly combine a host MCU, peripheral sensors, a display, a BLE module, and a mobile application. The project comparison on the [documentation home page](../index.md) identifies which components and functions apply to each product.

## Device architecture

The following diagram shows a typical gauge device. The main MCU reads the pressure sensors, controls the display, and exchanges data with the Bluetooth module. The Bluetooth module relays supported sensor information to the mobile application.

```mermaid
flowchart LR
    MCU["Main MCU"]
    BLE["Bluetooth module"]
    AirSensors["Four air pressure sensors"]
    AmbientSensor["Ambient pressure sensor"]
    Display["Display and touch input"]
    ExternalDevice["External device"]
    MobileApp["Mobile application"]

    MCU <-->|UART| BLE
    MCU <-->|I2C| AirSensors
    MCU <-->|I2C| AmbientSensor
    MCU <-->|Display interface| Display
    MCU <-->|Optional RS-232| ExternalDevice
    BLE <-->|Bluetooth Low Energy| MobileApp
```

## Typical runtime flow

1. Power is applied and the platform startup code runs.
2. If a bootloader is present, it checks for its supported update or recovery conditions and transfers control to the application.
3. The application initializes the required peripherals and product services.
4. The host communicates with its BLE module using the interface defined for that project.
5. A mobile application connects through the supported BLE services and displays the information exposed by the firmware.

## Firmware distribution flow

Firmware is built for a specific product and version. The release process validates the package and publishes it to the approved distribution channel. A product may support factory programming, an in-field update, a recovery flow, or a subset of these options.

### RW202 build and release pipeline

| Workflow               | Trigger                                                                                     | Project-specific behavior                                                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `docker-publish.yml` | Manual dispatch                                                                             | Builds and publishes the firmware build image to GitHub Container Registry.                                          |
| `build_fw.yml`       | Called by another workflow                                                                  | Checks out source and private submodules, builds bootloader and application, packages images, and uploads artifacts. |
| `test.yml`           | Manual dispatch                                                                             | Calls the reusable workflow with the `dev` configuration.                                                          |
| `build.yml`          | Push to `master` or manual dispatch                                                       | Calls the reusable workflow with the `production` configuration.                                                   |
| `release.yml`        | Push a `v*` tag<br />Pre-release: **v**xx.yy-**RC**zz<br />Release: **v**xx.yy.zz | Builds production firmware and publishes a GitHub **pre-release** or stable **release** according to the tag.      |

The build produces a combined manufacturing Intel HEX image and a signed OTA binary. Build, test, and release workflows send completion notifications with run and artifact links to the configured Microsoft Teams channel.

```mermaid
flowchart TD
    Docker["Publish Docker build image<br/>Manual, one-time or when toolchain changes"] --> Registry["GitHub Container Registry"]
    Registry -. "reused by" .-> Shared["Reusable firmware build<br/>workflow_call: checkout source, build bootloader + app,<br/>package HEX + signed OTA BIN, upload artifacts"]

    Test["Test workflow<br/>Manual dispatch"] -->|"dev/test build"| Shared
    PR["Build workflow<br/>Pull request to main"] -->|"automated build"| Shared
    Tag["Release workflow<br/>Version tag"] -->|"production build"| Shared

    Shared --> TestArtifacts["Test build artifacts<br/>available to the team"]
    Shared --> PRArtifacts["Build artifacts<br/>available to the team"]
    Shared --> Release["Create GitHub pre-release or release"]
    Release --> ReleaseArtifacts["Release artifacts<br/>available to the team"]

    PR -. "completion notification" .-> Teams["Microsoft Teams channel"]
    Tag -. "completion notification" .-> Teams
```

The PR-to-`main` path shows the requested workflow design. In the current checked-in configuration, `build.yml` runs on pushes to `master` and manual dispatch; it does not yet declare a pull-request trigger for `main`.

> **OTA rollout note:** OTA development began in 2026 and support is being introduced progressively. Not every product or firmware version supports every OTA step. Confirm compatibility for the exact device and firmware version before relying on an OTA update path.

Do not assume that bootloader behavior, memory layout, BLE services, or update transport is identical across projects. Refer to the central project overview and the shared topic guides before reusing a procedure.

## RW202 manufacturing process

This simplified flow is based on `Documentation/MfgProcessInteraction/202GaugeMfgProcessInteraction.drawio` (revision 1.2). It shows the first-boot path for factory programming and Chassis Info provisioning, followed by the normal customer startup path.

```mermaid
flowchart TD
    Program["Program MCU and BLE module firmware<br/>FirstBoot flag is set"] --> Power["Apply power"]
    Power --> FirstBoot{"FirstBoot flag set?"}

    FirstBoot -- Yes --> Diagnostics["Show diagnostic screen<br/>with touch-screen test"]
    SensorReadings["Barometric and air-sensor PSI readings are visible"] -.-> Diagnostics
    Diagnostics --> TouchTest["Operator presses the final Test button"]
    TouchTest --> ConfigureBT["Start Bluetooth interface<br/>in Configure Chassis Info mode"]
    ConfigureBT --> ChassisInfo{"Chassis Info already configured?"}
    ChassisInfo -- Yes --> ClearFlag["Clear FirstBoot flag"]
    ChassisInfo -- No --> Waiting["Show waiting-for-Chassis-Info screen"]
    Waiting --> MfgApp["Manufacturing app writes Chassis Info over Bluetooth"]
    MfgApp --> AllConfigured{"All Chassis Info configured?"}
    AllConfigured -- No --> Waiting
    AllConfigured -- Yes --> ClearFlag
    ClearFlag --> SystemInfo["Show System Info with current Chassis Info"]
    SystemInfo --> PowerOff["Operator disconnects gauge power"]

    FirstBoot -- No --> CustomerBT["Start Bluetooth interface<br/>in Customer API mode"]
    CustomerBT --> WizardComplete{"Setup Wizard complete?"}
    WizardComplete -- No --> SetupWizard["Start gauge in Setup Wizard mode"]
    WizardComplete -- Yes --> Measurement["Start gauge in Measurement mode"]
    SetupWizard --> Measurement

    ConfigureBT -. "May overwrite existing Chassis Info" .-> MfgApp
```

The manufacturing application can overwrite existing Chassis Info while the gauge is in the configuration mode. Product setup details and manufacturing tool instructions should follow the approved product-specific procedure.
