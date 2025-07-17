(modules-index)=
# SLM I/O Modules Overview

Welcome to the SLM I/O Modules documentation. This section provides information about Synergy Logic Micro (SLM) modules organized by function type. Click module links below for specifications, wiring diagrams, and configuration details.

## Module Reference Table

``````{tab-set}
`````{tab-item} Discrete Modules

````{tab-set}
```{tab-item} <span style="color:#4fc3f7;">Input Modules</span>
| Module | Description | Channels | Signal Type |
|--------|-------------|:--------:|-------------|
| [SLM-ACI-8](discrete-input/SLM-ACI-8) | AC Current Input Module | 8 | AC Current |
| [SLM-DCI-16NP](discrete-input/SLM-DCI-16NP) | DC Input Module | 16 | DC |
| [SLM-ACDCI-16NP](discrete-input/SLM-ACDCI-16NP) | AC/DC Input Module | 16 | AC/DC |
| [SLM-SIM-8](discrete-input/SLM-SIM-8) | Input Simulator Module | 8 | Toggle Switch |
```

```{tab-item} <span style="color:#ffb6b6;">Output Modules</span>
| Module | Description | Channels | Output Type |
|--------|-------------|:--------:|-------------|
| [SLM-DCO-15N](discrete-output/SLM-DCO-15N) | DC Output Module (Negative Logic) | 15 | DC (N) |
| [SLM-DCO-15P](discrete-output/SLM-DCO-15P) | DC Output Module (Positive Logic) | 15 | DC (P) |
| [SLM-RLY-16](discrete-output/SLM-RLY-16) | Relay Output Module | 16 | Relay |
| [SLM-RLY-8-ISO](discrete-output/SLM-RLY-8-ISO) | Isolated Relay Output Module | 8 | Isolated Relay |
```

```{tab-item} <span style="color:#d4b8ff;">Combo Modules</span>
| Module | Description | Input | Output |
|--------|-------------|-------|--------|
| [SLM-DCI-8NP-DCO-7N](discrete-combo/SLM-DCI-8NP-DCO-7N) | DC Input + DC Output | 8-Ch DC | 7-Ch DC (N) |
| [SLM-DCI-8NP-DCO-7P](discrete-combo/SLM-DCI-8NP-DCO-7P) | DC Input + DC Output | 8-Ch DC | 7-Ch DC (P) |
| [SLM-ACDCI-8NP-RLY8](discrete-combo/SLM-ACDCI-8NP-RLY8) | AC/DC Input + Relay Output | 8-Ch AC/DC | 8-Ch Relay |
```
````

`````

`````{tab-item} Analog Modules

````{tab-set}
```{tab-item} <span style="color:#4fc3f7;">Input Modules</span>
| Module | Description | Channels | Signal Type |
|--------|-------------|:--------:|-------------|
| [SLM-AI-8-mA](analog-input/SLM-AI-8-mA) | Current Input Module | 8 | 0-20mA / 4-20mA |
| [SLM-AI-8-V](analog-input/SLM-AI-8-V) | Voltage Input Module | 8 | ±10V / 0-10V |
| [SLM-THM-4](analog-input/SLM-THM-4) | Thermocouple Input Module | 4 | J, K, T, E, R, S, B, N |
```

```{tab-item} <span style="color:#ffb6b6;">Output Modules</span>
| Module | Description | Channels | Signal Type |
|--------|-------------|:--------:|-------------|
| [SLM-AO-8-mA](analog-output/SLM-AO-8-mA) | Current Output Module | 8 | 0-20mA / 4-20mA |
| [SLM-AO-8-V](analog-output/SLM-AO-8-V) | Voltage Output Module | 8 | ±10V / 0-10V |
```

```{tab-item} <span style="color:#d4b8ff;">Combo Modules</span>
| Module | Description | Input | Output |
|--------|-------------|-------|--------|
| [SLM-AI4-AO2-mA](analog-combo/SLM-AI4-AO2-mA) | Analog Input + Current Output | 4-Ch | 2-Ch (mA) |
| [SLM-AI4-AO2-V](analog-combo/SLM-AI4-AO2-V) | Analog Input + Voltage Output | 4-Ch | 2-Ch (V) |
```
````

`````
``````

## Module Categories
(discrete-modules)=
### Discrete Modules

#### <span style="color:#4fc3f7;">Discrete Input Modules</span>
| Module | Description |
|--------|-------------|
| [SLM-ACI-8](discrete-input/SLM-ACI-8) | 8-Channel AC Current Input Module |
| [SLM-DCI-16NP](discrete-input/SLM-DCI-16NP) | 16-Channel DC Input Module |
| [SLM-ACDCI-16NP](discrete-input/SLM-ACDCI-16NP) | 16-Channel AC/DC Input Module |
| [SLM-SIM-8](discrete-input/SLM-SIM-8) | 8-Channel Input Simulator Module |

#### <span style="color:#ffb6b6;">Discrete Output Modules</span>
| Module | Description |
|--------|-------------|
| [SLM-DCO-15N](discrete-output/SLM-DCO-15N) | 15-Channel DC Output Module |
| [SLM-DCO-15P](discrete-output/SLM-DCO-15P) | 15-Channel DC Output Module |
| [SLM-RLY-16](discrete-output/SLM-RLY-16) | 16-Channel Relay Output Module |
| [SLM-RLY-8-ISO](discrete-output/SLM-RLY-8-ISO) | 8-Channel Isolated Relay Output Module |

(analog-modules)=
### Analog Modules
#### <span style="color:#4fc3f7;">Analog Input Modules</span>
| Module | Description |
|--------|-------------|
| [SLM-AI-8-mA](analog-input/SLM-AI-8-mA) | 8-Channel Current Input Module |
| [SLM-AI-8-V](analog-input/SLM-AI-8-V) | 8-Channel Voltage Input Module |
| [SLM-THM-4](analog-input/SLM-THM-4) | 4-Channel Thermocouple Input Module |

#### <span style="color:#ffb6b6;">Analog Output Modules</span>
| Module | Description |
|--------|-------------|
| [SLM-AO-8-mA](analog-output/SLM-AO-8-mA) | 8-Channel Current Output Module |
| [SLM-AO-8-V](analog-output/SLM-AO-8-V) | 8-Channel Voltage Output Module |

(combo-modules)=
### Combination Modules

#### <span style="color:#d4b8ff;">Discrete Combination Modules</span>
| Module | Description |
|--------|-------------|
| [SLM-DCI-8NP-DCO-7N](discrete-combo/SLM-DCI-8NP-DCO-7N) | 8-Channel DC Input + 7-Channel DC Output |
| [SLM-DCI-8NP-DCO-7P](discrete-combo/SLM-DCI-8NP-DCO-7P) | 8-Channel DC Input + 7-Channel DC Output |
| [SLM-ACDCI-8NP-RLY8](discrete-combo/SLM-ACDCI-8NP-RLY8) | 8-Channel AC/DC Input + 8-Channel Relay Output |

#### <span style="color:#d4b8ff;">Analog Combination Modules</span>
| Module | Description |
|--------|-------------|
| [SLM-AI4-AO2-mA](analog-combo/SLM-AI4-AO2-mA) | 4-Channel Analog Input + 2-Channel Current Output |
| [SLM-AI4-AO2-V](analog-combo/SLM-AI4-AO2-V) | 4-Channel Analog Input + 2-Channel Voltage Output |