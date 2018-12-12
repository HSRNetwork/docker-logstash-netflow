# Dockerized Logstash with installed Netflow Plugin

This Docker image can be used to collect Netflow data using Logstash. The default configuration of this image just outputs the received flows and also stores them inside a file.

Pull the image directly from [pschmid/docker-logstash-netflow](https://hub.docker.com/r/pschmid/docker-logstash-netflow/) using the required version tag.

## Prerequisites
- Create a data directory which will be mounted to the Docker container and will be used to store the Netflow data dump file. By default this image uses the host directory `/opt/netflow-data` which will be mounted at `/data` inside the container.
- Change the `logstash.conf` configuration file according to your needs.

## Getting Started
To start the conainer use:
```bash
docker run -it \
-e "TZ=Europe/Zurich" \
-p 9995:9995/udp \
-v /opt/netflow-data:/data \
-v `pwd`/logstash.conf:/srv/logstash.conf \
pschmid/docker-logstash-netflow:latest
```

## Customization
You can customize the collection of netflow data by using [Netflow Codec Configuraiton Options](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-netflow.html#plugins-codecs-netflow-options).

### Logstash Start Command
This Docker image runs `logstash -f /srv/logstash.conf` by default. Override this `CMD` if you are using another logstash configuration file/path or would like to add some additional logstash command parameters.

### Netflow Definitions
Since there is no default value for the `netflow_definitions` provided by the [Netflow codec plugin](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-netflow.html), I added a default `netflow_definitions` configuration file inside this Docker image (from [logstash-plugins/logstash-codec-netflow](https://raw.githubusercontent.com/logstash-plugins/logstash-codec-netflow/master/lib/logstash/codecs/netflow/netflow.yaml)). To change this configuration simply "override" the definition by mounting a Docker volume at the same destination path. To achive this, use `-v /your/path/your_netflow.yaml:/srv/netflow.yaml`.
