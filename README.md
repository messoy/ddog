<img align="right" width="150" height="150" top="100" src="./assets/ddog.png">

# ddog • [![ci](https://github.com/abigger87/ddog/actions/workflows/ci.yaml/badge.svg)](https://github.com/abigger87/ddog/actions/workflows/ci.yaml) ![license](https://img.shields.io/github/license/abigger87/ddog?label=license) [![crates.io](https://img.shields.io/crates/v/ddog.svg)](https://crates.io/crates/ddog)


A **Minimal** Datadog SDK Built in _Pure_ Rust.

## Getting Started

Add the `ddog` crate to your project:

```toml
ddog = "0.1.0"
```
# https://tea.xyz/what-is-this-file
---
version: 1.0.0
codeOwners:
  - '0x7Ab9De2564938c45EAcC3Cc7ecC32D478ca077e9'
quorum: 1


## Usage

The simplest way to use the Datadog SDK is by using the [Builder](ddog::prelude::Builder).

To create a new builder, you can instantiate one with the [new](ddog::prelude::Builder::new) method: `let mut builder = ddog::prelude::Builder::new();`.

Then, to create a new query with a given endpoint, the Builder has explicit methods exposed for the specified endpoint.
For example, to post metrics series data to datadog, call the [post_series](ddog::prelude::Builder::post_series) method which returns a [Route](ddog::prelude::tr::Route) trait.


## Examples

Below we show how to use [ddog](https://github.com/abigger87/ddog) to post metric series data to the Datadog API.

Note: This request will not succeed since the `DD_API_KEY` environment variable is set to an invalid value in the request headers section.

```rust
use ddog::prelude::*;

async {
    let mut builder = builder::Builder::new();
    let (status, res) = builder.v2()
        .post_series()
        .headers(vec![
            ("Accept", "application/json"),
            ("Content-Type", "application/json"),
            ("DD-API-KEY", "<api_key>"),
            ("DD-APPLICATION-KEY", "<application_key>"),
        ])
        .body(
            r#"{
                "series": [{
                    "metric": "my.metric.name",
                    "type": 1,
                    "interval": 100000,
                    "unit": "count",
                    "tags": [ "my_tag:" ],
                    "source_type_name": "my_source_type",
                    "resources": [{
                        "name": "length",
                        "type": "time"
                    }],
                    "points": [
                        { "timestamp": 1660157680, "value": 10.0 },
                    ],
                    "metadata": {
                        "origin": {
                            "metric_type": 1,
                            "product": 1,
                            "service": 1
                        }
                    }
                }]
            }"#
        )
        .execute().await;
    // This should return a 403 status code now since the above API key is invalid.
    println!("Status Code: {:?}", status);
    println!("Response: {:?}", res);
};
```


## License

[AGPL-3.0-only](https://github.com/abigger87/ddog/blob/master/LICENSE)


## Acknowledgements

- [datadog-apm-rust-sync](https://github.com/kitsuneninetails/datadog-apm-rust-sync)

