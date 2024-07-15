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

# How to Deploy MakeMeAdmin with Microsoft Intune
MakeMeAdmin is deployed via a .msi file type which makes the deployment very quick and easy.

You can either deploy MakeMeAdmin via a Line-of-Business (LoB) app or a package the application into an .intunewin file (Win32). Packaging the application as a LoB is much faster and easier, but doesn't provide the ability to do requirements and also doesn't allow you to deploy the application via the Company Portal via device groups.

## Deploy as a Line-of-Business App

## Deploy as a Win32 App
 
# Deploy Policies for MakeMeAdmin with Microsoft Intune
