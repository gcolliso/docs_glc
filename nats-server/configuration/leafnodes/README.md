# Leaf Nodes

A _Leaf Node_ allows an extension to a cluter or supercluster that bridges accounts and security domains. This is useful in IoT and edge scenarios and when the local server traffic should be low RTT and local unless routed to the super cluster.

Leaf Nodes leverage [accounts](../securing_nats/auth_intro/jwt_auth.md) and JWTs to enable a server to connect to another and filter messages as per the leaf node's account user configuration.

This effectively means that the leaf node clusters with the other server at an account level:

* Leaf nodes clients authenticate locally \(or just connect if authentication is not required\)
* Traffic between the leaf node and the cluster assumes the restrictions of the user configuration used to create the leaf connection. 
  * Subjects that the user is allowed to publish are exported to the cluster. 
  * Subjects the user is allowed to subscribe to, are imported into the leaf node.

> Leaf Nodes are an important component as a way to bridge traffic between local NATS servers you control and servers that are managed by a third-party. Synadia's [NATS Global Service \(NGS\)](https://www.synadia.com/) allows accounts to use leaf nodes, but gain accessibility to the global network to inexpensively connect geographically distributed servers or small clusters.

[LeafNode Configuration Options](leafnode_conf.md)

## LeafNode Configuration Tutorial

Create a new operator called "O":

```text
> nsc add operator -n O
Generated operator key - private key stored "~/.nkeys/O/O.nk"
Success! - added operator "O"
```

Create an account called "A":

```text
> nsc add account -n A
Generated account key - private key stored "~/.nkeys/O/accounts/A/A.nk"
Success! - added account "A"
```

Create an user called "leaf":

```text
> nsc add user -n leaf
Generated user key - private key stored "~/.nkeys/O/accounts/A/users/leaf.nk"
Generated user creds file "~/.nkeys/O/accounts/A/users/leaf.creds"
Success! - added user "leaf" to "A"
```

Let's create an second user called 'nolimit'

```text
> nsc add user -n nolimit
Generated user key - private key stored "~/.nkeys/O/accounts/A/users/nolimit.nk"
Generated user creds file "~/.nkeys/O/accounts/A/users/nolimit.creds"
Success! - added user "nolimit" to "A"
```

Start a nats-account-server:

```text
> nats-account-server -nsc ~/.nsc/nats/O
```

Create the server configuration file \(server.conf\) with the following contents:

```text
operator: /Users/synadia/.nsc/nats/O/O.jwt
resolver: URL(http://localhost:9090/jwt/v1/accounts/)
leafnodes {
    listen: "127.0.0.1:4000"
}
```

The server configuration naturally requires an `operator` and `resolver` to deal with the JWT authentication and accounts. In addition the `leafnodes` configuration exposes a `listen` where the server will receive leaf nodes. In this case on the localhost on port 4000.

Start the nats-server:

```text
> nats-server -c server.conf
```

Create a subscriber on the server:

```text
> nats-sub -creds ~/.nkeys/O/accounts/A/users/nolimit.creds ">"
Listening on [>]
```

Create the leaf server configuration \(leaf.conf\) with the following contents:

```text
port: 4111
leafnodes {
    remotes = [ 
        { 
          url: "nats-leaf://localhost:4000"
          credentials: "/Users/synadia/.nkeys/O/accounts/A/users/leaf.creds"
        },
    ]
}
```

Note the leaf node configuration lists a number of `remotes`. The `url` specifies the port on the server where leaf node connections are allowed. The `credentials` configuration specifies the path to a user's credentials file.

The leaf server configuration \(leaf.conf\) also supports multiple URLs with `urls` such as the following:

```text
port: 4111
leafnodes {
    remotes = [ 
        { 
           urls: ["nats-leaf://host1:4000", "nats-leaf://host2:4000"]
           credentials: "/Users/synadia/.nkeys/O/accounts/A/users/leaf.creds"
        },
    ]
}
```

Create a subscriber on the leaf:

```text
> nats-sub -s localhost:4111 ">"
Listening on [>]
```

Publish a message on the server:

```text
> nats-pub -creds ~/.nkeys/O/accounts/A/users/leaf.creds foo foo
Published [foo] : 'foo'
```

Both the server and leaf subscriber print:

```text

```

Publish a message on the leaf:

```text
> nats-pub -s localhost:4111 bar bar
Published [bar] : 'bar'
```

Both the server and leaf subscribers print:

```text

```

The leaf forwards all local messages to the server where members of the account are able to receive them. Messages published on the server by the account are forwarded to the leaf where subscribers are able to receive them.

## Leaf Authorization

In some cases you may want to restrict what messages can be exported from the leaf node or imported from the account. For leaf servers this is simply a user account configuration, as users can have specific permissions on what subjects to publish and/or subscribe to.

Let's put some restrictions on the `leaf` user so that it can only publish to `foo` and subscribe to `bar`:

```text
> nsc edit user -n leaf --allow-pub foo --allow-sub bar
Updated user creds file "~/.nkeys/O/accounts/A/users/leaf.creds"
Success! - edited user "leaf" in account "A"

-----BEGIN NATS ACCOUNT JWT-----
eyJ0eXAiOiJqd3QiLCJhbGciOiJlZDI1NTE5In0.eyJqdGkiOiJVSk9RTFVSTUVFTVZXQVpVT0E2VlE1UVQ0UEdIV081WktDWlBLVFBJQVpLSldaSTJGNVpRIiwiaWF0IjoxNTU2ODM1MzU4LCJpc3MiOiJBRDU3TUZOQklLTzNBRFU2VktMRkVYQlBVQjdFWlpLU0tVUDdZTzNWVUFJTUlBWUpVNE1EM0NDUiIsIm5hbWUiOiJsZWFmIiwic3ViIjoiVUNEMlpSVUs1UE8yMk02MlNWVTZITzZJS01BVERDUlJYVVVGWDRRU1VTWFdRSDRHU1Y3RDdXVzMiLCJ0eXBlIjoidXNlciIsIm5hdHMiOnsicHViIjp7ImFsbG93IjpbImZvbyJdfSwic3ViIjp7ImFsbG93IjpbImJhciJdfX19.IeqSylTaisMQMH3Ih_0G8LLxoxe0gIClpxTm3B_ys_XwL9TtPIW-M2qdaYQZ_ZmR2glMvYK4EJ6J8RQ1UZdGAg
------END NATS ACCOUNT JWT------

> nsc describe user -n leaf
╭───────────────────────────────────────────╮
│                   User                    │
├─────────────────┬─────────────────────────┤
│ Name            │ leaf                    │
│ User ID         │ UCD2ZRUK5PO2            │
│ Issuer ID       │ AD57MFNBIKO3            │
│ Issued          │ 2019-05-02 22:15:58 UTC │
│ Expires         │                         │
├─────────────────┼─────────────────────────┤
│ Pub Allow       │ foo                     │
│ Sub Allow       │ bar                     │
├─────────────────┼─────────────────────────┤
│ Max Messages    │ Unlimited               │
│ Max Msg Payload │ Unlimited               │
│ Network Src     │ Any                     │
│ Time            │ Any                     │
╰─────────────────┴─────────────────────────╯
```

As we can see on the inspection of the user, the restrictions have been applied.

Let's repeat the experiment. This time we'll restart the leaf server so that the new user configuration is applied:

```text
> nats-server -c leaf.conf
```

You should see a new message on the leaf subscriber:

```text
Reconnected [nats://localhost:4111]
```

Let's publish a message on the leaf:

```text
> nats-pub -s localhost:4111 foo foo
Published [foo] : 'foo'
```

You should see a new message in all your subscriber windows:

```text

```

Now publish a new message on the leaf, but this time with the subject `bar`:

```text
> nats-pub -s localhost:4111 bar bar
Published [bar] : 'bar'
```

This time only the leaf subscriber will print `[#4] Received on [bar]: 'bar'`, the account subscriber won't print it because the leaf user doesn't have permissions to publish on 'bar'.

Let's try the flow of messages from the server to the leaf node:

```text
> nats-pub -creds ~/.nkeys/O/accounts/A/users/leaf.creds foo foo
Published [foo] : 'foo'
```

Only the server subscriber will receive the message as expected.

Repeat the publish this time with 'bar':

```text
> nats-pub -creds ~/.nkeys/O/accounts/A/users/leaf.creds bar bar
Published [bar] : 'bar'
```

Both subscribers will receive the message as expected.

As you can see:

* Messages to and from the leaf node to the server are limited by the user associated with the leaf node connection.
* Messages within the leaf node are as per the server's authentication and authorization configuration

