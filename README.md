# d1doorbellmqtt
Use a Wemos D1 mini with supporting electronics to inform you if somebody rang the door bell and push the open button.

## Wiring the D1
Per default (see the `config.h` file), the following pins are used:

| Pin | Connected to      | Comment                                                        |
|-----|-------------------|----------------------------------------------------------------|
| D2  | Indicator LED     | After a detected ringing, keeps on for a given amount of time. |
| D3  | Ringing Detection | Digital input when somebody is ringing (HIGH).                 |
| D4  | Opening Button    | I.e. a relais which presses the opening button for you.        |

Remember the you also have to supply power to your D1 Mini.

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

### Control
It subscribes to two MQTT topics where it listens to the payload `open` to trigger the door opening:
* `/d1doorbell/all`
* `/d1doorbell/MAC address`
