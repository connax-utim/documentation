=============================================
Python Tutorial - SSH password authentication
=============================================

This tutorial describes the how you can use generated UTIM session key to connect to server via ssh


Uhost
=====

Run Uhost as described in `Python Tutorial - Hello World </tutorial/python/tutorial-helloworld>`__.


Creating users
==============

Code of this example `here <https://github.com/connax-utim/uhost-python/blob/master/examples/ssh%20password%20authentication/generate_keys.py>`__.

This script creates new user for linux machine using UTIM ID's as username and UTIM's session key as password

Step:

1. Connect to database

2. Select IDs and keys of UTIMs

3. Create or delete (if exists) and create new user.


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


6. Use ``paramiko`` library to connect via ssh, execute command and print result of command executing:

   .. code-block:: python3

      ssh = paramiko.SSHClient()
      ssh.load_system_host_keys()
      ssh.connect(_SERVER, username=_UTIM_NAME.hex().upper(), password=session_key.hex())
      ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command('ifconfig')
      for line in iter(ssh_stdout.readline, ""):
          print(line, end="")
      ssh.close()


7. Finally, stop Utim (if not stopped) and ConnectivityManager before exit

   .. code-block:: python3

      concrete_utim.stop()

      cm1.stop()