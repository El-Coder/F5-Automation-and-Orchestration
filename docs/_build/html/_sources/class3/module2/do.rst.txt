Declarative Onboarding – Installing RPM file (Optional)
-----------------------------------

This is outside the scope of this lab guide since the RPM file is already installed however these would be the steps.

Download Declarative Onboarding and Application Services 3 Extensions
~~~~~~~~
1.	Open win2016 RDP
2.	Open Chrome
3.	Navigate to: https://github.com/F5Networks/f5-declarative-onboarding/blob/master/dist/ 

*	This is an extension for Declarative Onboarding
*	Download the RPM file

4.	Navigate to: https://github.com/F5Networks/f5-appsvcs-extension/tree/master/dist/latest

*	This is an extension for Application Services 3 Extension
*	Download the RPM file

Enable the Big-IP to Accept iControl LX Extensions
~~~~~~~~
1.	In your local browser, navigate to udf.f5.com
2.	Click ‘Deployments’
3.	Click ‘Details’ then ‘A&O ASE Demo’ then ‘Components’
4.	Under ‘F5 Products’ tab, click ‘Access’ dropdown ‘Web Shell’
5.	Note that you are Root in Big-IP

*	Enter command: “touch /var/config/rest/iapps/enable”
*	Examine that iControl LX Extension has been installed on the Big-IP GUI

6.	Open win2016 RDP 
7.	Open Chrome
8.	Navigate to Big-IP bookmark

*	Login with admin / admin credentials

9.	In Big-IP GUI

*	Click on iApps  iControl LX
*	Click upload
*	Upload both the Declarative Onboarding and Application Services 3 Extension