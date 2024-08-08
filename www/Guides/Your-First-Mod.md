---
title: Your First Mod
parent: Guides
grand_parent: Home
---

<h3>This page up to date for:</h3>

{: .warning}
> The template featured on this page is not yet complete. None of the features are expected to work at this time. Feel free to acquaint yourself with the process but do not follow along with commands at this time!

![Static Badge](https://img.shields.io/badge/Version-none-darkred?style=for-the-badge)

# Your First Mod

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

- [Your First Mod](#your-first-mod)
  - [Table of Contents](#table-of-contents)
  - [Initialize Your Plugin Repository](#initialize-your-plugin-repository)
    - [Install the Template](#install-the-template)
    - [Create the Remote Workspace](#create-the-remote-workspace)
    - [Create the Local Workspace](#create-the-local-workspace)
      - [Template Options](#template-options)
    - [Building and Running Your First Mod](#building-and-running-your-first-mod)
    - [Automated Publishing with a Github Action](#automated-publishing-with-a-github-action)
      - [Setting Up Your Publishing Key](#setting-up-your-publishing-key)
      - [Running the Publish Action](#running-the-publish-action)
    - [Troubleshooting](#troubleshooting)
      - [Type or Namespace BepinEx Could Not Be Found](#type-or-namespace-bepinex-could-not-be-found)
      - [If The Template Failed to Push Origin](#if-the-template-failed-to-push-origin)

## Initialize Your Plugin Repository

### Install the Template

{: .important}
> Always keep in mind that dotnet templates are arbitrary code that can execute on your machine. Make sure you trust the source of any template you install!

Before installing any template, take a moment to review all the files and be certain you trust the source. Our template's source code can be found [here](https://github.com/LethalCompanyModding/LCM-Template-TSPublishing).

When you're ready to install the template run the following command in your terminal `dotnet new install [NUGET PACKAGE ID]`

### Create the Remote Workspace

Create a [new repository](https://github.com/new) that will be the home of your plugin. Give it a name and description but otherwise do not use any other settings on this page. Do not create a readme and do not select a license, we will do this later.

Once you have created the new repository by clicking the [Create Repository] button and waited for github to finish working, take note of the URL in your address bar, this will be needed for creating the template in the next step.

### Create the Local Workspace

Create a new folder with your project name wherever you'd like it to be on your computer and then open your terminal inside that folder. Initialize the project using the template by using the following command after substituting your own information in for the bracketed terms. Don't worry if its a little confusing, I will describe each item and give an example below.

#### Template Options

{: .important}
> ALL of these options are mandatory

<dl>
  <dt>ProjectGUID</dt>
  <dd>
  
  This is your project's fully qualified name. It will only be used by BepinEx for the purpose of uniquely identifying your plugin in the chainloader. If you do not know how to create a unique reverse FQDN for your project then follow this format: com.github.your-user-name.repo-name. Make sure it is <strong>lower case</strong>

  </dd>

  <dt>ProjectAUTHOR</dt>
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
dotnet new LCM_TS_Publishing \
 --ProjectGUID com.github.robyn.mycoolmod \
 --ProjectAUTHOR RobynLlama \
 --ProjectDESC "The coolest mod ever made" \
 --ProjectURL https://github.com/LethalCompanyModding/LCM-Template-TSPublishing
```

If the template fails to push, see the [Troubleshooting](#if-the-template-failed-to-push-origin) section

### Building and Running Your First Mod

Now that the template has been successfully installed and your project has been created, you simply use your IDE's build task or run `dotnet build` in your project's folder. This will create an artifact at `./bin/Debug/YourProjectName.dll` which you may copy to your BepinEx plugins folder.

### Automated Publishing with a Github Action

The template comes pre-loaded with a github action that will allow you to automatically publish a new release to the Thunderstore in response to a version tag.

#### Setting Up Your Publishing Key

{: .note}
> If you already have a service account key or know how to set one up, feel free to skip this step

- Log in to your Thunderstore account that owns the team that will be publishing this mod
- Navigate to the [Thunderstore Teams](https://thunderstore.io/settings/teams/) page and click on the name of the team that will be publishing this mod
- On the left hand side navigation menu choose `Service Accounts` and then `Add Service Account`
- Copy the key somewhere safe for now

{: .warning}
> A service account key should be treated like a password, it can be used to publish to your team and cannot be viewed again once you leave the page. Github also encrypts all secrets so it is not possible to recover it once entered.

- Navigate to the `settings` menu for your repository on github and on the left hand navigation pane select `Secrets and Variables` then `Actions`. In this menu choose `New Repository Secret` and name it **EXACTLY** `TCLI_AUTH_TOKEN` then paste in your service account key as the secret and save it.

#### Running the Publish Action

Once ready, publishing your project is extremely simple.

- First, You need to edit your project's csproj file's `version` field to be a higher number than it was before (For example: If I previously released my project at version 1.0.0 then I would consider 1.0.1 as a valid higher version).
- Second, Create a new tag with the same name as the version you just set. Your IDE may have a specific way to create and push tags (For example: In VSCode press F1 and then type `create tag` and follow the prompts then F1 and `push tags` and choose `origin`). If your IDE does not integrate with git or you just want to do it the old fashioned way then this example will create an annotated 1.0.1 tag with the comment "Release version 1.0.1 and then push it:

```bash
git tag -a 1.0.1 -m "Release version 1.0.1"
git push origin --tags
```

Pushing a new tag like this will trigger the workflow to compile and upload your mod to the Thunderstore.

### Troubleshooting

#### Type or Namespace BepinEx Could Not Be Found

In some cases your IDE may not register the types from Nuget packages immediately. Try running `dotnet restore` in your project's folder and restarting your IDE.

#### If The Template Failed to Push Origin

The template should have run a post-install commit for you in order to properly setup your repository, but in the event that it failed or you were not logged in to your github when activating the template you can run the following code to push your remotes to your repository

{: .note}
> If you have a token in VSCode or VS then use the following command while in the project terminal inside your IDE, otherwise you may need to setup an [SSH key](https://github.com/settings/keys) for pushing to github from the terminal

```bash
git push origin --mirror
```
