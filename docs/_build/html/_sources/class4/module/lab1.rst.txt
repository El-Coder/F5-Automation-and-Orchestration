Deploy UDF
-----------
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
----------------- 

1. Open the RDP with the Credentials: Adminstrator - YLwbzexeWL9AT

2. Open Chrome.

.. image:: /_static/chrome.png

3. Go to “https://10.1.1.7/”.

4. Login into BIG-IP Credentials: admin - admin

.. image:: /_static/AS3_Login.png

5. Verify no application is already on BIG-IP

.. image:: /_static/noapp.png

Configure Postman App
--------------------- 
1. Open the Postman application

.. image:: /_static/desktop.png

2. At the bottom of the page in grey text select “Skip signing in and take me straight to the app”

.. image:: /_static/skip.png

3. If presented with one, exit the pop-up screen. Toggle off “SSL certification verification” by navigating to File then Settings then under the General tab, toggle SSL certification verification

.. image:: /_static/settings.png

.. image:: /_static/ssl.png


Create Postman Collection and Send Declaration
----------------------------------------------

1. In the Collections tab click Create a collection and name it "AS3 Collection" and click Create.

.. image:: /_static/AS3_Collection.png

2. In the newly created "AS3 Collection", click the three dots and select Add Request.

.. image:: /_static/AS3_Dots.png

3. In the pop-up window enter “AS3 Declaration” for Request name and click Save to “AS3 Collection”

.. image:: /_static/AS3_Request.png

4. Expand the AS3 Collection and click the “AS3 Declaration” request.

.. image:: /_static/AS3_Expanded.png

5. Click “GET”, which is the default method, and select “POST” from the dropdown.

.. image:: /_static/AS3_Post.png

6. In the “Enter request URL” bar enter:

.. code-block:: TMSH

    https://10.1.1.7/mgmt/shared/appsvcs/declare

7. Open the “Authorization” tab in the menu bar. Under the “Type” dropdown, click “Basic Auth” Enter in Username as “admin” and Password as "admin”

.. image:: /_static/AS3_Auth.png

8. Open the “Body” tab in the menu bar Select the “raw” radial button In the “Text” dropdown select “JSON (application/json)”

.. image:: /_static/AS3_blob.png

9. Paste the following declaration into the text box:

.. code-block:: JSON

    {
        "class": "AS3",
        "action": "deploy",
        "persist": true,
        
        "declaration": {
            "class": "ADC",
            "schemaVersion": "3.13.0",
            "controls": {
                "trace": true
            },
            
            "My_AS3_Tenant": {
                "class": "Tenant",
                
                "My_AS3_App": {
                    "class": "Application",
                    "template": "generic",


                    "Virtual_Server": {
                        "class": "Service_Generic",
                        "virtualPort": 80,
                        "virtualAddresses": [
                            "10.1.20.9"    
                        ],
                        "pool": "http_pool",
                        "profileHTTP": {"use": "http"}
                    },
                    
                    "http_pool": {
                        "class": "Pool",
                        "monitors": [
                            {
                                "bigip": "/Common/http_index"
                            },
                            {
                                "bigip": "/Common/http_basic"
                            },
                            {
                                "bigip": "/Common/http_default"
                            }
                        ],
                        "members":[
                            {
                                "servicePort": 80,
                                "serverAddresses": 
                                [
                                    "10.1.10.5"
                                ]
                            }
                        ]
                    }
                }
            }
        }
    }





10. Save the request by clicking the save button next to the send button.

.. image:: /_static/AS3_Save.png

11. Now we will configure our L4-L7 services by clicking the send button. You should get a 200 OK from Postman.

.. image:: /_static/AS3_Success.png

12. Open Chrome and check now BIG-IP has application. Also make sure you are in the correct partition.

.. image:: /_static/app.png

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

.. image:: /_static/AS3_ClearBeforeSend.png

4. Now we will delete the application by clicking the send button. You should get a 200 OK from Postman.

.. image:: /_static/AS3_ClearSuccess.png

5. Open Browser and check that BIG-IP has no application 

.. image:: /_static/noas3.png

.. NOTE:: This is the end of the lab