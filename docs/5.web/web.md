# Shared Firmware Distribution Guidance

When firmware is distributed through a web portal or cloud catalog, treat the catalog as part of the controlled release path.

## Recommended release metadata

Each published package should identify the product and hardware compatibility, firmware component, version, release channel, publication date, and source release. Keep host-MCU images distinct from BLE module images unless the update package intentionally contains both.

## Release and availability controls

- Publish only artifacts produced by the approved release pipeline.
- Validate package identity and compatibility before making a version available to customers.
- Keep development and test images out of production catalogs.
- Provide a clear path to withdraw a faulty package and restore the previous supported version.
- Coordinate catalog availability with mobile app behavior and release communications.

Document actual portal ownership, API contracts, and customer instructions in the project or service documentation; those details are not shared across all products by default.
