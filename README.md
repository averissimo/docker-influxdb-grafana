## Fork from @averissimo

> You probably don't wanna be here! see [nicolargo/docker-influxdb-grafana](https://github.com/nicolargo/docker-influxdb-grafana)

Personal notes:

Had to build my own grafana with plugins already installed.

This way it doesn't crash the docker image if there's no internet connection.

For that:

* `git clone https://github.com/grafana/grafana/ grafana-with-built-plugins`
* `cd grafana-with-built-plugins/packaging/docker/custom`
* Run command bellow *(network=host argument is needed as otherwise the container doesn't have internet.. at least in my pi)*

```
$ docker build --platform arm64 -t grafana:latest-with-plugins \
  --build-arg "GRAFANA_VERSION=master" \
  --build-arg GF_INSTALL_PLUGINS=grafana-clock-panel,briangann-gauge-panel,natel-plotly-panel,grafana-simple-json-datasource \
  --network=host .
```

---

# Docker-compose files for a simple uptodate
# InfluxDB
# + Grafana stack
# + Telegraf

Get the stack (only once):

```
git clone https://github.com/nicolargo/docker-influxdb-grafana.git
cd docker-influxdb-grafana
docker pull grafana/grafana
docker pull influxdb
docker pull telegraf
```

Run your stack:

```
sudo mkdir -p /srv/docker/grafana/data
docker-compose up -d
sudo chown -R 472:472 /srv/docker/grafana/data

```

Show me the logs:

```
docker-compose logs
```

Stop it:

```
docker-compose stop
docker-compose rm
```

Update it:

```
git pull
docker pull grafana/grafana
docker pull influxdb
docker pull telegraf
```

If you want to run Telegraf, edit the telegraf.conf to yours needs and:

```
docker exec telegraf telegraf
```
