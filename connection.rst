
.. _connection_benchmark:

==========================
Concurrent connection test
==========================

The test scenario test the MQTT connection between client and server side, the clients don't send out any data except MQTT control and ping packet. The purpose of test is to get EMQ performance data under different connection numbers.

--------------
TCP Connection
--------------

Overall: The response time is less than 15ms within 1 million connection, successful rate is 100%. Memory usage is in line with increasing of connections. CPU usage is stable, but observed surge of CPU usage when connections are disconnectd.

.. NOTE:: Test agent configuration：1 VM runs 50000 VU (2 dockers*25000 VU)

300k conn
---------

+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+
| Virtual user number | Conn per second | Test duration (Sec) | Average throughput | Average response time | Max response time | Min response time | Sussessful rate |
+=====================+=================+=====================+====================+=======================+===================+===================+=================+
|    300000           |        1000     |         600         |     912            |     9ms               |     7.02s         |    1ms            | 100%            | 
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+

+--------------------------------+--------------------------+-------------------------------------------------------------------+
|     CPU Load                   |      CPU usage           |                   Memory                                          |
+================================+==========================+===================================================================+
| CPU shortterm load reaches 2.3 | CPU usage between 4%-31% |  Memory grows with increasing of connections, max value is 4.2GB. |
+--------------------------------+--------------------------+-------------------------------------------------------------------+

300k connection online test report: https://www.xmeter.net/commercialPage.html#/testrunMonitor/895167331

500k conn
---------

+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+
| Virtual user number | Conn per second | Test duration (Sec) | Average throughput | Average response time | Max response time | Min response time | Sussessful rate |
+=====================+=================+=====================+====================+=======================+===================+===================+=================+
|      500000         |    1000         |         900         |      966           |       14ms            |     7.04s         |     1ms           |       100%      |
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+

+--------------------------------+--------------------------+-------------------------------------------------------------------+
|     CPU Load                   |      CPU usage           |                   Memory                                          |
+================================+==========================+===================================================================+
| CPU shortterm load reaches 3.9 | CPU usage between 7%-65% |  Memory grows with increasing of connections, max value is 7.05G  |
+--------------------------------+--------------------------+-------------------------------------------------------------------+

500k connection online test report: https://www.xmeter.net/commercialPage.html#/testrunMonitor/1231188268

1 million conn
--------------
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+
| Virtual user number | Conn per second | Test duration (Sec) | Average throughput | Average response time | Max response time | Min response time | Sussessful rate |
+=====================+=================+=====================+====================+=======================+===================+===================+=================+
|         1000000     |  1000           |        1200         |        961         |           7ms         |       10.01s      |        1ms        |     100%        |
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+

+--------------------------------+--------------------------+-------------------------------------------------------------------+
|     CPU Load                   |      CPU usage           |                   Memory                                          |
+================================+==========================+===================================================================+
| CPU shortterm Load reaches 6   | CPU usage between 6%-80% | Memory grows with increasing of connections, max value is 13.4G   |
+--------------------------------+--------------------------+-------------------------------------------------------------------+

1 million connection online test report: https://www.xmeter.net/commercialPage.html#/testrunMonitor/2084906193

------------------
SSL authentication
------------------

Overall: The EMQ response time is less than 50ms within 200k connections (configured with not auto reconnect after disconnect), the successful rate is 100%, the CPU and memory usage are also normal. But with the default configuration of fusesource (auto reconnrect after disconnect), then it produces lots of re-connect request, and with 300k connection test, we observed additional 50% of connect request, which actual established connection is less than 300k. It possibly caused by EMQ disconnect automatically when detecting same client re-connect for the 2nd time. In such configuration, all of 24GB mememory is exhausted.

.. NOTE:: Test agent configuration：1 VM 30000 VU (2 dockers*15000 VU)

100k conn
---------

+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+
| Virtual user number | Conn per second | Test duration (Sec) | Average throughput | Average response time | Max response time | Min response time | Sussessful rate |
+=====================+=================+=====================+====================+=======================+===================+===================+=================+
|       100000        |    500          |        400          |     467            |      41ms             |     7.58s         |     6ms           | 100%            |
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+

+--------------------------------+--------------------------+-------------------------------------------------------------------+
|     CPU Load                   |      CPU usage           |                   Memory                                          |
+================================+==========================+===================================================================+
| CPU shortterm Load reaches 5   | CPU usage < 60%          | Memory grows with increasing         | setConnectAttemptsMax=0    |
|                                |                          | of connections, max value is 11.88G  | setReconnectAttemptsMax=0  |
+--------------------------------+--------------------------+--------------------------------------+----------------------------+

200k conn
---------
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+
| Virtual user number | Conn per second | Test duration (Sec) | Average throughput | Average response time | Max response time | Min response time | Sussessful rate |
+=====================+=================+=====================+====================+=======================+===================+===================+=================+
|      200000         |    500          |          1000       |     463            |      49ms             |    7.04s          |     6ms           |       100%      |
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+

+--------------------------------+--------------------------+---------------------------------------------------------------------+
|     CPU Load                   |      CPU usage           |                   Memory                                            |
+================================+==========================+=====================================================================+
| CPU shortterm Load reaches 6   | CPU usage < 73%          | Memory grows with increasing         | setConnectAttemptsMax=0      |
|                                |                          | of connections, max value is 20.46G  | setReconnectAttemptsMax=0    |
+--------------------------------+--------------------------+--------------------------------------+------------------------------+

-----------
Dual SSL
-----------

Overall: Response time reaches second level in dual SSL connections (setConnectAttemptsMax and setReconnectAttemptsMax are set to 0), and successful rate is 86% with 200k connections, and memory is also exhausted. Comparing to SSL, dual SSL uses more than 3GB memory when there are 300k of connections.

.. NOTE:: Test agent configuration：1 VM 30000 VU (2 dockers*15000 VU)

100k conn
---------

+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+
| Virtual user number | Conn per second | Test duration (Sec) | Average throughput | Average response time | Max response time | Min response time | Sussessful rate |
+=====================+=================+=====================+====================+=======================+===================+===================+=================+
|      100000         |     500         |       420           |     454            |      1s               |     10.06s        |    22ms           | 99.4%           |
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+

+--------------------------------+--------------------------+---------------------------------------------------------------------+
|     CPU Load                   |      CPU usage           |                   Memory                                            |
+================================+==========================+=====================================================================+
| CPU shortterm Load reaches 6   | CPU usage < 66%          | Memory grows with increasing         | setConnectAttemptsMax=0      |
|                                |                          | of connections, max value is 14.92G  | setReconnectAttemptsMax=0    |
+--------------------------------+--------------------------+---------------------------------------------------------------------+

200k conn
---------

+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+
| Virtual user number | Conn per second | Test duration (Sec) | Average throughput | Average response time | Max response time | Min response time | Sussessful rate |
+=====================+=================+=====================+====================+=======================+===================+===================+=================+
|         200000      |    500          |          600        |     473            |        3s             |     11.95     s   |    22ms           | 86%             |
+---------------------+-----------------+---------------------+--------------------+-----------------------+-------------------+-------------------+-----------------+

+--------------------------------+--------------------------+---------------------------------------------------------------------+
|     CPU Load                   |      CPU usage           |                   Memory                                            |
+================================+==========================+=====================================================================+
| CPU shortterm Load reaches 6   | CPU usage < 77%          | Memory grows with increasing         | setConnectAttemptsMax=0      |
|                                |                          | of connections, max value is 23.82G  | setReconnectAttemptsMax=0    |
+--------------------------------+--------------------------+---------------------------------------------------------------------+
