# Queue Groups

NATS provides a built-in load balancing feature called distributed queues. Using queue subscribers will balance message delivery across a group of subscribers which can be used to provide application fault tolerance and scale workload processing.

To create a queue subscription, subscribers register a queue name. All subscribers with the same queue name form the queue group. This requires no configuration. As messages on the registered subject are published, one member of the group is chosen randomly to receive the message. Although queue groups have multiple subscribers, each message is consumed by only one.

One of the great features of NATS is that queue groups are defined by the application and their queue subscribers, not on the server configuration.

Queue subscribers are ideal for scaling services. Scale up is as simple as running another application, scale down is terminating the application with a signal that drains the in flight requests. This flexibility and lack of any configuration changes makes NATS an excellent service communication technology that can work with all platform technologies.

 `digraph nats_queues { rankdir=LR publisher [shape=box, style="rounded", label="Publisher"]; subject [shape=circle, label="Queue"]; sub1 [shape=box, style="rounded", label="Subscriber"]; sub2 [shape=box, style="rounded", label="Subscriber"]; sub3 [shape=box, style="rounded", label="Subscriber"]; publisher -> subject [label="msgs 1,2,3"]; subject -> sub1 [label="msg 2"]; subject -> sub2 [label="msg 1"]; subject -> sub3 [label="msg 3"]; }`

Try NATS queue subscriptions on your own, using a live server by walking through the [queueing tutorial](../developing-with-nats/intro-6/queues.md).

