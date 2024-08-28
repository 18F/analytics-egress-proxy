# Analytics Egress Proxy

A Caddy webserver implementation of a forward proxy to provide a controlled
egress for HTTP calls to the public internet for analytics.usa.gov project
components.

This repository is adapted from the [cg-egress-proxy](https://github.com/GSA-TTS/cg-egress-proxy)
project. See the linked repository for the original source along with detailed
docs around how the server is implemented.

The process for adding features to this repository and deployment of the server
is described in [Development and deployment process](docs/development_and_deployment_process.md).

## Starting the server

```bash
exec ./caddy run --config ./Caddyfile
```

## Configuring the server

The configuration for the server is defined in the `./Caddyfile`. See
[Caddy documentation](https://caddyserver.com/docs/caddyfile) for available
options. The Caddy binary is also compiled with the [forward_proxy plugin](https://github.com/caddyserver/forwardproxy)
which adds some additional configuration options.

## Cloud.gov setup

The proxy should be deployed to a Cloudfoundry space which has
public_networks_egress and trusted_local_networks_egress security groups. This
allows the egress to communicate to internal routes as well as the public
internet. Application instances which communicate to the proxy should be in
spaces which have only trusted_local_networks_egress security group to ensure
that the application only makes requests to the public internet through the
egress proxy.

Example setup for the egress proxy and space below use the Cloudfoundry CLI.

```bash
# Add public egress and trusted local egress permissions for the space running
# the proxy server if needed
cf bind-security-group public_networks_egress gsa-opp-analytics analytics-public-egress --lifecycle running
cf bind-security-group trusted_local_networks_egress gsa-opp-analytics analytics-public-egress --lifecycle running

# Deploy the egress proxy via CI to create the app and the internal app route
# for the proxy

# Setup network policies to allow communication between the public egress space
# and the application space for responses to proxy requests. The application
# requests may come from any randomly assigned port, so allow the full range of
# ports.
cf add-network-policy analytics-egress-proxy-dev analytics-reporter-develop -s analytics-dev -o gsa-opp-analytics --protocol tcp --port 1-65535

# Application components should add network-policies from their space which has
# only the trusted local egress security group to the proxy space for the proxy
# port 8080.
cf target -s analytics-dev
cf add-network-policy analytics-reporter-develop analytics-egress-proxy-dev -s analytics-public-egress -o gsa-opp-analytics --protocol tcp --port 8080
```

## Public domain

This project is in the worldwide [public domain](LICENSE.md). As stated in
[CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright
> and related rights in the work worldwide are waived through the
> [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication.
> By submitting a pull request, you are agreeing to comply with this waiver of
> copyright interest.
