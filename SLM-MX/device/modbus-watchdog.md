(watchdog-timer)=
# Watchdog Timer

This feature protects your device by pausing its normal operations if it doesn’t receive any commands within a set period of time. In other words, if the device goes too long without any Modbus commands (a communication standard), it stops processing certain tasks and sends back an error response (exception code `0x07`) for any further requests.

## How It Works
The device uses a special holding register (number 200) that you can read or write. By default, its value is `0`, which means the Watchdog is off.

### User Interactions:
- **Writing a Positive Value:**  
  Entering a number greater than `0` into the register starts the Watchdog timer. The number you write becomes the timeout period. If the device had paused its operations because it timed out, writing any positive number will restart normal operation with the new timeout.
- **Writing a Negative Value:**  
  Negative numbers are not allowed. If you try, the device will ignore the command and return an "Illegal Value" error.
- **Writing a Value Over 32767:**  
  Values larger than 32767 are also not accepted and will return an "Illegal Value" error.
- **Writing `0`:**  
  Writing `0` turns off the Watchdog timer, so the device resumes normal operations without the timer checks.

This simple setup ensures that the device stops itself from operating under abnormal conditions and only resumes when a proper command is given.

## Usage Example

It is recommended to set the watchdog timer by having a Modbus Write to register 400201 (offset 200) at the beginning of the cycle in order to trigger a safe shutdown if the host PLC powers off, and to reset the watchdog timer immediately after the host PLC powers on.

---

## Monitoring the Watchdog Timer

The status of the watchdog timer can be monitored through the SLM-MX State register (address: 400202, offset: 201). 
This shows the status of the SLM-MX state machine, which includes the watchdog timer status. It can be the following values:

- 1: Awaiting Configuration
- 2: Operation OK
- 3: User Halt
- 4: Watchdog Timer has been triggered
- 5: Slot Mismatch

The watchdog timer is triggered if the state register reads 4, and the device will enter the user-defined [safe shutdown state](#safe-shutdown).
