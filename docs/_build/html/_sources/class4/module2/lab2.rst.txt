Using AS3 with BIG-IQ – Getting Started 
-----------------------------------

You can use the same method to post a declaration to AS3 on BIG-IQ as for BIG-IP. F5 disables basic authentication for HTTP/HTTPS requests to the BIG-IQ API by default for security enhancement.  
Whenever you perform an authenticated login to the BIG-IQ, and request a token using the Auth Token, you receive both an access token and refresh token. You can use the access token to send HTTP/HTTPS requests to a BIG-IQ until the access token expires after 5 minutes.
For more information https://clouddocs.f5networks.net/products/big-iq/mgmt-api/v6.1.0/ApiReferences/bigiq_public_api_ref/r_auth_login.html

Log in and Declaration
-----------

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

3. **POST** The following declaration.

.. code-block:: JSON 
    
    https://10.1.1.7/mgmt/shared/appsvcs/declare

.. code-block:: JSON 

    {
        "class": "AS3",
        "declaration": {
            "class": "ADC",
            "schemaVersion": "3.7.0",
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

Clear Partition
---------------


1. Now we will delete the application using the POST command again to the following URI

.. code-block:: JSON 
    
    https://10.1.1.7/mgmt/shared/appsvcs/declare

.. code-block:: JSON 

    {
        "class": "AS3",
        "action": "deploy",
        "declaration": {
            "class": "ADC",
            "schemaVersion": "3.1.0",
            "BigIQ_App_Tenant": {
                "class": "Tenant"        
            }
        }
    }

2.  Open Browser and check that BIG-IQ has no application

.. image:: /_static/noas3.png

.. NOTE:: This is the end of the lab