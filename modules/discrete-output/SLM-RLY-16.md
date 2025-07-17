# SLM-RLY-16
`````{margin}
````{card}
<div style="text-align: center"><b>SLM-RLY-16</b></div>
^^^
```{image} /images/modules/SLM-RLY-16.png
:alt: SLM-RLY-16
:width: 70%
:align: center
```
---
+++
**Channels**: 16\
**Discrete Output Type**: Relay FORM A (SPST)\
**Rated Voltage**: 6.25-30 VDC, 6-120 VAC
**Register Type**: Discrete Output/Coil
+++
**Specifications**:\
{download}`Download SLM-RLY-16 Datasheet </docs/SLM-RLY-16-DS_RevXA_08232024.pdf>`
````
`````

## Description

- **16-channel relay output module**.  
- **Two common terminals (C1 and C2) for grouped relay control**.  
- **Supports 6.25-30 VDC or 6-120 VAC relay switching**.  
- Two common terminals (**C1 and C2**) allow grouping of relay outputs.
- Supports **both AC and DC loads**, providing flexibility in industrial applications.

---
## Configuration

- No configuration required

## Modbus Mapping
```{note}
The following register mappings assumes that it is the only module connected. Actual mapping may vary depending on the configuration of the system.
```

### Modbus Register Mapping

| Channel  | Modbus Register | Common Terminal | Access      |
|:--------:|:--------------:|:--------------:|:----------:|
| **OUT1** | **00001**      | **C1**         | **Read/Write** |
| **OUT2** | **00002**      | **C1**         | **Read/Write** |
| **OUT3** | **00003**      | **C1**         | **Read/Write** |
| **OUT4** | **00004**      | **C1**         | **Read/Write** |
| **OUT5** | **00005**      | **C1**         | **Read/Write** |
| **OUT6** | **00006**      | **C1**         | **Read/Write** |
| **OUT7** | **00007**      | **C1**         | **Read/Write** |
| **OUT8** | **00008**      | **C1**         | **Read/Write** |
| **OUT9** | **00009**      | **C2**         | **Read/Write** |
| **OUT10** | **00010**      | **C2**         | **Read/Write** |
| **OUT11** | **00011**      | **C2**         | **Read/Write** |
| **OUT12** | **00012**      | **C2**         | **Read/Write** |
| **OUT13** | **00013**      | **C2**         | **Read/Write** |
| **OUT14** | **00014**      | **C2**         | **Read/Write** |
| **OUT15** | **00015**      | **C2**         | **Read/Write** |
| **OUT16** | **00016**      | **C2**         | **Read/Write** |

**Notes:**
- **Outputs (OUT1-OUT16)** are **read/write** and correspond to **Modbus Coil Registers (00001 - 00016)**.

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
    client.write_multiple_coils(0, [True, False, True, False, True, False, True, False, True, False, True, False, True, False, True, False]); # Alternate values to coils 0-15
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
int numCoils = 16;         // The number of coils to write (0-15)
int coilValues[] = {1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0}; // Values for coils 0 through 15

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
    RLY16_1 AT %QB0 : BYTE;
    RLY16_2 AT %QB1 : BYTE;
    RLYCH1 AT QX0.0 : BOOL;
    RLYCH2 AT QX0.1 : BOOL;
    RLYCH3 AT QX0.2 : BOOL;
    RLYCH4 AT QX0.3 : BOOL;
    RLYCH5 AT QX0.4 : BOOL;
    RLYCH6 AT QX0.5 : BOOL;
    RLYCH7 AT QX0.6 : BOOL;
    RLYCH8 AT QX0.7 : BOOL;
    RLYCH9 AT QX1.0 : BOOL;
    RLYCH10 AT QX1.1 : BOOL;
    RLYCH11 AT QX1.2 : BOOL;
    RLYCH12 AT QX1.3 : BOOL;
    RLYCH13 AT QX1.4 : BOOL;
    RLYCH14 AT QX1.5 : BOOL;
    RLYCH15 AT QX1.6 : BOOL;
    RLYCH16 AT QX1.7 : BOOL;
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
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Productivity Suite**, visit **[Productivity Suite Example](/SLM-MX/example-projects/productivity/productivity-demo.md)**.
```
````

`````

---

## Specifications

[Download SLM-RLY-16 Datasheet](/docs/SLM-RLY-16-DS_RevXA_08232024.pdf)

## Safety Precautions
- Do not **add or remove** modules while field power is applied.
- Ensure proper grounding and wire connections to prevent electrical hazards.
- Follow all **local and national electrical codes**.
- Maintain **adequate ventilation** to prevent overheating.

For more details and technical support, visit **[Synergy Logic](https://www.synergy-logic.com)** or contact **support@synergy-logic.com**.