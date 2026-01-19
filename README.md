CAR HEATER AUTOMATION – README

Sorry for the swedish in advance =)

CAR HEATER AUTOMATION – README (OPTIMIZED, NO NOTIFICATIONS)

Overview
--------
This Home Assistant blueprint controls an engine heater (car heater)
based solely on the CURRENT outdoor temperature and a selected
departure time.

The automation is intentionally kept simple, robust, and predictable:
- No weather forecast
- No notifications
- Clear and strict conditions

The automation always uses TODAY'S DATE, regardless of the date stored
in the input_datetime helper.

Features
--------
- Uses real-time outdoor temperature only
- Automatic heating duration based on temperature:

  < -20 °C : 3 hours
  < -10 °C : 2 hours
  <  0 °C  : 1 hour
  <  5 °C  : 30 minutes
  ≥  5 °C  : Heater OFF

- Always uses today's date
- Optional weekdays-only mode
- Manual enable/disable helper
- Automatic stop at departure time
- No notifications
- No weather forecast
- Minimal service calls (only turn_on / turn_off)
- Runs safely every minute without double switching

Required Helpers
----------------
- input_datetime  – Departure time (time only)
- input_boolean   – Automation enabled
- input_boolean   – Weekdays only (optional)

Required Entities
-----------------
- Outdoor temperature sensor
- Switch controlling the engine heater

Logic Summary
-------------
1. Automation runs every minute
2. It exits immediately unless:
   - Automation is enabled
   - Temperature is below 5 °C
   - Departure time is in the future
   - Weekday requirement (if enabled) is met
3. Heating duration is calculated from current temperature
4. Heater turns ON once when start time is reached
5. Heater turns OFF automatically at departure time

Design Philosophy
-----------------
This blueprint prioritizes stability and clarity over complexity.
The minute-based trigger is intentional and safe.

Example Lovelace UI
-------------------
Required custom cards:
- vertical-stack-in-card
- time-picker-card

Example YAML:

type: custom:vertical-stack-in-card
cards:
  - type: entities
    show_header_toggle: false
    entities:
      - switch.motorvarmare
      - input_boolean.bilvarmare_aktiv
      - input_boolean.bilvarmare_endast_vardagar

  - type: custom:time-picker-card
    entity: input_datetime.bilvarmare_avgangstid

Notes
-----
- Mobile-friendly UI
- No forecast usage
- No notifications
- Safe to reuse for multiple cars

