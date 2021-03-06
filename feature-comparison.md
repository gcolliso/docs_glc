---
description: 'NATS Comparison to Kafka, Rabbit, gRPC, and others'
---

# Compare NATS

This feature comparison is a summary of a few of the major components in several of the popular messaging technologies of today. This is by no means an exhaustive list and each technology should be investigated thoroughly to decide which will work best for your implementation.

**Language & Platform Coverage**

**NATS**

Core NATS: 48 known client types, 11 supported by maintainers, 18 contributed by the community. NATS Streaming: 7 client types supported by maintainers, 4 contributed by the community. NATS servers can be compiled on architectures supported by Golang. NATS provides binary distributions.

**Kafka**

18 client types supported across the community and by Confluent. Kafka servers can run on platforms supporting java; very wide support.

**Rabbit**

At least 10 client platforms that are maintainer-supported with over 50 community supported client types. Servers are supported on the following platforms: Linux Windows, NT

**Pulsar**

7 client languages, 5 third-party clients - tested on macOS and Linux

**gRPC**

13 client languages

**Built in Patterns**

**NATS**

Streams and Services through built in publish/subscribe, request/reply, and load balanced queue subscriber patterns. Dynamic request permissioning and request subject obfuscation is supported.

**Kafka**

Streams through publish/subscribe. Load balancing can be achieved with consumer groups. Application code must correlate requests with replies over multiple topics for a service \(request/reply\) pattern.

**Rabbit**

Streams through publish/subscribe, and services with a direct reply-to feature. Load balancing can be achieved with a Work Queue. Applications must correlate requests with replies over multiple topics for a service \(request/reply\) pattern.

**Pulsar**

Streams through publish/subscribe. Multiple competing consumer patterns support load balancing. Application code must correlate requests with replies over multiple topics for a service \(request/reply\) pattern.

**gRPC**

One service, which may have streaming semantics, per channel. Load balancing for a service can be done either client-side or by using a proxy.

**Delivery Guarantees**

**NATS**

At most once, at least once, and exactly once is available in Jetstream.

**Kafka**

At least once, exactly once

**Rabbit**

At most once, at least once

**Pulsar**

At most once, at least once, and exactly once

**gRPC**

At most once

**Multitenancy and Sharing**

**NATS**

NATS supports true multi-tenancy and decentralized security through accounts and defining shared streams and services.

**Kafka**

Multitenancy is not supported.

**Rabbit**

Multitenancy is supported with vhosts; data sharing is not supported.

**Pulsar**

Multitenancy is implemented through tenants; built-in data sharing across tenants is not supported. Each tenant can have it’s own authentication and authorization scheme.

**gRPC**

N/A

**AuthN**

**NATS**

NATS supports TLS, NATS credentials, NKEYS \(NATS ED25519 keys\), username and password, or simple token.

**Kafka**

Supports Kerberos and TLS. Supports JAAS and an out-of-box authorizer implementation that uses ZooKeeper to store connection and subject.

**Rabbit**

TLS, SASL, username and password, and pluggable authorization.

**Pulsar**

TLS Authentication, Athenz, Kerberos, JSON Web Token Authentication.

**gRPC**

TLS, ALT, Token, channel and call credentials, and a plug-in mechanism.

**AuthZ**

**NATS**

Account limits including \# of connections, message size, \# of imports and exports. User level publish and subscribe permissions, connection restrictions, CIDR address restrictions, and time of day restrictions.

**Kafka**

Supports JAAS, ACLs for a rich set of Kafka resources including topics, clusters, groups and others.

**Rabbit**

ACLs dictate permissions for configure, write and read operations on resources like exchanges, queues, transactions, and others. Authentication is pluggable.

**Pulsar**

Permissions may be granted to specific roles for lists of operations such as produce and consume.

**gRPC**

Users can configure call credentials to authorize fine grained individual calls on a service.

**Message Retention and Persistence**

**NATS**

Supports memory, file, and database persistence. Messages can be replayed by time, count, or sequence number, and durable subscriptions are supported. With NATS streaming, scripts can archive old log segments to cold storage.

**Kafka**

Supports file based persistence. Messages can be replayed by specifying an offset, and durable subscriptions are supported. Log compaction is supported as well as KSQL.

**Rabbit**

Supports file based persistence. Rabbit supported queue based semantics \(vs log\), so no message replay is available.

**Pulsar**

Supports tiered storage including file, Amazon S3 or Google Cloud Storage \(GCS\). Pulsar can replay messages from a specific position and supports durable subscriptions. Pulsar SQL and topic compaction is supported, as well as Pulsar functions.

**gRPC**

N/A

**HA/FT**

**NATS**

Core NATS supports full mesh clustering with self-healing features to provide high availability to clients. NATS streaming has warm failover backup servers with two modes \(FT and full clustering\). Jetstream will support horizontal scalability with built-in mirroring.

**Kafka**

Fully replicated cluster members are coordinated via zookeeper.

**Rabbit**

Clustering Support with full data replication via federation plugins. Clusters require low-latency networks where network partitions are rare.

**Pulsar**

Pulsar supports clustered brokers with geo-replication.

**gRPC**

N/A. gRPC relies on external resources for HA/FT.

**Deployment**

**NATS**

The NATS network element \(server\) is a small static binary that can be deployed anywhere from large instances in the cloud to resource constrained devices like a Raspberry PI.

NATS supports the Adaptive Edge architecture which allows for large, flexible deployments. Single servers, leaf nodes, clusters, and superclusters \(cluster of clusters\) can be combined in any fashion for an extremely flexible deployment amenable to cloud, on-premise, edge and IoT. Clients are unaware of topology and can connect to any NATS server in a deployment.

**Kafka**

Kafka supports clustering with mirroring to loosely coupled remote clusters. Clients are tied to partitions defined within clusters. Kafka servers require a JVM, eight cores, 64 GB to128 GB of RAM, two or more 8-TB SAS/SSD disks, and a 10-Gig NIC.[\[1\]](feature-comparison.md)

**Rabbit**

Rabbit supports clusters and cross cluster message propagation through a federation plugin. Clients are unaware of topology and may connect to any cluster. The server requires the Erlang VM and dependencies.

**Pulsar**

Pulsar supports clustering and built-in geo-replication between clusters. Clients may connect to any cluster with an appropriately configured tenant and namespace. Pulsar requires a JVM and requires at least 6 Linux machines or VMs. 3 running ZooKeeper. 3 running a Pulsar broker and a BookKeeper bookie.[\[2\]](feature-comparison.md)

**gRPC**

gRPC is point to point and does not have a server or broker to deploy or manage but always requires additional pieces for production deployments.

**Monitoring**

**NATS**

NATS supports exporting monitoring data to Prometheus and has Grafana dashboards to monitor and configure alerts. There are also development monitoring tools such as nats-top. Robust side car deployment or a simple connect-and-view model with NATS surveyor is supported.

**Kafka**

Kafka has a number of management tools and consoles including Confluent Control Center, Kafka, Kafka Web Console, Kafka Offset Monitor.

**Rabbit**

CLI tools, a plugin-based management system with dashboards and third-party tools.

**Pulsar**

CLI tools, per-topic dashboards, and third-party tools.

**gRPC**

External components such as a service mesh are required to monitor gRPC.

**Management**

**NATS**

NATS separates operations from security. User and Accounts management in a deployment may be decentralized and managed through a CLI. Server \(network element\) configuration is separated from security with a command line and configuration file which can be reloaded with changes at runtime.

**Kafka**

Kafka has a number of management tools and consoles including Confluent Control Center, Kafka, Kafka Web Console, Kafka Offset Monitor.

**Rabbit**

CLI tools, a plugin-based management system with dashboards and third-party tools.

**Pulsar**

CLI tools, per-topic dashboards, and third-party tools.

**gRPC**

External components such as a service mesh are required to manage gRPC.

**Integrations**

**NATS**

NATS supports WebSockets, a Kafka bridge, an IBM MQ Bridge, a Redis Connector, Apache Spark, Apache Flink, CoreOS, Elastic, Elasticsearch, Prometheus, Telegraf, Logrus, Fluent Bit, Fluentd, OpenFAAS, HTTP, and MQTT \(coming soon\).

**Kafka**

Kafka has a large number of integrations in its ecosystem, including stream processing \(Storm, Samza, Flink\), Hadoop, database \(JDBC, Oracle Golden Gate\), Search and Query \(ElasticSearch, Hive\), and a variety of logging and other integrations.

**Rabbit**

RabbitMQ has many plugins, including protocols \(MQTT, STOMP\), WebSockets, and various authorization and authentication plugins.

**Pulsar**

Pulsar has many integrations, including ActiveMQ, Cassandra, Debezium, Flume, Elasticsearch, Kafka, Redis, and others.

**gRPC**

There are a number of third party integrations including HTTP, JSON, Prometheus, Grift and others.[\[3\]](feature-comparison.md)

| Test Data | Column 2 |
| :--- | :--- |
| data | data |
| data | data |

## NATS Feature Comparison

This feature comparison is a summary of a few of the major components in several of the popular messaging technologies of today. This is by no means an exhaustive list and each technology should be investigated thoroughly to decide which will work best for your implementation.

This comparison features NATS, Apache Kafka, RabbitMQ, Apache Pulsar, and gRPC.

|  **Language and Platform Coverage** |  **NATS** |  Core NATS: 48 known client types, 11 supported by maintainers, 18 contributed by the community. NATS Streaming: 7 client types supported by maintainers, 4 contributed by the community. NATS servers can be compiled on architectures supported by Golang. NATS provides binary distributions. |
| :--- | :--- | :--- |
|  **Kafka** |  18 client types supported across the community and by Confluent. Kafka servers can run on platforms supporting java; very wide support. |  |
|  **Rabbit** |  At least 10 client platforms that are maintainer-supported with over 50 community supported client types. Servers are supported on the following platforms: Linux Windows, NT. |  |
|  **Pulsar** |  7 client languages, 5 third-party clients - tested on macOS and Linux. |  |
|  **gRPC** |  13 client languages. |  |
|  **Built-in Patterns** |  **NATS** |  Streams and Services through built in publish/subscribe, request/reply, and load balanced queue subscriber patterns. Dynamic request permissioning and request subject obfuscation is supported. |
|  **Kafka** |  Streams through publish/subscribe. Load balancing can be achieved with consumer groups. Application code must correlate requests with replies over multiple topics for a service \(request/reply\) pattern. |  |
|  **Rabbit** |  Streams through publish/subscribe, and services with a direct reply-to feature. Load balancing can be achieved with a Work Queue. Applications must correlate requests with replies over multiple topics for a service \(request/reply\) pattern. |  |
|  **Pulsar** |  Streams through publish/subscribe. Multiple competing consumer patterns support load balancing. Application code must correlate requests with replies over multiple topics for a service \(request/reply\) pattern. |  |
|  **gRPC** |  One service, which may have streaming semantics, per channel. Load Balancing for a service can be done either client-side or by using a proxy. |  |
|  **Delivery Guarantees** |  **NATS** |  At most once, at least once, and exactly once is available in Jetstream. |
|  **Kafka** |  At least once, exactly once. |  |
|  **Rabbit** |  At most once, at least once. |  |
|  **Pulsar** |  At most once, at least once, and exactly once. |  |
|  **gRPC** |  At most once. |  |
|  **Multi-tenancy and Sharing** |  **NATS** |  NATS supports true multi-tenancy and decentralized security through accounts and defining shared streams and services. |
|  **Kafka** |  Multi-tenancy is not supported. |  |
|  **Rabbit** |  Multi-tenancy is supported with vhosts; data sharing is not supported. |  |
|  **Pulsar** |  Multi-tenancy is implemented through tenants; built-in data sharing across tenants is not supported. Each tenant can have it’s own authentication and authorization scheme. |  |
|  **gRPC** |  N/A |  |
|  **AuthN** |  **NATS** |  NATS supports TLS, NATS credentials, NKEYS \(NATS ED25519 keys\), username and password, or simple token. |
|  **Kafka** |  Supports Kerberos and TLS. Supports JAAS and an out-of-box authorizer implementation that uses ZooKeeper to store connection and subject. |  |
|  **Rabbit** |  TLS, SASL, username and password, and pluggable authorization. |  |
|  **Pulsar** |  TLS Authentication, Athenz, Kerberos, JSON Web Token Authentication. |  |
|  **gRPC** |  TLS, ALT, Token, channel and call credentials, and a plug-in mechanism. |  |
|  **AuthZ** |  **NATS** |  Account limits including \# of connections, message size, \# of imports and exports. User level publish and subscribe permissions, connection restrictions, CIDR address restrictions, and time of day restrictions. |
|  **Kafka** |  Supports JAAS, ACLs for a rich set of Kafka resources including topics, clusters, groups and others. |  |
|  **Rabbit** |  ACLs dictate permissions for configure, write and read operations on resources like exchanges, queues, transactions, and others. Authentication is pluggable. |  |
|  **Pulsar** |  Permissions may be granted to specific roles for lists of operations such as produce and consume. |  |
|  **gRPC** |  Users can configure call credentials to authorize fine grained individual calls on a service. |  |
|  **Message Retention and Persistence** |  **NATS** |  Supports memory, file, and database persistence. Messages can be replayed by time, count, or sequence number, and durable subscriptions are supported. With NATS streaming, scripts can archive old log segments to cold storage. |
|  **Kafka** |  Supports file based persistence. Messages can be replayed by specifying an offset, and durable subscriptions are supported. Log compaction is supported as well as KSQL. |  |
|  **Rabbit** |  Supports file based persistence. Rabbit supported queue based semantics \(vs log\), so no message replay is available. |  |
|  **Pulsar** |  Supports tiered storage including file, Amazon S3 or Google Cloud Storage \(GCS\). Pulsar can replay messages from a specific position and supports durable subscriptions. Pulsar SQL and topic compaction is supported, as well as Pulsar functions. |  |
|  **gRPC** |  N/A |  |
|  **High Availability/Fault Tolerance** |  **NATS** |  Core NATS supports full mesh clustering with self-healing features to provide high availability to clients. NATS streaming has warm failover backup servers with two modes \(FT and full clustering\). Jetstream will support horizontal scalability with built-in mirroring. |
|  **Kafka** |  Fully replicated cluster members are coordinated via Zookeeper. |  |
|  **Rabbit** |  Clustering Support with full data replication via federation plugins. Clusters require low-latency networks where network partitions are rare. |  |
|  **Pulsar** |  Pulsar supports clustered brokers with geo-replication. |  |
|  **gRPC** |  N/A. gRPC relies on external resources for HA/FT. |  |
|  **Deployment** |  **NATS** |  The NATS network element \(server\) is a small static binary that can be deployed anywhere from large instances in the cloud to resource constrained devices like a Raspberry PI. NATS supports the Adaptive Edge architecture which allows for large, flexible deployments. Single servers, leaf nodes, clusters, and superclusters \(cluster of clusters\) can be combined in any fashion for an extremely flexible deployment amenable to cloud, on-premise, edge and IoT. Clients are unaware of topology and can connect to any NATS server in a deployment. |
|  **Kafka** |  Kafka supports clustering with mirroring to loosely coupled remote clusters. Clients are tied to partitions defined within clusters. Kafka servers require a JVM, eight cores, 64 GB to128 GB of RAM, two or more 8-TB SAS/SSD disks, and a 10-Gig NIC.1 |  |
|  **Rabbit** |  Rabbit supports clusters and cross cluster message propagation through a federation plugin. Clients are unaware of topology and may connect to any cluster. The server requires the Erlang VM and dependencies. |  |
|  **Pulsar** |  Pulsar supports clustering and built-in geo-replication between clusters. Clients may connect to any cluster with an appropriately configured tenant and namespace. Pulsar requires a JVM and requires at least 6 Linux machines or VMs. 3 running ZooKeeper. 3 running a Pulsar broker and a BookKeeper bookie.2 |  |
|  **gRPC** |  gRPC is point to point and does not have a server or broker to deploy or manage, but always requires additional pieces for production deployments. |  |
|  **Monitoring** |  **NATS** |  NATS supports exporting monitoring data to Prometheus and has Grafana dashboards to monitor and configure alerts. There are also development monitoring tools such as nats-top. Robust side car deployment or a simple connect-and-view model with NATS surveyor is supported. |
|  **Kafka** |  Kafka has a number of management tools and consoles including Confluent Control Center, Kafka, Kafka Web Console, Kafka Offset Monitor. |  |
|  **Rabbit** |  CLI tools, a plugin-based management system with dashboards and third-party tools. |  |
|  **Pulsar** |  CLI tools, per-topic dashboards, and third-party tools. |  |
|  **gRPC** |  External components such as a service mesh are required to monitor gRPC. |  |
|  **Management** |  **NATS** |  NATS separates operations from security. User and Account management in a deployment may be decentralized and managed through a CLI. Server \(network element\) configuration is separated from security with a command line and configuration file which can be reloaded with changes at runtime. |
|  **Kafka** |  Kafka has a number of management tools and consoles including Confluent Control Center, Kafka, Kafka Web Console, Kafka Offset Monitor. |  |
|  **Rabbit** |  CLI tools, a plugin-based management system with dashboards and third-party tools. |  |
|  **Pulsar** |  CLI tools, per-topic dashboards, and third-party tools. |  |
|  **gRPC** |  External components such as a service mesh are required to manage gRPC. |  |
|  **Integrations** |  **NATS** |  NATS supports WebSockets, a Kafka bridge, an IBM MQ Bridge, a Redis Connector, Apache Spark, Apache Flink, CoreOS, Elastic, Elasticsearch, Prometheus, Telegraf, Logrus, Fluent Bit, Fluentd, OpenFAAS, HTTP, and MQTT \(coming soon\). |
|  **Kafka** |  Kafka has a large number of integrations in its ecosystem, including stream processing \(Storm, Samza, Flink\), Hadoop, database \(JDBC, Oracle Golden Gate\), Search and Query \(ElasticSearch, Hive\), and a variety of logging and other integrations. |  |
|  **Rabbit** |  RabbitMQ has many plugins, including protocols \(MQTT, STOMP\), WebSockets, and various authorization and authentication plugins. |  |
|  **Pulsar** |  Pulsar has many integrations, including ActiveMQ, Cassandra, Debezium, Flume, Elasticsearch, Kafka, Redis, and others. |  |
|  **gRPC** |  There are a number of third party integrations including HTTP, JSON, Prometheus, Grift and others.3 |  |
|  |  |  |

### Management

|   |  |
| :--- | :--- |
| **NATS** | NATS separates operations from security. User and Account management in a deployment may be decentralized and managed through a CLI. Server \(network element\) configuration is separated from security with a command line and configuration file which can be reloaded with changes at runtime. |
|  **Kafka** |  Kafka has a number of management tools and consoles including Confluent Control Center, Kafka, Kafka Web Console, Kafka Offset Monitor. |
| **Rabbit** |  |
|  **Rabbit** |  CLI tools, a plugin-based management system with dashboards and third-party tools. |
|  **Pulsar** |  CLI tools, per-topic dashboards, and third-party tools. |
|  **gRPC** |  External components such as a service mesh are required to manage gRPC. |

###  Integrations

|   |  |
| :--- | :--- |
| **NATS** |  NATS supports WebSockets, a Kafka bridge, an IBM MQ Bridge, a Redis Connector, Apache Spark, Apache Flink, CoreOS, Elastic, Elasticsearch, Prometheus, Telegraf, Logrus, Fluent Bit, Fluentd, OpenFAAS, HTTP, and MQTT \(coming soon\). |
|  **Kafka** |  Kafka has a large number of integrations in its ecosystem, including stream processing \(Storm, Samza, Flink\), Hadoop, database \(JDBC, Oracle Golden Gate\), Search and Query \(ElasticSearch, Hive\), and a variety of logging and other integrations. |
| **Rabbit** | RabbitMQ has many plugins, including protocols \(MQTT, STOMP\), WebSockets, and various authorization and authentication plugin. |
| **Pulsar** | Pulsar has many integrations, including ActiveMQ, Cassandra, Debezium, Flume, Elasticsearch, Kafka, Redis, and others. |
|  **gRPC** | There are a number of third party integrations including HTTP, JSON, Prometheus, Grift and others.3 |

### References

1 https://docs.cloudera.com/HDPDocuments/HDF3/HDF-3.1.0/bk\_planning-your-deployment/content/ch\_hardware-sizing.html\#:~:text=Kafka%20Broker%20Node%3A%20eight%20cores,and%20a%2010%2D%20Gige%20Nic%20.&text=75%20MB%2Fsec%20per%20node,therefore%2010GB%20Nic%20is%20required%20

2 https://pulsar.apache.org/docs/v1.21.0-incubating/deployment/cluster/

3 https://github.com/grpc-ecosystem

