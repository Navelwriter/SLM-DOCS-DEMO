# Arduino Example Project 

```cpp
/*
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

*/
#include <Ethernet.h>
#include <ArduinoRS485.h> // ArduinoModbus depends on the ArduinoRS485 library
#include <ArduinoModbus.h>

/*-------------------------------------------
Enter a MAC address for your controller below.
The IP address will be dependent on your local network:
-------------------------------------------*/
byte mac[] = {
  0xDE, 0xAD, 0x2C, 0x1E, 0xFE, 0xAA
};
IPAddress ip(192, 168, 5, 20);

EthernetClient ethClient;
ModbusTCPClient modbusClient(ethClient);

/*-------------------------------------------
Update with the IP Address of your SLM-MX device
-------------------------------------------*/
IPAddress server(192,168,5,21);    


void setup() {
  Serial.begin(115200);
  while(!Serial);           // Wait for serial port to connect
  Serial.println("SLM-MX Modbus Client Example");
  Ethernet.begin(mac, ip);
  delay(2000);
}

/*-------------------------------------------
IMPORTANT!!! Refer to SLM-MX Configuration for the modbus IO register map.
-------------------------------------------*/
void loop() {
  int result[8];
  int resultCompare[8];
  
  if (!modbusClient.connected()) {
    // client not connected, start the Modbus TCP client
    Serial.println("Attempting to connect to SLM-MX server");
    if (!modbusClient.begin(server, 502)) {
      Serial.println("Modbus TCP Client failed to connect!");
    } else {
      Serial.println("Modbus TCP Client connected");
    }
  }else{
    /*-------------------------------------------
    Read 8 discrete inputs starting at address 0
    -------------------------------------------*/
    modbusClient.requestFrom(DISCRETE_INPUTS, 0, 8);
    Serial.println("\nDiscrete Inputs 1-8:");
    for(int i = 0; i < 8; i++){
      result[i] = modbusClient.read();
      if(result[i] != -1){
        Serial.print("Discrete Input "); Serial.print(i + 1); Serial.print(" value: ");
        Serial.println(result[i]);
      }
    }
    delay(2000);

    /*-------------------------------------------
    Write the results from the discrete inputs to the 8 coils starting at address 0
    -------------------------------------------*/
    modbusClient.beginTransmission(COILS, 0, 8);
    for(int i = 0; i < 8; i++){
      modbusClient.write(result[i]);
    }
    modbusClient.endTransmission();

    /*-------------------------------------------
    Read the coil status of all 8 coils to verify successful write.
    -------------------------------------------*/
    modbusClient.requestFrom(COILS, 0, 8);
    Serial.println("\nCoil Statuses 1-8:");
    for(int i = 0; i < 8; i++){
      resultCompare[i] = modbusClient.read();
      if(resultCompare[i] != -1){
        Serial.print("Coil "); Serial.print(i + 1); Serial.print(" value: ");
        Serial.println(resultCompare[i]);
      }
    }
    delay(2000);

    /*-------------------------------------------
    # Read 8 analog inputs starting at address 0
    -------------------------------------------*/
    modbusClient.requestFrom(INPUT_REGISTERS, 0, 8);
    Serial.println("\nAnalog Inputs 1-8:");
    for(int i = 0; i < 8; i++){
      result[i] = modbusClient.read();
      if(result[i] != -1){
        Serial.print("Input Register "); Serial.print(i + 1); Serial.print(" value: ");
        Serial.println(result[i]);
      }
    }
    delay(2000);

    /*-------------------------------------------
    # Write Analog output channel 1 at address 0
    -------------------------------------------*/
    int counts[8] = {2047, 2047, 2047, 2047, 2047, 2047, 2047, 2047};   // Write half scale value of 12-bit analog output
    modbusClient.beginTransmission(HOLDING_REGISTERS, 0, 8);
    for(int i = 0; i < 8; i++){
      modbusClient.write(counts[i]);
    }
    modbusClient.endTransmission();

    /*-------------------------------------------
    Read 4 thermocouple input channels starting at address 9
    -------------------------------------------*/

    modbusClient.requestFrom(INPUT_REGISTERS, 9, 4);
    Serial.println("\nThermocouple Inputs 1-4:");
    for(int i = 0; i < 4; i++){
      result[i] = modbusClient.read();
      if(result[i] != -1){
        Serial.print("Input Register "); Serial.print(i + 9); Serial.print(" value: ");
        Serial.println(result[i]);
      }
    }
    delay(2000);
  }
}
```