# sentry-shared-kafka

A single-node Kafka broker that Sentry services share in local developer environments. [devservices](https://github.com/getsentry/devservices) runs it from [`devservices/config.yml`](devservices/config.yml) as the service `kafka`.

## Use it from your service

Add it as a remote dependency in your service's `devservices/config.yml`:

```yaml
x-sentry-service-config:
  dependencies:
    kafka:
      description: Shared instance of kafka used by sentry services
      remote:
        repo_name: sentry-shared-kafka
        branch: main
        repo_link: https://github.com/getsentry/sentry-shared-kafka.git
```

Then list `kafka` in the modes that need it. `devservices up` starts the broker before your service.

## Connect to the broker

| Client runs on | Bootstrap server |
| --- | --- |
| Your machine | `127.0.0.1:9092` |
| A container on the `devservices` network | `kafka:9093` |
| A container on the `devservices` network that needs the container name | `kafka-kafka-1:9094` |

## Defaults

- Runs in KRaft mode with one combined broker and controller. There is no ZooKeeper.
- Offsets topic has 1 partition and replication factor 1.
- Retains logs for 24 hours.
- Accepts messages up to 50 MB (`KAFKA_MESSAGE_MAX_BYTES`).
- Stores data in the `kafka-data` Docker volume.
