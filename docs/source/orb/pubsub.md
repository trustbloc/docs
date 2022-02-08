# AMQP Publisher/Subscriber

The publisher/subscriber subsystem in Orb is configurable to either use an
[AMQP](https://www.amqp.org/) message broker or an in-memory implementation.
(The in-memory implementation should only be used during development and for demos.)

The AMQP URL is specified by the startup parameter [mq-url](parameters.html#mq-url). If
this parameter is not set then the in-memory implementation is used.

## Publishers and Subscriber

A subscriber is a handler for a message posted to a queue. On startup, each Orb instance subscribes to various
queues. The queues are global to the domain, i.e. each Orb instance subscribes to the same queues.

A publisher posts messages to the queues. The posted message is handled by one (and only one) of the subscribers in
one of the Orb instances. If the handler replies with an _ack_, then the message is considered to be processed.

It is up to the AMQP implementation to direct messages to subscribers using a
load-balancing algorithm.

```{image} ../_static/orb/mq-pubsub.svg

```

## Message Redelivery

When a message is published to a queue, one of the subscribers in the Orb domain handles the
message. If a processing error occurs (such as the DB is temporarily unavailable) then the handler replies 
with a _nack_. In this case the message is sent to a _redelivery_ queue so that it may be retried at a later time.

The _redelivery_ queue is configured as the _dead-letter-queue_ for all queues in Orb.
When a message is rejected (_nacked_) by a subscriber, it is automatically sent to the _redelivery_
queue. The first time a message is rejected, the redelivery handler immediately redelivers
the message to the original destination queue. If the message is rejected again, it is
posted to a _wait_ queue and sets an _expiration_ header value that is calculated by a backoff
algorithm using the following config parameters:

- [Initial redelivery interval](parameters.html#mq-redelivery-initial-interval)
- [Redelivery multiplier](parameters.html#mq-redelivery-multiplier)
- [Maximum redelivery interval](parameters.html#mq-redelivery-max-interval)

The backoff algorithm increases the expiration with each redelivery attempt. For example, if the initial interval
is set to 2s and the multiplier is set to 1.5 then the expiration is set 3s. The next time a redelivery of the
message occurs, the expiration will be set to 4.5s. Expiration time is limited by parameter
[Maximum redelivery interval](parameters.html#mq-redelivery-max-interval).

The _wait_ queue has no subscribers, so the message sits there until it expires. The _redelivery_ queue is also
configured as the _dead-letter-queue_ for the _wait_ queue, so when the message expires it is automatically sent
back to the _redelivery_ queue and the redelivery handler processes the message again.

The redelivery handler looks at the _reason_ field in the message header. If _reason_ is set to 'expired' then the
message is posted to the original destination queue, otherwise (if reason is 'rejected') it is posted to the _wait_
queue with an even greater expiration value. This process repeats until the
[maximum](parameters.html#mq-redelivery-max-attempts) number of redelivery attempts has been reached, at which
point redelivery for the message is aborted.

```{image} ../_static/orb/mq-pubsub-redeliver.svg

```

## Publisher Pool

The Publisher publishes messages over an AMQP channel to an AMQP server. There may be multiple publisher channels
over a single connection and (for performance reasons) it is advisable to use multiple channels to publish
messages concurrently. Also, channels should be reused and not recreated each time (since there is also a performance
penalty for creating and closing channels). A publisher channel pool is created when the startup
parameter [mq-publisher-channel-pool-size](parameters.html#mq-publisher-pool) is greater than zero.

## Subscriber Pool

Each AMQP subscription is handled synchronously. If the handler takes a long time then subsequent messages in the queue
need to wait until the previous message is processed. A subscriber pool may be configured for a given queue such that
multiple subscribers concurrently process messages from the same queue. This setting is available for the following queues:
- [mq-op-pool](parameters.html#mq-op-pool)
- [mq-observer-pool](parameters.html#mq-observer-pool)

Typically, all subscriber channels are created on the same AMQP connection, although an AMQP server may
have a limit to the number of channels that can be opened for a single connection. Therefore, the limit for the number
of subscriber channels for a single connection is specified by parameter
[mq-max-connection-subscription](parameters.html#mq-max-connection-subscriptions). If the size of the subscriber pool
reaches this limit then a new connection is automatically opened for any new subscriber channel.
