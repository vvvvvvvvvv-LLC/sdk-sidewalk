.. _variant_sidewalk_hello:

Hello Sidewalk
##############

This sample demonstrates a simple Sidewalk end node application.
It can send the ``hello`` message to Sidewalk cloud on button press, as well as represent the Sidewalk status through LEDs.

Configuration options
*********************

The Hello Sidewalk application variant uses the ``OVERLAY_CONFIG="overlay-hello.conf"`` configuration, which is applied by default.
The sample variant supports the following Kconfig options:

.. include:: ../../includes/include_kconfig_common.txt

* ``CONFIG_SID_END_DEVICE_ECHO_MSGS`` -- The sample echoes the received Sidewalk messages (``GET`` and ``SET`` type) to the Sidewalk cloud.

.. _sidewalk_hello_user_interface:

User Interface
**************

.. tabs::

   .. group-tab:: nRF52 and nRF53 DKs

      Button 1 (long press):
         Enter DFU state - This action disables the Sidewalk stack and starts the Bluetooth LE SMP Server.
         You can update the firmware image using the nRF Connect for mobile application.
         To exit the DFU state, long press **Button 1**.

      Button 2 (long press):
         Factory reset - The application informs the Sidewalk stack about the factory reset event.
         The Sidewalk library clears its configuration from the non-volatile storage.
         After a successful reset, the device needs to be registered with the cloud services again.

      Button 3 (long press):
         Toggles the Sidewalk link mask - This action switches from Bluetooth LE to FSK, from FSK to LoRa, and from LoRa to Bluetooth LE.
         A log message informs about the link mask switch and the status of the operation.

      Button 1 (short press):
         Send Hello -  This action queues a message to the cloud.
         If Sidewalk is not ready, it displays an error without sending the message.
         On Bluetooth LE, the application performs Connection Requests before sending the message.

      Button 2 (short press):
         Set Connection Request - The device requests the Sidewalk Gateway to initiate a connection while the device advertises through Bluetooth LE.
         Gateways may not process this request, as it depends on the number of devices connected to it.

      Button 3 (short press):
         Toggles the Sidewalk link mask - This action switches from Bluetooth LE to FSK, from FSK to LoRa, and from LoRa to Bluetooth LE.
         A log message informs about the link mask switch and the status of the operation.

      LED 1:
         Indicates that the application established a link and connected successfully.
         The application is ready to send and receive Sidewalk messages.

      LED 2:
         Indicates that the application time synced successfully.

      LED 3:
         Indicates that the application registered successfully.

      LED 4:
         Indicates that the application woke up successfully.

   .. group-tab:: nRF54 DKs

      Button 0 (long press):
         Enter DFU state - This action disables the Sidewalk stack and starts the Bluetooth LE SMP Server.
         You can update the firmware image using the nRF Connect for mobile application.
         To exit the DFU state, long press **Button 0**.

      Button 1 (long press):
         Factory reset - The application informs the Sidewalk stack about the factory reset event.
         The Sidewalk library clears its configuration from the non-volatile storage.
         After a successful reset, the device needs to be registered with the cloud services again.

      Button 2 (long press):
         Toggles the Sidewalk link mask - This action switches from Bluetooth LE to FSK, from FSK to LoRa, and from LoRa to Bluetooth LE.
         A log message informs about the link mask switch and the status of the operation.

      Button 0 (short press):
         Send Hello -  This action queues a message to the cloud.
         If Sidewalk is not ready, it displays an error without sending the message.
         On Bluetooth LE, the application performs Connection Requests before sending the message.

      Button 1 (short press):
         Set Connection Request - The device requests the Sidewalk Gateway to initiate a connection while the device advertises through Bluetooth LE.
         Gateways may not process this request, as it depends on the number of devices connected to it.

      Button 2 (short press):
         Toggles the Sidewalk link mask - This action switches from Bluetooth LE to FSK, from FSK to LoRa, and from LoRa to Bluetooth LE.
         A log message informs about the link mask switch and the status of the operation.

      LED 0:
         Indicates that the application established a link and connected successfully.
         The application is ready to send and receive Sidewalk messages.

      LED 1:
         Indicates that the application time synced successfully.

      LED 2:
         Indicates that the application registered successfully.

      LED 3:
         Indicates that the application woke up successfully.

Building and running
********************

.. include:: ../../includes/include_building_and_running.txt

Testing
=======

See `Testing and debugging an application`_ for information on testing and debugging in the nRF Connect SDK.

After successfully building the sample and flashing manufacturing data, the sample is ready to use.
To refresh the logs, restart the program by pressing the :guilabel:`RESET` button on your development kit.

.. note::
   If you have multiple Echo devices registered under a single Amazon account, only one of those devices will be able to operate as an FSK gateway even if other devices have the same capability.
   If you need to operate multiple FSK gateways, we recommend setting them up under separate Amazon accounts.

Starting Sidewalk
-----------------

To start Sidewalk, do the following:

.. tabs::

   .. group-tab:: nRF52 and nRF53 DKs

      #. Connect your Nordic device to the PC through USB.
         Set the device's power switch to **ON**.

      #. Flash the sample application with the manufacturing data as described in the building and running section of the respective sample.

         You should see the following logs:

         .. code-block:: console

            *** Booting Sidewalk v2.7.99-ae200c29600e ***
            *** Using nRF Connect SDK v2.8.99-152acb8205de ***
            *** Using Zephyr OS v3.7.99-08f1c5bc9f20 ***
            ----------------------------------------------------------------
            Sidewalk SDK        = 1.17.1.18
            APP_BUILD_VERSION   = ae200c29600e
            APP_NAME            = sidewalk
            build time          = Oct 30 2024 15:07:09
            board               = nrf52840dk/nrf52840
            ----------------------------------------------------------------
            [00:00:01.095,637] <inf> sid_mfg: Need to parse mfg data
            [00:00:01.098,853] <inf> sid_mfg_parser_v8: MFG_ED25519 import success
            [00:00:01.101,607] <inf> sid_mfg_parser_v8: MFG_SECP_256R1 import success
            [00:00:01.109,493] <inf> sid_mfg: Successfully parsed mfg data

         When Sidewalk sample starts, **LED 4** turns on.

      #. Wait for the device to complete the registration.

         You should see the following logs:

         .. code-block:: console

            [00:00:31.045,471] <inf> sid_thread: Device Is registered, Time Sync Fail, Link status Up

         When Sidewalk registration status changes, **LED 3** turns on.

      #. Wait for the device to complete time sync with the Sidewalk network.

         You should see the following logs:

         .. code-block:: console

            [00:00:35.827,789] <inf> sid_thread: status changed: ready
            [00:00:35.827,850] <inf> sid_thread: Device Is registered, Time Sync Success, Link status Up

         When Sidewalk gets Time Sync, **LED 2** turns on.

      #. Wait for the status change.

         * For a Bluetooth LE device, status change occurs on request. Gateway connects over Bluetooth LE before sending down-link message, and device sends connection request before up-link message. Sidewalk automatically disconnects Bluetooth LE after some inactivity period.

         * For a LoRa and FSK device, the following messages appear in the log:

            .. code-block:: console

               [00:45:31.597,564] <inf> sid_thread: status changed: init
               [00:45:31.597,564] <dbg> sid_thread: on_sidewalk_status_changed: Device Is registered, Time Sync Success, Link status Up

         When Sidewalk Link Status is Up, **LED 1** turns on.

   .. group-tab:: nRF54 DKs

      #. Connect your Nordic device to the PC through USB.
         Set the device's power switch to **ON**.

      #. Flash the sample application with the manufacturing data as described in the building and running section of the respective sample.

         You should see the following logs:

         .. code-block:: console

            *** Booting Sidewalk v2.7.99-ae200c29600e ***
            *** Using nRF Connect SDK v2.8.99-152acb8205de ***
            *** Using Zephyr OS v3.7.99-08f1c5bc9f20 ***
            ----------------------------------------------------------------
            Sidewalk SDK        = 1.17.1.18
            APP_BUILD_VERSION   = ae200c29600e
            APP_NAME            = sidewalk
            build time          = Oct 30 2024 15:07:09
            board               = nrf54l15dk/nrf54l15/cpuapp
            ----------------------------------------------------------------
            [00:00:01.095,637] <inf> sid_mfg: Need to parse mfg data
            [00:00:01.098,853] <inf> sid_mfg_parser_v8: MFG_ED25519 import success
            [00:00:01.101,607] <inf> sid_mfg_parser_v8: MFG_SECP_256R1 import success
            [00:00:01.109,493] <inf> sid_mfg: Successfully parsed mfg data

         When Sidewalk sample starts, **LED 3** turns on.

      #. Wait for the device to complete the registration.

         You should see the following logs:

         .. code-block:: console

            [00:00:31.045,471] <inf> sid_thread: Device Is registered, Time Sync Fail, Link status Up

         When Sidewalk registration status changes, **LED 2** turns on.

      #. Wait for the device to complete time sync with the Sidewalk network.

         You should see the following logs:

         .. code-block:: console

            [00:00:35.827,789] <inf> sid_thread: status changed: ready
            [00:00:35.827,850] <inf> sid_thread: Device Is registered, Time Sync Success, Link status Up

         When Sidewalk gets Time Sync, **LED 1** turns on.

      #. Wait for the status change.

         * For a Bluetooth LE device, status change occurs on request. Gateway connects over Bluetooth LE before sending down-link message, and device sends connection request before up-link message. Sidewalk automatically disconnects Bluetooth LE after some inactivity period.

         * For a LoRa and FSK device, the following messages appear in the log:

            .. code-block:: console

               [00:45:31.597,564] <inf> sid_thread: status changed: init
               [00:45:31.597,564] <dbg> sid_thread: on_sidewalk_status_changed: Device Is registered, Time Sync Success, Link status Up

         When Sidewalk Link Status is Up, **LED 0** turns on.

Sending message to AWS MQTT
---------------------------

.. tabs::

   .. group-tab:: nRF52 and nRF53 DKs

      You can use `AWS IoT MQTT client`_ to view the received and republished messages from the device.

      #. Enter ``#`` and click :guilabel:`Subscribe to topic`.
         You are now subscribed to the republished device messages.

      #. To see the data republished into the subscribed MQTT topic, press **Button 1** on your development kit.

         .. code-block:: console

               # Logs from DK after pressing "Button 1"
               [00:04:57.461,029] <inf> sid_end device: Pressed button 1
               [00:04:57.461,120] <inf> sid_thread: sending counter update: 0
               [00:04:57.461,456] <inf> sid_thread: queued data message id:3


               # Logs from MQTT test client
            {
               "MessageId": "4c5dadb3-2762-40fa-9763-8a432c023eb5",
               "WirelessDeviceId": "5153dd3a-c78f-4e9e-9d8c-3d84fabb8911",
               "PayloadData": "MDA=",
               "WirelessMetadata": {
                  "Sidewalk": {
                     "CmdExStatus": "COMMAND_EXEC_STATUS_UNSPECIFIED",
                     "MessageType": "CUSTOM_COMMAND_ID_NOTIFY",
                     "NackExStatus": [],
                     "Seq": 2,
                     "SidewalkId": "BFFFFFFFFF"
                  }
               }

         Payload data is presented in base64 format.
         You can decode it using Python:

         .. code-block:: console

            python -c "import sys,base64;print(base64.b64decode(sys.argv[1].encode('utf-8')).decode('utf-8'))" MDA=
            00

         Data is republished into the subscribed MQTT topic.

         .. figure:: /images/Step7-MQTT-Subscribe.png

   .. group-tab:: nRF54 DKs

      You can use `AWS IoT MQTT client`_ to view the received and republished messages from the device.

      #. Enter ``#`` and click :guilabel:`Subscribe to topic`.
         You are now subscribed to the republished device messages.

      #. To see the data republished into the subscribed MQTT topic, press **Button 0** on your development kit.

         .. code-block:: console

               # Logs from DK after pressing "Button 0"
               [00:04:57.461,029] <inf> sid_end device: Pressed button 0
               [00:04:57.461,120] <inf> sid_thread: sending counter update: 0
               [00:04:57.461,456] <inf> sid_thread: queued data message id:3


               # Logs from MQTT test client
            {
               "MessageId": "4c5dadb3-2762-40fa-9763-8a432c023eb5",
               "WirelessDeviceId": "5153dd3a-c78f-4e9e-9d8c-3d84fabb8911",
               "PayloadData": "MDA=",
               "WirelessMetadata": {
                  "Sidewalk": {
                     "CmdExStatus": "COMMAND_EXEC_STATUS_UNSPECIFIED",
                     "MessageType": "CUSTOM_COMMAND_ID_NOTIFY",
                     "NackExStatus": [],
                     "Seq": 2,
                     "SidewalkId": "BFFFFFFFFF"
                  }
               }

         Payload data is presented in base64 format.
         You can decode it using Python:

         .. code-block:: console

            python -c "import sys,base64;print(base64.b64decode(sys.argv[1].encode('utf-8')).decode('utf-8'))" MDA=
            00

         Data is republished into the subscribed MQTT topic.

         .. figure:: /images/Step7-MQTT-Subscribe.png

Receiving message from AWS MQTT
-------------------------------

#. To be able to use AWS CLI, you must first complete `Installing or updating the latest version of the AWS CLI`_.

#. Run the following command to send a message to your Sidewalk Endpoint:

   .. code-block:: console

      aws iotwireless send-data-to-wireless-device --id=<wireless-device-id> --transmit-mode 0 --payload-data="<payload-data>" --wireless-metadata "Sidewalk={Seq=<sequence-number>}"

   * ``<wireless-device-id>`` is the Wireless Device ID of your Sidewalk Device.

     You can find it in the :file:`WirelessDevice.json` file, generated with the :file:`Nordic_MFG.hex` file during :ref:`setting_up_sidewalk_prototype`.
     If you have sent a message before, you can also find your Wireless Device ID in the messages sent from your device to AWS.

   * ``<payload-data>`` is base64 encoded.
   * ``<sequence-number>`` is an integer and should be different for each subsequent request.

#. Prepare a message payload in the base64 format by running the following command:

   .. code-block:: console

      python -c "import sys,base64;print(base64.b64encode(sys.argv[1].encode('utf-8')).decode('utf-8'))" "Hello   Sidewalk!"
      SGVsbG8gICBTaWRld2FsayE=

#. Increase a sequence number on every message.
   The device will not receive a message with lower or equal sequence number.

   Once you have populated the command with data, it should look similar to the following:

   .. code-block:: console

      aws iotwireless send-data-to-wireless-device --id=5153dd3a-c78f-4e9e-9d8c-3d84fabb8911 --transmit-mode 0 --payload-data="SGVsbG8gICBTaWRld2FsayE=" --wireless-metadata "Sidewalk={Seq=1}"

   Successfully sent response should look as follows:

   .. code-block:: console

      {
          "MessageId": "eabea2c7-a818-4680-8421-7a5fa322460e"
      }

   In case you receive the following error, make sure your IAM user or role has permissions to send data to your wireless device:

   .. code-block:: console

      {
         "Message": "User: arn:aws:iam::[AWS Account ID]:user/console_user is not authorized to perform:
         iotwireless:SendDataToWirelessDevice on resource: arn:aws:iotwireless:us-east-1:[AWS Account ID]:
         WirelessDevice/[Wireless Device ID]"
      }

   Data will be received in Sidewalk logs:

   .. code-block:: console

       [00:06:56.338,134] <inf> sid_thread: Message data:
                                     48 65 6c 6c 6f 20 20 20  53 69 64 65 77 61 6c 6b |Hello    Sidewalk
                                     21                                               |!

.. include:: ../../includes/include_testing_nordic_dfu.txt
