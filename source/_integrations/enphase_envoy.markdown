---
title: Enphase Envoy
description: Instructions on how to setup Enphase Envoy with Home Assistant.
ha_category:
  - Energy
ha_release: 0.76
ha_iot_class: Local Polling
ha_domain: enphase_envoy
ha_zeroconf: true
ha_config_flow: true
ha_codeowners:
  - '@bdraco'
  - '@cgarwood'
  - '@dgomes'
  - '@joostlek'
  - '@catsmanac'
ha_platforms:
  - binary_sensor
  - diagnostics
  - number
  - select
  - sensor
  - switch
ha_integration_type: integration
---

An integration for the [Enphase Envoy](https://enphase.com/en-us/products-and-services/envoy-and-combiner) solar energy gateway. This integration works with older models that only have production metrics (ie. Envoy-C) and newer models that offer both production and consumption metrics (ie. Envoy-S). Firmware version 3.9 or newer is required.

{% include integrations/config_flow.md %}

## Capabilities

This integration will offer various sensors depending on the configuration of your Enphase system. Sensors are available for the following:

- Current energy production & consumption
- Historical energy production & consumption
- Power production per-inverter

Production and consumption sensors for each phase are available for Envoy S Metered / IQ Gateway Metered with installed and configured current transformers (CT) on more than 1 phase.

_Consumption sensors require your Envoy to be properly configured with consumption CT sensors installed._

For Enphase Ensemble systems with the Enpower/IQ System Controller and Encharge/IQ Batteries installed, additional features are available:

- Sensors for battery status and usage
- Sensors for grid status
- Sensors for the state of the Enpower's 4 load-shedding relays
- A switch allowing you to take your system on-grid and off-grid. Note that the Enpower has a slight delay built-in between receiving these commands and actually switching the system on or off grid.
- A switch allowing you to enable or disable charging the Encharge/IQ Batteries from the power grid.
- Support for changing the battery storage mode between full backup, self-consumption, and savings mode and setting the reserve battery level for outages.

## Envoy authentication requirements

For newer models running firmware 7 and greater, you will need your Enlighten cloud username and password. The integration will use these credentials to obtain an Envoy access token from the Enlighten cloud.

For models running firmware 5 and older, use `installer` for the username. No password is required. The integration will automatically detect the `installer` password.

## Enpower load shedding relays

The Enphase Enpower has 4 load shedding relays that can be used to control non-essential loads in your home. These have two main modes of operation:

### Standard
When the mode entity is set to standard, you can simply set the state of the relay to be powered or not powered for each mode of operation: on grid, off grid, and on generator.

### Battery level
When the relay mode is set to battery level, the relays will turn on and off based on the remaining battery level of your Encharge batteries. Two number entities are available to control the cutoff and restore levels for the relays. When the battery level drops below the cutoff level, the relays will turn off. When the battery level rises above the restore level, the relays will turn back on.

## Polling interval
The default polling interval is 60 seconds. To customize the polling interval, refer to [defining a custom polling interval](/common-tasks/general/#defining-a-custom-polling-interval). Specify the envoy device as a target of the service using the `+ choose device` button. Updating the envoy will also update the related devices like the inverters; there is no need to split them into separate entities or add all inverter devices. When using multiple Envoys, add them as targets or create separate custom polling intervals as needed.
