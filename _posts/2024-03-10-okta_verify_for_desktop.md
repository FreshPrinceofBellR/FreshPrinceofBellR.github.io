---
title: "Deploy Okta Verify via Microsoft Intune"
date: 2024-05-22 13:30:00 +0000
categories: [Microsoft Intune, Okta, Application]
tags: [intune, microosft intune, okta]
author: Richard Bell
---

# How to Deploy Okta Verify with Microsoft Intune
Okta Verify for Desktop can be deployed via Microsoft Intune with a series of specifications to meet your organisations needs, below will go through the process of how to package the Okta Verify application for Windows along with detailing the avaliable settings to deploy the application with.

## Packaging the Application
To pacakge the application, you will require the Microsoft Win32 Content Prep tool. This application will be used to bundle the executable file into a .intunewin file format which will be used to upload the application to Microsoft Intune.

You can download the Microsoft Win32 Content Prep Tool from [GitHub.](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool).

For a guide on how to run the Win32 Content Prep Tool, follow [this comprehensive guide](https://www.prajwaldesai.com/deploy-win32-apps-with-intune/) from Prajwal Desai.

## Deployment
To deploy the Okta Verify application to Microsoft Intune, follow these steps:
1. Log into your Microsoft Intune admin portal
2. Navigate to the Apps section within Microsoft Intune
3. Click Add and then select Windows app (Win32) as the App type
4. Click Select and then find your packaged .intunewin file and upload it
5. Fill out the relevant application information such as the name and details about the application
![Example application information](image.png)
7. 


### More Information