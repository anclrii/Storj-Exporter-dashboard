# A quick-start guide

This is a guide for minimal setup of the monitoring stack using docker-compose. 

## Overview

Monitoring stack includes the following components:

* [Storj-Exporter](https://github.com/anclrii/Storj-Exporter) - collects storagenode metrics via storj api and exposes them on port 9651 for Prometheus server to scrape
* [Prometheus](https://prometheus.io/) - scrapes metrics from all configured `targets` and stores in the internal database and keeps last 15 days of historic data, exposes an api interface (datasource) for Grafana to pull historic data and visualise it
* [Grafana](https://grafana.com/) - pulls data from the Prometheus datasource to populate Storj-Exporter-dashboard panels
    * Storj-Exporter-dashboard - a template for Grafana with some preset panels for Storj-Exporter metrics

Here's how metrics flow through the stack:

---

**Storagenode => Storj-Exporter => Prometheus server => Grafana (Storj-Exporter-dashboard)**

---

## Prerequisites

* Storj-exporter running alongside each storagenode (see https://github.com/anclrii/Storj-Exporter#installation)
* A host with Docker installed that will run Prometheus/Grafana (can be the same host running the storagenode)
    * Host needs to have network connectivity to Storj-Exporter ip:port 
        * check with curl (replace `storagenode1.example.com:9651` with your Storj-Exporter ip:port):
        ```
        # curl -s storagenode1.example.com:9651/metrics
        ...
        # TYPE storj_sat_month_ingress gauge
        storj_sat_month_ingress{satellite="12rfG3sh9NCWiX3ivPjq2HtdLmbqCrvHVEzJubnzFzosMuawymB",type="repair",url="europe-north-1.tardigrade.io:7777"} 2.362392576e+09
        storj_sat_month_ingress{satellite="12rfG3sh9NCWiX3ivPjq2HtdLmbqCrvHVEzJubnzFzosMuawymB",type="usage",url="europe-north-1.tardigrade.io:7777"} 5.6985088e+07
        ```
    * Docker-compose installed on the host https://docs.docker.com/compose/install/
    * A target filesystem/directory with sufficient space to hold persistent monitoring data for Prometheus/Grafana
    * ~ 200MB free ram available

## Setup

Follow these steps to get the setup running:

* Create directories for persistent storage and allow anyone to modify them.
```
mkdir /<edit_this_path_to>/prometheus /<edit_this_path_to>/grafana
chmod 777 /<edit_this_path_to>/prometheus /<edit_this_path_to>/grafana
```
* Clone Storj-Exporter-dashboard repo `git clone https://github.com/anclrii/Storj-Exporter-dashboard.git`
* Go to quick_start directory `cd Storj-Exporter-dashboard/quick_start`    
* Edit environment variables passed to docker-compose `vim .env` or any text editor, make sure to change the `edit_this_path_to` part to point to the directories created before. Also change the `GF_SECURITY_ADMIN_PASSWORD` that will be used to login to Grafana web ui.
* Edit the Prometheus config `vim prometheus/prometheus.yml` - ` - targets:` at the end of the file. Replace `storj-exporter1:9651` with your storj-exporter address:port and add more targets as needed.
* That's all set, now run `docker-compose up -d` to bring up the containers.

Open http://localhost:3000 in your browser to log on to Grafana. Use host ip instead of `localhost` if you're not on the same host. Use username `admin` and `GF_SECURITY_ADMIN_PASSWORD` as set in `.env`. You will see a link to `Storj-Exporter-Boom-Table` dashboard on the home page. If all set correctly the dashboard will start showing data shortly after a few minutes as Prometheus collects metrics from exporters.

## Security considerations

Objective of this guide is to provide a working example setup of the monitoring stack that can be used with storj-exporter. These steps do not address security of the setup in any way. You might need to adjust permissions for `/<edit_this_path_to>/prometheus /<edit_this_path_to>/grafana` directories and configure firewalls for the new ports `3000` and `9090` etc.