Setup AWS CloudWatch IAM User
-----------------------------

1. In your local browser, navigate to “https://federate.f5.com”

.. image:: /_static/federate.png

2. Click on the “AWS Console” tile.

.. image:: /_static/aws.png

3. Open “Services” dropdown in the top menu bar.

.. image:: /_static/services.png

4. Under “Security, Identity and Compliance”, click on “IAM”.

.. image:: /_static/iam.png

5. Click “Users” in the left menu bar.

.. image:: /_static/users.png

6. Click on the “Add User” button.

.. image:: /_static/add_user.png

7. Under Set User Details enter a user name “my_user”.

8. Under Select AWS access type select Programmatic access.

9. Select the Blue “Next: Permissions” button on the bottom right.

.. image:: /_static/my_user.png

10. Under “Add user to group” select any group with “Administrator Access” as an Attached Policy.

11. Select the Blue “Next: Tags” button on the bottom right.

.. image:: /_static/next_tags.png

12. Dismiss the “Add User” section by selecting the Blue “Next: Review” button on the bottom right.

13. Dismiss the “Review” section by selecting the Blue “Create User” button on the bottom right.

14. Copy the Access Key and Secret access key and paste them in Notes or Stickies. Close the window by clicking “Close”.

.. image:: /_static/access.png

Setup AWS CloudWatch Log Group
------------------------------

1. In your local browser, navigate to “https://federate.f5.com”

2. Click on the “AWS Console” tile.

.. image:: /_static/aws.png

3. Open “Services” dropdown in the top menu bar.

.. image:: /_static/aws_tile.png

4. Open “Services” dropdown in the top menu bar.

.. image:: /_static/services.png

5. Under “Management & Governance”, click on “Cloudwatch”.

.. image:: /_static/cloudwatch.png

6. Click “Logs” in the left menu bar.

.. image:: /_static/logs.png

7. Click the blue “Let’s Get Started” button.

.. image:: /_static/start.png

8. Click the grey “Create Log Group” button.

.. image:: /_static/create_log.png

9. In “Log Group Name” enter “my_log_group”, click “Create log group”.

.. image:: /_static/my_log.png

10. Click “my_log_group”.

.. image:: /_static/my_log_g.png

11. Click “Create Log Stream” button.

.. image:: /_static/logstream.png

12. In “Log Stream Name” enter “test_log_stream”, click “Create Log Stream”.

.. image:: /_static/test_log.png

Deploy and Start UDF
----------------
.. image:: /_static/udf.png

1. Navigate to https://udf.f5.com

2. Open “Blueprints” in the left menu bar.

3. In the search bar enter, “A&O Toolchain Demo”.

4. Click the green “Deploy” button.

5. Open “Deployments” in the left menu bar.

6. In the “A&O Toolchain Demo” tile, click “Start”, Open “Components” in the menu bar
wait until all components have started.

.. image:: /_static/udf1.png

7. Under the “Systems” in the “win2016” tile, click the “Access” dropdown and click “RDP”.

.. image:: /_static/rdp.png

8. Open the RDP session download. Login credentials can be found by navigating to “Details” in the win2016 component.

.. image:: /_static/details.png

9. Once in the win2016 component navigate to the “Details” tab and find the credentials for the RDP session highlighted below.

.. image:: /_static/component.png

10. Enter the credentials in your RDP client to begin your session.


Configure Postman
-----------------

1. Once in the RDP session, open the Postman application.

.. image:: /_static/desktop.png

2. At the bottom of the page in grey text select “Skip signing in and take me straight to the app”.

.. image:: /_static/skip.png

3. If presented with one, exit the pop-up screen. Toggle off “SSL certification verification” by navigating to File then Settings then under the General tab, toggle SSL certification verification.

.. image:: /_static/settings.png

.. image:: /_static/ssl.png

Send Declaration
----------------
1. Click the collection Lab 4.1 - Telemetry Streaming.

.. image:: /_static/4_1-1.png

2. Click the request "Get Telemetry Services Version Info" then click the send button. This returns version and release information for the instance of Telemetry Streaming you are using. It also shows current and minimum required versions of the Telemetry Streaming schema.

.. image:: /_static/4_1-1.png

3. Now we will post a declaration. Click the request "POST Telemetry Streaming Services to AWS CloudWatch" then click the send button. 

.. image:: /_static/4_1-3.png

3. Click the request "Get Telemetry Services Info" then click the send button. This will retrieve the declarations you previously sent to Telemetry Streaming.

.. image:: /_static/4_1-2.png


Generate Traffic on BIG-IP 
-------------------------- 
1. In the RDP session, open Chrome.

.. image:: /_static/chrome.png

2. Click on the “BIG-IP (10.1.1.7)” bookmark.

.. image:: /_static/search.png

3. Login to the BIG-IP with the username “admin” and the password “admin”

4. In the Common partition navigate to Local Traffic -> Network Map.

.. NOTE:: The virtual server, Existing_VS, along with its pool, existing_pool, have been preconfigured for this lab to produce traffic for Telemetry Streaming.

.. image:: /_static/vs.png

5. Generate traffic by opening a new tab and navigating to the virtual server IP address and port combination “10.1.20.29:8080”.

6. Reload the page 5+ times.

.. image:: /_static/company.png


View Logs in AWS CloudWatch 
---------------------------

1. Login to AWS console from https://federate.f5.com 

.. image:: /_static/federate.png

2. Click on services.

.. image:: /_static/image01.png

3. Click on cloudwatch.

.. image:: /_static/image02.png

4. Click on dashboard.

.. image:: /_static/image003.png

5. Click on create dashboard.

.. image:: /_static/image004.png

6. Give your dashboard a name and click create button.

.. image:: /_static/image005.png

7. Select query results and click add to dashboard.

.. image:: /_static/image006.png

8. You can see the dashboard is created.

.. image:: /_static/image007.png

9. Go back to cloudwatch menu and click on insight under Logs.

.. image:: /_static/image008.png

10. Select your log group in the top for results, select your duration (ex. 1 day) and run query to get results from logs.

.. image:: /_static/image009.png

11. Click on the actions tab and add the current results from logs to dashboard we created.

.. image:: /_static/image010.png

.. image:: /_static/image011.png

12. Select dashboard name, query results tab and add to dashboard.

.. image:: /_static/image012.png

13. Go back to dashboard in cloudwatch and select your dashboard name.

.. image:: /_static/image013.png

14. Click on any log from list to inspect the entries.

.. image:: /_static/image014.png

.. NOTE:: This is the end of the module