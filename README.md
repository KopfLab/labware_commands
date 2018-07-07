---
title: "Labware Commands"
author: "Sebastian Kopf"
output:
  pdf_document: default
  html_document: default
geometry: margin=1in
---

# `DeviceController` commands (`<cmd>` options):

  - `state-log on` to turn web logging of state changes on (letter `S` shown in the state overview)
  - `state-log off` to turn web logging of state changes off (no letter `S` in state overview)
  - `data-log on` to turn web logging of data on (letter `D` in state overview)
  - `data-log off` to turn web logging of data off
  - `lock on` to safely lock the device (i.e. no commands will be accepted until `lock off` is called) - letter `L` in state overview
  - `lock off` to unlock the device if it is locked
  - `reset data` to reset the data stored in the device

# `SerialDeviceController` commands:

  - all `DeviceController` commands PLUS:
  - `read-period <options>` to specify how frequently data should be read (letter `R` + subsequent in state overview), `<options>`:
    - `manual` don't read data unless externally triggered in some way (device specific) - `RM` in state overview
    - `200 ms` read data every 200 (or any other number) milli seconds (`R200ms` in state overview)
    - `5 s` read data every 5 (or any other number) seconds (`R5s` in state overview)
  - `log-period <options>` to specify how frequently read data should be logged (after letter `D` in state overview, although the `D` only appears if data logging is actually enabled), `<options>`:
    - `3 x` log after every 3rd (or any other number) successful data read (`D3x` in state overview if logging is active, just `3x` if not), works with `manual` or time based `read-period`, set to `1 x` in combination with `manual` to log every externally triggered data event immediately
    - `2 s` log every 2 seconds (or any other number), must exceed the `read-period` (`D2s` in state overview if data logging is active, just `2s` if not)
    - `8 m` log every 8 minutes (or any other number)
    - `1 h` log every hour (or any other number)

# `ScaleController` commands:

  - all `DeviceController` and `SerialDeviceController` commands PLUS:
  - `calc-rate <options>` to specify whether to calculate a mass flow rate and if so, which time units to use, `<options>`:
    - `off` don't calculate the flow rate
    - `s` calculate flow rate as mass/s
    - `m` calculate flow rate as mass/minute
    - `h` calculate flow rate as mass/hours
    - `d` calculate flow rate as mass/day
    
# `PumpController` commands:

  - `start` to start the pump (at the currently set speed and microstepping)
  - `stop` to stop the pump and disengage it (no holding torque applied)
  - `hold` to stop the pump but hold the position (maximum holding torque)
  - `rotate <x>` to have the pump do `<x>` rotations and then execute a `stop` commands
  - `ms <x>` to set the microstepping mode to `<x>` (1= full step, 2 = half step, 4 = quarter step, etc.)
  - `ms auto` to set the microstepping mode to automatic in which case the lowest step mode that the current speed allows will be automatically set
  - `speed <x> rpm` to set the pump speed to `<x>` rotations per minute (if the pump is currently running, it will change the speed to this and keep running). if microstepping mode is in `auto` it will automatically select the appropriate microstepping mode for the selected speed. If the microstepping mode is fixed and the requested rpm exceeds the maximally possible speed for the selected mode (or if in `auto` mode, the requested rpm exceeds the fastest possible on full step mode), the maximum speed will automatically be set instead and a warning return code will be issued.
  - `direction cc` to set the direction to counter clockwise
  - `direction cw` to set the direction to clockwise
  - `direction switch` to reverse the direction (note that any direction changes stops the pump if it is in `rotate <x>` mode)
  - `lock` to lock the pump (i.e. no commands will be accepted until `unlock` is called)
  - `unlock` to unlock the pump if it is locked
  - to be continued (more commands in progress)...
