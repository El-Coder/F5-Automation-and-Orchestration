Telemetry Streaming
===================

This section of the demo is for the integration of Telemetry Streaming. The goal is to facilitate an automated configuration of Telemetry Streaming with a Big-IP and connect the log stream to AWS CloudWatch. 

While there are many methods to consume Telemetry Streaming data, this demo uses AWS CloudWatch IAM users. We will collect data from the application hosted at 10.1.20.9 on Big-IP 10.1.1.7.

IAM user access key and secret key in UDF Documentation.


**Prerequisites**


Get access to AWS CloudWatch IAM – Note that this could take an extended amount of time (2 days avg)
In your local browser, navigate to https://federate.f5.com


- Click on the Service Now tile

.. image:: /_static/Picture1.png
- Click on “Submit Ticket”

- Click on “Information Technology”

Under “Cloud Services”, click on “Access to a cloud account”

- In the “Select a platform” dropdown, select AWS
- In the “Do you know the cloud account you want access to?” dropdown, enter your information
- For “Business justification”, enter the relevant information

Expected time to complete: **1 hour**

.. toctree::
   :maxdepth: 1
   :glob:

   */*