Using AS3 with BIG-IQ 7.0 
-------------------------

We can upload AS3 templates to BIG-IQ 7.0 and deploy applications to target BIG-IPs (10.1.1.7 for this lab). 


View BIG-IQ Without AS3 Templates 
---------------------------------

1. Open Chrome and navigate to the bookmark titled "(BIG-IQ 10.1.1.8)". 

2. Login to the BIG-IQ: username = admin, password = Admin123@

3. Click the Applications tab. In the left menu bar, select Application Templates.

.. image:: /_static/7_1.png

3. Notice above that AS3 Templates is empty.


Upload AS3 Templates to BIG-IQ  
------------------------------

.. NOTE:: Find templates https://clouddocs.f5.com/products/big-iq/mgmt-api/v6.1.0/ApiReferences/bigiq_public_api_ref/r_as3_template.html

1. Open the Postman application on the Windows desktop and click "BIG-IQ 7.0 Collection" and open the request, "Authenticate to BIG-IQ".

2. Open the "Body" tab to view the JSON declaration as seen below.

.. image:: /_static/7_4.png

3. To send this declaration to the URI "https://10.1.1.8/mgmt/shared/authn/login" click the blue "Send" button on the right.

4. Ensure that the response is 200.

.. image:: /_static/7_5.png

.. NOTE:: Auth tokens expire every 5 minutes. "Invalid registered claims." means the token has expired. Resend "Authenticate to BIG-IQ" request to fix.

5. Click into the request "Upload HTTP Template to BIG-IQ". Open the "Body" tab to view the JSON declaration.

.. image:: /_static/7_6.png

6. Click the blue "Send" button on the right. Ensure that response is 200.

.. image:: /_static/7_7.png


Create an Application with an AS3 Template in BIG-IQ
----------------------------------------------------

1. Open Chrome and navigate to the bookmark titled "BIG-IQ (10.1.1.8)".

2. Login to the BIG-IQ: username = admin, password = Admin123@

3. Click the Applications tab. In the left menu bar, select Application Templates. Now you will see your uploaded template.

.. image:: /_static/7_8.png

3. Publish the template by checking the box next to the template name and then clicking "publish".
When you receive the pop-up, click the blue "Publish" button.

.. image:: /_static/7_9.png


Enable RBAC
-----------
1. In the top menu bar, select Application. Under Role Management select "Roles". Under Custom Roles select "Application Roles". Select "Admin Role".

.. NOTE:: If not created, create a new custom application role

.. image:: /_static/7_18.png

2. For the Active Users and Groups section ensure admin is selected.

3. For the section AS3 Templates section ensure your template is selected.

.. image:: /_static/7_19.png

4. Click Save & Close


Create a new Application via AS3 Template
-----------------------------------------

1. Open the Postman Application

.. NOTE:: You might have to re-auth by sending the Authenticate to BIG-IQ declaration

2. Open and send the "Create HTTP App with Template" declaration. 

.. image:: /_static/7_20.png

3. Open Chrome and view the application on the BIG-IP (10.1.1.7). Login is "admin", "admin".

.. image:: /_static/7_21.png


Move the Application to a new App Service
-----------------------------------------

1. Navigate to the BIG-IQ at 10.1.1.8

2. Click on the Application tab and open the Unknown Applications titled

3. Select the newly created application and select "Move".

4. Enter "AS3 Apps" for the Application Name.

.. image:: /_static/7_22.png

5. Now navigate back to the Applications and see your newly created tile.

.. image:: /_static/7_23.png


.. NOTE:: End of the lab.