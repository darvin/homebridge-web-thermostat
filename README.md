# homebridge-web-thermostat

[![npm](https://img.shields.io/npm/dt/homebridge-web-thermostat.svg)](https://www.npmjs.com/package/homebridge-web-thermostat) [![npm](https://img.shields.io/npm/v/homebridge-web-thermostat.svg)](https://www.npmjs.com/package/homebridge-web-thermostat)

## Description

This [homebridge](https://github.com/nfarina/homebridge) plugin exposes a web-based thermostat to Apple's [HomeKit](http://www.apple.com/ios/home/). Using simple HTTP requests, you can set the thermostat mode, control the termparature and even more.

## Installation

1. Install [homebridge](https://github.com/nfarina/homebridge#installation-details)
2. Install this plugin: `npm install -g homebridge-web-thermostat`
3. Update your `config.json` file

## Configuration

```json
"accessories": [
     {
       "accessory": "Thermostat",
       "name": "Thermostat",
       "apiroute": "http://myurl.com"
     }
]
```

### Core
| Key | Description | Default |
| --- | --- | --- |
| `accessory` | Must be `Thermostat` | N/A |
| `name` | Name to appear in the Home app | N/A |
| `apiroute` | Root URL of your Thermostat device (excluding the rest of the requests) | N/A |
| `pollInterval` _(optional)_ | Time (in seconds) between when homebridge will check the `/status` of your thermostat | `60` |

### Optional fields
| Key | Description | Default |
| --- | --- | --- |
| `temperatureDisplayUnits` _(optional)_ | Whether you want °C (`0`) or °F (`1`) as your units | `0` |
| `currentRelativeHumidity` _(optional)_ | Whether to include `currentRelativeHumidity` as a field in `/status` | `false` |
| `maxTemp` _(optional)_ | Upper bound for the temperature selector in the Home app | `30` |
| `minTemp` _(optional)_ | Lower bound for the temperature selector in the Home app | `15` |
| `temperatureThresholds` _(optional)_ | Whether you want the thermostat accessory to have heating and cooling temperature thresholds | `false` |
| `coolingThresholdTemperature` _(optional)_ | Cooling threshold temperature if thresholds are enabled | `30` |
| `heatingThresholdTemperature` _(optional)_ | Heating threshold temperature if thresholds are enabled | `20` |

### Additional options
| Key | Description | Default |
| --- | --- | --- |
| `timeout` _(optional)_ | Time (in milliseconds) until the accessory will be marked as "Not Responding" if it is unreachable | `3000` |
| `http_method` _(optional)_ | The HTTP method used to communicate with the thermostat | `GET` |
| `username` _(optional)_ | Username if HTTP authentication is enabled | N/A |
| `password` _(optional)_ | Password if HTTP authentication is enabled | N/A |
| `model` _(optional)_ | Appears under the "Model" field for the device | `homebridge-web-thermostat` |
| `serial` _(optional)_ | Appears under the "Serial" field for the device | apiroute |
| `manufacturer` _(optional)_ | Appears under the "Manufacturer" field for the device | `Tom Rodrigues` |

## API Interfacing

Your API should be able to:

1. Return thermostat info when it receives `/status` in the JSON format like below:
```
{
    "targetHeatingCoolingState": INT_VALUE_0_TO_3,
    "targetTemperature": FLOAT_VALUE,
    "currentHeatingCoolingState": INT_VALUE_0_TO_2,
    "currentTemperature": FLOAT_VALUE
}
```

**Note:** You must also include the following fields in `/status` if enabled in the `config.json`:

- `currentRelativeHumidity`
- `coolingThresholdTemperature` & `heatingThresholdTemperature`

2. Set `targetHeatingCoolingState` when it receives:
```
/targetHeatingCoolingState/{INT_VALUE_0_TO_3}
```

3. Set `targetTemperature` when it receives:
```
/targetTemperature/{FLOAT_VALUE}
```

### Additional (if enabled in the configuration):

4. Set `coolingThresholdTemperature` when it receives:
```
/coolingThresholdTemperature/{FLOAT_VALUE}
```

5. Set `heatingThresholdTemperature` when it receives:
```
/heatingThresholdTemperature/{FLOAT_VALUE}
```

## HeatingCoolingState Key

| Number | Name |
| --- | --- |
| `0` | Off |
| `1` | Heat |
| `2` | Cool |
| `3` | Auto |
