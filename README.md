![logo_transparent](https://user-images.githubusercontent.com/5942370/88551418-d623ea00-cff0-11ea-87d8-e9b94174aaa2.png)

Grav is an embedded distributed message bus. It is designed with a very narrow purpose, and is not meant to replace more general-purpose systems such as RabbitMQ, Kafka, or others. Grav is designed to allow interconnected components of your systems to communicate effectively in a reliable, asynchronous manner. HTTP and RPC are hard to scale well in modern distributed systems, but Grav is designed to be performant and resilient in various distributed environments. 

Since Grav is embedded, it is instantiated as a `grav.Grav` object which your application code connects to in order to send and recieve messages. Grav connects to other nodes via transport plugins such as [gravwebsocket](./transport/gravwebsocket/README.md) which extends the Grav core to become a networked distributed messaging system. Grav does not require a centralized broker, and as such has some limitations, but for certain applications it is vastly simpler (and more extensible) than a centralized messaging system.

This project has several goals and a few non-goals:

Goals:
- Have very low resource and memory consumption.
- Be resilient against data loss due to node failure.
- Act as a reliable core upon which more complex behaviour can be built.
- Define a minimal data format that is meant to be extended for a particular purpose.
- Support request/reply and broadcast message patterns.
- Support internal (in-process) and external (networked, via transport plugins) publishers and consumers equally.

Non-Goals:
- Replace a brokered message queue for large-scale systems.
- Have extremely high throughput capabilities.
- Support every type of messaging pattern.

## Background

In a search for the right messaging system to use in concert with Hive and Vektor, many options were evaluated. Unfortunately, every option either required the use of CGO, or relied on a centralized broker which would complicate deployments. Reluctantly, the decision was made to implement a message bus of our own. I say reluctantly as there is nothing worse than re-inventing the wheel, but alas none of the mainstream projects support the use-cases that Suborbital's frameworks are aiming to handle. To avoid "Yak Shaving" as much as possible, Grav is designed to be a powerful core that is focused on being a very reliable, performant message bus. Anything beyond that, including the transport layer, is provided via a plugin, not Grav itself.

## Transports
Grav has two "first-party" transports:
- gravhttp, a simplistic transport using HTTP requests to emit messages. [Read more here](./transport/gravhttp/README.md)
- gravwebsocket, a streaming transport based on standard websockets. [Read more here](./transport/gravwebsocket/README.md)

Grav transports are designed as plugins, and as such anyone can create one for their own purposes. Transports for various popular products such as NATS and Kafka are planned. See the [transport](./transport) directory to see example transport code.

## Project status

Grav is currently in beta, and is being developed alongside (and is designed to integrate with) [Vektor](https://github.com/suborbital/vektor) and [Hive](https://github.com/suborbital/hive). It is also the messaging core that powers Suborbital's flagship project, [Atmo](https://github.com/suborbital/atmo).

Full documentation will be coming soon. See the various `*_test.go` files if you are looking for examples on how to use Grav. There is a transport-enabled example in [the gravwebsocket directory](./transport/gravwebsocket/test/main.go).

Copyright Suborbital contributors 2020.