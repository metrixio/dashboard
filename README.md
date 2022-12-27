# Metrics dashboard

Dashboard is a collection of pre-made Grafana dashboards that let you track and analyze data from Twitter, GitHub, Docker, and Packagist. 

It's super handy for developers and other professionals who need to keep an eye on certain metrics, but don't want to spend time building their own dashboards from scratch. 

All you have to do is clone the repository and choose the metrics collectors you want to use, and you'll be able to see your data in real-time. Plus, with Grafana, you can create dashboards that show live data and set up alerts to let you know if anything changes or seems off.

[<img src="https://user-images.githubusercontent.com/773481/209725543-b60baee8-ec48-4d51-bf73-94e9c62151b5.gif" width="50%">](https://youtu.be/DtZELp2sok4)
> View full video on [youtube](https://youtu.be/DtZELp2sok4)

## Requirements

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Installation

Clone the repository:

```bash
git clone https://github.com/metrixio/dashboard.git

cd ./dashboard

chmod 0777 runtime/ -R
```

## Configuration

To configure the metrics collectors you want to use, you will need to edit the `docker-compose.yaml` file and add the appropriate service definitions for the collectors you want to use.

### Twitter

![twitter](https://user-images.githubusercontent.com/773481/209433204-d3a5efb4-80f8-495b-bfbf-f4806f4d094b.png)

To use the twitter collector, you will need to have a Twitter developer account and create
a [Twitter API credentials](https://developer.twitter.com/en/docs/basics/authentication/guides/access-tokens.html).
Once you have obtained your API key, you can start collecting data. Then you just need to specify the list of
account IDS to follow in `TWITTER_ACCOUNTS` environment variable.

> **Note**:
> You can find out the account ID via some of the services like - [tweeterid.com](https://tweeterid.com/).

```yaml
services:
  twitter-metrics:
    image: ghcr.io/metrixio/twitter:latest
    environment:
      TWITTER_CONSUMER_KEY: xxx
      TWITTER_CONSUMER_SECRET: xxx
      TWITTER_ACCESS_TOKEN: xxx
      TWITTER_ACCESS_TOKEN_SECRET: xxx
      TWITTER_ACCOUNTS: 17227608,25073877,783214
    restart: on-failure

  ...
```

Configure the Prometheus server to scrape metrics from the Twitter metrics collector in `prometheus/prometheus.yml`
file:

```yaml
scrape_configs:
  - job_name: 'twitter'
    scrape_interval: 60s
    static_configs:
      - targets: [ 'twitter-metrics:2112' ]
  ...
```

Put dashboards you want to use in `grafana/provisioning/dashboards` directory from
the [dashboards repository](https://github.com/metrixio/twitter/tree/master/grafana)

### Docker

![docker](https://user-images.githubusercontent.com/773481/209433247-decbb4f6-e722-4862-8063-d4e4f0bf3c29.png)

To use the docker collector, you just need to specify the list of repositories to follow in `DOCKER_REPOSITORIES`
environment variable:

```yaml
services:
  docker-metrics:
    image: ghcr.io/metrixio/docker:latest
    environment:
      DOCKER_REPOSITORIES: spiralscout/roadrunner,butschster/buggregator
    restart: on-failure

  ...
```

Configure the Prometheus server to scrape metrics from the Twitter metrics collector in `prometheus/prometheus.yml`
file:

```yaml
scrape_configs:
  - job_name: 'docker'
    scrape_interval: 60s
    static_configs:
      - targets: [ 'docker-metrics:2112' ]
  ...
```

Put dashboards you want to use in `grafana/provisioning/dashboards` directory from
the [dashboards repository](https://github.com/metrixio/docker/tree/master/grafana)

### Gituhb public

![github](https://user-images.githubusercontent.com/773481/209463759-1a359047-3263-454b-b8ae-3444b5102bc8.png)

To use the package, you just need to create a Github API token. Once you have
obtained your API token, you can use the package's functions to authenticate and start collecting data.

Then you need to specify the list of repositories to follow in `GITHUB_REPOSITORIES` environment variable:

```yaml
services:
  github-public-metrics:
    image: ghcr.io/metrixio/github-public:latest
    environment:
      GITHUB_TOKEN: xxx
      GITHUB_REPOSITORIES: spiral/framework,spiral/roadrunner
    restart: on-failure

  ...
```

Configure the Prometheus server to scrape metrics from the Twitter metrics collector in `prometheus/prometheus.yml`
file:

```yaml
scrape_configs:
  - job_name: 'github-public'
    scrape_interval: 60s
    static_configs:
      - targets: [ 'github-public-metrics:2112' ]
  ...
```

Put dashboards you want to use in `grafana/provisioning/dashboards` directory from
the [dashboards repository](https://github.com/metrixio/github-public/tree/master/grafana)

### Packagist

![packagist](https://user-images.githubusercontent.com/773481/209584409-3275bfa7-f131-44de-b4c1-341d4b0cd3d3.png)

To use the packagist collector, you just need to specify the list of repositories to follow in `PACKAGIST_REPOSITORIES`
environment variable:

```yaml
services:
  packagist-metrics:
    image: ghcr.io/metrixio/packagist:latest
    environment:
      PACKAGIST_REPOSITORIES: spiral/framework
    restart: on-failure

  ...
```

Configure the Prometheus server to scrape metrics from the packagist metrics collector in `prometheus/prometheus.yml`
file:

```yaml
scrape_configs:
  - job_name: 'docker'
    scrape_interval: 60s
    static_configs:
      - targets: [ 'packagist-metrics:2112' ]
  ...
```

Put dashboards you want to use in `grafana/provisioning/dashboards` directory from
the [dashboards repository](https://github.com/metrixio/packagist/tree/master/grafana)


## Usage

```bash
docker compose up
```

Metrics will be available on http://127.0.0.1:3000

Default username: **admin**
Default password: **secret**


# Enjoy!
