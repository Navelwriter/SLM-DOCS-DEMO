# CODESYS Example Project
## **Project Overview**
The project simulates a temperature-controlled system where the temperature reading from the SLM-THM-4 is used to control the SLM-RLY-16 relays. 

The core function maps a temperature range (75°F - 125°F) to the 16 relay channels, creating a bar-graph representation of the temperature as each relay channel corresponds to an LED.

```{admonition} Before you Start
:class: tip

Before you start, please ensure you have the following:
- CODESYS Control Win V3 x64 installed
- SLM-MX configured and connected to your network
- Basic knowledge of CODESYS Control Win V3 x64
- Basic knowledge of Structured Text programming
```

## **Project Files**
The CODESYS project file, as well as the SLM-MX configuration file can be found in the {download}`zip file here <codesys-demo.zip>`.

Please download and extract the zip file to an easily accessible directory.

## Hardware Configuration
![SLM-MX Configuration](/images/example-projects/codesys/codesys-modules.png)

There are two modules used:

Module 1: [SLM-RLY-16](/modules/discrete-output/SLM-RLY-16)

Module 2: [SLM-THM-4](/modules/analog-input/SLM-THM-4)

### Module Configuration
Inside the extracted zip file, there should be a file called THMDemo.slmmx. This is the SLM-MX configuration file to import into the SLM-MX Configurator.

Please refer to the [Importing Configuration Files](#import-config) page for instructions on how to import the configuration file.

## Software Configuration
The version of CODESYS used for this project is `3.5 SP20 Patch 5`. 

The target platform is a Windows 11 machine running CODESYS Control Win V3, essentially using the PC as the main controller with the SLM-MX as the remote I/O module. 

This also keeps the project easy to deploy to any PC with CODESYS installed.

### Importing the project
You can import the project by first extracting the {download}`zip file <codesys-demo.zip>` into a easily accessible directory.

Then, start CODESYS and create a new Project.

Select the Standard Project and save the project in any location you wish .

![Codesys New Project](/images/example-projects/codesys/codesys-setup.png)

Set the device type to CODESYS Control Win V3, and PLC_PRG as anything (Structured Text as default).

Next, on the device tree on the left, click on the project name at the top of the tree to highlight it, and select Project -> Import Project. 

Make sure to select the 'THMDEMO.export' file in the extracted zip file. With everything highlighted, press 'OK' to import the project.

```{figure} /images/example-projects/codesys/codesys-import-project.png
:align: left
:width: 80%
```

With the project imported, you should see the modules in the device tree.

```{sidebar} Device/Project Tree
![project-structure](/images/example-projects/codesys/codesys-project-structure.png)
```

### **Project Configuration**

#### Device (CODESYS Control Win V3 x64)
This refers to the device the project is being built for. In this case, it is a PC running CODESYS Control Win V3 x64. For more information on the CODESYS Control Win V3 x64, please refer to the [CODESYS Control Setup Guide](https://content.helpme-codesys.com/en/CODESYS%20Control/_rtsl_load_and_start_application_win.html) page.

Just make sure that your device is connected under the `Communication Settings` tab. 
![Codesys Device Configuration](/images/example-projects/codesys/codesys-device-config.png)

#### Ethernet
This is the network device that will be used to connect to the SLM-MX. It is vital that your Device is recognized so that you can select a `Network Interface`, which automatically populates the `IP Address` and `Subnet Mask`.
![Codesys Network Configuration](/images/example-projects/codesys/codesys-ethernet-config.png)

Underneath the Ethernet are the `Modbus_TCP_Client` and `Modbus_TCP_Server` items. The only thing to configure here is the Server IP address under the `Modbus_TCP_Server` item. This should be set to your SLM-MX IP address. 

### Modbus Mapping
The Modbus addresses for the SLM-MX have already been defined under Ethernet -> Modbus_TCP_Client -> Modbus_TCP_Server -> Modbus Server Channel. 

Channel | Name         | Access Type                        | Trigger     | READ Offset | Length | Error Handling | WRITE Offset | Length |
|---------|--------------|------------------------------------|-------------|-------------|--------|----------------|--------------|--------|
| 0       | RLY-16       | Write Multiple Coils (Function 15) | Application |             |        |                | 16#0000      | 16     |
| 1       | THM-4        | Read Input Registers (Function 04) | Application | 16#0000      | 8      | Keep last value |              |        |
| 2       | SLM-STATUS   | Read Holding Registers (Function 03) | Application | 16#00C3      | 2      | Keep last value |              |        |
| 3       | THM-4-STATUS | Read Input Registers (Function 04) | Application | 16#03E8      | 14     | Set to zero    |              |        |

```{admonition} Important Considerations
:class: warning

**Trigger Type:** All Modbus channels are triggered by the "Application."  **Avoid using cyclical triggers.** Cyclical triggers can lead to performance timing issues, potentially resulting in race conditions where data is not read or written accurately.

**READ and WRITE offsets:** Please note the READ and WRITE offsets are the offsets from the [modbus address table](/SLM-MX/configurator/modbus-addresses.md) for the SLM-MX.
```

#### I/O Mapping
The I/O mapping is done under Modbus_TCP_Server -> ModbusCPServer I/O Mapping.

![Codesys I/O Mapping](/images/example-projects/codesys/codesys-io-mapping.png)

Here, RLY-16[0] and RLY-16[1] are directly mapped to the PLC_PRG's Relay1 and Relay2 variables.

The other channels are not mapped and can be accessed through the IEEE format registers.\
For example for the THM-4, I can access the 32-bit float value of channel 1 with `THMCH1 AT %IW0 : REAL;`

## **Project Structure**

The CODESYS project implements a temperature monitoring system that controls relays based on temperature readings. The project is organized with the following components:

**Data Structures**
- [**GVL (Global Variables)**](#GVL): Contains shared variables for Modbus communication control, temperature values, and status information
- [**ModbusState_Type (ENUM)**](#Enum): Defines states for the Modbus communication state machine

**Function Blocks**
- [**FB_ModbusHandler (FB)**](#FB): Manages Modbus communication using a state machine

**Programs**
- [**ModbusRefresh (PRG)**](#ModbusRefresh): Handles Modbus communication cycles using multiple FB_ModbusHandler instances (10ms cycle)
- [**PLC_PRG (PRG)**](#PLC_PRG): Main program that processes temperature data and controls relay outputs (200ms cycle)
- [**StatusRefresh (PRG)**](#StatusRefresh): Updates THM registers and status registers (20ms cycle)

(GVL)=
### Global Variables (GVL)
The GVL contains the global variables for the project.
```{admonition} GVL Structured Text
:class: tip, dropdown
```{literalinclude} gvl.txt
:start-at: VAR
:end-at: END_VAR
```

#### Variable Explanation
- `EnableModbusWrite/Read/Status`: Boolean to enable/disable Modbus write/read/status operations from FB_ModbusHandler.
- `Write/Read/StatusChannelIndex`: Index of the write/read/status channel to be used.
- `THM_CH`: Array of words to store the temperature values.
- `THM_STATUS`: Array of words to store the temperature status values.

(Enum)=
### Enum (ModbusState_Type)
The ModbusState_Type enum is used to define the state of the Modbus communication.
```{admonition} ModbusState_Type Structured Text
:class: tip, dropdown
```{literalinclude} modbusState.txt
:start-at: TYPE
:end-at: END_TYPE
```
#### States:
- `Idle`
- `StartOperation`
- `WaitForCompletion`
- `ErrorState`

These states are used in the FB_ModbusHandler state machine to determine the current state of the Modbus communication.
(FB)=
### Function Blocks (FB)
The FB_ModbusHandler is a function block that handles the Modbus communication and organizes it as a state machine.
``````{admonition} FB_ModbusHandler Structured Text
:class: tip, dropdown
`````{tab-set}

````{tab-item} Variables
```{literalinclude} fb_modbus_code.txt
:start-at: VAR
:end-at: END_VAR
```
````

````{tab-item} Function Code
```{literalinclude} fb_modbus_code.txt
:start-at: CASE
:end-at: END_CASE
```
````

`````
``````
#### Explanation and Usage
The FB_ModbusHandler function block simplifies Modbus communication by using a state machine approach, enabling multiple operations to be performed in parallel.

- **Inputs**: `xEnable` (starts an operation) and `iChannelIndex` (selects the Modbus channel)
- **Outputs**: `xBusy`, `xDone`, `xError`, and `ModbusError` for operation status tracking

The state machine moves through four states:
1. **Idle**: Waits for the enable signal
2. **StartOperation**: Begins the Modbus operation
3. **WaitForCompletion**: Monitors until operation finishes
4. **ErrorState**: Handles errors before returning to Idle

Basic usage:
```
// Create and call the function block
MyModbusHandler : FB_ModbusHandler;
MyModbusHandler(xEnable := TRUE, iChannelIndex := 1);

// Check results
IF MyModbusHandler.xDone THEN
    // Process data after successful completion
ELSIF MyModbusHandler.xError THEN
    // Handle error
END_IF
```

In this project, three separate instances handle different Modbus channels independently.

### Tasks
(ModbusRefresh)=
#### ModbusRefresh
`Priority`: 0\
`Cycle Time`: 10ms

``````{admonition} ModbusRefresh Structured Text
:class: tip, dropdown
`````{tab-set}

````{tab-item} Variables
```{literalinclude} modbusRefresh.txt
:lines: 1-6
```
````

````{tab-item} Function Code
```{literalinclude} modbusRefresh.txt
:lines: 8-33
```
````

`````
``````

This task creates three instances of the FB_ModbusHandler, one for each operation. This allows for parallel operations to be performed without blocking the main program, and the state machine of FB_ModbusHandler prevents any race conditions by waiting until the previous operation is complete.

This also resets the Enable Global Variables to FALSE when the operation is complete. 

This task refreshes every 10ms to ensure the Modbus communications are synced up properly.

(PLC_PRG)=
#### PLC_PRG
`Priority`: 1\
`Cycle Time`: 200ms

``````{admonition} PLC_PRG Structured Text
:class: tip, dropdown
`````{tab-set}

 ````{tab-item} Variables
```{literalinclude} plc_prg.txt
:lines: 1-29
```
````

````{tab-item} Function Code
```{literalinclude} plc_prg.txt
:lines: 31-78
```
````

`````
``````

This task gets the temperature from THMCH1 AT %IW0 and stores it as a REAL, converting it from the IEEE-754 format to a REAL floating point number.

It maps the temperature to a percentage of the temperature range (75°F - 125°F), and then calculates how many relays should be active based on the percentage.

It first maps it to a 0-15 array, and splits this into two bytes to assign to Relay1 and Relay2 that are directly mapped to the SLM-RLY-16 module.

(StatusRefresh)=
#### StatusRefresh
`Priority`: 2\
`Cycle Time`: 20ms

````{admonition} StatusRefresh Structured Text
:class: tip, dropdown
```
// Read Modbus Registers
GVL.EnableModbusStatus := TRUE;
GVL.EnableModbusRead := TRUE;
```
````
This simply refreshes the values of the THM registers and the status registers every 20ms.



