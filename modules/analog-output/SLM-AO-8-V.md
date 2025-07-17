# SLM-AO-8-V

`````{margin}
````{card}
<div style="text-align: center"><b>SLM-AO-8-V</b></div>
^^^
```{image} /images/modules/SLM-AO-8-V.png
:alt: SLM-AO-8-V
:width: 70%
:align: center
```
---
+++
**Channels**: 8\
**Output Range**: 0-10V\
**Resolution**: 13-bit (0-8191)\
**Register Type**: Analog Output/Holding Registers
+++
**Specifications**:\
{download}`Download SLM-AO-8-V Datasheet </docs/SLM-AO-8-V-DS_RevXA1_03262025.pdf>`
````
`````

## Description

- **8-channel 0-10V** output module.  
- **13-bit Resolution** for precision control
- **Designed for industrial control applications**.  


---
## Configuration

```{image} /images/modules/config/SLM-AO-8-V-config.png
:alt: SLM-AO-8-V Config Menu
:width: 70%
```

```{admonition} Set Analog Timeout Value
:class: note, dropdown

During a communication failure and the watchdog timer is triggered, the module will output the value set in the **Analog Timeout Value** field.

See **[Watchdog Timer](modbus-watchdog)** for more information.
```

---

## Modbus Mapping
```{note}
The following register mappings assumes that it is the only module connected. Actual mapping may vary depending on the configuration of the system.
```

### Modbus Register Mapping

| Channel  | Modbus Register | Access      | Register Type | Data Length |
|:--------:|:--------------:|:----------:|:-------------:|:----------:|
| **Ch1**  | **40001**      | **Read/Write** | **Holding Register** | 16-bit |
| **Ch2**  | **40002**      | **Read/Write** | **Holding Register** | 16-bit |
| **Ch3**  | **40003**      | **Read/Write** | **Holding Register** | 16-bit |
| **Ch4**  | **40004**      | **Read/Write** | **Holding Register** | 16-bit |
| **Ch5**  | **40005**      | **Read/Write** | **Holding Register** | 16-bit |
| **Ch6**  | **40006**      | **Read/Write** | **Holding Register** | 16-bit |
| **Ch7**  | **40007**      | **Read/Write** | **Holding Register** | 16-bit |
| **Ch8**  | **40008**      | **Read/Write** | **Holding Register** | 16-bit |

**Notes:**
- Each **Modbus Holding Register** corresponds to a **single analog output channel**.
- **0-10V outputs** are **read/write** and mapped to **Modbus Holding Registers (40001 - 40008)**.
- Values range from **0-8191** corresponding to **0-10V** output.

---
### Status Register Mapping
**Note:** The status registers are read-only booleans and are mapped to **Discrete Input Status** registers (101001 - 101002).
| Status  | Modbus Register | Access      | Register Type | Data Length |
|:--------:|:--------------:|:----------:|:-------------:|:----------:|
| **Module diagnostics failure**  | **101001**      | **Read-Only** | **Input Register** | 1-bit |
| **24V Missing**  | **101002**      | **Read-Only** | **Input Register** | 1-bit |

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

# Convert 0-10V to 0-8191
def V_to_raw(V):
    return int(V * (8191 / 10))

# Convert 0-8191 to 0-10V
def raw_to_V(raw):
    return raw * (10 / 8191)

# Set output to 5V (mid-range)
raw_value = V_to_raw(5)
client.write_single_register(0, raw_value)

# Read current output value
current_raw = client.read_holding_registers(0, 1)[0]
current_V = raw_to_V(current_raw)
print(f"Current output: {current_V} V")
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
int startAddress = 0;     // The starting address of the holding registers
int numRegisters = 8;     // The number of registers to read/write

void setup() {
    # Setup modbus client
}

void loop() {
    // Convert 0-10V to 0-8191
    int rawValue = map(5, 0, 10, 0, 8191);  // Set to 5V
    
    // Write to output
    ModbusRTUClient.beginTransmission(slaveId, HOLDING_REGISTERS, startAddress, 1);
    ModbusRTUClient.write(rawValue);
    ModbusRTUClient.endTransmission();
    
    // Read current value
    ModbusRTUClient.requestFrom(slaveId, HOLDING_REGISTERS, startAddress, 1);
    int currentRaw = ModbusRTUClient.read();
    float current_V = map(currentRaw, 0, 8191, 0, 10);
    
    delay(1000);
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
![Codesys Modbus Server Channel](/images/modules/example/AO-codesys-config.png)
Under Modbus_TCP_Server -> Modbus Server Channel -> I/O Mapping
![Codesys Modbus Server I/O Mapping](/images/modules/example/AO-codesys-mapping.png)
**Usage**
```
VAR
    AO1 AT %QW0 : WORD;
    AO2 AT %QW1 : WORD;
    AO3 AT %QW2 : WORD;
    AO4 AT %QW3 : WORD;
    AO5 AT %QW4 : WORD;
    AO6 AT %QW5 : WORD;
    AO7 AT %QW6 : WORD;
    AO8 AT %QW7 : WORD;
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
| AO1 | Integer, 16 Bit |
| AO2 | Integer, 16 Bit |
| AO3 | Integer, 16 Bit |
| AO4 | Integer, 16 Bit |
| AO5 | Integer, 16 Bit |
| AO6 | Integer, 16 Bit |
| AO7 | Integer, 16 Bit |
| AO8 | Integer, 16 Bit |

**Usage**

![Productivity Suite Modbus Config](/images/modules/example/AO-prod-config.png)   
```{note}
This is an explanatory section on how to read/write to a module. For a more complete example using **Productivity Suite**, visit **[Productivity Suite Example](/SLM-MX/example-projects/productivity/productivity-demo.md)**.
```
````

`````

---

## Specifications

[Download SLM-AO-8-V Datasheet](/docs/SLM-AO-8-V-DS_RevXA1_03262025.pdf)

## Safety Precautions
- Do not **add or remove** modules while field power is applied.
- Ensure proper grounding and wire connections to prevent electrical hazards.
- Follow all **local and national electrical codes**.
- Maintain **adequate ventilation** to prevent overheating.

For more details and technical support, visit **[Synergy Logic](https://www.synergy-logic.com)** or contact **support@synergy-logic.com**.