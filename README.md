# cg-egress-proxy

A Caddy webserver implementation of a forward proxy to provide an egress for
HTTP calls to the public internet for analytics.usa.gov project components.

The original source for this project is here: [cg-egress-proxy](https://github.com/GSA-TTS/cg-egress-proxy)

## Starting the server

```bash
exec ./caddy run --config ./Caddyfile
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
