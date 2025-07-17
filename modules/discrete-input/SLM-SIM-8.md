# SLM-SIM-8

`````{margin}
````{card}
<div style="text-align: center"><b>SLM-SIM-8</b></div>
^^^
```{image} /images/modules/SLM-SIM-8.png
:alt: SLM-SIM-8
:width: 70%
:align: center
```
---
+++
**Channels**: 9\
**Discrete Input Type**: Toggle Switch\
**Register Type**: Discrete Input/Input Status
+++
**Specifications**:\
{download}`Download SLM-SIM-8 Datasheet </docs/SLM-SIM-8-DS_RevXA_07302024.pdf>`
````
`````

## Description

- **Input Simulator** with **toggle switch** inputs.  
- **8 input points** for industrial control applications.  
- **Compatible with Synergy Logic systems** .

---
## Configuration

- No configuration required

---

## Modbus Mapping

```{note}
The following register mappings are based on a single-module configuration. Actual mappings may differ based on the physical slot arrangement in your system.
```

### Modbus Register Mapping

| Channel | Modbus Input Status Register | Access    |
|:-------:|:----------------------------:|:---------:|
| **CH1**  | **10001**  | **Read-Only** |
| **CH2**  | **10002**  | **Read-Only** |
| **CH3**  | **10003**  | **Read-Only** |
| **CH4**  | **10004**  | **Read-Only** |
| **CH5**  | **10005**  | **Read-Only** |
| **CH6**  | **10006**  | **Read-Only** |
| **CH7**  | **10007**  | **Read-Only** |
| **CH8**  | **10008**  | **Read-Only** |

**Notes:**
- There is a **one-to-one mapping** between **channels and registers** (e.g., CH1 → 10001, CH2 → 10002, etc.).

---

## Specifications

[Download SLM-SIM-8 Datasheet](/docs/SLM-SIM-8-DS_RevXA_07302024.pdf)

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

value = client.read_discrete_inputs(address = 0, count = 16) # Read all 16 inputs
print(value)
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
int startAddress = 0;     // The starting address of the inputs
int numInputs = 16;         // The number of inputs to read (0-15)

enum modbusType {
    INPUT_STATUS,
    INPUT_REGISTER,
    HOLDING_REGISTER,
    COIL
};

void setup() {
    # Setup modbus client
}

void loop() {
    uint16_t inputs[numInputs];
    ModbusRTUClient.requestFrom(slaveId, INPUT_STATUS, startAddress, numInputs);
    for (int i = 0; i < numInputs; i++) {
        inputs[i] = ModbusRTUClient.read();
    }
    for (int i = 0; i < numInputs; i++) {
        Serial.println(inputs[i]);
    }
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
![Codesys Modbus Server Channel](/images/modules/example/DI-codesys-config.png)
Under Modbus_TCP_Server -> Modbus Server Channel -> I/O Mapping
![Codesys Modbus Server I/O Mapping](/images/modules/example/DI-codesys-mapping.png)
**Usage**
```
VAR
    DCI16_1 AT %IB0 : BYTE;
    DCI16_2 AT %IB1 : BYTE;
    DCICH1 AT IX0.0 : BOOL;
    DCICH2 AT IX0.1 : BOOL;
    DCICH3 AT IX0.2 : BOOL;
    DCICH4 AT IX0.3 : BOOL;
    DCICH5 AT IX0.4 : BOOL;
    DCICH6 AT IX0.5 : BOOL;
    DCICH7 AT IX0.6 : BOOL;
    DCICH8 AT IX0.7 : BOOL;
    DCICH9 AT IX1.0 : BOOL;
    DCICH10 AT IX1.1 : BOOL;
    DCICH11 AT IX1.2 : BOOL;
    DCICH12 AT IX1.3 : BOOL;
    DCICH13 AT IX1.4 : BOOL;
    DCICH14 AT IX1.5 : BOOL;
    DCICH15 AT IX1.6 : BOOL;
    DCICH16 AT IX1.7 : BOOL;
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
| DI1 | Boolean |
| DI2 | Boolean |
| DI3 | Boolean |
| DI4 | Boolean |
| DI5 | Boolean |
| DI6 | Boolean |
| DI7 | Boolean |
| DI8 | Boolean |
| DI9 | Boolean |
| DI10 | Boolean |
| DI11 | Boolean |
| DI12 | Boolean |
| DI13 | Boolean |
| DI14 | Boolean |
| DI15 | Boolean |
| DI16 | Boolean |

**Usage**

![Productivity Suite Modbus Config](/images/modules/example/DI-prod-config.png)   
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