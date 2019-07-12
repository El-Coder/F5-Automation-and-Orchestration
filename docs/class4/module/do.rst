Declarative Onboarding – Getting Started
-----------------------------------

Deploy UDF
~~~~~~~~
.. image:: /_static/udf.png

Navigate to https://udf.f5.com

Open “Blueprints” in the left menu bar

In the search bar enter, “A&O Toolchain Demo”

Click the green “Deploy” button

Open “Deployments” in the left menu bar

In the “A&O Toolchain Demo” tile, click “Start”

In the “A&O Toolchain Demo” tile, click “Details”

Open “Components” in the menu bar

Wait until all components have started

Under the “Systems” in the “win2016” tile, click the “Access” dropdown and click “RDP”


Open Browser
~~~~~~~~
Check Big-IP is blank 


Postman App
~~~~~~~~
1.	Open win2016 RDP
2.	Open the Postman application
3.	In the top left, click Import

*	Click “Import from link” – will set up a link for collection and environment

4.	In Preferences, toggle the SSL Certificate Verification off


.. NOTE::
   In DO you can only **POST** and **GET**. Can’t **PATCH** You will overwrite things

First we will check the version of DO with th **GET** command to the URI

.. code-block:: TMSH

 https://<BIG-IP>/mgmt/shared/declarative-onboarding/info

Now we will onboard a big a Big-IP by using **POST** to the URI 

.. code-block:: TMSH

 https://<BIG-IP>/mgmt/shared/declarative-onboarding

with the following declaration

.. code-block:: JSON

    {
        "schemaVersion": "1.0.0",
        "class": "Device",
        "async": true,
        
        "Common": {
            "class": "Tenant",
            "hostname": "<hostname-of-bigip>",
            "myLicense": {
                "class": "License",
                "licenseType": "regKey",
                "overwrite": "true",
                "regKey": "<Reg-Key>"
            }
            
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
                "address": "10.1.10.10/24",
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
                "address": "10.1.20.10/24",
                "vlan": "external",
                "allowService": "none",
                "trafficGroup": "traffic-group-local-only"
            },
            
            "external_default_gateway": {
                "class": "Route",
                "gw": "10.1.10.1",
                "network": "default",
                "mtu": 1500
            }
            
        }
    }

Open Browser
~~~~~~~~
Check Big-IP is activated 

.. NOTE::
   Don't revoke license 