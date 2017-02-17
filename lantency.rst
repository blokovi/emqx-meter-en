
.. _lantency_benchmark:

============
Latency Test
============

------------------------------
Latency of 20000 QoS1 Messages
------------------------------

PUB and SUB should collaborate in order to measure latency. Test Scenario is designed as following:

1. Message payload that PUB sends out includes:

   a) the unique id that identifies this particular message：hostname + thread id + message id

   b) timestamp when the message is sent

   c) actual payload data, size of this part depends on the real scenario between sender and receiver

2. EMQ server processing, i.e. message routing 

3. When SUB receives the message，it decodes the payload，"message latency" = "current time" - "timestamp in message payload". We store message id and latency into a text file: ${hostname}_ data_entries.log.

4. Parse ${hostname}_ data_entries.log，calculate average latency and message loss rate.

.. NOTE:: If PUB and SUB reside on different machines but system time is not synchronized among them，the calculated latency will be inaccurate, as generally latency is in millisecond scale. Therefore, this test scenario requires PUB and SUB runs on the same machine, or machine time is strictly synchronized.

Summary Result: 
With normal workload, latency of 20000 QOS1 messages is around 60ms (test target and load generator are both within QingCloud using internal network, so the network latency is very small), and there's no message loss.  
With heavier workload, message latency increases very quickly. In test scenario 2, message latency increases to 77 seconds, still no message loss which is a good thing, but it takes much longer time to send out all the messages by PUB.

+-------------+----------+-------+--------+--------+----------+-----------+--------------+
| Scenario ID  |  Payload |  Qos  | PUB    | SUB    | Message Rate | Background Connection  |  Avg Throughput  |
+=============+==========+=======+========+========+==========+===========+==============+
|     1       |    1k    |   1   | 20 x 1 | 1000   | 20k      |    20k    |    19.35k    |
+-------------+----------+-------+--------+--------+----------+-----------+--------------+

+--------------------------+---------------------+-----------------------------------------+
|           CPU            |           Mem       |       Message Forwarding                      |
+==========================+=====================+=========================================+
| Used CPU <= 86%， | Memory Used: < 1GB    | Actual fanout message per second by EMQ server as expected, |
| shortterm load <= 7    |                     | message latency is 61ms，no message loss             |
+--------------------------+---------------------+-----------------------------------------+

.. NOTE:: Note1：Configuration of test machine: 1 VM, supports 20000 VU(virtual user), with 2 docker contains each simulates 10000 VU
.. NOTE:: Note2：PUB Column (m*n)，where m means number of PUB endpoints，n means message rate, i.e. how many messages to be sent in one second. e.g. 10*1 means 10 PUB, each sends 1 message per second.
.. NOTE:: Note3：Row with green colored background indicates a successful test execution, while red color implies problems in the test, please click test run ID to view more comprehensive test report.

