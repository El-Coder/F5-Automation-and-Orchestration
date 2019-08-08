Using AS3 with BIG-IQ – Getting Started 
-----------------------------------

You can use the same method to post a declaration to AS3 on BIG-IQ as for BIG-IP. F5 disables basic authentication for HTTP/HTTPS requests to the BIG-IQ API by default for security enhancement.  
Whenever you perform an authenticated login to the BIG-IQ, and request a token using the Auth Token, you receive both an access token and refresh token. You can use the access token to send HTTP/HTTPS requests to a BIG-IQ until the access token expires after 5 minutes.
For more information https://clouddocs.f5networks.net/products/big-iq/mgmt-api/v6.1.0/ApiReferences/bigiq_public_api_ref/r_auth_login.html

Log in 
------

1. To use BIG-IQ you must **POST** login for authentication.

.. code-block:: JSON 
    
    https://10.1.1.7/mgmt/shared/appsvcs/declare

.. code-block:: JSON

    {
    "username":"admin",
    "password":"*******"
    } 
.. image:: /_static/iq.png

2. From the response above **POST** the refresh token below after access token expires.

.. code-block:: JSON 
    
    https://10.1.1.7/mgmt/shared/appsvcs/declare

.. code-block:: JSON 

    {
    "refreshToken": {
    "token": "insert token here"}
    }

Create Postman Collection and Send Declaration
----------------------------------------------    

1. In the Collections tab click Create a collection and name it "AS3 To BIG-IQ Collection" and click Create.

.. image:: /_static/AS3_BIGIQCollection.png

2. In the newly created "AS3 To BIG-IQ Collection", click the three dots and select Add Request.

.. image:: /_static/AS3_BIGIQRequest.png

3. In the pop-up window enter “AS3 Declaration” for Request name and click Save to “AS3 Collection”

.. image:: /_static/AS3_BIGIQDeets.png

4. Expand the AS3 To BIG-IQ Collection and click the “AS3 Declaration” request.

.. image:: /_static/AS3_BIGIQExpanded.png

5. Click “GET”, which is the default method, and select “POST” from the dropdown.

.. image:: /_static/AS3_BIGIQPost.png

6. In the “Enter request URL” bar enter:

.. code-block:: TMSH

    https://https://10.1.1.7/mgmt/shared/appsvcs/declare

7. Open the “Authorization” tab in the menu bar. Under the “Type” dropdown, click “Basic Auth” Enter in Username as “admin” and Password as "admin”

.. image:: /_static/AS3_BIGIQAuth.png

8. Open the “Body” tab in the menu bar Select the “raw” radial button In the “Text” dropdown select “JSON (application/json)”

.. image:: /_static/AS3_BIGIQblob.png

9. Paste the following declaration into the text box:

.. code-block:: JSON 

    {
        "class": "AS3",
        "declaration": {
            "class": "ADC",
            "schemaVersion": "3.13.0",
            "id": "bigiq_app_test",
            "label": "Test",
            "remark": "Generic app",
            "target": {
                "hostname": "ip-10-1-1-7.us-west-2.compute.internal"
            },
            "BigIQ_App_Tenant": {
                "class": "Tenant",
                "BigIQApp": {
                    "class": "Application",
                    "template": "generic",
                    "BigIQVS": {
                        "class": "Service_Generic",
                        "virtualAddresses": [
                            "10.1.20.9"
                        ],
                        "virtualPort":80,
                        "pool": "pool_with_vip"
                    },
                
                    "pool_with_vip": {
                        "class": "Pool",
                        "monitors": [
                            "http",
                            {"use": "http_basic"},
                            {"use": "http_index"},
                            {"use": "http_default"}
                        ],
                        "members": [{
                        "servicePort": 80,
                        "serverAddresses": [
                            "10.1.10.5"
                        ]
                        }]
                    },
                
                    "http_basic": {
                        "class": "Monitor",
                        "monitorType": "http",
                        "interval":4,
                        "timeout": 13,
                        "send": "GET /basic.html\r\n"
                    },
                
                    "http_index": {
                        "class": "Monitor",
                        "monitorType": "http",
                        "interval":5,
                        "timeout": 16,
                        "send": "GET /index.html\r\n"
                    },
                
                    "http_default": {
                        "class": "Monitor",
                        "monitorType": "http",
                        "interval":8,
                        "timeout": 25,
                        "send": "GET /index.html\r\n"
                    }
                }
            }
        }
    }    

10. Save the request by clicking the save button next to the send button.

11. Now we will configure our L4-L7 services by clicking the send button. You should get a 200 OK from Postman.

.. image:: /_static/AS3_BIGIQSuccess.png

12. Open Chrome and check now BIG-IP has application. Also make sure you are in the correct partition.

.. image:: /_static/bigiqapp.png

Add New Request To Collection
----------------------------- 

1. Follow the steps from 2-8 in the previous section this time the request name will be called "Clear Partition".

2. Paste the following declaration.

.. code-block:: JSON

    {
    "class": "AS3",
    "action": "deploy",
    "declaration": {
        "class": "ADC",
        "schemaVersion": "3.13.0",
        "new_partition": {
            "class": "Tenant"        
        }
    }
 }

3. Save the request

.. image:: /_static/AS3_BIGIQClearBeforeSend.png

4. Now we will delete the application by clicking the send button. You should get a 200 OK from Postman.

.. image:: /_static/AS3_BIGIQClearSuccess.png

5. Open Browser and check that BIG-IP has no application 

.. image:: /_static/noas3.png

.. NOTE:: This is the end of the lab