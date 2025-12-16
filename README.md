# d1doorbellmqtt
Use a Wemos D1 mini with supporting electronics to inform you if somebody rang the door bell and push the open button.

## Wiring the D1
Per default (see the `config.h` file), the following pins are used:

| Pin | Connected to      | Comment                                                                        |
|-----|-------------------|--------------------------------------------------------------------------------|
| D1  | Indicator LED     | After a detected ringing, keeps on for a given amount of time.                 |
| D2  | Opening Button    | I.e. a relais which presses the opening button for you.                        |
| D3  | Ringing Detection | Digital input when somebody is ringing (default `HIGH` to `LOW` when ringing). |
| A0  | Battery Voltage   | Optional: Connect via voltage divider (100kΩ + 33kΩ) for battery monitoring.  |

Remember the you also have to supply power to your D1 Mini.

### Battery Monitoring (Optional)
If you want to monitor battery voltage, connect the battery positive terminal to A0 through a voltage divider:

```
Battery+ ----[100kΩ]----+----[33kΩ]---- GND
                        |
                        A0
```

This brings the 18650 voltage (3.0-4.2V) down to the ESP8266 ADC safe range (0-1.0V). Configure the resistor values in `config.h` if you use different resistors.

## Configuration
See the `config.h.example` file which has to be copied to `config.h` to be used.

## MQTT interface

### Discovery
It publishes its MAC address regularly to `/d1doorbell/discovery/MAC` with the
current version as value.

### Last will
It sets its last will to `/d1doorbell/lastwill/MAC` with the MAC as message. This
message will be retained and cleared on start.

### Door Ring Event
If a door ringing is detectet, it sends the payload `ringing` to `/d1doorbell/MAC address`.
The pin is expected to be `HIGH` in normal state and `LOW` when ringing.

### Control
It subscribes to two MQTT topics where it listens to the payload `open` to trigger the door opening:
* `/d1doorbell/all`
* `/d1doorbell/MAC address`

### Battery Monitoring (Optional)
If battery monitoring is enabled in `config.h`:

#### Battery Voltage
Published regularly (default: every 5 minutes) to `/d1doorbell/battery/MAC` with the current battery voltage in volts (e.g., "3.85").

#### Battery Status
Published to `/d1doorbell/battery/status/MAC` with one of these values:
* `ok` - Battery voltage is healthy
* `low` - Battery voltage below LOW_BATTERY_THRESHOLD (default: 3.3V)
* `critical` - Battery voltage below CRITICAL_BATTERY_THRESHOLD (default: 3.1V)
