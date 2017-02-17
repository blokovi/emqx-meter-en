
.. _faq:

================================
Frequently Asked Questions (FAQ)
================================

Q1. Do we have reconnect configured in TCP connection test? If reconnect is not used, shall we think that actual connections are not as many as the number we planned in the test?
A1.
EMQ：Reconnection is not configure. Actual connections are reported by EMQ server via stats, listener metrics，it's nothing to do with reconnection. EMQ didn't set max connection limit but just test server CPU and memory utilization under 1 million connections. In EMQ 1.0, we tested up to 1.3 million connections and 0.9 million in ZAKER news client production environment.

XMeter: No reconnection in the test. There's a ping package from each connection every 5 minutes in order to keep connection active. Actual connection statistics are acquired via EMQ server stats,listener metrics.


Q2. Regarding response time in connection test, is this the time spent from connection request to successfully connected?

A2. EMQ：Response time is from TCP connection creation, MQTT CONNECT package transmit, to CONNACK package arrival.


Q3. The report says there's no package loss in throughput test, do we have more details about this result?
A3.
EMQ: Throughput test is performed in QingCloud pek3a region. The main purpose is to measure how many messages per second EMQ can process without package loss. Within the EMQ server capacity, QoS0 message will not lose in the intranet environment, QoS1/2 message support confirmation and retransmission to avoid package loss.

XMeter: We didn't explicitly measure package loss during the test, but there's our observation: 1) If EMQ server works under normal workload, all packages can be forwarded to subscriber, and there's no package loss; 2) Under heavy workload, it takes longer time for PUB client to send out messages, but no message loss either.


Q4. How do we design topic number in throughput test?
A4.
EMQ: We establish 100000 background connections and 200000 topics firstly.

XMeter: In all connection test including background connection test, every connection will subscribe to a particular topic. One million connection subscribe to 1 million topics, and as part of throughput test, 100000 background connections involves 100000 topics. However, all PUB and SUB clients in the throughput test share a same topic, so that all messages will be transferred from PUB to SUB.


Q5. In throughput test, fan-in and fan-out are calculated separately. There's no SUB in fan-in test. Do we have test results that have both fan-in and fan-out measurements? 

A5. EMQ: Shared subscription test is bi-directional. In most application scenarios, PUB messages need to be consumed via shared subscription to balance the load.


Q6. In our usage scenario, messages flow from a large number of publishers to a small number of subscribers. How do we best handle this pattern?

A6. EMQ: Use shared subscription or Fastlane subscription, which is designed for more publisher less subscriber scenarios.

