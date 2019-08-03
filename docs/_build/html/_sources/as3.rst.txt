AS3 – Getting Started
-----------------------------------

Using Postman we will modify |bip| by using POST commands to the following URI 
#. how to highlight this line gray

.. code-block:: TMSH

    https://<BIG-IP>/mgmt/shared/appsvcs/declare

Clear Partition
~~~~~~~~

.. code-block:: JSON

    {
    "class": "AS3",
    "action": "deploy",
    "declaration": {
        "class": "ADC",
        "schemaVersion": "3.1.0",
        "new_partition": {
            "class": "Tenant"        
        }
    }
 }