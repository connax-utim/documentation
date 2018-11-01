========================
Uhost Installation Guide
========================

Before the start
================

First of all, to start you need to have:

1. `MySQL <https://dev.mysql.com/doc/mysql-installation-excerpt/5.7/en/>`__ database to store UTIMs

2. One of messaging broker for communicating between server and client sides

   2.1 `Mosquitto <https://mosquitto.org/download/>`__

   2.2 `RabbitMQ <https://www.rabbitmq.com/download.html>`__


Installation
============

Use ``pip`` for python3:

.. code-block:: bash

   pip3 install --extra-index-url https://test.pypi.org/simple/ uhost


Launch
======

Example of Uhost launcher is `here </user/about>`.

Before you run launcher you need:

1. Set environment variable ``UHOST_MASTER_KEY``. Value of this variable is in hex format. For example:

.. code-block:: bash

   UHOST_MASTER_KEY=6b6579

2. Edit ``config.ini`` file (in the same folder), or create config file in the other place and set environment variable ``UHOST_CONFIG``. Value of this variable is a absolute path to ``config.ini``.

.. code-block:: bash

   ; Configuration
   ; Sections (required):
   ; * UHOST:
   ;   * uhostname - name of uhost in hex format (for example, uhostname=74657374 for value 'test')
   ;   * messaging_protocol - MQTT or AMQP
   ; * MYSQLDB
   ; Sections (optional, according UHOST.messaging_protocol):
   ; * MQTT
   ; * AMQP

   [UHOST]
   uhostname = 74657374
   messaging_protocol = MQTT

   [MQTT]
   hostname = localhost
   username = test
   password = test
   reconnect_time = 60

   [AMQP]
   hostname = localhost
   username = test
   password = test
   reconnect_time = 60

   [MYSQLDB]
   hostname = localhost
   username = test
   password = test

3. After running in output you should see config for Utim like that:

.. code-block:: bash

   ########################################################

   Use this configuration to start Utim:

   UHOST_NAME=74657374
   MASTER_KEY=6b6579
   MESSAGING_PROTOCOL=MQTT
   MESSAGING_HOSTNAME=localhost
   MESSAGING_USERNAME=test
   MESSAGING_PASSWORD=test

   NOTE: UHOST_NAME and MASTER_KEY are in hex format

   ########################################################

.. note::

   Do this steps before launch any UTIM instance**

   1. Connect to database (from your ``config.ini``)

   2. Select schema which name is ``uhost_{UHOST_NAME}``

   3. Add Utim ID in hex format to ``device_id`` column of ``udata`` table
