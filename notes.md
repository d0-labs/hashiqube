# Notes to Self

1. When running HashiQube on M1 Mac, be sure to unset the `DOCKER_DEFAULT_PLATFORM` environment variable, if it is set.

2. The Docker image is available to the host machine via `localhost`. Which means that to use `localhost` subdomains (e.g. `traefik.localhost`), update `/etc/hosts` as follows:

    ```
    # For HashiQube
    127.0.0.1   traefik.localhost
    127.0.0.1   2048-game.localhost
    127.0.0.1   otel-collector-http.localhost
    127.0.0.1   otel-collector-grpc.localhost
    ```

3. We need to expose the gRPC port used by Traefik (i.e. 7233) in our Vagrantfile, in order to send telemetry to the OTel Collector via gRPC, or for any other app which uses gRPC.