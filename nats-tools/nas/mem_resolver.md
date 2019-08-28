# Memory Resolver

The `MEMORY` resolver is a built-in resolver for JWTs. It is mostly used by test setups but can be used to test the simplest of environments where there is one or very few accounts, and the account JWTs don't change often.

The basic configuration for the server requires:

* The operator JWT
* `resolver` set to `MEMORY`
* `resolver_preload` set to an object where account public keys are mapped to account JWTs.

## Create Required Entities

Let's create the setup:

```text
> nsc add operator -n memory
Generated operator key - private key stored "~/.nkeys/memory/memory.nk"
Success! - added operator "memory"

> nsc add account --name A
Generated account key - private key stored "~/.nkeys/memory/accounts/A/A.nk"
Success! - added account "A"

> nsc describe account -W
╭──────────────────────────────────────────────────────────────────────────────────────╮
│                                   Account Details                                    │
├───────────────────────────┬──────────────────────────────────────────────────────────┤
│ Name                      │ A                                                        │
│ Account ID                │ ACSU3Q6LTLBVLGAQUONAGXJHVNWGSKKAUA7IY5TB4Z7PLEKSR5O6JTGR │
│ Issuer ID                 │ ODWZJ2KAPF76WOWMPCJF6BY4QIPLTUIY4JIBLU4K3YDG3GHIWBVWBHUZ │
│ Issued                    │ 2019-04-30 20:21:34 UTC                                  │
│ Expires                   │                                                          │
├───────────────────────────┼──────────────────────────────────────────────────────────┤
│ Max Connections           │ Unlimited                                                │
│ Max Leaf Node Connections │ Unlimited                                                │
│ Max Data                  │ Unlimited                                                │
│ Max Exports               │ Unlimited                                                │
│ Max Imports               │ Unlimited                                                │
│ Max Msg Payload           │ Unlimited                                                │
│ Max Subscriptions         │ Unlimited                                                │
│ Exports Allows Wildcards  │ True                                                     │
├───────────────────────────┼──────────────────────────────────────────────────────────┤
│ Imports                   │ None                                                     │
│ Exports                   │ None                                                     │
╰───────────────────────────┴──────────────────────────────────────────────────────────╯

> nsc add user --name TA
Generated user key - private key stored "~/.nkeys/memory/accounts/A/users/TA.nk"
Generated user creds file "~/.nkeys/memory/accounts/A/users/TA.creds"
Success! - added user "TA" to "A"
```

## Create the Server Config

The `nsc` tool can generate a configuration file automatically. You provide a path to the server configuration and operator jwt. The `nsc` tool will copy the operator JWT to the file specified, and generate the server config for you:

```text
> nsc generate config --mem-resolver --config-file /tmp/server.conf --operator-jwt /tmp/memory.jwt
Success!! - generated "/tmp/server.conf"
            generated "/tmp/memory.jwt"
```

If you require additional settings, you may want to consider using [`include`](../../nats-server/configuration/#include-directive) in your main configuration, to reference the generated files. Otherwise, you can start a server and reference the generated configuration:

```text
> nats-server -c /tmp/server.conf
```

You can then [test it](mem_resolver.md#testing-the-configuration).

## Manual Server Config

While generating a configuration file is easy, you may want to craft one by hand to know the details. With the entities created, and a standard location for the `.nsc` directory. You can reference the operator JWT and the account JWT in a server configuration. Remember that your configuration will be in `$NSC_HOME/nats/<operator_name>/<operator_name>.jwt` for the operator. The account JWT will be in `$NSC_HOME/nats/<operator_name>/accounts/<account_name>/<account_name>.jwt`

For the configuration you'll need:

* The path to the operator JWT
* A copy of the contents of the account JWT file

The format of the file is:

```text
operator: <path to the operator jwt>
resolver: MEMORY
resolver_preload: {
    <public key for an account>: <contents of the account jwt>
    ### add as many accounts as you want
    ...
}
```

In this example this translates to:

```text
operator: /Users/synadia/.nsc/nats/memory/memory.jwt
resolver: MEMORY
resolver_preload: {
ACSU3Q6LTLBVLGAQUONAGXJHVNWGSKKAUA7IY5TB4Z7PLEKSR5O6JTGR: eyJ0eXAiOiJqd3QiLCJhbGciOiJlZDI1NTE5In0.eyJqdGkiOiJPRFhJSVI2Wlg1Q1AzMlFJTFczWFBENEtTSDYzUFNNSEZHUkpaT05DR1RLVVBISlRLQ0JBIiwiaWF0IjoxNTU2NjU1Njk0LCJpc3MiOiJPRFdaSjJLQVBGNzZXT1dNUENKRjZCWTRRSVBMVFVJWTRKSUJMVTRLM1lERzNHSElXQlZXQkhVWiIsIm5hbWUiOiJBIiwic3ViIjoiQUNTVTNRNkxUTEJWTEdBUVVPTkFHWEpIVk5XR1NLS0FVQTdJWTVUQjRaN1BMRUtTUjVPNkpUR1IiLCJ0eXBlIjoiYWNjb3VudCIsIm5hdHMiOnsibGltaXRzIjp7InN1YnMiOi0xLCJjb25uIjotMSwibGVhZiI6LTEsImltcG9ydHMiOi0xLCJleHBvcnRzIjotMSwiZGF0YSI6LTEsInBheWxvYWQiOi0xLCJ3aWxkY2FyZHMiOnRydWV9fX0._WW5C1triCh8a4jhyBxEZZP8RJ17pINS8qLzz-01o6zbz1uZfTOJGvwSTS6Yv2_849B9iUXSd-8kp1iMXHdoBA
}
```

Save the config at server.conf and start the server:

```text
> nats-server -c server.conf
```

You can then [test it](mem_resolver.md#testing-the-configuration).

## Testing the Configuration

To test the configuration, simply use one of the standard tools:

```text
> nats-pub -creds ~/.nkeys/memory/accounts/A/users/TA.creds hello world
Published [hello] : 'world'
```

