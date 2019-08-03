Setup AWS CloudWatch IAM User
-----------------------------

1. In your local browser, navigate to “https://federate.f5.com”

.. image:: /_static/federate.png

2. Click on the “AWS Console” tile

.. image:: /_static/aws.png

3. Open “Services” dropdown in the top menu bar

.. image:: /_static/services.png

4. Under “Security, Identity and Compliance”, click on “IAM”

.. image:: /_static/iam.png

5. Click “Users” in the left menu bar

.. image:: /_static/users.png

6. Click on the “Add User” button

.. image:: /_static/add_user.png

7. Under Set User Details enter a user name “my_user”

8. Under Select AWS access type select Programmatic access

9. Select the Blue “Next: Permissions” button on the bottom right

.. image:: /_static/my_user.png

10. Under “Add user to group” select any group with “Administrator Access” as an Attached Policy

11. Select the Blue “Next: Tags” button on the bottom right

.. image:: /_static/next_tags.png

12. Dismiss the “Add User” section by selecting the Blue “Next: Review” button on the bottom right

13. Dismiss the “Review” section by selecting the Blue “Create User” button on the bottom right

14. Copy the Access Key and Secret access key and paste them in Notes or Stickies. Close the window by clicking “Close”

.. image:: /_static/access.png

Setup AWS CloudWatch Log Group
------------------------------

1. In your local browser, navigate to “https://federate.f5.com”

2. Click on the “AWS Console” tile

.. image:: /_static/aws.png

3. Open “Services” dropdown in the top menu bar

.. image:: /_static/aws_tile.png

4. Open “Services” dropdown in the top menu bar

.. image:: /_static/services.png

5. Under “Management & Governance”, click on “Cloudwatch”

.. image:: /_static/cloudwatch.png

6. Click “Logs” in the left menu bar

.. image:: /_static/logs.png

7. Click the blue “Let’s Get Started” button

.. image:: /_static/start.png

8. Click the grey “Create Log Group” button

.. image:: /_static/create_log.png

9. In “Log Group Name” enter “my_log_group”, click “Create log group”

.. image:: /_static/my_log.png

10. Click “test_log_group”

.. image:: /_static/my_log_g.png

11. Click “Create Log Stream” button

.. image:: /_static/logstream.png

12. In “Log Stream Name” enter “test_log_stream”, click “Create Log Stream”

.. image:: /_static/test_log.png

Deploy and Start UDF
----------------
.. image:: /_static/udf.png

1. Navigate to https://udf.f5.com

2. Open “Blueprints” in the left menu bar

3. In the search bar enter, “A&O Toolchain Demo”

4. Click the green “Deploy” button

5. Open “Deployments” in the left menu bar

6. In the “A&O Toolchain Demo” tile, click “Start”. Open “Components” in the menu bar
Wait until all components have started.

.. image:: /_static/udf1.png

7. Under the “Systems” in the “win2016” tile, click the “Access” dropdown and click “RDP”

.. image:: /_static/rdp.png

8. Open the RDP session download. Login credentials can be found by navigating to “Details” in the win2016 component

.. image:: /_static/details.png

9. Once in the win2016 component navigate to the “Details” tab and find the credentials for the RDP session highlighted below

.. image:: /_static/components.png

Enter the credentials for the RDP session to begin your session


Configure Postman
----------------

1. Once in the RDP session, open the Postman application

.. image:: /_static/desktop.png

2. At the bottom of the page in grey text select “Skip signing in and take me straight to the app”

.. image:: /_static/skip.png

3. If presented with one, exit the pop-up screen. Toggle off “SSL certification verification” by navigating File  Settings  under the General tab, toggle SSL certification verification

.. image:: /_static/settings.png

.. image:: /_static/ssl.png

4. For Collection Name enter “Telemetry Streaming Collection” and click “Create”

.. image:: /_static/collection.png

5. In the newly created Telemetry Streaming Collection, click the three dots and select “Add Request”

.. image:: /_static/request.png

6. In the pop-up window enter “BIG-IP to CloudWatch” for “Request name” and click Save to “Telemetry Streaming Collection”

.. image:: /_static/re.png

7. Expand the Telemetry Streaming Collection and click the “BIG-IP to CloudWatch” request

.. image:: /_static/ts_collection.png

8. lick “GET”, which is the default method, and select “POST” from the dropdown

.. image:: /_static/get.png

9. In the “Enter request URL” bar enter:
https://10.1.1.7/mgmt/shared/telemetry/declare

.. NOTE:: We’ll see the BIG-IP in a future section. Also API is being exposed because we alredy uploaded the RPM package.

.. image:: /_static/url.png

11. Open the “Authorization” tab in the menu bar.
Under the “Type” dropdown, click “Basic Auth”
Enter in Username as “admin” and Password as “admin”

.. image:: /_static/ts_collection.png

12. Open the “Body” tab in the menu bar
Select the “raw” radial button
In the “Text” dropdown select “JSON (application/json)”

.. image:: /_static/Picture3.png

Copy and paste the following into the body of the declaration:

13.

.. code-block:: HTTPS

 POST: https://10.1.1.7/mgmt/shared/telemetry/declare

.. code-block:: JSON

    {
        "class": "Telemetry",
        "My_System": {
            "class": "Telemetry_System",
            "systemPoller": {
                "interval": 60
            }
        },
        "My_Listener": {
            "class": "Telemetry_Listener",
            "port": 6514
        },
        "My_Consumer": {
            "class": "Telemetry_Consumer",
            "type": "AWS_CloudWatch",
            "region": "<< region of BIG-IP >>",
            "logGroup": "<< Log Group >>",
            "logStream": "<< Log Stream >>",
            "username": "<< AWS Access Key >>",
            "passphrase": {
                "cipherText": "<< AWS Secret Key >>"
            }
        }
    }

14. Create a new request named – BIG-IP-AWS-check info
Auth - basic auth, admin admin

.. code-block:: HTTPS

 GET: https://10.1.1.7/mgmt/shared/telemetry/info

15. Create a new request named –BIG-IP-AWS-Check if TS Avail
Auth - basic auth, admin admin

.. code-block:: HTTPS

 GET: https://10.1.1.7/mgmt/shared/telemetry/declare

Generate Traffic on BIG-IP 
-------------------------- 
1. In the RDP session, open Chrome

.. image:: /_static/chrome.png

2. Click on the “BIG-IP (10.1.1.7)” bookmark

.. image:: /_static/bookmark.png

3. Login to the BIG-IP with the username “admin” and the password “admin”

4. In the Common partition navigate to Local Traffic -> Network Map 

.. NOTE:: The virtual server, Existing_VS, along with its pool, existing_pool, have been preconfigured for this lab to produce traffic for Telemetry Streaming.

.. image:: /_static/network.png

5. Generate traffic by opening a new tab and navigating to the virtual server IP address and port combination “10.1.20.29:8080”

6. Reload the page 5+ times

.. image:: /_static/company.png

View Logs in AWS CloudWatch
-------------------------------

1. In your local browser, navigate to https://federate.f5.com 
*find photo above*
2. Click on the “AWS Console” tile
*find photo above*
3. Open “Services” dropdown in the top menu bar
*find photo above*
4. Under “Management & Governance”, click on “Cloudwatch”
*find photo above*

5. Click “Logs” in the left menu bar
*find photo above*
6. Select “my_log_group” (or the log group you’ve created) under Log Groups
*find photo above*
7. Select “test_log_stream” (or the log stream you’ve created) under Log Streams
*find photo above*


View Logs in AWS CloudWatch
---------------------------


####################################################
Show Streamed Traffic in AWS CloudWatch 
----------------
1. Navigate to AWS CloudWatch
Under Services Tab in top left select CloudWatch

2. To view logs—
In the left Menu Bar select Logs
Select AO_Log_Group
Select AO_Log_Stream

3. To view graph —
Scroll down to Default Dashboard

Create Custom Metric
----------------
1. Select Logs in left menu bar
2. Select 0 filters under Metric Filters
3. Select Add Metric Filter
4. Select Assign Metric
5. Under Create Metric Filter and Assign a Metric

    - Filter Name: custom_filtter_1
    - Metric Name: custom_metric_1



Login to UDF
----------------
1. Login to BIG-IP
2. Open Firefox
3. Search “https://10.1.1.7/”
4. Login Credentials: admin - admin
5. Open AWS CloudWatch
6. Open Browser on local machine
7. Navigate to Federate >> AWS 4261 Console

Optional: Show Zero Traffic in AWS CloudWatch 

8. Navigate to AWS CloudWatch
9. Under Services Tab in top left select CloudWatch

To view logs—
In the left Menu Bar select Logs
Select AO_Log_Group
Select AO_Log_Stream
Show that there’s no logs.

To view graph —
Scroll down to Default Dashboard 

.. Aniket to update

.. NOTE:: This is the end of the lab