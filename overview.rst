
.. _overview:

============
Test spec
============

--------
Tools
--------

`XMeter`_ Online performanct test service provider

`JMeter-MQTT`_ MQTT JMeter plugin

`JMeter`_ 4.0

`fusesource-1.14`_ MQTT client Java lib

--------
Test env
--------

EMQ version: `emqx-ubuntu18.04-v3.0.0.zip`_

EMQ server: QingCloud 8 core CPU，32GB Memory，Ubuntu-18.04.1

Test agent：QingCloud 8 core CPU，8GB Memeory，CentOS 7

Network：Intranet of 3rd zone, Beijing QingCloud with 1Gbps.

-------------
Test scenario
-------------

The test includes concurrent connection test and message throughput test, totally 33 test scenarios. Please refer to related report for detailed test scenario. The concurrent connection test includes TCP, SSL and dual SSL, totally 7 test scenarios. Message throughput test includes multi-sub/few-pub, few-sub/multi-pub, capacity, shared-topic etc.

It could be a big test scenario combination, after considering test coverage and test result comparision etc, we designed the 33 test scenario.




.. _XMeter: http://xmeter.net
.. _JMeter-MQTT: https://github.com/emqtt/mqtt-jmeter
.. _JMeter: http://jmeter.apache.org
.. _fusesource-1.14: https://github.com/fusesource/mqtt-client
.. _emqx-ubuntu18.04-v3.0.0.zip: https://github.com/emqx/emqx/releases/download/v3.0.0/emqx-ubuntu18.04-v3.0.0.zip
