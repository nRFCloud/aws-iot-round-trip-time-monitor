# AWS IoT Round-trip Time Monitor

Monitors the round-trip time of AWS IoT messages from different regions
to the MQTT broker in your *main region*.

The Round-trip time is the time it takes from publishing the message 
to the broker until it reaches a subscribed lambda function.

## `publisher.yaml`

Install this as a stack-set in all regions from which you want to 
measure MQTT round-trip time. You can also just install it as a stack
in your *main region*.

It will publish 10 messages to the `monitoring` MQTT topic in the 
*main region*. These messages include a timestamp value of the time
when the message was constructed. This timestamp will be read by the
monitoring lambda to calculate the round-trip time.

## `monitor.yaml`

Install this as a stack in the *main region*. 
It creates a lambda function which subscribes to messages sent the 
*monitoring* MQTT topic and calculates the round-trip time based on
the timestamp in the message.

The calculated round-trip times will be published as CloudWatch metrics.
