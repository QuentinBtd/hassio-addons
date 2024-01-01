# Home Assistant QBR Add-on: Victoria Metrics

The analytics platform for all your metrics.

VictoriaMetrics is a high-performance, scalable time series database known for its efficiency in storing and querying large volumes of metrics data. It's an ideal replacement for Prometheus and InfluxDB, offering advanced data compression and fast query execution. Key features include native Prometheus support, robust querying capabilities, and cost-effective long-term storage solutions. Designed with simplicity and ease of use in mind, VictoriaMetrics provides a reliable platform for monitoring and analytics at scale. Its architecture ensures optimal resource utilization, making it suitable for both small and large-scale deployments.

Combine this add-on with the Grafana add-on to get insanely powerful insights to your home.

## Installation

The installation of this add-on is pretty straightforward and not different in
comparison to installing any other Home Assistant add-on.

1. Add `https://github.com/QuentinBtd/hassio-addons` to the add-on repositories list.

2. Click the Home Assistant My button below to open the add-on on your Home
   Assistant instance.

   [![Open this add-on in your Home Assistant instance.][addon-badge]][addon]

3. Click the "Install" button to install the add-on.
4. Start the "Victoria Metrics" add-on.
5. Check the logs of the "Victoria Metrics" to see if everything went well.
6. Open the Web UI.

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

Example add-on configuration:

```yaml
retention: "99y"
enable_prometheus_scrape: true
global_config: |
  scrape_interval: 1m
  scrape_timeout: 15s
homeassistant_scrape_config: |
  - job_name: "home-assistant"
    scrape_interval: "20s"
    scrape_timeout: "15s"
    metrics_path: /api/prometheus
    authorization:
      credentials: "BEARER TOKEN HERE"
    scheme: http
    static_configs:
      - targets: ['homeassistant:8123']
additional_scrape_configs: |
  # Node Exporter job scrape example
  # - job_name: "node-exporter"
  #   scrape_interval: "30s"
  #   scrape_timeout: "15s"
  #   metrics_path: /metrics
  #   scheme: http
  #   static_configs:
  #     - targets: ['localhost:9100']
additional_arguments: ""
storage_data_path: "/share/victoria-metrics-data"
custom_promscape_file: false
custom_promscape_file_path: "/homeassistant/victoria-metrics/promscrape.yaml"
env_vars: []
log_level: "INFO"
ssl: true
certfile: fullchain.pem
keyfile: privkey.pem
```

**Note**: _This is just an example, don't copy and paste it! Create your own!_

### Option: `retention`

This option sets the data retention period for Victoria Metrics. The value `99y` indicates that data will be kept for 99 years, providing a long-term storage solution for time series data. Adjust this value to meet your specific data retention requirements.

### Option: `enable_prometheus_scrape`

When set to `true``, this option enables Prometheus scraping, allowing Victoria Metrics to ingest metrics from Prometheus targets. This is essential for integrating Victoria Metrics with Prometheus-based monitoring setups.

### Option: `global_config`

This section allows you to configure global settings for scraping metrics. It includes parameters like `scrape_interval`, set to `1m`` (one minute), and `scrape_timeout`, set to `15s`` (fifteen seconds), which define how frequently metrics are scraped and the timeout for each scrape.

### Option: `homeassistant_scrape_config`

This configuration specifies how Victoria Metrics should scrape metrics from Home Assistant. It includes settings such as job name, scrape interval, timeout, metrics path, authorization credentials, and the targets for scraping (in this case, `homeassistant:8123`).

### Option: `additional_scrape_configs`

Here, you can specify additional configurations for scraping other services. The provided example is commented out but shows how to set up a job for scraping a Node Exporter, including job name, scrape interval, metrics path, and targets.

### Option: `additional_arguments`

Use this option to pass additional arguments to Victoria Metrics. This field is left blank (`""`) by default, but you can specify any additional parameters required for your specific setup.

### Option: `storage_data_path`

This option defines the file path where Victoria Metrics data will be stored. The default path is `/share/victoria-metrics-data`. Ensure this path is correctly set based on your storage preferences.

### Option: `custom_promscape_file`

Set this to `true` if you want to use a custom Prometheus scrape file. By default, this is set to `false`, indicating that the standard scraping configuration will be used.

### Option: `custom_promscape_file_path`

If `custom_promscape_file` is `true`, use this option to specify the path to your custom Prometheus scrape file. The default provided path is `/homeassistant/victoria-metrics/promscrape.yaml`.

### Option: `log_level`

The `log_level` option controls the level of log output by the addon and can
be changed to be more or less verbose, which might be useful when you are
dealing with an unknown issue. Possible values are:

- `INFO`: Normal (usually) interesting events.
- `WARN`: Exceptional occurrences that are not errors.
- `ERROR`: Runtime errors that do not require immediate action.
- `FATAL`: Something went terribly wrong. Add-on becomes unusable.
- `PANIC`: Something went more terribly wrong. Add-on becomes unusable.

Please note that each level automatically includes log messages from a
more severe level, e.g., `WARN` also shows `INFO` messages. By default,
the `log_level` is set to `INFO`, which is the recommended setting unless
you are troubleshooting.

### Option: `env_vars`

This option allows you to tweak every aspect of Victoria Metrics by setting
configuration options using environment variables. See the example at the
start of this chapter to get an idea of how the configuration looks.

For more information about using these variables, see the official Victoria Metrics
documentation:

<https://docs.victoriametrics.com/#environment-variables>

## Known issues and limitations

- This add-on does support ARM-based devices, nevertheless, they must
  at least be an ARMv7 device. (Raspberry Pi 1 and Zero is not supported).

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality.

Releases are based on [Semantic Versioning][semver], and use the format
of `MAJOR.MINOR.PATCH`. In a nutshell, the version will be incremented
based on the following:

- `MAJOR`: Incompatible or major changes.
- `MINOR`: Backwards-compatible new features and enhancements.
- `PATCH`: Backwards-compatible bugfixes and package updates.

## Support

Got questions?

You could [open an issue here][issue] GitHub.

## Authors & contributors

The original setup of this repository is by [Quentin BERTRAND][QuentinBtd].

For a full list of all authors and contributors,
check [the contributor's page][contributors].

## License

MIT License

Copyright (c) 2023-2024 Quentin BERTRAND

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[addon-badge]: https://my.home-assistant.io/badges/supervisor_addon.svg
[addon]: https://my.home-assistant.io/redirect/supervisor_addon/?addon=TO_SET_victoria-metrics&repository_url=https%3A%2F%2Fgithub.com%2FQuentinBtd%2Fhassio-addons
[contributors]: https://github.com/QuentinBtd/hassio-addon-victoria-metrics/graphs/contributors
[QuentinBtd]: https://github.com/QuentinBtd
[grafana-addon]: https://github.com/hassio-addons/addon-grafana
[issue]: https://github.com/QuentinBtd/hassio-addon-victoria-metrics/issues
[reddit]: https://reddit.com/r/homeassistant
[releases]: https://github.com/QuentinBtd/hassio-addon-victoria-metrics/releases
[semver]: https://semver.org/spec/v2.0.0.html
