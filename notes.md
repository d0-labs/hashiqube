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

3. We need to expose the ports used by our apps to the Dockerfile. 

    For example, for the OTel Collector, we need `4317` and `4318`. We would normally set these ports in the job spec like this:

    ```h
    port "otlp" {
        to = 4317
    }
    ```

    Nomad would assign a dynamic port on the container end, and would map it to host port `4317`. But we need to know both ports, so that we can expose it in our dockerfile (via Vagrantfile). Do do this, we must modify it as follows:

    ```h
    port "otlp" {
        static = 4317
        to = 4317
    }
    ```

    This also means that our OTel Collector is available:
    * Via gRPC at `otel-collector-grpc.localhost:4317`
    * Via HTTP at `otel-collector-grpc.localhost:4318`

    In Traefik, we have configured gRPC to go through `7233`, but because we're specifying both `to` and `static` to go to `4317`, the `7233` config is ignored.