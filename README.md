# ReToon ESPHome

ESPHome configurations for [ReToon](https://github.com/ToonSoftwareCollective/ReToon-images),
the replacement system for the Toon thermostat.

## OpenTherm over WiFi (beta)

An ESP32 or ESP8266 with an OpenTherm adapter at the boiler, running ESPHome,
instead of the Toon's boiler module and its cable. The Toon keeps doing the
thermostat work and talks to the ESP over the network (ESPHome native API,
encrypted).

You will not need to write the configuration yourself: ReToon's web interface
makes a short one for your adapter, with its own encryption key, that uses the
package in this repository:

```yaml
packages:
  retoon: github://ToonSoftwareCollective/ReToon-esphome/opentherm.yaml@v1
```

**Beta: new, and not tested with a real boiler or heat pump yet.** Try it
while you can keep an eye on the heating, and tell us how it works (an issue
here). ReToon supports it from 1.65: Thermostat → Installation, Connection,
"Wireless: OpenTherm over WiFi (ESPHome)".

### What the ESP does

- It is the OpenTherm master for the boiler, as the Toon's boiler module is.
- The Toon sets the heating switch and the flow setpoint every minute. Five
  minutes without that (the Toon off, WiFi gone) and the ESP stops asking
  for heat; hot water and the boiler's own frost protection keep working.
- At start it asks for no heat until the Toon does.
- The hot water setpoint and the maximum flow temperature are only read:
  OpenTherm here writes what it controls in every round, and boilers keep
  these two in memory that wears out.
- Its readings (water temperatures, pressure, modulation, flame, faults,
  counters) go to the Toon, which shows them and passes them on to Home
  Assistant. The ESP can be added to Home Assistant as well to watch it;
  a change made there is overwritten by the Toon within a minute.
- `temperature.yaml` (optional, added by the web interface when ticked): a
  1-wire temperature sensor (DS18B20) on the adapter, as "Module
  temperature". The Toon passes it on to Home Assistant. It switches on the
  OpenTherm hub's `sync_mode`, as ESPHome advises with a Dallas sensor: the
  sensor's reading switches interrupts off, which would otherwise disturb
  OpenTherm messages.
