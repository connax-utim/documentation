=======================
UTIM Installation Guide
=======================

Installation
============

Use ``pip`` for python3:

.. code-block:: bash

   pip3 install --extra-index-url https://test.pypi.org/simple/ utim


Launch
======

Example of UTIM launcher is `here </user/about>`.

Before you run launcher you need:

1. Set environment variable ``UTIM_MASTER_KEY``. Value of this variable is in hex format. For example:

.. code-block:: bash

   UTIM_MASTER_KEY=6b6579

2. Edit ``config.ini`` file (in the same folder), or create config file in the other place and set environment variable ``UTIM_CONFIG``. Value of this variable is a absolute path to ``config.ini``.

.. code-block:: ini

   ; Configuration
   ; Sections (required):
   ; * UTIM:
   ;   * utimname - name of UTIM in hex format (for example, utimname=74657374 for value 'test')
   ;   * messaging_protocol - MQTT or AMQP
   ; * MYSQLDB
   ; Sections (optional, according UTIM.messaging_protocol):
   ; * MQTT
   ; * AMQP

   [UTIM]
   uhostname = 74657374
   utimname = 7574696d
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
