This is an [OpenWeatherMap](https://openweathermap.org/) exporter for [Prometheus](https://prometheus.io/).

It exposes weather metrics to the [pushgateway](https://github.com/prometheus/pushgateway).

A minimal `openweathermap_exporter.yml` would look like:

```yml
api_key: <yourkey>
cities:
  - Munich
  - Ulaanbaatar
  - Isachsen
latlons:
  - [40.2010176,-72.6806685]
zips:
  - 90210
city_code: 
  - 4259418
```

Either `cities`, `lathlons`, `zips`, or `city_code` can be provided.


The city_codes that I added can be found **[here](http://bulk.openweathermap.org/sample/)**  


The `unit_format` in the `openweathermap_exporter` can be changed to provide different output based on your region  
```pl
# Options are: Default (kelvin), Metric (centegrade), or Imperial (farenheit)  
# This will update all readings to the unit format you select  
my $unit_format = "Imperial";
```
---

**Requirements:**
 - Prometheus' Pushgateway installed and running on `localhost:9091`
 - Perl with packages `liblwp-useragent-determined-perl`, `libjson-perl` and `libconfig-yaml-perl`


An example `prometheus.yml` configuration looks like:

```yml
  - job_name: 'pushgateway'
    scrape_interval: 60s # Note this is NOT the interval the scripts will run; this is just the collection interval from the Pushgateway to Prometheus
    honor_labels: true
    static_configs:
      - targets: ['localhost:9091']
        labels:
          alias: 'openweathermap'
```
