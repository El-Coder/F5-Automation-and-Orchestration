Setup AWS CloudWatch IAM User
----------------

.. image:: /_static/user.png

1. In your local browser, navigate to “https://federate.f5.com”

2. Click on the “AWS Console” tile

3. Open “Services” dropdown in the top menu bar

4. Under “Security, Identity and Compliance”, click on “IAM”

5. Click “Users” in the left menu bar

6. Click on the “Add User” button

7. Under Set User Details enter a user name (test_user)

8. Under Select AWS access type select Programmatic access

9. Under Add user to group select NE-Sales-Admins, Click “Next: Tags”

10. Click “Next: Review”

11. Click “Create User”

12. Copy the Access Key and Secret access key and paste them in Notes or Stickies

13. Click “Close”


Setup AWS CloudWatch Log Group
----------------
1. In your local browser, navigate to “https://federate.f5.com”

2. Click on the “AWS Console” tile

3. Open “Services” dropdown in the top menu bar

4. Under “Management & Governance”, click on “Cloudwatch”

5. Click “Logs” in the left menu bar

6. Under the “Actions” button dropdown, click “Create log group”

7. In “Log Group Name” enter “test_log_group”, click “Create log group”

8. Click “test_log_group”

9. Click “Create Log Stream” button

10. In “Log Stream Name” enter “test_log_stream”, click “Create Log Stream”


Deploy and Start UDF
----------------
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


Login to UDF
----------------
1. Login to Big-IP
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



Open Postman App
----------------

1. Create a new collection name it – Telemetry Streaming Collection
2. Create a new Folder named – Telemetry Streaming 
3. Create a new request named – Big-IP-AWS-CloudWatch
Auth - basic auth, admin admin

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
            "region": "<< region of Big-IP >>",
            "logGroup": "<< Log Group >>",
            "logStream": "<< Log Stream >>",
            "username": "<< AWS Access Key >>",
            "passphrase": {
                "cipherText": "<< AWS Secret Key >>"
            }
        }
    }

4. Create a new request named – Big-IP-AWS-check info
Auth - basic auth, admin admin

.. code-block:: HTTPS

 GET: https://10.1.1.7/mgmt/shared/telemetry/info

5. Create a new request named – Big-IP-AWS-Check if TS Avail
Auth - basic auth, admin admin

.. code-block:: HTTPS

 GET: https://10.1.1.7/mgmt/shared/telemetry/declare

Generate Traffic 
----------------
1. Open another tab in Firefox in Windows Server
2. Search 10.1.20.9
3. Refresh the page a few times

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






