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
# Introducción

En este ejercicio mostraremos cómo puede interactuar con VMware Cloud on AWS a través de medios programáticos. Veremos cómo podemos usar PowerShell como un medio para interactuar con la plataforma de soluciones de la nube, así como con la instancia de vCenter. Después, profundizaremos en cómo podemos interactuar con el API REST de VMware Cloud on AWS y realizar acciones tanto en la pestaña integrada de "Developer Center" en la consola, como a través de populares clientes REST de código abierto y de otras herramientas. Continuaremos nuestro ejercicio haciendo uso de "Postman" como nuestro cliente REST.

## Usando PowerShell

### Abra la ventana de PowerShell CLI

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs1.jpg)

1\. Haga click en *Start*, y baje hasta que vea el menú *Windows PowerShell*

2\. Haga click derecho en el acceso directo de PowerShell CLI y seleccione *Run as
Administrator*

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

7\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs9.jpg)

8\. Haga click en la caja desplegable junto a **Name/Org ID**

9\. Haga click en *My Account*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs10.jpg)

Ahora cree un nuevo refresh token para su ID vinculada a esta Org

10\. Haga click en **API Tokens**

11\. Haga click en **New token**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs11.jpg)

12\. Haga click en **Continue**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs12.jpg)

13\. Haga click en **Copy to Clipboard**

Ahora agréguela al servidor VMC

Escriba **connect-vmc -refreshtoken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"** y presione intro.

*NOTA:* Dentro de la ventana de PowerShell usted puede hacer click derecho y copiar el código, las comillas son opcionales

*NOTA:* Pegue el refresh token que copió en el ejercicio anterior.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs13.jpg)

Ahora podemos ver a cuales Orgs tenemos acceso

Escriba **Get-VMCorg** y presione intro.

14\. Note el Org Display_Name y ID

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs14.jpg)

Ahora que sabemos el Org Display_Name podemos buscar información sobre el SDDC dentro de nuestra org.

Escriba **Get-VMCSDDC -Org VMC-WS#** y presione intro.

*NOTA:* reemplace # con su número de estación de trabajo

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/APIs15.png)

Otra cosa interesante que se puede hacer es ver las Credenciales por Defecto de su SDDC

Escriba **Get-VMCSDDCDefaultCredential -org VMC-WS#** y presione intro.

*NOTA:* reemplace # con su número de estación de trabajo.


## REST APIs usando Developer Center

En este módulo se usarán REST APIs para obtener información básica sobre la organización y el SDDC. Para hacer esto se usará el nuevo Developer Center, que fue creado para el uso de APIs y scripts con los que se puede crear, agregar y eliminar SDDCs, además de poder conectarse y usar todas las APIs de vCenter. Para iniciar, regrese al ambiente VMC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter1.jpg)

Abra Chrome

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter2.jpg)

Si todavia no ha ingresado, hagalo a su VMC org.

1\. Desde dentro de la pestaña VMware Cloud on AWS, haga click en el menu Developer Center

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter3.jpg)

En el Developer Center hay muchos recursos para explorar. Por ejemplo, revisemos código de ejemplo que fue cargado por uno de los programadores de nuestra API. Si recorre esta pantalla verá diferentes ejemplos de código para (un Ambiente de Desarrollo para REST API ), Python, PowerCLI, y muchos otros. Cualquiera puede contribuir con ejemplos de código para la comunidad, si tiene interés visite http://code.vmware.com ó haga click en el enlace **VMware{code} Sample Exchange**.

2\. Haga click en *Code Samples* en el menú

3\. Haga click en *Download* en el cuadro PowerCLI - VMC Example Scripts

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter4.jpg)

Luego de que el script se descargue

4\. Haga click en la flecha desplegable

5\. Haga click en *Show in Folder*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter5.jpg)

6\. Haga click derecho en el script descargado

7\. Haga click en edit

Esto abrirá el ambiente PowerShell ISE. Ahora puede ver los PowerShell cmdlets que fueron usados en el módulo anterior asi como otros cmdlets que pueden ser usados con el SDDC.

Cierre las ventanas de PowerShell ISE

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter6.jpg)

Ahora se realizarán algunas invocaciones REST API simples en Developer Center, regrese a Chrome

8\. Haga click en el menu API Explorer

9\. Asegúrese de seleccionar su SDDC

10\. Haga click en la  echa desplegable al lado de Organizations

11\. Haga click en la flecha desplegable al lado del primer "GET" API

12\. Haga click en *Execute*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter7.jpg)

¿Qué falta? No fue ingresada la información de autenticación para realizar esta consulta. La razón es que estamos usando la autenticación de sesión para ejecutar estos comandos. Para ejecutar estos comandos en otra aplicación, como PowerShell o Postman, será necesario tener el recurso y el token de sesión antes de pueda invocar estas APIs.

Revisemos la respuesta.

13\. Aqui podrá ver el nombre alfanumérico de la Organization. Que puede encontrar en \#15

14\. El *ID*. *NOTA:* Copie el número de ID, sin comillas para uso en el siguiente paso.

15\. El *Display_Name* de la organización

16\. La Version de organización

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter8.jpg)

En este paso, se obtendrá información usando el metodo GET acerca de su organización GET

17\. Haga click en la flecha desplegable cerca a los SDDCs

18\. Haga click en *GET*

19\. El Org ID debería estar completado, otra característica incluida por los programadores
basada en la retroalimentación de los clientes. *NOTA:* Si el Org ID no se completa
automaticamente, péguelo.

20\. Haga click en *Execute*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/DeveloperCenter9.jpg)

Ahora revisemos el cuerpo de la respuesta

21\. La fecha de creación del SDDC

22\. La SDDC ID

23\. El estado del SDDC

## Postman

En este módulo, exploraremos como usar Postman para hacer invocaciones REST API y construir automatizaciones usando colecciones. Postman es una herramienta para el desarrollo de APIs. Por ejemplo, se pueden crear variables para usar dentro de APIs, probar respuestas y usar webhooks para integrarse con plataformas de colaboración.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman1.jpg)

Postman es facil de instalar. Para instalar Postman.

1\. Abra una nueva pestaña de browser y visite https://www.getpostman.com

2\. Haga click en *Download the App*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman2.jpg)

3\. Busque Postman para **Windows (64-bit)**, haga click en **Download**, haga doble click en el archivo que fue descargado, la instalación se completará sin interacción.

*NOTA:* Cierre las pestañas de Postman en Chrome

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman3.jpg)

4\. Haga click en el texto: *Take me straight to the app. I'll create an account another time.*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman4.jpg)

5\. Desmarque *Show this window on launch*

6\. Cierre esta ventana

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman5.jpg)

Regrese al browser, si no tiene una pestaña abierta para VMware Cloud

7\. Abra una nueva pestaña
    Escriba: https://vmc.vmware.com
    Ingrese con su student ID
    username : **corp\vmcws#** (su número de estudiante)
    password : **VMware1!**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman6.jpg)

Nuestro equipo interno de desarrollo de API ha creado un SDK para los lenguajes más populares de la actualidad. Para este módulo, se usará el SDK para REST para mostrar que es muy fácil importar y reusar algunas colecciones pre creadas para crear las nuestras.

8\. Haga click en *Developer Center*

9\. Haga click en *View Source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman7.jpg)

10\. Haga click en el menú de descarga

11\. Haga click en *Open*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman8.jpg)

12\. Haga Click en *Extract*

13\. Haga Click en *Extract all*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman9.jpg)

Se conservará la ubicación por defecto.

14\. Desmarque

15\. Haga click en *Extract*

Cierre las ventanas del explorador de Windows y la pestaña de github

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman10.jpg)

Ahora que tenemos Postman instalado y el repositorio de github en nuestro sistema local, vamos a importar la colección VMC y usar algunas peticiones para construir nuestra propia colección.

16\. Haga click en *Import*

17\. Haga click en *Choose Files*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman11.jpg)

Para importar el archivo json de la colección de VMC que acabamos de descargar.

18\. Recorra el directorio donde se hizo la descarga. El directorio debería ser *C:\downloads\vsphere-automation-sdk-rest-master\vsphere-automation-sdk-rest-master\samples\postman*

19\. Haga click en *VMware Cloud on AWS APIs.postman_collection.json*

20\. Haga click en *Open*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman12.jpg)

Ahora es necesario obtener el refresh token para nuestra Org en VMC. Regrese a la pestaña de VMware Cloud en el browser

21\. Haga click en la caja desplegable al lado de *Student Name/Org ID*

22\. Haga click en *My Account*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman13.jpg)

23\. Haga click en la pestaña *API Tokens*

24\. Haga click en *NEW TOKEN*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman14.jpg)

Ahora creamos un token de actualización para su ID vinculado a esta Org

25\.	Haga click en *Continue*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman15.jpg)

26\. Haga click en **Copy to Clipboard**
*NOTA:* Si no ha generado en token, haga click en Generate y copiélo en el portapapeles.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman16.jpg)

Regrese a Postman. Ahora es necesario configurar un ambiente en Postman para usarlo con VMC. Un ambiente es donde se crearán y almanecerán las variables. Estas variables pueden ser locales o globales, dependiendo del uso que se les de en Postman. En este módulo, solo se usarán variables locales.

27\. Haga click en *New*
28\. Haga click en *Environment*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman17.jpg)

29\. Dele el nombre de **VMC** al ambiente

30\. En la columna Key escriba **refresh_token**

31\. En la columba Value column use CTRL-V para pegar el refresh token que fue copiado en el
paso anterior.

32\. Haga click en *Add*

33\. Cierre la ventana

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman18.jpg)

Ahora configure este ambiente como por defecto. *NOTA:* Si no lo configura el ambiente *VMC* como el por defecto, las variables generadas no serán accesibles.

34\. Haga click en el menú desplegable

35\. Seleccione *VMC*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman19.jpg)

Ahora se comenzará a construir la colección usando una petición de las incluidas en el SDK que fue importada anteriormente.

36\. Haga click en *Collections*

37\. Haga click en - *Authentication and Login*

38\. Vea como esta petición tiene la variable refresh token que se definió en el paso anterior.
*NOTA:* Si el ambiente no fue configurado con VMC, este paso fallará porque la variable refresh_token no estará definida.

39\. Haga click en *Send*

40\. Ahora será accesible el token que fué generado con refresh token. Este es el cuerpo o
payload de la respuesta de la petición.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman20.jpg)

41\. Haga click en el ícono con el ojo

Se verá que el token de acceso ha sido almacenado en la variable para invocaciones futuras. ¿Cómo se hizo esto? Se ejecuto una prueba en el cuerpo de la respuesta. Se verá en el próximo paso.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman21.jpg)

42\. Haga click en *Tests*

La variable access_token fue cargada usando javascript en el cuerpo de la respuesta. También se usa la función setEnvironmentVariable de Postman para crearla.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman22.jpg)

Ahora se salva la petición en la colección para poder usarla luego.

43\. Haga Click en el menú desplegable

44\. Haga Click en *Save As*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman23.jpg)

45\. Cambie Request name por *Authorize*

46\. Cambie Request description por *Get Access Token*

47\. Haga click en *+Create Collection*

48\. Escriba **Workshop** y haga click en la caja de verificación *check box*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman24.jpg)

49\. Seleccione la carpeta *Workshop*

50\. Haga click en *Save to Workshop*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman25.jpg)

Una nueva ventana se desplegará indicando que se ha creado una nueva colección. No se hará nada más acá.

51\. Cierre la ventana

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman26.jpg)

Creemos algunos detalles de nuestra Org para enviarlos a Slack.

52\. Haga click en *Orgs* y *List Orgs*

53\. Haga click en *Headers*

54\. Haga click *Send*

55\. Ahora se verá aquí como se hace uso de la variable **access_token** como **csp-auth-token**. Esto
autorizará la petición. *NOTA:* Este token de acceso tiene una duración de 30 minutos. Si al ejecutar la petición y aparece un mensaje **400 unauthorized**, regrese y ejecute la petición de autorización.

56\. Revise el cuerpo de respuesta y encuentre el **display_name** de la Org

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman27.jpg)

Para guardar este petición en nuestra colección y poder usarla luego.

57\. Haga click en el menú desplegable

58\. Haga click en *Save As*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman28.jpg)

59\. Cambie Request name a **Org list**

60\. Cambie Request description a **Get a list of your Orgs**

61\. Asegúrese que *Workshop* esté seleccionado en **Select a collection or folder to save to:**

62\. Haga click en *Save to Workshop*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman29.jpg)

Es necesario reemplazar el código de prueba que viene con el SDK para poder crear las variables que sean necesarias para usar el mensaje a Slack.

63\. Haga click en *Tests*
    Copie y pegue el siguiente código en la sección **Tests**. *NOTA:* Es posible que tenga que oprimir CTRL-V para pegar el texto.

64\. Haga click en *Send*

                   var jsonData = JSON.parse(responseBody);

                    if (responseCode.code === 200) {
                      for (i = 0; i < jsonData.length; i++) {
                         pm.environment.set("name", jsonData[i].display_name);
                         pm.environment.set("ID", jsonData[i].id);
                         pm.environment.set("version", jsonData[i].version);
                         pm.environment.set("state", jsonData[i].project_state);
                      }
                    }

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman30.jpg)

Ahora es posible verificar si las variables han sido creadas y sus valores asignados.

65\. Haga click en el ícono con el ojo

66\.  Baje hasta encontrar las nuevas variables creadas.
      Una vez verificadas haga click en el ícono con el ojo de nuevo para cerrar la ventana

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman31.jpg)

Guardar los cambios hechos a esta solicitud.

67\. Haga click en *Save*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman32.jpg)

Ahora que se tienen los detalles de la Org, se enviará un mensaje a Slack.
Para enviar mensajes a Slack un enlace debe ser generado para el canal de Slack en que queremos publicar. Esta ya ha sido realizado y esta en la parte inferior. Uno de los instructores tendrá el canal de Slack en uno de los monitores. Para que puedan ver los resultados.

Slack channel URL: https://hooks.slack.com/services/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

Ahora es necesario configurar la petición:

68\. Haga click en el signo **+** para crear una nueva petición

69\. Cambie el request type a *POST*

70\. Corte y pegue la información del canal de Slack en el campo *address*

71\. Seleccione *Body*

72\. Cambie el format type a *raw*

73\. Escriba el código que esta a continuación, o corte y péguelo en la sección Body. *NOTA:* Es
posible que sea necesario oprimir CTRL-V para pegar el texto.

      {
        "text" : "Your Org ID is: {{ID}}\nYour Org version is: {{version}}\nAnd your Org state is: {{state}}",
        "username" : "{{name}}"
      }

74\. Haga click en *Send*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman33.jpg)

Salvar la petición en nuestra colección para usarla luego.

75\. Haga click en el menú desplegable

76\. Haga click en *Save As*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman34.jpg)

77\. Cambie Request name a **Post to Slack**

78\.  Cambie Request description a **Post some Org details to slack**
      Asegúrese de que Workshop esté seleccionado en *Select a collection or folder to save to:*

79\. Click on *Save to Workshop*

Revise si la petición publicó Nombre, ID, Version, y Status de la Org.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman35.jpg)

Ahora se mostrará como automatizar tareas usando una colección de Postman. Hasta ahora se han construido colecciones. Como se ve en la imagen hay 3 tareas en la colección Workshop.

80\. Haga click en la flecha en la ventana Workshop

81\. Click on *Run*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman36.jpg)

82\. Haga click en *Run Workshop*

83\. Asegúrese de que **Environment** esté configurado a VMC

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/APIs/Postman37.jpg)

Si todo fue salvado y es ejecutado individualmente, debería correr exitosamente también.

84\. Verifique el estado de cada petición.
