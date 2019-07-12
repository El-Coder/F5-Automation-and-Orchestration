Telemetry Streaming – Getting Started
-----------------------------------


Setup AWS CloudWatch IAM User
----------------
In your local browser, navigate to “https://federate.f5.com”

Click on the “AWS Console” tile

Open “Services” dropdown in the top menu bar

Under “Security, Identity and Compliance”, click on “IAM”

Click “Users” in the left menu bar

Click on the “Add User” button

Under Set User Details enter a user name (test_user)

Under Select AWS access type select Programmatic access

Under Add user to group select NE-Sales-Admins, Click “Next: Tags”

Click “Next: Review”

Click “Create User”

Copy the Access Key and Secret access key and paste them in Notes or Stickies

Click “Close”


Setup AWS CloudWatch Log Group
----------------
In your local browser, navigate to “https://federate.f5.com”

Click on the “AWS Console” tile

Open “Services” dropdown in the top menu bar

Under “Management & Governance”, click on “Cloudwatch”

Click “Logs” in the left menu bar

Under the “Actions” button dropdown, click “Create log group”

In “Log Group Name” enter “test_log_group”, click “Create log group”

Click “test_log_group”

Click “Create Log Stream” button

In “Log Stream Name” enter “test_log_stream”, click “Create Log Stream”

AWS CloudWatch IAM user
- Create a ticket in ServiceNow for access to an AWS IAM use
AWS CloudWatch Setup – Log Group, Log Stream, Grap
https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html

- Create a Log Group

    - Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/.
    - In the navigation pane, choose Logs.
    - Choose Actions, Create log group. 
    - Type a name for the log group and choose Create log group.

- Create a Log Stream

    - Click Log Group
    - Click Create Log Stream

- Create Graph 


Deploy and Start UDF
----------------
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

Login to UDF
----------------
Login to Big-IP
Open Firefox
Search “https://10.1.1.7/”
Login Credentials: admin - admin

Open AWS CloudWatch
Open Browser on local machine
Navigate to Federate >> AWS 4261 Console

Optional: Show Zero Traffic in AWS CloudWatch 
Navigate to AWS CloudWatch
Under Services Tab in top left select CloudWatch

To view logs—
In the left Menu Bar select Logs
Select AO_Log_Group
Select AO_Log_Stream
Show that there’s no

To view graph —
Scroll down to Default Dashboard


Postman App
----------------

Create a new collection name it – Telemetry Streaming Collection
Create a new Folder named – Telemetry Streaming 
Create a new request named – Big-IP-AWS-CloudWatch
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


Generate Traffic 
----------------
Open another tab in Firefox in Windows Server
Search 10.1.20.9
Refresh the page a few times

Show Streamed Traffic in AWS CloudWatch 
----------------
Navigate to AWS CloudWatch
Under Services Tab in top left select CloudWatch

To view logs—
In the left Menu Bar select Logs
Select AO_Log_Group
Select AO_Log_Stream

To view graph —
Scroll down to Default Dashboard

Create Custom Metric
----------------
Select Logs in left menu bar
Select 0 filters under Metric Filters
Select Add Metric Filter
Select Assign Metric
Under Create Metric Filter and Assign a Metric
-	Filter Name: custom_filtter_1
-	Metric Name: custom_metric_1




