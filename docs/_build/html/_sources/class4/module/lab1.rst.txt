Deploy UDF
-----------
.. image:: /_static/udf.png

1. Navigate to https://udf.f5.com

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
----------------- 

1. Open the RDP with the Credentials: Adminstrator - YLwbzexeWL9AT

2. Open Chrome.

.. image:: /_static/chrome.png

3. Go to “https://10.1.1.7/”.

4. Login into BIG-IP Credentials: admin - admin

.. image:: /_static/AS3_Login.png

5. Verify no application is already on BIG-IP.

.. image:: /_static/noapp.png

Configure Postman App
--------------------- 
1. Open the Postman application.

.. image:: /_static/desktop.png

2. At the bottom of the page in grey text select “Skip signing in and take me straight to the app”.

.. image:: /_static/skip.png

3. If presented with one, exit the pop-up screen. Toggle off “SSL certification verification” by navigating to File then Settings then under the General tab, toggle SSL certification verification.

.. image:: /_static/settings.png

.. image:: /_static/ssl.png


Send Declarations
-----------------
1. Click the collection Lab 3.1 - Application Service Deployments with AS3.

.. image:: /_static/3_1-1.png

2. Click the request "Get AS3 Services Version Info". Then click the send button. This returns version and release information for the instance of AS3 you are using. It also shows current and minimum required versions of the AS3 schema.

3. Now we will deploy our L4-7 services. Click the request "Deploy HTTP_Service". Then click the send button. Ensure that the response is 200.

.. image:: /_static/3_1-2.png

5. Login into BIG-IP to ensure services have been deployed. 

.. image:: /_static/app.png

6. Click the request "Get Deployed AS3 Services". Then click the send button. This returns the most-recently-deployed declaration with all tenants that AS3 knows about.

.. image:: /_static/3_1-3.png

7. Now we will delete all services from this partition. Click the request "POST to Delete All Services Under Tenant Class". Then click the send button. You should get a 200 OK from Postman.

.. image:: /_static/3_1-4.png

8. Open Browser and check that BIG-IP has no other services than from Common partition. 

.. image:: /_static/noas3.png

.. NOTE:: This is the end of the lab