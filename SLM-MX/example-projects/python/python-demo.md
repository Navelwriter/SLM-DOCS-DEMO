# Python Example Project 

```py
"""
    Example: Modbus

    This example shows how to connect to remote IO modules through modbusTCP.

    This example works with all SLM Series:
     - Discrete input modules such as SLM-SIM-8.
     - Analog input modules such as SLM-AI-8V.
     - Input/Output combo modules usch as SLM-ACDCI-8NP-RLY8
     - Relay Coil modules such as SLM-RLY-16
     - Analog output modules such as SLM-AO-8-V
     - Thermocouple input modules such as SLM-THM-4


    Modules used:
        Slot 01:    SLM-ACDCI-8NP-RLY8
        Slot 02:    SLM-AI-8V
        Slot 03:    SLM-AO-8-V
        Slot 04:    SLM-THM-4
     _____  _____  _____  _____  _____   
    |  S  ||  S  ||  S  ||  S  ||  S  |
    |  L  ||  L  ||  L  ||  L  ||  L  |
    |  M  ||  O  ||  O  ||  O  ||  O  |
    |  -  ||  T  ||  T  ||  T  ||  T  |
    |  M  ||     ||     ||     ||     |
    |  X  ||  0  ||  0  ||  0  ||  0  |
    |     ||  1  ||  2  ||  3  ||  4  |
    |     ||     ||     ||     ||     |
     ¯¯¯¯¯  ¯¯¯¯¯  ¯¯¯¯¯  ¯¯¯¯¯  ¯¯¯¯¯ 
    Written by Synergy Logic
    Copyright (c) 2025 Synergy Logic, LLC
    Licensed under the MIT license.

"""
from pymodbus.client import ModbusTcpClient
import time


# Ip address of SLM-MX
client_ip = "192.168.1.105"
unit_id = 255

# Open modbus connection 
client = ModbusTcpClient(client_ip)
client.connect()


# IMPORTANT!!! Refer to SLM-MX Configuratior for the modbus IO register map. 
while True:

    # Read 8 discrete inputs starting at address 0 
    result = client.read_discrete_inputs(0, count=8)

    print("\nDiscrete Inputs 1-8:")
    for index, value in enumerate(
        result.bits ): # Use .bits for discrete input parsed data
        print(f"Input# {index + 1}: {value}")

    # Write the results from the discrete inputs to the 8 coils starting at address 0
    client.write_coils(0, values=result.bits)

    # Read the coil status of all 8 coils to verify successful write. 
    client.read_coils(0, count = 8)
    print("\nCoil Statuses 1-8:")
    for index, value in enumerate(
        result.bits ): # Use .bits for reading coil status parsed data
        print(f"Input# {index + 1}: {value}")

    # Read 8 analog inputs starting at address 0
    result = client.read_input_registers(0, count=8)
    print("\nAnalog Inputs 1-8:")
    for index, value in enumerate(
        result.registers): # Use .registers for reading analog count parsed data
        volts = round(value/8192 * 10, 3) # Convert counts to volts. value in engineering units = counts / 2^(bit resolution) * max value
        print(f"Holding Register#{index +1 }: {value} counts, {volts}V")
    
    # Write Analog output channel 1 at address 0
    counts  = int(3.3 / 10.0 * 4096) # Convert volts to counts. counts = value in engineering units / max value * 2^(bit resolution)
    client.write_register(0, counts)

    # Read 4 thermocouple input channels starting at address 9
    result = client.read_input_registers(9, count = 4)
    print("\nThermocouple Inputs 1-4:")
    for index, value in enumerate(
        result.registers): # Use .registers for reading analog count parsed data
        print(f"Thermocouple channel#{index +1 }: {value} C")
    
    #loop delay
    time.sleep(1)
```