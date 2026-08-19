# Mosquitto — Unraid Community App

Unraid Community App template for [Eclipse Mosquitto 2.x](https://mosquitto.org/) (official `eclipse-mosquitto:2` image).

## Why

The existing `spants/mqtt` Community App is based on Mosquitto **1.x** and has not been updated since 2020:

- Mosquitto 2.x writes persistent database format **v6**; the 1.x image cannot read it (`Unsupported persistent database format version 6 (need version 3)`), so existing data folders break the old template.
- Modern Home Assistant requires **MQTT protocol 5**, which 1.x does not support.

This template runs the official 2.x image with a standard `/config` layout and host networking.

## Setup

1. Add the container from Community Apps (or from the template in this repo).
2. Create the broker config on first start, e.g. `/mnt/user/appdata/Mosquitto/mosquitto.conf`:

   ```
   listener 1883
   allow_anonymous false
   password_file /config/passwords.mqtt
   persistence true
   persistence_location /config/data/
   ```

3. Create a user:

   ```
   docker exec -it MQTT sh -c "mosquitto_passwd -c -b /config/passwords.mqtt iotuser 'your-password'"
   ```

4. Restart the container.

## Notes

- Host networking: the broker listens directly on the Unraid host IP (port 1883 by default).
- The default data folder is `/mnt/user/appdata/Mosquitto` (changeable in the template).
- Support: [GitHub issues](https://github.com/47Hunter47/mosquitto-unraid-ca/issues)

## License

[MIT](LICENSE)
