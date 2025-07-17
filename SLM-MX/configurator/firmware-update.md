# Firmware Update

The SLM-MX Configurator provides tools for updating the firmware of your SLM-MX device.

## Checking Current Firmware Version

There are multiple ways to check the firmware version:
- On the initial scanning page, the firmware version will be displayed for all connected SLM-MX devices. 
- On a connected SLM-MX, the firmware version will be displayed on the expanded SLM-MX device card.

## Downloading Firmware Updates

To download the latest firmware:

1. Visit the Synergy Logic website at [www.synergy-logic.com](https://www.synergy-logic.com)
2. Download the latest firmware package

## Updating SLM-MX Firmware

To update the SLM-MX firmware:

1. Connect to your SLM-MX device
2. Navigate to the "Update Firmware" tab
3. Click "Browse" and select the firmware file you downloaded
4. Click "Update Firmware" and confirm the update when prompted. It will also show you your current firmware version and the version of the firmware you are updating to.

```{figure} /images/configurator/firmwareUpdateConfirmation.png
:align: center
:width: 80%
```
5. Wait for the update process to complete
6. The device will restart automatically after the update

## Firmware Update Best Practices

For successful firmware updates:

- Always back up your device configuration before updating firmware
- Ensure stable power during the update process
- Do not disconnect the device during the update
- Make sure to not have active modbus communication with the device during the update
- If the update fails, try restarting the device and configurator software before attempting the update again.
- Contact Synergy Logic support if problems persist
---

**Previous**: [Creating and Importing Offline Configuration Files](offline-configuration) | **Next**: [SLM-MX Configurator](index) 