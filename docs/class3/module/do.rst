Declarative Onboarding – Getting Started
-----------------------------------

.. NOTE:: This lab already has RPM file. 
   To manually install follow https://clouddocs.f5.com/products/extensions/f5-declarative-onboarding/latest/installation.html

Deploy UDF
~~~~~~~~~~ 
.. image:: /_static/udf.png

1. Navigate to https://udf.f5.com

2. Open “Blueprints” in the left menu bar

3. In the search bar enter, “A&O Toolchain Demo”

4. Click the green “Deploy” button

5. Open “Deployments” in the left menu bar

6. In the “A&O Toolchain Demo” tile, click “Start”

7. In the “A&O Toolchain Demo” tile, click “Details”

8. Open “Components” in the menu bar

9. Wait until all components have started

10. Under the “Systems” in the “win2016” tile, click the “Access” dropdown and click “RDP”

.. image:: /_static/rdp.png

Open Browser in RDP
~~~~~~~~~~~~~~~~~~~
.. image:: /_static/chrome.png

1. Open Chrome
2. Search “https://10.1.1.7/”
3. Login Credentials: admin - admin
4. Check BIG-IP is blank 

Postman App
~~~~~~~~~~~ 
1.	Open the Postman application

.. image:: /_static/desktop.png

2.	In Preferences, toggle the SSL Certificate Verification off

.. image:: /_static/ssl.png

.. NOTE::
   In DO you can only **POST** and **GET**. Can’t **PATCH** You will overwrite things

5. First we will check the version of DO with the **GET** command to the URI

.. image:: /_static/post.png

.. code-block:: TMSH

 https://<BIG-IP>/mgmt/shared/declarative-onboarding/info

6. Now we will check to see the iControl LX extensions install on the BIG-IP with the **GET** command to the URI.

.. code-block:: TMSH

 https://10.1.1.7/mgmt/shared/iapp/installed-packages

7. Now we will check to see the current config on the BIG-IP with the **GET** command to the URI.

.. code-block:: TMSH

 https://10.1.1.7/mgmt/shared/declarative-onboarding?show=full

8. Now we will onboard a big a BIG-IP by using **POST** to the URI 

.. code-block:: TMSH

 https://<BIG-IP>/mgmt/shared/declarative-onboarding

with the following declaration:

.. code-block:: JSON

    {
        "schemaVersion": "1.0.0",
        "class": "Device",
        "async": true,
        
        "Common": {
            "class": "Tenant",
            "hostname": "ip-10-1-1-4.us-west-2.compute.internal",

            "myDns": {
                "class": "DNS",
                "nameServers": [
                    "8.8.8.8"
                ],
                "search": [
                    "f5.com"
                ]
            },
            
            "myNtp": {
                "class": "NTP",
                "servers": [
                    "0.pool.ntp.org",
                    "1.pool.ntp.org"
                ],
                "timezone": "UTC"
            },
            
            "internal": {
                "class": "VLAN",
                "interfaces": [
                    {
                        "name": "1.1",
                        "tagged": false
                    }
                ]
            },
            
            "internal-self": {
                "class": "SelfIp",
                "address": "10.1.10.99/24",
                "vlan": "internal",
                "allowService": "default",
                "trafficGroup": "traffic-group-local-only"
            },
            
            "external": {
                "class": "VLAN",
                "interfaces": [
                    {
                        "name": "1.2",
                        "tagged": false
                    }
                ]
            },
            
            "external-self": {
                "class": "SelfIp",
                "address": "10.1.20.99/24",
                "vlan": "external",
                "allowService": "none",
                "trafficGroup": "traffic-group-local-only"
            },
            
            "external_default_gateway": {
                "class": "Route",
                "gw": "10.1.10.9",
                "network": "default",
                "mtu": 1500
            }
            
        }
    }

9. Now we will check tasks with the **GET** command to the URI.

.. code-block:: TMSH

 https://10.1.1.4/mgmt/shared/declarative-onboarding/task/

Open Browser in RDP
~~~~~~~~~~~~~~~~~~~ 
1. Check BIG-IP is activated 

.. image:: /_static/activated.png

.. IMPORTANT::
   Don't revoke license 

.. NOTE:: This is the end of the lab