---
title: "Setup and Deploy MakeMeAdmin via Microsoft Intune"
date: 2024-07-12 13:30:00 +0000
categories: [Microsoft Intune, Application, MakeMeAdmin, Admin]
tags: [intune, microosft intune, admin]
---

# What is MakeMeAdmin?

MakeMeAdmin is an open-source application for Windows which grants standard user accounts elevated administrator access on a tempory-access basis.

Where-as I'd never recommend providing local administrator rights to any user, there is always a case of when it may be seen as a useful thing to have. If this is the case, it's good to have a secure setup which ensure end-users only have administrator rights for a limit amount of time.

You can find more information and the download for [MakeMeAdmin on pseymour's (Patrick S. Seymour) GitHub.](https://github.com/pseymour/MakeMeAdmin)

## How to Deploy MakeMeAdmin with Microsoft Intune

MakeMeAdmin is deployed via a .msi file type which makes the deployment very quick and easy.

You can either deploy MakeMeAdmin via a Line-of-Business (LoB) app or a package the application into an .intunewin file (Win32). Packaging the application as a LoB is much faster and easier, but doesn't provide the ability to do requirements and also doesn't allow you to deploy the application via the Company Portal via device groups.

### Deploy as a Line-of-Business App

---

To deploy MakeMeAdmin as a LoB application, follow these steps:

1. Log into your Microsoft Intune tenant and navigate to Apps
2. Click Add followed by Line-of-business app from the dropdown list
3. Click Select and then upload the MakeMeAdmin msi file
4. Click Next and name your application along with giving it a relevant description and any other information you wish to fill out
5. Click Next and then select the groups you wish to have MakeMeAdmin required by or available to.

### Deploy as a Win32 App

---

Before you can deploy a Win32 application you need to package the application.

To pacakge the application, you will require the Microsoft Win32 Content Prep tool. This application will be used to bundle the executable file into a .intunewin file format which will be used to upload the application to Microsoft Intune.

You can download the Microsoft Win32 Content Prep Tool from [GitHub.](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) For a guide on how to run the Win32 Content Prep Tool, follow [this comprehensive guide](https://www.prajwaldesai.com/deploy-win32-apps-with-intune/) from Prajwal Desai.

#### To deploy MakeMeAdmin as a Win32 application, follow these steps:

1. Log into your Microsoft Intune tenant and navigate to Apps
2. Click Add followed by Windows App (Win32) from the dropdown list
3. Find and upload the MakeMeAdmin intunewin file, in this case you should only require to package the msi file into an intunewin file
4. Fill out the relevant application information such as the name and details about the application
5. Click Next and the Install command and Uninstall command should be auto-filled, but if they're not, refer to the following:
   > - Install command: `msiexec /i "Make Me Admin_2.3.81_Machine_X64_wix_en-US.msi" /qn`
   > - Uninstall command: `msiexec /i "Make Me Admin_2.3.81_Machine_X64_wix_en-US.msi" /qn`
6. Set the install behavior as System
7. Set the Device restart behavior to No specific action
8. Click Next and choose your requirements (if any)
9. Click Next and fill out your detection rules. If you select MSI as the option then the product code detection will be autofilled.
10. Click Review + Save and upload your MakeMeAdmin application.

### Deploy Policies for MakeMeAdmin with Microsoft Intune

---

Setting up the policies for MakeMeAdmin is the most tedious part of this setup. pseymour's GitHub provides a set of Group Policy files which in theory can be uploaded to Microsoft Intune's Import ADMX feature, but uploading the ADMX directly creates an issue with the Windows.admx not being present.

Usually this issue is resolvable by uploading the Windows.admx into Microsoft Intune, but I still ran into issues with the MakeMeAdmin ADMX even while having the Windows.admx uploaded to Microsoft Intune. To resolve this, I instead built the MakeMeAdmin ADMX setup by using a custom OMA-URI configuration profile.

#### To create this custom OMA-URI configuration profile, follow these steps:

1. Log into your Microsoft Intune tenant and navigate to Devices followed by Configuration
2. Click Create and then New Policy
3. Select the Platform as Windows 10 or later followed by Template as the Profile type
4. Select Custom from the list of Templates and then click Create
5. Name your configuration profile and click Next
6. Click Add and then provide provide the following information in the Add Row pane
   > - Name: `SinclairBase ADMX`
   > - Description: `Imports the SinclairBase ADMX`
   > - OMA-URI: `./Device/Vendor/MSFT/Policy/ConfigOperations/ADMXInstall/MakeMeAdmin/Policy/SinclairBase`
   > - Data type: `String`
   > - Value: [Refer to this .admx file for the value of the OMA-URI](https://github.com/FreshPrinceofBellR/freshprinceofbellr.github.io/blob/f8063f6a0af13d7993552f29c88f1253e84bb074/assets/files/>FreshPrinceofBellR_Sinclair-Base.admx)
7. Click Add and then provide provide the following information in the Add Row pane
   > - Name: `MakeMeAdmin ADMX`
   > - Description: `Imports the MakeMeAdmin ADMX`
   > - OMA-URI: `./Device/Vendor/MSFT/Policy/ConfigOperations/ADMXInstall/MakeMeAdmin/Policy/SinclairMakeMeAdminADMX`
   > - Data type: `String`
   > - Value: [Refer to this .admx file for the value of the OMA-URI](https://github.com/FreshPrinceofBellR/freshprinceofbellr.github.io/blob/f8063f6a0af13d7993552f29c88f1253e84bb074/assets/files/>FreshPrinceofBellR_Sinclair-MakeMeAdmin.admx)
8. Click Next and choose your assignments and then save the configuration profile

The above setup ingests the ADMX configurations required to use the rest of the policies avaliable for the MakeMeAdmin application. You can find all of the avaliable settings by reviewing the ADMX file, but the current avaliable settings are as follows:

- AllowedEntities
- RemoteAllowedEntitites
- AutomaticallyAddAllowedEntitites
- DeniedEntitites
- RemoteDeniedEntitites
- AutomaticallyAddDeniedEntitites
- AdminRightsTimeout
- TimoutOverrideList
- RemoveOnLogout
- OverrideOutsideProcess
- AllowRemoteRequests
- EndRemoteSessionsOnExpiration
- SyslogSevers

#### To add any of the above policies, follow these steps:

1. Navigate to the previously created Custom OMA-URI configuration profile
2. Add a new row into the configuration profile
3. Add the following information into the new row (For example, AdminRightsTimeout):
   > - Name: `AdminRightsTimeout`
   > - Description: `Sets the timeout for administrator access`
   > - OMA-URI: `./Device/Vendor/MSFT/Policy/Config/MakeMeAdmin~Policy~MakeMeAdmin/AdminRightsTimeout`
   > - Data type: `String`
   > - Value: `<enabled/><data id="AdminRightsTimeoutValue" value="20"/>`
4. Click Save and then Review + Save to save the changes to the configuration profile
