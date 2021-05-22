# d1doorbellmqtt
Use a Wemos D1 mini with supporting electronics to inform you if somebody rang the door bell and push the open button.

## Configuration
See the `config.h.example` file.

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
