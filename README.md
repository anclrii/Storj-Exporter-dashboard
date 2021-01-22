# Storj Exporter dashboard
Storj-exporter Grafana dashboard to visualise [Storj-Exporter](https://github.com/anclrii/Storj-Exporter) metrics for multiple Storj storage nodes. See my [quick-start guide](quick_start/) to set up the whole stack using docker-compose.

<img src="storj-exporter-boom-table.png" alt="0x187C8C43890fe4C91aFabbC62128D383A90548Dd" hight=490 width=490 align="right"/> 

## Support
Feel free to raise issues if you find them and also raise a PR if you'd like to contribute.

<img src="https://github.com/anclrii/Storj-Exporter/raw/master/qr.png" alt="0x187C8C43890fe4C91aFabbC62128D383A90548Dd" hight=100 width=100 align="right"/> 

If you wish to support my work :coffee:, please find my eth wallet address below or scan the qr code:


`0x187C8C43890fe4C91aFabbC62128D383A90548Dd`

## Installation on existing Grafana instance:
Import Storj-Exporter-Boom-Table.json via your Grafana UI ("+" -> Import), select your Prometheus datasorce at the top-left of the dashboard

## Installing full monitoring stack
Monitoring stack required for this dashboard includes the following components:

* [Storj-Exporter](https://github.com/anclrii/Storj-Exporter) - collects storagenode metrics
* [Prometheus](https://prometheus.io/) server - scrapes and stores metrics from Storj-Exporter
* [Grafana](https://grafana.com/) - pulls data from Prometheus and visualises it
    * Storj-Exporter-dashboard - a template dashboard for Storj-Exporter metrics

Here's how metrics flow through the stack:

---

**Storagenode => Storj-Exporter => Prometheus server => Grafana (Storj-Exporter-dashboard)**

---

See my [quick-start guide](quick_start/) to set up the whole stack using docker-compose.

Alternatively check out links below for other ways to install/configure these components:

* Storj-Exporter - https://github.com/anclrii/Storj-Exporter#installation
* Prometheus - https://prometheus.io/docs/prometheus/latest/installation/
* Grafana - https://grafana.com/docs/grafana/latest/installation/

### Ansible role
There's an ansible role [toconspiracy/storj-beastmode](https://github.com/toconspiracy/storj-beastmode/tree/stable) for storagenodes that features all above monitoring components.  
