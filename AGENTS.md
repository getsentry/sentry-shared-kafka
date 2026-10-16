# AGENTS.md

devservices config for the shared local Kafka broker. All config is in `devservices/config.yml`. User docs are in `README.md`.

## Check

CI runs `getsentry/action-validate-devservices-config` when `devservices/`, `requirements-dev.txt`, or `.github/workflows/ci.yml` changes. Run the same check locally:

```sh
pip install -r requirements-dev.txt
devservices up
devservices status
devservices down
```

## Invariants

- Other repos connect by the hostnames `kafka` and `kafka-kafka-1` on ports 9092, 9093, and 9094. A change to the service name, a listener, or a port breaks those repos.
