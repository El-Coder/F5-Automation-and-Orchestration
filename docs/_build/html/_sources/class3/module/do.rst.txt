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
1. Open the Postman application

.. image:: /_static/desktop.png

2. At the bottom of the page in grey text select “Skip signing in and take me straight to the app”

.. image:: /_static/skip.png

3. If presented with one, exit the pop-up screen. Toggle off “SSL certification verification” by navigating to File then Settings then under the General tab, toggle SSL certification verification

.. image:: /_static/settings.png

.. image:: /_static/ssl.png

Create Postman Collection and Send Declaration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~    

1. In the Collections tab click Create a collection and name it "Declarative Onboarding Collection" and click Create.

.. image:: /_static/DO_Collection.png

2. In the newly created "Declarative Onboarding Collection", click the three dots and select Add Request.

.. image:: /_static/DO_Request.png

3. In the pop-up window enter “Declarative Onboarding Declaration” for Request name and click Save to “Declarative Onboarding Collection”

.. image:: /_static/DO_Declaration.png

4. Expand the Declarative Onboarding Collection and click the “Declarative Onboarding Declaration” request.

.. image:: /_static/DO_Expanded.png

5. Click “GET”, which is the default method, and select “POST” from the dropdown.

.. image:: /_static/DO_Post.png

6. In the “Enter request URL” bar enter:

.. code-block:: TMSH

    https://https://10.1.1.4/mgmt/shared/declarative-onboarding/

7. Open the “Authorization” tab in the menu bar. Under the “Type” dropdown, click “Basic Auth” Enter in Username as “admin” and Password as “GoodBaklava123!@#”

.. image:: /_static/DO_Auth.png

8. Open the “Body” tab in the menu bar Select the “raw” radial button In the “Text” dropdown select “JSON (application/json)”

.. image:: /_static/DO_blob.png

9. Paste the following declaration into the text body then click the save button next to the send button, then click the send button.

.. NOTE:: Use your license key here.

.. code-block:: JSON

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
                "regKey": "IELDT-MUCZT-GMSMR-KQOKZ-NOVFIBG"
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

10. Now we will check status of the declaration, change to the **GET** method and the URI to the line below, then click the send button.

.. code-block:: TMSH

    https://10.1.1.4/mgmt/shared/declarative-onboarding/info

You should get a 200 OK from Postman.

.. image:: /_static/DO_Okay.png

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