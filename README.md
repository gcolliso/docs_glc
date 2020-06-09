# Introduction

## The Importance of Messaging

Developing and deploying applications and services that communicate in distributed systems can be complex and difficult. However there are two basic patterns, request/reply or RPC for services, and event and data streams. A modern technology should provide features to make this easier, scalable, secure, location independent and observable.

```text
```bash
> nsc describe account A
╭──────────────────────────────────────────────────────────────────────────────────────╮
│                                   Account Details                                    │
├───────────────────────────┬──────────────────────────────────────────────────────────┤
│ Name                      │ A                                                        │
│ Account ID                │ ADETPT36WBIBUKM3IBCVM4A5YUSDXFEJPW4M6GGVBYCBW7RRNFTV5NGE │
│ Issuer ID                 │ OAFEEYZSYYVI4FXLRXJTMM32PQEI3RGOWZJT7Y3YFM4HB7ACPE4RTJPG │
│ Issued                    │ 2019-12-04 20:19:19 UTC                                  │
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
╰───────────────────────────┴──────────────────────────────────────────────────────────╯

╭─────────────────────────────────────────────────────────────────────────────╮
│                                   Exports                                   │
├────────────────┬─────────┬────────────────┬────────┬─────────────┬──────────┤
│ Name           │ Type    │ Subject        │ Public │ Revocations │ Tracking │
├────────────────┼─────────┼────────────────┼────────┼─────────────┼──────────┤
│ help           │ Service │ help           │ Yes    │ 0           │ -        │
│ private.help.* │ Service │ private.help.* │ No     │ 0           │ -        │
╰────────────────┴─────────┴────────────────┴────────┴─────────────┴──────────╯
```

```

### Distributed Computing Needs of Today

A modern messaging system needs to support multiple communication patterns, be secure by default, support multiple qualities of service, and provide secure multi-tenancy for a truly shared infrastructure. A modern system needs to include:

* Secure by default communications for microservices, edge platforms and devices
* Secure multi-tenancy in a single distributed communication technology
* Transparent location addressing and discovery
* Resiliency with an emphasis on the overall health of the system
* Ease of use for agile development, CI/CD, and operations, at scale
* Highly scalable and performant with built-in load balancing and dynamic auto-scaling
* Consistent identity and security mechanisms from edge devices to backend services

### NATS

NATS was built to meet the distributed computing needs of today and tomorrow. NATS is simple and secure messaging made for developers and operators who want to spend more time developing modern applications and services than worrying about a distributed communication system.

* Easy to use for developers and operators
* High-Performance
* Always on and available
* Extremely lightweight
* At Most Once and At Least Once Delivery
* Support for Observable and Scalable Services and Event/Data Streams
* Client support for over 30 different programming languages
* Cloud Native, a CNCF project with Kubernetes and Prometheus integrations

### Use Cases

NATS can run anywhere, from large servers and cloud instances, through edge gateways and even IoT devices. Use cases for NATS include:

* Cloud Messaging
  * Services \(microservices, service mesh\)
  * Event/Data Streaming \(observability, analytics, ML/AI\)
* Command and Control
  * IoT and Edge
  * Telemetry / Sensor Data / Command and Control
* Augmenting or Replacing Legacy Messaging Systems

