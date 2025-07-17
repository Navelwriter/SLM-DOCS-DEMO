# LED Status

The device uses two LEDs to show its current status.

## During Initialization or When Waiting for Settings
- Run LED: ![Off](/images/LEDs/solid_black.gif)
- CPU LED: ![Red LED Blinking](/images/LEDs/blinking_red_rectangle.gif)

## Operating Normally (Operation OK)
- Run LED: ![Green LED Solid](/images/LEDs/solid_green.gif)
- CPU LED: ![Off](/images/LEDs/solid_black.gif)

## Watchdog Timer Triggered
- Run LED: ![Off](/images/LEDs/solid_black.gif)
- CPU LED: ![Red LED Solid](/images/LEDs/solid_red.gif)

(slot-mismatch-status)=
## Slot Mismatch (Connected modules don't match config)
- Run LED: ![Green LED Blinking](/images/LEDs/blinking_green_rectangle.gif)
- CPU LED: ![Red LED Blinking](/images/LEDs/blinking_red_rectangle_start_off.gif)

## Device Firmware Updating
- Run LED: ![Off](/images/LEDs/solid_black.gif)
- CPU LED: ![Red LED Blinking](/images/LEDs/blinking_red_rectangle_fast.gif)