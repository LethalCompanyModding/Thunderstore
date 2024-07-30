---
title: Your First Mod
parent: Guides
grand_parent: Home
---

<h3>This page up to date for:</h3>

![Static Badge](https://img.shields.io/badge/Version-56-blue?style=for-the-badge)

# Overview

![Static Badge](https://img.shields.io/badge/Skill_Level-Beginner-blue?style=for-the-badge)  
![Static Badge](https://img.shields.io/badge/Estimated_Time-20_Minutes-blue?style=for-the-badge)

You will learn:
- How to use the LCMTemplate with Publishing to setup a new plugin project
- How to configure your Thunderstore publishing workflow to automatically publish when your plugin updates
- How to deploy your plugin to the game folder for debugging

Prerequisites:
- [Visual Studio](https://visualstudio.microsoft.com/) / [VS Code](https://code.visualstudio.com/) / C# Compatible IDE
- [Dotnet 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) installed
- [Git for Windows](https://gitforwindows.org/) / Git installed
- A [Github](https://www.github.com) account
- A [Thunderstore Account](https://thunderstore.io) (for publishing)
  - A Thunderstore [Team](https://thunderstore.io/settings/teams/) to publish to.
  - A Thunderstore Service Account to automatically publish to your team

## Table of Contents

- [Overview](#overview)
  - [Table of Contents](#table-of-contents)
- [Initialize Your Plugin Repository](#initialize-your-plugin-repository)
  - [Install the Template](#install-the-template)
  - [Create the Remote Workspace](#create-the-remote-workspace)
  - [Create the Local Workspace](#create-the-local-workspace)
    - [Template Options](#template-options)

# Initialize Your Plugin Repository

## Install the Template

{: .important}
> Always keep in mind that dotnet templates are arbitrary code that can execute on your machine. Make sure you trust the source of any template you install!

Before installing any template, take a moment to review all the files and be certain you trust the source. Our template's source code can be found [here](https://github.com/LethalCompanyModding/LCM-Template-TSPublishing).

When you're ready to install the template run the following command in your terminal `dotnet new install [NUGET PACKAGE ID]`

## Create the Remote Workspace

Create a [new repository](https://github.com/new) that will be the home of your plugin. Give it a name and description but otherwise do not use any other settings on this page. Do not create a readme and do not select a license, we will do this later.

Once you have created the new repository by clicking the [Create Repository] button and waited for github to finish working, take note of the URL in your address bar, this will be needed for creating the template in the next step.

## Create the Local Workspace

Create a new folder with your project name wherever you'd like it to be on your computer and then open your terminal inside that folder. Initialize the project using the template by using the following command after substituting your own information in for the bracketed terms. Don't worry if its a little confusing, I will describe each item and give an example below.

### Template Options

{: .important}
> ALL of these options are mandatory

<dl>
  <dt>ProjectGUID</dt>
  <dd>
  
  This is your project's fully qualified name. It will only be used by BepinEx for the purpose of uniquely identifying your plugin in the chainloader. If you do not know how to create a unique reverse FQDN for your project then follow this format: com.github.your-user-name.repo-name. Make sure it is <strong>lower case</strong>

  </dd>

  <dt>ProjectAuthor</dt>
  <dd>
  
  This is the exact name of your Thunderstore Team as it appears on the Thunderstore. <del>Double</del> Triple check that this is correct.
  
  </dd>

  <dt>ProjectDESC</dt>
  <dd>
  
  This is a brief description of what your mod does. It should be fewer than 250 characters or publishing will fail.

  </dd>
  <dt>ProjectURL</dt>
  <dd>
  
  This is a link to your newly created Github repository from the previous step (Remember when I told you to save that address?)

  </dd>
</dl>


Example Command:
```bash
dotnet new LCM_TS_Publishing --ProjectGUID com.github.robyn.mycoolmod --ProjectAuthor RobynLlama --ProjectDESC "The coolest mod ever made" --ProjectURL https://github.com/LethalCompanyModding/LCM-Template-TSPublishing
```
