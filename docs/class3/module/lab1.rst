Deploy UDF
-----------
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
-----------

.. NOTE:: That no application is already on big-ip

.. image:: /_static/noapp.png

Open Postman
-----------
Using Postman we will first check the verison of AS3 the |bip| has by using a **GET** command to the following URI 

.. code-block:: TMSH

    https://<BIG-IP>/mgmt/shared/appsvcs/info

Using Postman we will modify |bip| by using **POST** commands to the following URI 


.. code-block:: TMSH

    https://<BIG-IP>/mgmt/shared/appsvcs/declare

.. code-block:: JSON

        {
        "class": "AS3",
        "action": "deploy",
        "persist": true,
        
        "declaration": {
            "class": "ADC",
            "schemaVersion": "3.0.0",
            "controls": {
                "trace": true
            },
            
            "new_partition": {
                "class": "Tenant",
                
                "Application_1": {
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
                            "http"	
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
                    },
                    
                    "http": {
                        "class":"HTTP_Profile"
                    }
                }
            }
        }
    }

Open Browser and check now Big-IP has application 



Clear Partition
-----------

Now we will delete the application using the **POST** command again to the following URI 

.. code-block:: TMSH

    https://<BIG-IP>/mgmt/shared/appsvcs/declare

.. code-block:: JSON

    {
    "class": "AS3",
    "action": "deploy",
    "declaration": {
        "class": "ADC",
        "schemaVersion": "3.8.0",
        "new_partition": {
            "class": "Tenant"        
        }
    }
 }

Open Browser and check now Big-IP has no application 