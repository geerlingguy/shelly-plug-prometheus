# Shelly Plug Prometheus Exporter

I am a simple man.

I have a [Shelly Plug](https://shelly.cloud/products/shelly-plug-us-smart-home-automation-device/). I don't want to flash a different firmware on it just to get a Prometheus endpoint. Though I might do it someday if I'm not lazy.

I want a Prometheus endpoint where I can scrape metrics for the Shelly Plug.

So I built this.

Could I use MQTT and use some sort of preconfigured broker / metrics generator? Yeah probably. But it's literally easier (if you don't already use MQTT and know how to set it up) to just write a little metrics scraper like this for a one-off.

## How to Use

This thing is a PHP script that runs in a Docker container. That's it.

You could even run the thing without using Docker, it doesn't care, it's just a PHP script.

### Set environment variables

I recommend adding a basic HTTP auth username and password to your Shelly Plug to give at least minimal security.

Run the PHP script inside Docker like so:

```
docker run -d -p 9924:80 --name shelly-plug \
  -e SHELLY_HOSTNAME='my-shelly-plug-host-or-ip' \
  -e SHELLY_HTTP_USERNAME='username' \
  -e SHELLY_HTTP_PASSWORD='password' \
  -v "$PWD":/var/www/html \
  php:8-apache
```

Or you can set it up inside a docker-compose file like so:

```yaml
---
version: "3"

services:
  shelly-plug:
    container_name: shelly-plug
    image: php:8-apache
    ports:
      - "9924:80"
    environment:
      SHELLY_HOSTNAME='my-shelly-plug-host-or-ip'
      SHELLY_HTTP_USERNAME='username'
      SHELLY_HTTP_PASSWORD='password'
    volumes:
      - './:/var/www/html'
    restart: unless-stopped
```

If you run the script outside of Docker, make sure you set the necessary environment variables.

After it's running, test that it's working with `curl`:

```
$ curl http://localhost:9924/metrics
# HELP power Current real AC power being drawn, in Watts
# TYPE power gauge
power 91.81
# HELP is_valid Whether power metering self-checks OK
# TYPE is_valid gauge
is_valid 1
# HELP total Total energy consumed by the attached electrical appliance in Watt-minute
# TYPE total gauge
total 24239
```

If you get an error, check your environment variables and make sure they're correct, and make sure the Docker container (and the host it's on) can access your Shelly Plug over HTTP!

## License

MIT.

## Author

[Jeff Geerling](https://www.jeffgeerling.com).
