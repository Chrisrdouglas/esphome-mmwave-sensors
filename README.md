# ESPHome integration for mmWave sensors

Collection of header files and esphome yaml configs to integrate some Seeed Studio mmWave sensors into Home Assistant.

Currently the following sensors are partially implemented

* Seeed Studio
  * `MR24HPB1` - 24 GHz Human Stationary Sensor
  * `MR24HPC1` - 24 GHz Human Static Presence Lite
  * `MR60BHA1` - 60 GHz Respiratory Heartbeat Sensor

## Table of Contents

- [ESPHome integration for mmWave sensors](#esphome-integration-for-mmwave-sensors)
  - [Table of Contents](#table-of-contents)
  - [Roadmap](#roadmap)
  - [Installation](#installation)
    - [Files](#files)
    - [Hardware](#hardware)
      - [MCU](#mcu)
      - [Wiring](#wiring)
    - [ESPHome Device](#esphome-device)
  - [Supported Functions](#supported-functions)
  - [MR24HPC1](#mr24hpc1)
    - [Sensor](#sensor)
    - [Device Website](#device-website)
    - [Home Assistant](#home-assistant)
  - [MR60HBA1](#mr60hba1)
    - [Sensor](#sensor-1)
    - [Device Website](#device-website-1)
    - [Home Assistant](#home-assistant-1)
  - [Contributing](#contributing)
  - [License](#license)

## Roadmap

* Implement missing SeeedStudio Sensors (MR24BSD1 & MR60FDA1)
* Make documentation cleaner and enhance with screenshots and plots
* Implement sensors as proper external components
  * Are there proper docs on how the config validation/generation part of esphome works?

## Installation

### Files

If you don't bother or use all the supported sensors just copy the `header` and `packages` directories into your Home Assistant `config/esphome` directory.

Otherwise these are the files needed for each sensor:

* `MR24HPB1`
  * Headers
    * headers/mr24hpb1.h
    * headers/mr24hpb1_frame.h
  * Config
    * packages/mr24d.yaml
* `MR24HPC1`
  * Headers
    * headers/mr24hpc1.h
    * headers/mrx_frame.h
  * Config
    * packages/mr24hpc1.yaml
* `MR60BHA1`
  * Headers
    * headers/mr60bha1.h
    * headers/mrx_frame.h
  * Config
    * packages/mr60bha1.yaml

You can change the name of the `headers` directory in home assistant if it doesn't apply to your naming scheme. You can adjust the file locations via substitutions.

```
substitutions:
  header_frame: headers/rx_frame.h
  header_sensor: headers/mr24hpc1.h
```

### Hardware

#### MCU 

The template assumes the use of a `ESP32`. I specifically used the `AZDelivery ESP32 Devkit C`, but any other `ESP32` and `ESP8266` should work I guess. But I have not tested this.

#### Wiring

The template assumes the following wiring:

| MCU              | mmWave Sensor |
|------------------|---------------|
| 5V               | 5V            |
| GND              | GND           |
| GPIO16 (UART RX) | TX            |
| GPIO17 (UART TX) | RX            |

The uart pins can be changed in the ESPHome yaml via substitutions

```
substitutions:
  uart_rx_pin: "16"
  uart_tx_pin: "17"
```

### ESPHome Device

Take a look at the yaml files in the `examples` directory, these should give you a basic understanding of what to do when you create a new device in ESPHome.

Basically the section `substitutions` and `packages` are relevant. When you create a new device the other user relevant section will be generated by ESPHome, the before mentions sections need to be added from the provided example yaml files.

## Supported Functions

The `MR24HPC1` and `MR60BHA1` sensor also support multiple inputs to reset, change operation modes and query specific sensor states.

|                     | `MR24HPB1` | `MR24HPC1` | `MR60BHA1` | Info                                                                      |
|---------------------|---------|--------|--------|---------------------------------------------------------------------------|
| presence            | x       | x      | x      | binary (detected/clear)                                                   |
| motion              | x       | x      | x      | binary (detected/clear)                                                   |
| body movement       | x       | x      | x      | measurement  (0: unoccupied,  1: static presence,  2-100: activity level) |
| proximity           | x       | x      |        | measurement (<0: approaching,  0 none or stationary,  >0 moving away)     |
| distance            |         |        | x      | measurement (distance to object in cm)                                    |
| x, y, z angle       |         |        | x      | measurements  (relative position of object to sensor)                     |
| heartrate           |         |        | x      | measurement                                                               |
| breathing rate      |         |        | x      | measurement                                                               |
| breathing info      |         |        | x      | measurement  (1: normal,  2: fast,  3: slow, 4: none)                     |
| radar out of bounds |         |        | x      | measurement (0: out of range, 1: within range)                            |


## MR24HPC1

### Sensor

[<img src="images/mr24hpc1_sensor.jpg" alt="MR24HPC1 Sensor" width="400">](/images/mr24hpc1_sensor.jpg)

### Device Website

[<img src="images/mr24hpc1_web_server.png" alt="MR24HPC1 Sensor" width="800">](/images/mr24hpc1_sensor.jpg)

### Home Assistant

[<img src="images/mr24hpc1_home_assistant.png" alt="MR24HPC1 Sensor" width="800">](/images/mr24hpc1_sensor.jpg)

## MR60HBA1

### Sensor

[<img src="images/mr60bha1_sensor.jpg" alt="MR60HBA1 Sensor" width="400">](/images/mr60bha1_sensor.jpg)

### Device Website

[<img src="images/mr60bha1_web_server.png" alt="MR60HBA1 Sensor" width="800">](/images/mr60bha1_sensor.jpg)

### Home Assistant

[<img src="images/mr60bha1_home_assistant.png" alt="MR60HBA1 Sensor" width="800">](/images/mr60bha1_sensor.jpg)

## Contributing

Since C++ is not my strongest of languages and this is my first implementation which is interacting with ESPHome, Home Assistant and the uart protocol in general, there are probably a lot of things that can be done easier or cleaner.

Please feel free to dive in! [Open an issue](https://github.com/thefipster/esphome_mmwave_sensors/issues/new) or submit PRs.

## License

MIT © thefipster