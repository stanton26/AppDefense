---
layout: single
title: "VMware Cloud on AWS API Lab Manual"
date: 2018-07-18
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introduction

In this lab exercise we will be showing how you can intereact with the VMware Cloud on AWS platform through programmatic means. We will go through how we can use PowerShell as a means to interact with the Cloud Solution Platform as well as the vCenter instance. We will then delve into how we can interact with the VMware Cloud on AWS REST API and perform actions in both the interegrated "Developer Center" view in the console, and also through popular third party and open source REST clients. For the purposes of our lab exercise we will be making use of "Postman" as our REST Client.

## Using PowerShell

### Open the PowerShell CLI

1. Click on **Start**, and scroll down until you see the Windows PowerShell menu
2. Right click on the **PowerShell CLI** shortcut icon and select **Run as Administrator**

Install the VMware PowerCLI module if not loaded

``` powershell
install-Module VMware.PowerCLI
```

We now need to set the execution policy to Remote Signed in the PowerShell session.

``` powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -force
```

You now will need to set the PowerCLI Configuration to Ignore Invalid Certificates. Please type the following command to action this.

``` powershell
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false
```

NOTE: Be sure the "i" in "Ignore" is capitalized if you are not using copy/paste to input this command

We now need to install the VMware CLI commands

``` powershell
Install-Module -name VMware.VMC -scope AllUsers
```

Let's take a quick look at the VMware CLI commands.

``` powershell
Get-VMCCommand
```

![get-vmccommand](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+13-22-31.png)

We now need to get your Refresh Token from the VMC console.

Switch back to or open the web browser and log into **vmc.vmware.com**

If you are not already logged in

1. Open a new tab
2. Click on the **VMware Cloud on AWS** bookmark shortcut on the Bookmarks bar
3. Fill in your email address
4. click on **Next**
5. Click on the drop down next to your Name/Org ID
6. Click on **OAuth Refresh Token**

Now we can create a refresh token for your ID tied to this Org

1. Click on **Create a new token**
2. Click on **Copy to Clipboard**

Now let's attach to the VMC server

``` powershell
connect-vmc -refreshtoken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

NOTE: Within the PowerShell window you can just right-click to paste the code,quotes are optional

Now we can see what Orgs we have access to

``` powershell
Get-VMCorg
```

Note the Org Display_Name and ID

Now that we know the Org Display_Name we can find out information about the SDDC's inside our org.

```powershell
Get-VMCSDDC -Org VMC-WS#
```

NOTE:replace # with your Student Number

Another thing you can do is see the Default Credentials for your SDDC

```powershell
Get-VMCSDDCDefaultCredential -org VMC-WS#
```

NOTE: Replace # with your Student Number

## REST APIs through Developer Center

In this module we will be using the VMware Cloud on AWS REST API to get some basic information about your VMware Cloud on AWS Organization and SDDC deployment. To do this we will be using the new Developer Center feature in VMware Cloud on AWS. This was built specifically to focus on using APIs and scripts to create SDDCs, add and remove hosts, plus connect to and use the full vCenter API set. To get started, let go back to your VMC environment.

Launch the Chrome browser on your Student View Desktop

If you are not already logged in, log into your VMC org.

1. From within the VMware Cloud on AWS tab, click on the Developer Center menu

In the Developer Center there are a lot of great resources for you to explore. For example, let's check out a code sample that was uploaded by one of our API developers. If you scroll through this screen you will see there are code samples for Postman (a REST API Development Tool), Python, PowerCLI, and many others. Anyone can contribute code samples to the community, if that interests you go to <http://code.vmware.com> or click on the link **VMware{code} Sample Exchange**.

1. Click on Code Samples in the menu
2. Click on Download in the PowerCLI - VMC Example Scripts box
3. After the script downloads. Click on the dropdown arrow
4. Click on **show in folder**
5. Right click on the downloaded script
6. Click on **Edit**

This will open the PowerShell ISE environment. Now you can see the PowerShell commands you used in the previous module as well as other commands you can use with your SDDC.

Close the PowerShell ISE Window

Let's now run some simple REST API commands built into Developer Center, go back to your chrome browser

1. Click on the **API Explorer** menu
2. Click on the drop down arrow next to "Organization"
3. Click on the drop down arrow next to the first "GET" API section
4. Click on **Execute**

What did we not do?? We did not put in any authentication to pull this data. The reason is we are using the session authentication to execute these commands. To run these commands in other application, like PowerShell or Postman, you will need to get your resource and session tokens before you can run these commands. THe Developer Center does all this for you

Let's look through the response.

1. Here you see the Organization's alphanumeric name. Which you can also find in #3
2. The organization ID. NOTE: Copy the ID number, without the quotes, for possible use in the next step.
3. The organization Display_Name
4. The organization Version

In this step, we will GET some information about our organization

1. Click on the drop down arrow by SDDCs
2. Click on **GET**
3. The Org ID should already be filled in for you.
4. Click on **Execute**

Now let's look at the response body

1. The creation date of the SDDC
2. the SDDC ID
3. the SDDC state

## Postman

In this module, we will be exploring how to use Postman to execute REST API requests and build automation through collections. Postman is an API development tool. As an example, you can create variables for use within the APIs, test the response, and use webhooks to integrate with collaboration platforms.

Postman is very easy to install, so let's get started.

1. Open a new browser tab and go to <https://www.getpostman.com>
2. Click on **Download the App**
3. Find Postman for Windows (64-bit)
4. Click **Download**
5. Double-click on the downloaded file, the install will execute without interaction.

NOTE: For cleanup you can close all postman tabs in Chrome

1. Click on the text: Take me straight to the app. I'll create an account another time.
2. Close this Window
3. Uncheck Show this window on launch
4. Close this window

Go back to your browser window, if you do not have a tab opened for VMware Cloud on AWS, follow the below instructions

1. Open a new tab
2. Navigate to <https://vmc.vmware.com>
3. Log in with you email address which you used to register for the VMware Cloud on AWS Experience Day

Our internal API development team has done a great job pre-creating SDKs for many of the popular languages in use today. For this module, we will be using the SDK for REST to show you how you can easily import and reuse some pre-built collections to create your own.

1. Click on **Developer Center**
2. Click on **SDK**
3. Click on **Source on Github for REST**

You will be redirected to the vsphere-automation -sdk github page specifically for vSphere's REST API. There is a lot of great information here about how to use the vSphere REST API and VMware Cloud on AWS. We will be pulling down this github repository so we can import the VMC collection.

1. Click on **Clone or download**
2. Click on **Download ZIP**

NOTE: This is only 1 way to download a repository. If you want to actively use or even contribute to the development of the code in this repository, you can install git tools in Windows or in Linux or Mac and clone the repository in order to push commits to the repository.

1. Click on the **Download menu**
2. Click on **Open**
3. Click on **Extract**
4. Click on **Extract all**

We will keep the default file path.

1. Uncheck the box
2. Click on Extract

Now that we have Postman installed and the github repository on our local system, lets import the VMC collection and use some the requests to build our own collection.

1. Click on **Import**
2. Click on **Choose Files**

To import the VMC collection json file we downloaded earlier.

1. Browse to the directory we saved it to earlier. That directory should be "C:\downloads\vsphere-automation-sdk-rest-master\vsphere-automation-sdk-rest-master\samples\postman"
2. Click **VMware Cloud on AWS APIs.postman_collection.json**
3. Click **Open**

We now need to get our refresh token for our Org in VMC. Go back to your VMware Cloud on AWS tab in your browser

1. Click on the drop down next to your Student Name/Org ID
2. Click on **OAuth Refresh Token**
3. Click on **Copy to Clipboard**

NOTE: If you have not generated a token yet, click on **Generate** and then copy to clipboard.

Return to the Postman app. We now need to setup a Postman environment for use with VMC. An environment is where we will be creating and storing our variables. These variables can be local or global, depending on your use within Postman. In this module, we will only be using local variables.

1. Click on **New**
2. Click on **Environment**
3. Name the Environment "VMC"
4. In the Key column type in "refresh_token"
5. In the Value column use CTRL-V to paste your actual refresh token you copied in a previous step.
6. Click on **Add**
7. Close the Window

Now set this as our default environment.

NOTE: If you don't set the default environment to VMC, then the variables that get created will not be accessible.

1. Click on the drop down arrow
2. Select VMC

Now we will start to build our own collection by using some request that came in the SDK we imported earlier.

1. Click on **Collections**
2. Click on **Authentication and Login**
3. See how this request is our refresh token variable we defined in an earlier step.
    NOTE: If the environment is not set to VMC, this will request will fail because the refresh_token variable is not defined.
4. Click on **Send**
5. You will now see the access token that was generated with the refresh token. This is the body or payload of the response to our request.
6. Click on the Eye icon

You will see that we have stored your access token into a variable so we can use it for future calls. How did we do that? We ran a "test" on the response body. You will see how in the next step.

1. Click on Tests

The access_token variable was set by running some java script code against the response. We are also using the Postman setEnvironmentVariable function to create it.

Lets save this request to our own collection so we can use it later.

1. Click on the drop down arrow
2. Click on **Save As**
3. Change the Request name to "Authorize"
4. Change the Request description to "Get Access Token"
5. Click on **Create Collection**
6. Type Workshop and click the **check box**
7. Select the Workshop folder
8. Click on **Save to Workshop**

A new window will pop open indicating that you created a new collection. We will not do anything here at this time.

1. Close this window

Lets request some details from our Org so we can send them to Slack.

1. Click on **Orgs** and **List Orgs**
2. Click on **Headers**
3. Click **Send**
4. You see here how we are using the access_token variable for the csp-auth-token. This will authorize our request. NOTE: This access token is only good for 30 minutes. If you run this request and get a response of 400 unauthorized, go back and run the authorize request.
5. Look through the response body for your Org's display_name

Lets save this request to our own collection so we can use it later.

1. Click on the drop down arrow
2. Click on **Save As**
3. Change the Request name to Org list
4. Change the Request description to "Get a list of your Orgs"
5. Be sure Workshop is selected under Select a collection or folder to save to:
6. Click on **Save to Workshop**

We need to replace the Test code that came with the SDK so we can create variable we want to use when send our message to Slack.

1. Click on **Tests**
2. Copy and paste the below code into the Tests section. NOTE: You may have to press CTRL-V to past into the text box.
3. Click **Send**

``` javascript
var jsonData = JSON.parse(responseBody);
if (responseCode.code === 200) {
for (i = 0; i < jsonData.length; i++) {
  pm.environment.set("name", jsonData[i].display_name);
  pm.environment.set("ID", jsonData[i].id);
  pm.environment.set("version", jsonData[i].version);
  pm.environment.set("state", jsonData[i].project_state);
  }
}
```

We can verify if the variables have been created and assigned values.

1. Click on the **eye** icon
2. Scroll down to see if the new variables were created.
3. Once verified click on the **eye** icon again to close the window

Lets save the changes we made to this request.

1. Click on **Save**

Now that we have details of our Org lets send them to Slack in a message.

To post to slack, a link needs to be generated for the slack channel that we want to post to. This has already been done for you and is listed below. One of the instructors will have this slack channel displayed on the screens. So you can see the results. Slack channel URL: <https://hooks.slack.com/services/T9HQFCTC1/B9JBL5SV7ArgKjF4zZDh7dnaWRyKNJfRY>

Now we need to setup the request:

1. Click on the "+" sign for a new request
2. Change the request type to POST
3. Cut and paste the above slack channel URL to the address box
4. Select Body
5. Change the format type to raw
6. Type the below code, or cut and paste it into the Body section. NOTE: You may have to press CTRL-V

``` json
{
"text" : "Your Org ID is: {{ID}}\nYour Org version is: {{version}}\nAnd your Org
state is: {{state}}",
"username" : "{{name}}"
}
```

Lets save this request to our own collection so we can use it later.

1. Click on the drop down arrow
2. Click on **Save As**
3. Change the Request name to "Post to Slack"
4. Change the Request description to "Post some Org details to slack"
5. Be sure Workshop is selected under Select a collection or folder to save to:
6. Click on **Save to Workshop**

Check and see if your request posted the Name, ID, Version, and Status of your Org.

The last thing to show you with Postman is the way that you can run a collection to automate a series of tasks. What we have been doing in this module is building a collection. As you see in the screen shot there are 3 tasks in the Workshop collection.

1. Click on the **Arrow** in the Workshop window
2. Click on **Run**
3. Click on **Run Workshop**
4. Be sure the Environment is set to **VMC**

If all your work was saved and ran individually, they should run here as well.

1. Check out the status of each request.

If you have "200 OK" then you will see another post in slack for your workshop Org.