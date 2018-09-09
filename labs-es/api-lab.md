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

En este ejercicio mostraremos cómo puede interactuar con VMware Cloud on AWS a través de medios programáticos. Veremos cómo podemos usar PowerShell como un medio para interactuar con la plataforma de soluciones de la nube, así como con la instancia de vCenter. Después, profundizaremos en cómo podemos interactuar con el API REST de VMware Cloud on AWS y realizar acciones tanto en la pestaña integrada de "Developer Center" en la consola, como a través de populares clientes REST de código abierto y de otras herramientas. Continuaremos nuestro ejercicio haciendo uso de "Postman" como nuestro cliente REST.

## Usando PowerShell

### Abra la ventana de PowerShell CLI

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs1.jpg)

1\. Haga click en **Start**, y baje hasta que vea el menú **Windows PowerShell**

2\. Haga click derecho en el acceso directo de PowerShell CLI y seleccione **Run as
Administrator**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs2.jpg)

3\. Haga click en **Yes**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs3.jpg)

Instale el módulo VMware PowerCLI si no está cargado Escriba **install-Module VMware.PowerCLI** y pulse intro

*Nota:* Es posible usar tabulación para completar el comando. ej...escriba install-mod y luego tabulación. Podría haber un pequeño retraso la primera vez pero el comando **install-module** se completará.

*Nota:* Se preguntará si desea instalar el NuGet provider, elija la opción por defecto o escriba Y y luego intro, luego se preguntará si desea confiar en un repositorio no confiado, **NO ELIJA** la opción por defecto y escriba **Y** y luego intro.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs4.jpg)

Ahora es necesario cambiar la politica de ejecución a Remote Signed.

Escriba **Set-ExecutionPolicy -ExecutionPolicy RemoteSigned** y presione intro

Para autocompletar usando tabulación, escriba **Set-Ex{tab} -Exe{tab} Rem{tab}** y presione intro

*Nota:* Se preguntará si desea cambiar la politica de ejecución, escriba **Y** y presione intro

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs5.jpg)

Ahora será necesario la Configuración de PowerCLI a Ignore Invalid Certificates.

**PASO IMPORTANTE:**

Escriba **Set-PowerCLICon guration -InvalidCertificateAction Ignore** y pulse intro

*NOTA:* Asegúrese de que la "i" en "Ignore" sea mayúscula

*Nota:* Se le preguntará si desea actualizar la Configuración de PowerCLI, escriba **Y** y presione intro

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs6.jpg)

Ahora es necesario instalar los VMware CLI cmdlets

Escriba **Install-Module -name VMware.VMC -scope AllUsers** y presione intro

*Nota:* Se le preguntará si desea confiar en un repositorio no confiado, escriba **Y** y presione intro

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs7.jpg)

Hagamos una revisión rápida de los VMware CLI cmdlets.

Escriba **Get-VMCCommand** y presione intro

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs8.jpg)

Ahora necesitará obtener el Refresh Token desde la consola de VMC. Cambie al browser o abra uno e ingrese a **vmc.vmware.com**.

Si todavía no ha ingresado
4\. Abra una nueva pestaña

5\. Haga click en el acceso directo VMware Cloud on AWS

6\. Escriba su correo electrónico

7\. Haga click en **Next**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs9.jpg)

8\. Haga click en la caja desplegable junto a **Name/Org ID**

9\. Haga click en **OAuth Refresh Token**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs10.jpg)

Ahora cree un nuevo refresh token para su ID vinculada a esta Org

10\. Haga click en **Create a new token**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs11.jpg)

11\. Haga click en **Copy to Clipboard**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs12.jpg)

Ahora agréguela al servidor VMC

Escriba **connect-vmc -refreshtoken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"** y presione intro.

*NOTA:* Dentro de la ventana de PowerShell usted puede hacer click derecho y copiar el código, las comillas son opcionales

*NOTA:* Pegue el refresh token que copió en el ejercicio anterior.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs13.jpg)

Ahora podemos ver a cuales Orgs tenemos acceso

Escriba **Get-VMCorg** y presione intro.

12\. Note el Org Display_Name y ID

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs14.jpg)

Ahora que sabemos el Org Display_Name podemos buscar información sobre el SDDC dentro de nuestra org.

Escriba **Get-VMCSDDC -Org VMC-WS#** y presione intro.

*NOTA:* reemplace # con su número de estación de trabajo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs15.png)

Otra cosa interesante que se puede hacer es ver las Credenciales por Defecto de su SDDC

Escriba **Get-VMCSDDCDefaultCredential -org VMC-WS#** y presione intro.

*NOTA:* reemplace # con su número de estación de trabajo.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs16.png)


## REST APIs through Developer Center

In this module we will be using the VMware Cloud on AWS REST API to get some basic information about your VMware Cloud on AWS Organization and SDDC deployment. To do this we will be using the new Developer Center feature in VMware Cloud on AWS. This was built specifically to focus on using APIs and scripts to create SDDCs, add and remove hosts, plus connect to and use the full vCenter API set. To get started, let go back to your VMC environment.

Launch the Chrome browser on your Student View Desktop

If you are not already logged in, log into your VMware Cloud on AWS organisation.

1. From within the VMware Cloud on AWS tab, click on the Developer Center menu

![Dev Center](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+13-57-33.png)

In the Developer Center there are a lot of great resources for you to explore. For example, let's check out a code sample that was uploaded by one of our API developers. If you scroll through this screen you will see there are code samples for Postman (a REST API Development Tool)

![Postman Samples](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+14-00-33.png)

You will also find samples for Python, PowerCLI, and many others. Anyone can contribute code samples to the community, if that interests you go to <http://code.vmware.com> or click on the link **VMware{code} Sample Exchange**.

1. Click on Code Samples in the menu
2. Click on **Download** in the "PowerCLI - VMC Example Script" box
3. After the script downloads. Unzip
4. Click on **show in folder**
5. Right click on the downloaded script
6. Click on **Edit**

This will open the PowerShell ISE environment. Now you can see the PowerShell commands you used in the previous module as well as other commands you can use with your SDDC.

Close the PowerShell ISE Window

Let's now run some simple REST API commands built into Developer Center. Navigate back to your chrome browser and into your VMware Cloud on AWS page.

1. Click on the **API Explorer** heading
2. Click on the drop down arrow next to "Organizations"
    ![dev center organisations](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+14-08-46.png)
3. Click on the drop down arrow next to the first "GET" API section (/orgs)
4. Click on **Execute**

What did we not do?? We did not put in any authentication to pull this data. The reason is we are using the session authentication to execute these commands. To run these commands in other application, like PowerShell or Postman, you will need to get your resource and session tokens before you can run these commands. THe Developer Center does all this for you.

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

In this module, we will be exploring how to use Postman to execute REST API requests and build automation through collections. Postman is an API Explorer tool. As an example, you can create variables for use within the APIs, test the response, and use webhooks to integrate with collaboration platforms.

Postman is very easy to install, so let's get started.

1. Open a new browser tab and go to <https://www.getpostman.com>
2. Click on **Download the App**
3. Select Postman for Windows (64-bit)
4. Click **Download**
5. Double-click on the downloaded file, the install will execute without interaction.

NOTE: For cleanup you can close all postman tabs in Chrome

1. Click on the text: "Skip Signing in and Take me straight to the app"
2. Close the Pop up Window which you are presented with, Uncheck Show this window on launch check box before closing the pop up.

Go back to your browser window, if you do not have a tab opened for VMware Cloud on AWS, follow the below instructions

1. Open a new tab
2. Navigate to <https://vmc.vmware.com>
3. Log in with you email address which you used to register for the VMware Cloud on AWS Experience Day

Our internal API development team has done a great job pre-creating SDKs for many of the popular languages in use today. For this module, we will be using the SDK for REST to show you how you can easily import and reuse some pre-built collections to create your own.

1. Click on **Developer Center**
2. Click on **SDK**
3. Find **vSphere Automation SDK For REST**
4. Click **View Source**

This will download a vsphere-automation-sdk-rest-master.zip file to your machine. Please extract the contents of this zip file.

Now that we have Postman installed and our REST samples on our local system, lets import the VMC collection and use some the requests to build our own collection.

1. In the Postman Application
2. Click on **Import**
3. Click on **Choose Files**

To import the VMC collection json file we downloaded earlier.

1. Browse to the directory we extracted the zip file to earlier. That directory should be "C:\downloads\vsphere-automation-sdk-rest-master\vsphere-automation-sdk-rest-master\samples\postman"
2. Click **VMware Cloud on AWS APIs.postman_collection.json**
    ![postman collection](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+15-26-05.png)
3. Click **Open**

We now need to get our refresh token for our Org in VMC. Go back to your VMware Cloud on AWS tab in your browser

1. If you do not have VMware Cloud on AWS, Open a new tab in your browser
2. Click on the **VMware Cloud on AWS** bookmark shortcut on the Bookmarks bar
3. Login with the email address which you used to sign up to the VMware Cloud on AWS Experience day
4. Click on the drop down next to your Name/Org ID
5. Click on **My Account** under "User Settings"
6. In the My Account page, click on **API Tokens**
7. You will see your token here which you created in the previous Powershell section. Please copy this token to your clipboard

Return to the Postman app. We now need to setup a Postman environment for use with VMC. An environment is where we will be creating and storing our variables. These variables can be local or global, depending on your use within Postman. In this module, we will only be using local variables.

1. Click on **New**
2. Click on **Environment**
3. Name the Environment "VMC"
4. In the Key column type in "refresh_token"
5. In the Value column paste your token you copied in a previous step.
6. Click on **Add**
7. Close the Window
8. Click on the drop down arrow oin the top right hand corner of the app labelled "No Environment"
9. Select the "VMC" Environment we just created

Now we will start to build our own collection by using some request that came in the SDK we imported earlier.

1. Click on **Collections** in the left hand pane
2. Expand the "VMware Cloud on AWS APIs" collection
3. Expand **Authentication**
4. Click on the Login "Post" entry
5. See how this request has our refresh token variable we defined in an earlier step.
    NOTE: If the environment is not set to VMC, this will request will fail because the refresh_token variable is not defined.
    ![refresh token](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+15-54-04.png)
6. Click on **Send**
7. You will now see the access token that was generated with the refresh token. This is the body or payload of the response to our request.
8. Click on the Eye icon
    ![eye icon](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+15-55-29.png)

You will see that we have stored your access token into a variable so we can use it for future calls. How did we do that? We ran a "test" on the response body. You will see how in the next step.

![access token](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+15-57-43.png)

1. Click on **Tests**

The access_token variable was set by running some java script code against the response. We are also using the Postman setEnvironment variable function to create it.

![Tests](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+16-10-43.png)

Lets save this request to our own collection so we can use it later.

1. Click on the drop down arrow next to "Save"
    ![save request](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+16-15-09.png)
2. Click on **Save As**
3. Change the Request name to "Authorize"
4. Change the Request description to "Get Access Token"
5. Click on **Create Collection**
6. Type Workshop and click the **check box**
7. Select the Workshop folder
8. Click on **Save to Workshop**

![save workshop](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+16-18-26.png)

A new window will pop open indicating that you created a new collection. We will not do anything here at this time.

1. Close this window

Lets request some details from our Org so we can send them to Slack.

1. In the VMware Cloud on AWS APIs collection, click on **Orgs** and **List Orgs**
2. Click on **Headers**
3. Click **Send**

You see here how we are using the access_token variable for the csp-auth-token. This will authorize our request.

NOTE: This access token is only good for 30 minutes. If you run this request and get a response of 400 unauthorized, go back and run the authorize request.

1. Look through the response body for your Org's display_name

Lets save this request to our own collection so we can use it later.

1. Click on the drop down arrow next to "Save"
2. Click on **Save As**
3. Change the Request name to "Org list"
4. Change the Request description to "Get a list of your Orgs"
5. Be sure Workshop is selected under Select a collection or folder to save to:
6. Click on **Save to Workshop**

We need to replace the Test code that came with the SDK so we can create the variables we want to use when send our message to Slack.

1. Click on **Tests**
2. Copy and paste the below code into the Tests section.
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

Now we can verify if the variables have been created and assigned values.

1. Click on the **eye** icon next to the "VMC" Environment in the top right hand corner
2. Scroll down to see if the new variables were created.
3. Once verified click on the **eye** icon again to close the window

Lets save the changes we made to this request.

1. Click on **Save**

Now that we have details of our Org lets send them to Slack in a message.

If you are not familiar with Slack, Slack is a collaboration hub tool which brings people together in "Channels". The tool allows for inbound and outbound API calls for integrations with the systems which Slack teams interact with which allows them to get their work done faster.

To post to slack, a link needs to be generated for the slack channel that we want to post to. This has already been done for you and is listed below. One of the instructors will have this slack channel displayed on the screens. So you can see the results. Slack channel URL: <https://hooks.slack.com/services/T9HQFCTC1/B9JBL5SV7ArgKjF4zZDh7dnaWRyKNJfRY>

Now we need to setup the request:

1. Click on "+" across the top to open a new tab
    ![new tab](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/api-lab/Screenshot+at+Jul+19+16-47-57.png)
2. Change the request type to "POST"
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

You have completed this lab. Please continue to the AWS Integration Lab which you can access from this [AWS Integration Lab Link](https://vmc-field-team.github.io/labs/aws-integration-lab/)

Please add comments below if you would like to give feedback on this lab.
