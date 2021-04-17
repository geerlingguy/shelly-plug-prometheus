# Shelly Plug Prometheus Exporter

I am a simple man.

I have a Shelly Plug. I don't want to flash a different firmware on it just to get a Prometheus endpoint. Though I might do it someday if I'm not lazy.

I want a Prometheus endpoint where I can scrape metrics for the Shelly Plug.

So I built this.

Could I use MQTT and use some sort of preconfigured broker / metrics generator? Yeah probably. But it's literally easier (if you don't already use MQTT and know how to set it up) to just write a little metrics scraper like this for a one-off.

## How to Use

This thing is a PHP script that runs in a Docker container. That's it.

TODO.

## License

MIT.

## Author

[Jeff Geerling](https://www.jeffgeerling.com).
