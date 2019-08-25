Using AS3 with BIG-IQ 7.0 
-------------------------

With BIG-IQ version 7.0 we can upload AS3 templates to the BIG-IQ with Postman. 
In the BIG-IQ GUI we can set values (IP addresses, ports, Tenant name) 
and then send that AS3 template to a target BIG-IP (10.1.1.7 for this lab). 


View BIG-IQ Without AS3 Templates 
---------------------------------

1. Open Chrome and navigate to the bookmark titled "(BIG-IQ 10.1.1.8)". 

2. Login to the BIG-IQ: username = admin, password = GoodBaklava123@

3. Click the Applications tab. In the left menu bar, select Application Templates.

.. image:: /_static/7_1.png

4. Notice above that AS3 Templates is empty.


Configure Postman App
~~~~~~~~~~~~~~~~~~~~~  
1. Open the Postman application

.. image:: /_static/desktop.png

2. At the bottom of the page in grey text select “Skip signing in and take me straight to the app”

.. image:: /_static/skip.png

3. If presented with one, exit the pop-up screen. Toggle off “SSL certification verification” by navigating to File then Settings then under the General tab, toggle SSL certification verification

.. image:: /_static/settings.png

.. image:: /_static/ssl.png


Send AS3 Templates to BIG-IQ with Postman  
-----------------------------------------

.. NOTE:: The declarations titled "AS3-F5-\*-big-iq-default" are requests made by F5 that work with BIG-IQ 7.0. 
When we send the declaration, the template will be stored on the BIG-IQ.

1. Click on the Collections tab then click on "Lab 3.2 - Using AS3 with BIG-IQ 7.0" and open the request, "Authenticate to BIG-IQ".

2. Open the "Body" tab to view the JSON declaration as seen below.

.. image:: /_static/7_4.png

3. To send this declaration to the URI "https://10.1.1.8/mgmt/shared/authn/login" click the blue "Send" button on the right.

4. Ensure that the response is 200.

.. image:: /_static/7_5.png

*** If error: "Invalid registered claims.", the token has expired. Resend "Authenticate to BIG-IQ" request to fix.***

5. Click into the request "AS3-F5-HTTP-lb-template-big-iq-default". Open the "Body" tab to view the JSON declaration.

.. image:: /_static/7_6.png

6. Click the blue "Send" button on the right. Ensure that response is 200.

.. image:: /_static/7_7.png


Create an Application with an AS3 Template in BIG-IQ
----------------------------------------------------

1. Open Chrome and navigate to the bookmark titled "BIG-IQ (10.1.1.8)".

2. Login to the BIG-IQ: username = admin, password = GoodBaklava123@

3. Click the Applications tab. In the left menu bar, select Application Templates. Now you will see your uploaded template.

.. image:: /_static/7_8.png

3. Publish the template by checking the box next to the template name and then clicking "publish".
When you receive the pop-up, click the blue "Publish" button.

.. image:: /_static/7_9.png

4. In the left menu bar, select Application and select the "Create" button.

.. image:: /_static/7_10.png

5. Enter an "Application Name". Here I've named mine, "app_made_by_as3_template".

6. From the "Template Type" dropdown, select our imported template.

.. image:: /_static/7_11.png

7. For the "General Properties" section, enter similar information. For "Target", make sure to select the corresponding hostname to 10.1.1.7 from the dropdown

.. image:: /_static/7_12.png

8. For the "Pool" section, add "10.1.10.13" in "Server Addresses"

.. image:: /_static/7_13.png

9. For the "Service_HTTP" section, add "10.1.20.50" in "Virtual addresses". 

.. image:: /_static/7_14.png

10. Click the blue "Create" button. The page will load to below.

.. image:: /_static/7_15.png


View Created Application on BIG-IP (10.1.1.7) 
----------------------------------------------

1. Open a new tab in Chrome

2. Open Chrome and navigate to the bookmark titled "(BIG-IP 10.1.1.87)". 

3. Login to the BIG-IQ: username = admin, password = admin

4. Change partition to your new partition.

5. Open Local Traffic --> Network Map

.. image:: /_static/7_16.png

6. Open a new tab and enter the VIP 10.1.20.50 in the browser to view the app.

.. image:: /_static/7_17.png


.. NOTE:: End of the lab.