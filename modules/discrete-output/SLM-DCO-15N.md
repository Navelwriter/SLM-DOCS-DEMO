# SLM-DCO-15N
`````{margin}
````{card}
<div style="text-align: center"><b>SLM-DCO-15N</b></div>
^^^
```{image} /images/modules/SLM-DCO-15N.png
:alt: SLM-DCO-15N
:width: 70%
:align: center
```
+++
**Channels**: 15\
**Voltage Ranges**: 3.3-24 VDC\ 
**Output Type**: Sink\
**Data Type**: Binary, each channel has its own input-status register
+++
**Specifications**:\
{download}`Download SLM-DCO-15N Datasheet </docs/SLM-DCO-15N-DS_RevXA1_03262025.pdf>`
````
`````

## Description

- **15-channel 24VDC sinking discrete outputs**.  
- Designed for **industrial control applications** requiring **multiple DC outputs**.  
- Outputs use **sinking (NPN) transistor switching**.

---
## Configuration

- No configuration required

## Typical Modbus Register Mapping

| Channel  | Modbus Register | Access      |
|:--------:|:--------------:|:----------:|
| **CH1**  | **00001**      | **Read/Write** |
| **CH2**  | **00002**      | **Read/Write** |
| **CH3**  | **00003**      | **Read/Write** |
| **CH4**  | **00004**      | **Read/Write** |
| **CH5**  | **00005**      | **Read/Write** |
| **CH6**  | **00006**      | **Read/Write** |
| **CH7**  | **00007**      | **Read/Write** |
| **CH8**  | **00008**      | **Read/Write** |
| **CH9**  | **00009**      | **Read/Write** |
| **CH10** | **00010**      | **Read/Write** |
| **CH11** | **00011**      | **Read/Write** |
| **CH12** | **00012**      | **Read/Write** |
| **CH13** | **00013**      | **Read/Write** |
| **CH14** | **00014**      | **Read/Write** |
| **CH15** | **00015**      | **Read/Write** |

**Notes:**
- **Outputs (CH1-CH15)** are **read/write** and correspond to **Modbus Coil Registers (00001 - 00015)**.
- What should be **Output (CH16)** is reserved, but not used in this 15 channel module.

---

## Specifications

[Download SLM-DCO-15N Datasheet](/docs/SLM-DCO-15N-DS_RevXA1_03262025.pdf)

## Example Usage
```{note}
The following examples assume that it is the only module connected. Actual mapping may vary depending on the configuration of the system.
```

### Getting Example Readings
`````{tab-set}

````{tab-item} Python
```python
import time
from pyModbusTCP.client import ModbusClient
import struct

client_ip = '192.168.1.255'  # Replace with your device's IP
client = ModbusClient(host=client_ip, port=502, auto_open=True)

while True:
    client.write_single_coil(3, True); # Write 1 to coil address 3 (channel 4)
    time.sleep(1)
    client.write_multiple_coils(0, [True, False, True, False, True, False, True, False, True, False, True, False, True, False, True]); # Alternate values to coils 0-14
    time.sleep(1)
```
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Python**, visit **[Python Example](/SLM-MX/example-projects/python/python-demo.md)**.
```
````

````{tab-item} Arduino
```arduino
#include <ArduinoModbus.h> // Include Modbus library

// Assume ModbusRTUClient is already initialized (e.g., ModbusRTUClient.begin(...))
int slaveId = 0x01;       // The ID of the Modbus slave device
int startAddress = 0;     // The starting address of the coils
int numCoils = 15;         // The number of coils to write (0-14)
int coilValues[] = {1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1}; // Values for coils 0 through 14

void setup() {
    # Setup modbus client
}

void loop() {
    writeMultipleCoilsPattern_BeginWriteEnd();
    delay(1000);
}

void writeMultipleCoilsPattern_BeginWriteEnd() {
  // Start writing multiple coils
  ModbusRTUClient.beginTransmission(slaveId, COILS, startAddress, numCoils);

  // Write the pattern 1, 0, 1, 0, 1, 0, 1, 0 for coils 0-7
  for (int i = 0; i < numCoils; i++) {
    // Write 1 for even coils (0, 2, 4, 6), 0 for odd coils (1, 3, 5, 7)
    ModbusRTUClient.write(coilValues[i]);
  }

  // Send the write request
  ModbusRTUClient.endTransmission();
}
```
```{note}
The above code uses the [ArduinoModbus library](https://docs.arduino.cc/learn/communication/modbus/) and is not a complete program. 
```
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Arduino**, visit **[Arduino Example](/SLM-MX/example-projects/arduino/arduino-demo.md)**.
```
````

````{tab-item} Codesys
Under Modbus_TCP_Server -> Modbus Server Channel 
![Codesys Modbus Server Channel](/images/modules/example/DO-codesys-config.png)
Under Modbus_TCP_Server -> Modbus Server Channel -> I/O Mapping
![Codesys Modbus Server I/O Mapping](/images/modules/example/DO-codesys-mapping.png)
**Usage**
```
VAR
    DCO15_1 AT %QB0 : BYTE;
    DCO15_2 AT %QB1 : BYTE;
    DCOCH1 AT QX0.0 : BOOL;
    DCOCH2 AT QX0.1 : BOOL;
    DCOCH3 AT QX0.2 : BOOL;
    DCOCH4 AT QX0.3 : BOOL;
    DCOCH5 AT QX0.4 : BOOL;
    DCOCH6 AT QX0.5 : BOOL;
    DCOCH7 AT QX0.6 : BOOL;
    DCOCH8 AT QX0.7 : BOOL;
    DCOCH9 AT QX1.0 : BOOL;
    DCOCH10 AT QX1.1 : BOOL;
    DCOCH11 AT QX1.2 : BOOL;
    DCOCH12 AT QX1.3 : BOOL;
    DCOCH13 AT QX1.4 : BOOL;
    DCOCH14 AT QX1.5 : BOOL;
END_VAR
```
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Codesys**, visit **[Codesys Example](/SLM-MX/example-projects/codesys/codesys-demo.md)**.
```
````

````{tab-item} Productivity Suite
**Tag**
| Tag Name | Type |
|:---------:|:----:|
| DO1 | Boolean |
| DO2 | Boolean |
| DO3 | Boolean |
| DO4 | Boolean |
| DO5 | Boolean |
| DO6 | Boolean |
| DO7 | Boolean |
| DO8 | Boolean |
| DO9 | Boolean |
| DO10 | Boolean |
| DO11 | Boolean |
| DO12 | Boolean |
| DO13 | Boolean |
| DO14 | Boolean |
| DO15 | Boolean |

**Usage**

![Productivity Suite Modbus Config](/images/modules/example/DO-prod-config.png)   
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Productivity Suite**, visit **[Productivity Suite Example](/SLM-MX/example-projects/productivity/productivity-demo.md)**.
```
````

`````

## Safety Precautions
- Do not **add or remove** modules while field power is applied.
- Ensure proper grounding and wire connections to prevent electrical hazards.
- Follow all **local and national electrical codes**.
- Maintain **adequate ventilation** to prevent overheating.

For more details and technical support, visit **[Synergy Logic](https://www.synergy-logic.com)** or contact **support@synergy-logic.com**.