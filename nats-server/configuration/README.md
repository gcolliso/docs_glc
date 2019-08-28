# Configuration

While the NATS server has many flags that allow for simple testing of features, the NATS server products provide a flexible configuration format that combines the best of traditional formats and newer styles such as JSON and YAML.

The NATS configuration file supports the following syntax:

* Lines can be commented with `#` and `//`
* Values can be assigned to properties with:
  * Equals sign: `foo = 2`
  * Colon: `foo: 2`
  * Whitespace: `foo 2`
* Arrays are enclosed in brackets: `["a", "b", "c"]`
* Maps are enclosed in braces: `{foo: 2}`
* Maps can be assigned with no key separator
* Semicolons can be used as terminators

## Strings and Numbers

The configuration parser is very forgiving, as you have seen:

* values can be a primitive, or a list, or a map
* strings and numbers typically do the right thing

String values that start with a digit _can_ create issues. To force such values as strings, quote them.

_BAD Config_:

```text
listen: 127.0.0.1:4222
authorization: {
    # BAD!
    token: 3secret
}
```

Fixed Config:

```text
listen: 127.0.0.1:4222
authorization: {
    token: "3secret"
}
```

## Variables

Server configurations can specify variables. Variables allow you to reference a value from one or more sections in the configuration.

Variables:

* Are block-scoped
* Are referenced with a `$` prefix.
* Can be resolved from environment variables having the same name

> If the environment variable value begins with a number you may have trouble resolving it depending on the server version you are running.

```text
# Define a variable in the config
TOKEN: "secret"

# Reference the variable
authorization {
    token: $TOKEN
}
```

A similar configuration, but this time, the value is in the environment:

```text
# TOKEN is defined in the environment
authorization {
    token: $TOKEN
}
```

export TOKEN="hello"; nats-server -c /config/file

## Include Directive

The `include` directive allows you to split a server configuration into several files. This is useful for separating configuration into chunks that you can easily reuse between different servers.

Includes _must_ use relative paths, and are relative to the main configuration \(the one specified via the `-c` option\):

server.conf:

```text
listen: 127.0.0.1:4222
include ./auth.conf
```

> Note that `include` is not followed by `=` or `:`, as it is a _directive_.

auth.conf:

```text
authorization: {
    token: "f0oBar"
}
```

```text
> nats-server -c server.conf
```

## Configuration Properties

| Property | Description |
| :--- | :--- |
| [`authorization`](securing_nats/auth_intro/) | configuration map for client authentication/authorization |
| [`cluster`](clustering/cluster_config.md) | configuration map for clustering configuration |
| `debug` | if `true` enable debug log messages |
| [`gateway`](gateways/gateway.md) | Gateway configuration map |
| `host` | host for client connections |
| [`http_port`](monitoring.md) | http port for server monitoring |
| [`https_port`](monitoring.md) | https port for server monitoring |
| [`leafnode`](leafnodes/leafnode_conf.md) | Leafnode configuration map |
| `listen` | host/port for client connections |
| `max_connections` | Maximum number of active client connections |
| `max_control_line` | Maximum length of a protocol line \(including subject length\) |
| `max_payload` | Maximum number of bytes in a message payload |
| `max_pending` | Maximum number of bytes buffered for a connection |
| `max_subscriptions` | Maximum numbers of subscriptions for a client connection |
| [`operator`](../../nats-tools/nsc/nsc.md#nats-server-configuration) | path to an operator JWT |
| [`ping_interval`](../../developing-with-nats/intro/pingpong.md) | Interval in seconds in which the server checks if a connection is active |
| `port` | port for client connections |
| [`resolver`](../../nats-tools/nsc/nsc.md#nats-server-configuration) | Resolver type `MEMORY` or `URL` for account JWTs |
| [`tls`](securing_nats/tls.md#tls-configuration) | configuration map for tls for client and http monitoring |
| `trace` | if `true` enable protocol trace log messages |
| `write_deadline` | Maximum number of seconds the server will block when writing a to a client \(slow consumer\) |

## Configuration Reloading

A server can reload most configuration changes without requiring a server restart or clients to disconnect by sending the nats-server a [signal](../nats_admin/signals.md):

```text
> nats-server --signal reload
```

