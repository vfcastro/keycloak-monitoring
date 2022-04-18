# Keycloak Monitoring

Instructions for setup monitoring of a Keycloak installation in Kubernetes with the Bitnami Helm Chart: 

https://github.com/bitnami/charts/tree/master/bitnami/keycloak

## Helm

Use the `values.yaml` file provided, which enables the metrics component, monitoring events, and registers the metrics-events listener.

```sh
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm upgrade --install keycloak bitnami/keycloak -f values.yaml
```

References: 

Metrics: https://github.com/aerogear/keycloak-metrics-spi

Config Cli: https://github.com/adorsys/keycloak-config-cli

## Service Monitor

The Prometheus ServiceMonitor shipped with the Helm Chart does not allow for custom scrape paths, needed for custom metrics exposed by the keycloak-metrics-spi component. Therefore, we need to setup one by our own, with the `service-monitor.yaml` file provided:

```sh
$ kubectl apply -f service-monitor.yaml
```

## Grafana

Import the dashboard json file `dash.json` within your Grafana installation.

The dashboard is based on the public available version, with minor changes, on: https://grafana.com/grafana/dashboards/10441

*NOTE*: This dashboard use the pie-chart plugin. To enable this plugin in your Grafana setup, use the following command within Grafana server:

```sh
$ grafana-cli plugins install grafana-piechart-panel
```