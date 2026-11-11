# Shared Tooling and Manufacturing Guidance

Toolchains and factory processes differ by MCU and hardware revision. Keep reusable guidance here and list exact versions, commands, and approved equipment in each project's documentation.

## Development and CI tools

For each project, document the IDE, compiler, device packs or SDK, host utilities, container image, and supported operating systems. Pin or record versions used by CI so a build can be reproduced. Keep credentials and signing keys in the approved secret store.

## Manufacturing and provisioning

Document the programming image, programmer model, fixture, programming mode, device-specific provisioning fields, and verification steps. Identify which utility writes each field and how programming success is checked. Version-control manufacturing software and configuration where permitted.

## Media and diagrams

Store documentation media under `assets/images/`:

```text
assets/images/
├── shared/                         # reused across products
├── rw202-samd51/                   # RW202-specific media
├── bgm220-ble-firmware/            # BLE module firmware media
└── pic24f-bgm220p/                 # PIC24F/BGM220P media
```

Use relative links in Markdown. Keep editable source files with exported diagrams where practical. Avoid placing credentials, private keys, or customer data in documentation media.
