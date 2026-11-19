# ReToon ESPHome

ESPHome configurations for [ReToon](https://github.com/ToonSoftwareCollective/ReToon-images),
the replacement system for the Toon thermostat.

## OpenTherm over WiFi (in preparation)

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

Not usable yet: the package and the support in ReToon are being built.
