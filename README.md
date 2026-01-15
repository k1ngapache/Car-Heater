CAR HEATER AUTOMATION – README

Overview
--------
This Home Assistant blueprint automatically controls an engine heater (car heater)
based on current outdoor temperature and a selected departure time.
Only real-time temperature is used – no weather forecast.

Features
--------
- Uses current outdoor temperature only
- Automatic start time based on temperature:
  < -20 °C : 3 hours
  < -10 °C : 2 hours
  <  0 °C  : 1 hour
  <  5 °C  : 30 minutes
  >= 5 °C  : Off
- Always uses today’s date
- Optional weekdays-only mode
- Manual enable/disable switch
- Automatic stop at departure time
- Two selectable mobile notification targets
- Notification is sent only when the heater actually turns ON
- Works with Home Assistant Companion App (Android & iOS)

Required Helpers
----------------
- input_datetime  (departure time)
- input_boolean   (automation enabled)
- input_boolean   (weekdays only)

Notifications
-------------
The blueprint supports two notification targets.
Each target is a notify.mobile_app_* service selected when creating the automation.
You can select one, two, or none.

Logic Summary
-------------
- Automation runs every minute
- Departure time is always interpreted as today
- Heater starts only once per day
- Heater stops automatically at departure time

Example Lovelace UI
-------------------
Required custom cards:
- vertical-stack-in-card
- time-picker-card

Example YAML:

type: custom:vertical-stack-in-card
cards:
  - type: entities
    entities:
      - input_text.bilvarmare_status
      - switch.motorvarmare
      - input_boolean.bilvarmare_aktiv
      - input_boolean.bilvarmare_endast_vardagar
  - type: custom:time-picker-card
    entity: input_datetime.bilvarmare_avgangstid

Notes
-----
- Mobile-friendly UI
- No forecast dependencies
- Safe to reuse for multiple cars

