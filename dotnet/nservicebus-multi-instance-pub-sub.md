# NServiceBus multi instance pub sub

When usin nservicebus with a transport that does not support nativ pub/sub, message based subscription is used. This works by having subscribers sending dedicated subscription messages to the publisher on startup. The publisher then stores this information in a database table, which looks as follows: 

| Subscriber             | Endpoint      | MessageType                 | PersistenceVersion     |
| -----------------------| --------------| ----------------------------| -----------------------|
| endpoint1@mymachine1   | endpoint1     | Contracts.Events.SomeEvent  | 1.0.0.4                |
| endpoint1@mymachine2   | endpoint1     | Contracts.Events.SomeEvent  | 1.0.0.4                |
| endpoint2@mymachine1   | endpoint2     | Contracts.Events.SomeEvent  | 1.0.0.4                |


Each row is identified by the subscriber column, with the endpoint name (queue name in msmq) and the machine instance name. As seen in the example subscription table abow, 'endpoint1' is scalled out to two instances, and two machines, thus the two rows in the subscription data table. 

When the publisher wants to publish a message, a lookup is made on the message type column, and the message sent to all the registered subscribers. 

## Learning
By default only one _physical_ instance of a _logical_ endpoint will receive the message. This means that only one of the 'endpoint1' instances will receive published messages.

See [Sender Side Distribution](https://docs.particular.net/transports/msmq/sender-side-distribution) for more information
