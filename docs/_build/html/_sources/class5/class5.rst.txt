Telemetry Streaming
===================

This section of the demo is for the integration of Telemetry Streaming. The goal is to facilitate an automated configuration of Telemetry Streaming with a BIG-IP and connect the log stream to AWS CloudWatch. 

While there are many methods to consume Telemetry Streaming data, this demo uses AWS CloudWatch IAM users. We will collect data from the application hosted at 10.1.20.9 on Big-IP 10.1.1.7.

IAM user access key and secret key in UDF Documentation.


**Assumptions**

This lab guide assumes you know how to use postman. If not follow this tutorial https://clouddocs.f5.com/training/community/waf/html/class7/module1/lab1/lab1.html


**Prerequisites**

1. Get access to AWS CloudWatch IAM (this could take an extended amount of time 2 days average).
In your local browser, navigate to https://federate.f5.com


2. Click on the Service Now tile

.. image:: /_static/service_now.png

3. Click on “Submit Ticket”

.. image:: /_static/submit.png

4. Click on “Information Technology”

.. image:: /_static/it.png

5. To submit a ticket - Search for “aws”,  click on “Access to a cloud account”

- In the “Select a platform” dropdown, select AWS
- In the “Do you know the cloud account you want access to?” dropdown, enter your information
- For “Business justification”, enter the relevant information

.. image:: /_static/business.png


Expected time to complete: **1 hour**

.. toctree::
   :maxdepth: 1
   :glob:

   */*