# SLM-RLY-8-ISO
`````{margin}
````{card}
<div style="text-align: center"><b>SLM-RLY-8-ISO</b></div>
^^^
```{image} /images/modules/SLM-RLY-8-ISO.png
:alt: SLM-RLY-8-ISO
:width: 70%
:align: center
```
---
+++
**Channels**: 8\
**Discrete Output Type**: 6 Relays - FORM A (SPST), 2 Relays - FORM B (SPDT)\
**Rated Voltage**: 6-24 VDC, 6-120 VAC\
**Register Type**: Discrete Output/Coil
+++
**Specifications**:\
{download}`Download SLM-RLY-8-ISO Datasheet </docs/SLM-RLY-8-ISO-DS_RevXA_09132024.pdf>`
````
`````
## Description

- **8-channel relay output module with per-channel isolation**.   
- **Supports 120/240VAC or 24VDC relay switching**.  
- **Designed for industrial control applications requiring independent relay outputs**.  
- Each relay output is **individually isolated**, ensuring electrical separation between channels.
- Supports **both AC and DC loads**, providing flexibility in industrial applications.
- Channels 1 and 5 are **FORM C** relays, meaning that they have both a normally closed and normally open contact.
- Channels 2, 3, 4, 6, 7, and 8 are **FORM A**, or normally open, relays.

---
## Configuration

- No configuration required

---

## Typical Modbus Register Mapping
```{note}
The following examples assume that it is the only module connected. Actual mapping may vary depending on the configuration of the system.
```
| Channel  | Modbus Register | Relay Form | Access      |
|:--------:|:--------------:|:--------------:|:----------:|
| **NC1/NO1** | **00001**      | **FORM C**         | **Read/Write** |
| **NO2** | **00002**      | **FORM A**         | **Read/Write** |
| **NO3** | **00003**      | **FORM A**         | **Read/Write** |
| **NO4** | **00004**      | **FORM A**         | **Read/Write** |
| **NC5/NO5** | **00005**      | **FORM C**         | **Read/Write** |
| **NO6** | **00006**      | **FORM A**         | **Read/Write** |
| **NO7** | **00007**      | **FORM A**         | **Read/Write** |
| **NO8** | **00008**      | **FORM A**         | **Read/Write** |

**Notes:**
- **Outputs (OUT1-OUT8)** are **read/write** and correspond to **Modbus Coil Registers (00001 - 00008)**.
---

## Example Usage
```{note}
The following examples assume that it is the only module connected. Actual mapping may vary depending on the configuration of the system.
```

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
    client.write_multiple_coils(0, [True, False, True, False, True, False, True, False]); # Alternate values to coils 0-7
    time.sleep(1)
```
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Python**, visit **[Python Example](/SLM-MX/example-projects/python/python-demo.md)**.
```
````

````{tab-item} Arduino
```arduino
// arduino code here
#include <ArduinoModbus.h> // Include Modbus library

// Assume ModbusRTUClient is already initialized (e.g., ModbusRTUClient.begin(...))
int slaveId = 0x01;       // The ID of the Modbus slave device
int startAddress = 0;     // The starting address of the coils
int numCoils = 8;         // The number of coils to write (0-7)
int coilValues[] = {1, 0, 1, 0, 1, 0, 1, 0}; // Values for coils 0 through 7

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
    RLY8 AT %QB0 : BYTE;
    RLYCH1 AT QX0.0 : BOOL;
    RLYCH2 AT QX0.1 : BOOL;
    RLYCH3 AT QX0.2 : BOOL;
    RLYCH4 AT QX0.3 : BOOL;
    RLYCH5 AT QX0.4 : BOOL;
    RLYCH6 AT QX0.5 : BOOL;
    RLYCH7 AT QX0.6 : BOOL;
    RLYCH8 AT QX0.7 : BOOL;
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

**Usage**

![Productivity Suite Modbus Config](/images/modules/example/DO-prod-config.png)   
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Productivity Suite**, visit **[Productivity Suite Example](/SLM-MX/example-projects/productivity/productivity-demo.md)**.
```
````

`````

---

## Specifications

[Download SLM-RLY-8-ISO Datasheet](/docs/SLM-RLY-8-ISO-DS_RevXA_09132024.pdf)

## Safety Precautions
- Do not **add or remove** modules while field power is applied.
- Ensure proper grounding and wire connections to prevent electrical hazards.
- Follow all **local and national electrical codes**.
- Maintain **adequate ventilation** to prevent overheating.

For more details and technical support, visit **[Synergy Logic](https://www.synergy-logic.com)** or contact **support@synergy-logic.com**.