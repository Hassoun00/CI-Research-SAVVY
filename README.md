# CI-Research-SAVVY

# Jenkins Unity Build Pipeline


* Mr.Faisal Bin Shulayyil
* Mr.Mohammad Hassoun


## Problem

The process to build and distribute an app takes too much manual labor. Let’s automate it!

## Solution

Automatically create new builds for a Unity app whenever code changes on GitHub. The final compiled builds for Android and Windows Phone will be sent to and distribute by HockeyApp.

## Pipeline steps

![Build Pipeline Drawing](pipeline.jpg "Jenkins Unity Build Pipeline")

### Step 1: GitHub

Commit code to GitHub Repo:https://github.com/Hassoun00/SAVVY-GAME-

### Step 2: Jenkins on  IOS , Android and Windows.

Our Jenkins instance pulls the code changes from GitHub

### Step 3: Unity on Jenkins

Jenkins then triggers Unity to use out build scripts to make an Android and Windows Phone build of our project.

### Step 4A: Upload Android Build to HockeyApp

The Android apk built in step 3 is uploaded to HockeyApp

### Step 4B: Build and Upload Windows Phone app to HockeyApp

#### Substep 1: Upload WP build to VSTS

We trigger the following script to take the output of the WSA Unity build and upload it to a different git repo hosted on VSTS.

```
cd ../
mkdir tmp
cd tmp
git clone <VSTS URL> repo
cd repo
git rm -r .
cp -R ../../<Name of folder on server>/Builds/WSA/* ./
git add . -f
git commit -m "New version!"
git push
```

#### Substep 2: VSTS Build

VSTS will then build the new Visual Studio project updated by the previous git commit

#### Substep 3: Upload to HockeyApp

The appxupload file created in substep 2 is then uploaded to HockeyApp

### Step 5: Distribute to users

HockeyApp then distributes the appropiate app file to our users/testers. Basically, each time a build is successful, an updated version of the app appears for them to download in hockey app. 

## Recreation process

* Windows Agent/Virtual machine with the following installed:
    * Jenkins
    * Unity
    * Android SDK
    * JDK
    * Windows Phone SDK
    * Visual Studio
    * Visual Studio Team Services Agent
* Jenkins must have the following plugins:
    * HockeyApp https://wiki.jenkins-ci.org/display/JENKINS/HockeyApp+Plugin
    * Unity3dBuilder https://wiki.jenkins-ci.org/display/JENKINS/Unity3dBuilder+Plugin
    * Windows Azure Storage https://wiki.jenkins-ci.org/display/JENKINS/Windows+Azure+Storage+Plugin
    * IOS 
    https://plugins.jenkins.io/mac/
    
    * Visual Studio Team Services https://github.com/Hassoun00/CI-Research-SAVVY/edit/main/README.md
    * GitHub
* GitHub
    * DumbGame - Contains main unity project
    * JenkinsUnityPipeline - Contains templates for Azure VMs for Jenkins/VSTS Agent
* Visual Studio Team Services
    * Contains a project for the game
    * The project contains a git repo for the Windows, IOS , Andriod Phone Visual Studio project
    * Build process to build the Visual Studio Solution
    * Pushes VS builds onto HockeyApp
* Hockey App
    * Hosts projects for each app type (Android & Windows)
    * Invite users to the projects and provide them downloads 
