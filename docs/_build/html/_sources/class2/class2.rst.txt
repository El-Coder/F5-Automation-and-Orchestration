Environments & Lab Topology 
===========================

.. WARNING::
   All work for this lab will be performed exclusively from the Windows Jumphost. No installation or interaction with your local system is required.


**Lab Environments**
The following components have been included in your lab environment:

- 3 x F5 BIG-IP VE (1 for v13.x, 2 for v14.x)
- 2 x F5 BIG-IQ VE (6.x)
- 1 x Linux Server
- 1 x Windows Jumphost


**Lab Topology**

.. image:: /_static/network_map.png

A few key things to point out 10.1.1.4 is setup for onboarding a BIG-IP with Declarative Onboarding. 
Outside of the lab is an AWS environment for interacting with Telemetry Streaming. 
The Windows Jumppost contains Google Chrome and Postman which will be the main way we will interact with making REST calls to the BIG-IP and BIG-IQ. 
Once you are approved for access to AWS environment you can interact with it through Chrome on the Jumphost or your local machine's browser if you prefer.

.. list-table::
    :widths: 20 10 10 20
    :header-rows: 1

    * - **Component**
      - **Management IP**
      - **VLAN/IP Address(es)**
      - **Credentials**
    * - Windows Jumphost
      - 10.1.1.5
      - External: 10.1.20.5
      - Administrator/YLwbzexeWL9AT
    * - BIG-IP - AS3 & TS
      - 10.1.1.7
      - Internal: 10.1.10.10
        External: 10.1.20.10
      - admin/admin
    * - BIG-IP - Test Traffic Pool
      - 10.1.1.9
      - Internal: 10.1.10.20
        External: 10.1.20.11
      - admin/GoodBaklava123!@#
    * - BIG-IP - DO
      - 10.1.1.4
      - Internal: 10.1.10.14
        External: 10.1.20.24
      - admin/GoodBaklava123!@#
    * - BIG-IQ 
      - 10.1.1.8
      - External: 10.1.20.200
      - admin/ Admin123@
    * - BIG-IQ - DCD
      - 10.1.1.10
      - External: 10.1.20.199
      - admin/Admin123@


