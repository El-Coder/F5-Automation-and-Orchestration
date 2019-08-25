Declarative Onboarding – Getting Started
-----------------------------------

.. NOTE:: This lab already has RPM file. 
   To manually install follow https://clouddocs.f5.com/products/extensions/f5-declarative-onboarding/latest/installation.html

Deploy UDF
~~~~~~~~~~ 
.. image:: /_static/udf.png

1. Navigate to https://udf.f5.com.

2. Open “Blueprints” in the left menu bar.

3. In the search bar enter, “A&O Toolchain Demo”.

4. Click the green “Deploy” button.

5. Open “Deployments” in the left menu bar.

6. In the “A&O Toolchain Demo” tile, click “Start”.

7. In the “A&O Toolchain Demo” tile, click “Details”.

8. Open “Components” in the menu bar.

9. Wait until all components have started.

10. Under the “Systems” in the “win2016” tile, click the “Access” dropdown and click “RDP”.

.. image:: /_static/rdp.png

Login into BIG-IP
~~~~~~~~~~~~~~~~~

1. Open the RDP with the Credentials: Adminstrator - YLwbzexeWL9AT

2. Open Chrome.

.. image:: /_static/chrome.png

3. Go to “https://10.1.1.4/”.

4. Login Credentials: admin - GoodBaklava123!@#

.. image:: /_static/DO_Chrome.png

5. Verify BIG-IP isn't already configured.  

Configure Postman App
~~~~~~~~~~~~~~~~~~~~~  
1. Open the Postman application.

.. image:: /_static/desktop.png

2. At the bottom of the page in grey text select “Skip signing in and take me straight to the app”.

.. image:: /_static/skip.png

3. If presented with one, exit the pop-up screen. Toggle off “SSL certification verification” by navigating to File then Settings then under the General tab, toggle SSL certification verification.

.. image:: /_static/settings.png

.. image:: /_static/ssl.png


Send Declaration
~~~~~~~~~~~~~~~~    

1. In the Collections tab click "Declarative Onboarding Collection".

2. Click the request "Get Declarative Onboarding Version Info". 

3. Click the send button and ensure that the response is 200.

.. image:: /_static/2_1-1.png

.. NOTE:: When you log onto BIG-IP it is not onboarded.

.. image:: /_static/2_1.png

4. Now we will onboard the BIG-IP. Click the request "POST DO Declaration to Configue BIG-IP". 

.. NOTE:: For the regKey field use a valid license key here.

.. code-block:: JSON
   :linenos:
   :emphasize-lines: 11

    {
        "schemaVersion": "1.0.0",
        "class": "Device",
        "async": true,
        "Common": {
            "class": "Tenant",
            "hostname": "ip-10-1-1-4.us-west-2.compute.internal",
            "myLicense": {
                "class": "License",
                "licenseType": "regKey",
                "regKey": "AAAAA-BBBBB-CCCCC-DDDDD-EEEEEEE"
            },
            "myProvisioning": {
                "class": "Provision",
                "ltm": "nominal"
            },
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

.. NOTE::
   In DO you can only **POST** and **GET**. Can’t **PATCH** You will overwrite things

5. Click the send button. If you get a status code of 202 wait a few minutes.

.. image:: /_static/2_1-2.png

6. Click the request "See Status of Declaration" then click the send button to see if onboarding is complete. You should get a 200 OK from Postman once onboarding process is done.

.. image:: /_static/2_1-3.png

Verify Changes on BIG-IP
~~~~~~~~~~~~~~~~~~~~~~~~ 

1. Log back into BIG-IP and to see changes for DNS go to configuration utility, click the Configuration tab then under the Device tab click DNS.  

.. image:: /_static/dns.png

2. To see changes for NTP go to configuration utility, click the Configuration tab then under the Device tab click NTP.  

.. image:: /_static/ntp.png

3. To see changes for the internal and external VLANs go to configuration utility, click the Network tab then under the VLANs tab click VLAN list.  

.. image:: /_static/vlan.png

4. To see changes for the internal-self and external-self VLANs go to configuration utility, click the Network tab then click the Self IPs tab.  

.. image:: /_static/selfip.png

5. To see changes for external_default_gateway go to configuration utility, click the Network tab then click the Routes tab.  

.. image:: /_static/external_gw.png


.. IMPORTANT::
   Don't revoke license 

.. NOTE:: This is the end of the lab