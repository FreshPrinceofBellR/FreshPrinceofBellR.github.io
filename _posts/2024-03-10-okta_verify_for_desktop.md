---
title: "Deploy Okta Verify via Microsoft Intune"
date: 2024-05-22 13:30:00 +0000
categories: [Microsoft Intune, Okta, Application]
tags: [intune, microosft intune, okta]
author: "Richard Bell"
---

# How to Deploy Okta Verify with Microsoft Intune
Okta Verify for Desktop can be deployed via Microsoft Intune with a series of specifications to meet your organisations needs, below will go through the process of how to package the Okta Verify application for Windows along with detailing the avaliable settings to deploy the application with.

## Packaging the Application
To pacakge the application, you will require the Microsoft Win32 Content Prep tool. This application will be used to bundle the executable file into a .intunewin file format which will be used to upload the application to Microsoft Intune.

You can download the Microsoft Win32 Content Prep Tool from [GitHub.](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool)

For a guide on how to run the Win32 Content Prep Tool, follow [this comprehensive guide](https://www.prajwaldesai.com/deploy-win32-apps-with-intune/) from Prajwal Desai.

## Deployment
### Application Configurations
Okta Verify has multiple parameteres that can be set during the installation of the application. [You can find a full list of configurations here.](https://help.okta.com/oie/en-us/content/topics/identity-engine/devices/managed-app-configs-win.htm)

You will require at least the **OrgUrl** to install the application. Examples of installation commands will be detailed in the installation step.

### Uplaod the Application
To deploy the Okta Verify application to Microsoft Intune, follow these steps:
1. Log into your Microsoft Intune admin portal
2. Navigate to the Apps section within Microsoft Intune
3. Click Add and then select Windows app (Win32) as the App type
4. Click Select and then find your packaged .intunewin file and upload it
5. Fill out the relevant application information such as the name and details about the application

### Installation
Okta Verify has an option for users to use Windows Hello or Okta Verify Passcodes as their authentication method for Okta FastPass. There are two different methods of installing the application depending on which option you wish to use.

#### With Windows Hello
To use Windows Hello as the authentication method, use the following installation command. By default Okta Verify will use Windows Hello as it's authentication method, so not many parameteres need to be specified.

An example the most basic installation command:

*`.\OktaVerify.exe /q OrgUrl=[Your Okta Organization URL]`*

And then an example where multiple parameteres are set such as automatic update timings and enabling the zero trust assessement plugin.

*`.\OktaVerify.exe /q OrgUrl=[Your Okta Organization URL] AutoUpdateDeferredByDays=1 AutoUpdatePollingInSecond=3600 EnableZTAPlugin=TRUE`*

#### With Okta Verify Passcode
To use Okta Verify Passcode as the authentication method then multiple parameters must be in place, such as specifying the AuthenticatorOperationMode and UserVerificationType.

An example of an installation command that deploys Okta Verify with the Okta Verify Passcode option:

*`.\OktaVerify.exe /q OrgUrl=[Your Okta Organization URL] AuthenticatorOperationMode=VirtualDesktopStatic UserVerificationType=OktaVerifyPasscode`*