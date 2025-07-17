(device-index)=
# SLM-MX
:::::{margin}
::::{card}
<div style="text-align: center"><b>SLM-MX</b></div>
^^^
:::{image} /images/modules/SLM-MX.png
:alt: SLM-MX
:width: 70%
:align: center
:::
---
+++
**External Power**: 24VDC @ 5W (+1.25W per additional I/O module)\
**Max TCP Connections**: 8\
**Max Number of Slots**: 8

---

**Network Configuration**: DHCP (on by default) or user-defined static IP address\
**Port Configuration**: 502\
**Default IP Address (APIPA fallback)**: 169.254.10.10\
**Default Subnet Mask**: 255.255.xxx.0\
**Default Gateway**: 192.168.0.1
+++
**Specifications**:\
{download}`Download SLM-MX Datasheet </docs/SLM-MX-DS_RevXA_11042024.pdf>`
::::
:::::
The **SLM-MX** is a **Modbus TCP device** designed to map up to [**8 connected slots**](../../modules/index.md) into a **single register table**.

 It is built for **quick, intuitive, and expandable configuration**, making it adaptable to a wide range of applications with minimal initial setup effort.

## **Getting Started**  
### 1. Powering Up and Network Connection
   - Connect the **SLM-MX** to a power source.  
   - Use an **Ethernet cable** to link the device to your network.  
   - On first boot, **DHCP is enabled by default**, allowing the device to automatically obtain an IP address.  

### 2. Discovering the Device
   - Open the [**configuration tool**](configurator-index) and scan for available devices. See more information in [scanning for devices](../configurator/scanning-for-device.md) page.  
   - The **SLM-MX** should appear in the scanned device list.  
   - Select and connect to the device.  
   - The main menu will display all **physically connected slots**.  

### 3. Device Status and LED Indicators
   - Upon initial power-up, the **SLM-MX LEDs** will indicate that the device is **awaiting configuration** (see LED behavior details [here](led-status.md)).  
   - Once configured using the **configuration tool**, the LED behavior will change, and all [**Modbus register addresses**](../configurator/modbus-addresses.md) will become fully accessible.

## **Device Initialization**
<p style="font-size: 1.2em;;">Please use the <a href="../configurator/index.html"><strong>configuration tool</strong></a> to initialize the device.</p>

## **Network Configuration**

```{note}
**Important Setup Requirements:**

Connect the RJ-45 (ethernet) cable to the device and network **before powering up** for optimal network discovery.
```

### **Default Network Behavior**
- **DHCP is enabled by default** - the SLM-MX will automatically obtain an IP address from the network on power up
- We recommend [configuring a static IP address](network-configuration) for the SLM-MX and storing this IP address for future connections after the initial setup

### **DHCP Fallback (APIPA)**
If DHCP fails, the device will default to an APIPA (Automatic Private IP Addressing) fallback address of **169.254.10.10**.

**To connect when using APIPA:**
- Connect the SLM-MX to your local subnet—either directly to your computer's ethernet port or through a switch that is isolated from the external network
- Manually enter the IP address **169.254.10.10** into the configurator software for initial setup
- See [**APIPA manual connection**](manual-connection) for detailed instructions

### **Security Considerations**

```{warning}
**Network Security**

The SLM-MX communicates using Modbus TCP, an industrial protocol designed for reliable operation **in trusted networks**. For secure deployment:

- Deploy the device on a **secure, isolated network segment**
- Use **firewalls and access controls** to restrict network access as needed
- Follow your organization's **industrial network security policies**
- Consult with your IT team for **remote access requirements**

These practices help ensure reliable and secure operation in industrial environments.
```

## **See also**

## 1. [Led Status](led-status)
Describe the behavior and control of LED indicators, and how they reflect various system statuses and events.

## 2. [Factory Reset](factory-reset-device)
Explain the process for restoring the device to factory settings, detailing the conditions and actions involved in a reset.

## 3. [Firmware Update](firmware-update)
Cover the mechanism for updating the firmware—from data reception and validation to writing to flash and finalizing the update.

## 4. [Slot Mismatch](slot-mismatch)
Detail the detection and handling of discrepancies between the configured I/O slots and the actual hardware slots.

## 5. [Modbus Watchdog](modbus-watchdog)
Describe the Modbus watchdog mechanism that monitors communication integrity and triggers safety measures upon timeout.