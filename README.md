# wiim-mqtt
Send commands and receive status updates to WiiM Minis via MQTT

## Running

This is based on [mqtt-usvc](https://github.com/denwilliams/mqtt-usvc) so it is run in the same way.

## Configuration

Outside of the usual configuration for mqtt-usvc, the following configuration is required:

1) A list of devices to connect to with an ID (used for MQTT topics) and a host being an IP address or hostname of the device.
2) The interval at which to poll the devices for status updates.

```yaml
devices:
  - id: "mydevice"
    host: "192.168.1.100"
```

See `config.example.yml` for a full example.

## MQTT API

You can run any of the commands found here: [https://wiimhome.com/pdf/HTTP%20API%20for%20WiiM%20Mini.pdf](https://wiimhome.com/pdf/HTTP%20API%20for%20WiiM%20Mini.pdf)

The commands are sent to MQTT topics under `wiim/run/*`. Responses to get commands are sent to `wiim/status/*`.

For example to get the status of the WiiM Mini you would send a message to `wiim/run/mydevice/getStatusEx` with no payload. The response will be sent to `wiim/status/mydevice/statusEx`... where mydevice is the ID given in the config.

If the command requires parameters these should be sent as a JSON array in the payload. For example to mute the the device you would send a message to `wiim/run/mydevice/setPlayerCmd` with the payload `["mute", 1]`. This will run the command `setPlayerCmd:mute:1` via the HTTP API.

In addition `getPlayerStatus` will be run automatically at the polling interval, and following any `setPlayerCmd` command.
