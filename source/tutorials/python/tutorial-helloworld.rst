=============================
Python Tutorial - Hello World
=============================

This tutorial describes the simplest implementation of UTIM-Uhost usage


Uhost
=====

Code of this example `here <https://github.com/connax-utim/uhost-python/blob/master/examples/uhost_launcher.py>`__.

Steps:

1. Import from Uhost library

   .. code-block:: python3

      from uhost import uhost
      from uhost.utilities.exceptions import UtimConnectionException, UtimInitializationError

2. Create Uhost object and run it

   .. code-block:: python3

      uh1 = uhost.Uhost()
      uh1.run()

3. Finally, stop Utim before exit

   .. code-block:: python3

      uh1.stop()

UTIM
====

Code of this example `here <https://github.com/connax-utim/utim-python/blob/master/examples/utim_launcher.py>`__.

Steps:

1. Import from UTIM library

   .. code-block:: python3

      from utim.connectivity.manager import ConnectivityConnectError
      from utim.utim import Utim
      from utim.connectivity import DataLinkManager, TopDataType
      from utim.connectivity.manager import ConnectivityManager
      from utim.utilities.tag import Tag
      from utim.utilities.exceptions import UtimConnectionException, UtimInitializationError

2. Initialize two queues - first is for receiving and second is for transmitting

   .. code-block:: python3

      rx_queue = queue.Queue()
      tx_queue = queue.Queue()

3. Initialize ConnectivityManager - utility to read data from queues. To use queues to send and receive data you should set argument ``dl_type=DataLinkManager.TYPE_QUEUE`` to ConnectifityManager

   .. code-block:: python3

      cm1 = ConnectivityManager()
      cm1.connect(dl_type=DataLinkManager.TYPE_QUEUE, rx=tx_queue, tx=rx_queue)

3. Create UTIM object and run it

   .. code-block:: python3

      concrete_utim = Utim()
      concrete_utim.connect(dl_type=DataLinkManager.TYPE_QUEUE, rx=rx_queue, tx=tx_queue)
      concrete_utim.run()

4. Send data to start communication:

   .. code-block:: python3

      data1 = [TopDataType.DEVICE, Tag.INBOUND.NETWORK_READY]
      cm1.send(data1)

5. Wait for session key and stop it when the key is received

   .. code-block:: python3

      while True:
          data = cm1.receive()
          if data:
              session_key = data[1]
              concrete_utim.stop()
              break

6. Finally, stop Utim (if not stopped) and ConnectivityManager before exit

   .. code-block:: python3

      concrete_utim.stop()

      cm1.stop()