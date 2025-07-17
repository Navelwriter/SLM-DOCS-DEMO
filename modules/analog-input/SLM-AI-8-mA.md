# SLM-AI-8-mA
`````{margin}
````{card}
<div style="text-align: center"><b>SLM-AI-8-mA</b></div>
^^^
```{image} /images/modules/SLM-AI-8-mA.png
:alt: SLM-AI-8-mA
:width: 70%
:align: center
```
---
+++
**Channels**: 8\
**Input Range**: 0-20mA\
**Resolution**: 13-bit (0-8191)\
**Register Type**: Analog Input/Input Registers
+++
**Specifications**:\
{download}`Download SLM-AI-8-mA Datasheet </docs/SLM-AI-8-mA-DS_RevXA1_03262025.pdf>`
````
`````

## Description

- **8-channel 0-20mA** input module.  
- **13-bit Resolution** for precision control
- **Designed for industrial control applications**.  

---
## Configuration

```{image} /images/modules/config/SLM-AI-8-mA-config.png
:alt: SLM-AI-8-mA Config Menu
:width: 70%
```

```{admonition} Channel Select
:class: note, dropdown

Enable or disable the channels to be read. The first channel cannot be disabled.
```

---

## Modbus Mapping
```{note}
The following register mappings assumes that it is the only module connected. Actual mapping may vary depending on the configuration of the system.
```

### Modbus Register Mapping

| Channel  | Modbus Register | Access      | Register Type | Data Length |
|:--------:|:--------------:|:----------:|:-------------:|:----------:|
| **Ch1**  | **30001**      | **Read-Only** | **Input Register** | 16-bit |
| **Ch2**  | **30002**      | **Read-Only** | **Input Register** | 16-bit |
| **Ch3**  | **30003**      | **Read-Only** | **Input Register** | 16-bit |
| **Ch4**  | **30004**      | **Read-Only** | **Input Register** | 16-bit |
| **Ch5**  | **30005**      | **Read-Only** | **Input Register** | 16-bit |
| **Ch6**  | **30006**      | **Read-Only** | **Input Register** | 16-bit |
| **Ch7**  | **30007**      | **Read-Only** | **Input Register** | 16-bit |
| **Ch8**  | **30008**      | **Read-Only** | **Input Register** | 16-bit |

**Notes:**
- Each **Modbus Holding Register** corresponds to a **single analog input channel**.
- **0-20mA inputs*** are **read-only** and mapped to **Modbus Input Registers (30001 - 30008)**.
---
### Status Register Mapping
**Note:** The status registers are read-only booleans and are mapped to **Discrete Input Status** registers (101001 - 101018).
| Status  | Modbus Register | Access      | Register Type | Data Length |
|:--------:|:--------------:|:----------:|:-------------:|:----------:|
| **Module diagnostics failure**  | **101001**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **24V Missing**  | **101002**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch1 Under Range**  | **101003**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch2 Under Range**  | **101004**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch3 Under Range**  | **101005**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch4 Under Range**  | **101006**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch5 Under Range**  | **101007**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch6 Under Range**  | **101008**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch7 Under Range**  | **101009**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch8 Under Range**  | **101010**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch1 Over Range**  | **101011**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch2 Over Range**  | **101012**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch3 Over Range**  | **101013**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch4 Over Range**  | **101014**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch5 Over Range**  | **101015**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch6 Over Range**  | **101016**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch7 Over Range**  | **101017**      | **Read-Only** | **Discrete Input Status** | 1-bit |
| **Ch8 Over Range**  | **101018**      | **Read-Only** | **Discrete Input Status** | 1-bit |

---

# Example Usage
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
    ch_raw = client.read_input_registers(0, 2)
    # Convert 0-8191 to 0-20mA
    # Outputs as integer 0-8191 so simple conversion to float is possible
    ch1_mA = ch_raw[0] * (20 / 8192)
    ch2_mA = ch_raw[1] * (20 / 8192)
    print(f"Channel 1 Raw: {ch_raw[0]} mA")
    print(f"Channel 2 mA: {ch2_mA} mA")
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
int startAddress = 0;     // The starting address of the input registers
int numInputs = 1;         // The number of input registers to read (0-7)

void setup() {
    # Setup modbus client
}

void loop() {
    float currentInput_mA = readCurrentInput();
    Serial.println(currentInput_mA);
    delay(1000);
}

float readCurrentInput() {
  ModbusRTUClient.requestFrom(slaveId, INPUT_REGISTERS, startAddress, numInputs);
  int16_t currentInput = ModbusRTUClient.read();
  float currentInput_mA = currentInput * (20 / 8192);
  return currentInput_mA;
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
![Codesys Modbus Server Channel](/images/modules/example/AI-codesys-config.png)
Under Modbus_TCP_Server -> Modbus Server Channel -> I/O Mapping
![Codesys Modbus Server I/O Mapping](/images/modules/example/AI-codesys-mapping.png)
**Usage**
```
VAR
    AI1 AT %IW0 : WORD;
    AI2 AT %IW1 : WORD;
    AI3 AT %IW2 : WORD;
    AI4 AT %IW3 : WORD;
    AI5 AT %IW4 : WORD;
    AI6 AT %IW5 : WORD;
    AI7 AT %IW6 : WORD;
    AI8 AT %IW7 : WORD;
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
| AI1 | Integer, 16 Bit |
| AI2 | Integer, 16 Bit |
| AI3 | Integer, 16 Bit |
| AI4 | Integer, 16 Bit |
| AI5 | Integer, 16 Bit |
| AI6 | Integer, 16 Bit |
| AI7 | Integer, 16 Bit |
| AI8 | Integer, 16 Bit |

**Usage**

![Productivity Suite Modbus Config](/images/modules/example/AI-prod-config.png)   
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Productivity Suite**, visit **[Productivity Suite Example](/SLM-MX/example-projects/productivity/productivity-demo.md)**.
```
````

`````

---

## Specifications

[Download SLM-AI-8-mA Datasheet](/docs/SLM-AI-8-mA-DS_RevXA1_03262025.pdf)

## Safety Precautions
- Do not **add or remove** modules while field power is applied.
- Ensure proper grounding and wire connections to prevent electrical hazards.
- Follow all **local and national electrical codes**.
- Maintain **adequate ventilation** to prevent overheating.

For more details and technical support, visit **[Synergy Logic](https://www.synergy-logic.com)** or contact **support@synergy-logic.com**.

