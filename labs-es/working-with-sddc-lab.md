---
layout: single
title: "Working with your SDDC Lab Manual"
date: 2018-07-17
tags: workshop
toc: true
classes: wide
author_profile: false
categories: labs
comments: true
---
# Introducción

En este módulo, comenzaremos con las tareas básicas que realizará en la interfaz de usuario de VMware Cloud on AWS cuando administre la plataforma.

## Agregue un Host a su SDDC

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC1.jpg)

1\. En el *Student Workshop #* SDDC asignado, haga click en el botón *View Details*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC2.jpg)

2\. Haga click en el botón *Actions*

3\. Haga click en *Add Hosts*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC3.jpg)

4\. Como solo se va a agregar un host, revise el campo resaltado

5\. Haga click en el botón *Add Hosts*

Felicitaciones! Ha completado este paso.

Agregar un host adicional a un SDDC existente toma alrededor de 10 minutos para completar.

## Configuración de Reglas de Firewall en un SDDC

En VMware Cloud on AWS tenemos dos Edge Gateways los cuales protegen las dos redes de un VMware Cloud on AWS SDDC. La **Management Gateway** y la **Compute Gateway**. Cuando iniciamos nuestro ambiente SDDC por primera vez, por defecto todo el trafico de las redes de Management y Compute esta prohibido. En este ejercicio seguiremos los pasos necesarios para crear reglas de firewall para poder administrar el SDDC y no solamente las cargas de trabajo, también podremos permitirles comunicarse con los servicios nativos de AWS.

### Reglas de Firewall para el Management Gateway

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC99.jpg)

Por defecto, el Firewall está configurado para denegar todo el tráfico tanto entrante como saliente del Management Gateway. En este ejercicio, se agregará una regla de Firewall para permitir el tráfico de vCenter. Para tener acceso al vCenter Server del SDDC, se debe agregar que lo permita al vCenter Server.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC100.jpg)

1\. Haga click en la pestaña titulada *Networking & Security*

 2\. Haga click en *Gateway Firewall*

3\. Haga click y seleccione *Management Gateway*

4\. Haga click en el botón *ADD NEW RULE*

5\. Nombre la regla de Firewall *vCenter Inbound*

6\. Haga click en *Set Source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC101.jpg)

1\. Haga click en *Any* para seleccionar 

2\. Haga click en el botón de *Save*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC102.jpg)

1\. Haga click en *Set Destination*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC103.jpg)

1\. Haga click en *System Defined Groups*

2\. Seleccione *vCenter*

3\. Haga click en el botón de *SAVE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC104.jpg)

1\. Haga click en el espacio bajo *Services* y escoja *HTTPS (TCP 443)* para permitir el acceso al Servidor de vCenter

2\. Publique la regla haciendo click en el boton *PUBLISH*

vCenter ahora tiene acceso para que se pueda conectar de cualquier punto en el internet.

### Reglas de Firewall para el Compute Gateway

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC105.jpg)

Por defecto, el Compute NSX Edge Services Gateway también esta configurado para denegar todo el tráfico tanto entrante como saliente. Será necesario crear reglas de firewall para permitir el tráfico de acuerdo a las necesidades.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC106.jpg)

Cree una Regla de Firewall bajo la sección Compute Gateway para el acceso nativo a los servicios de AWS.

1\. Haga click en *Compute Gateway*

2\. Haga click en *ADD NEW RULE*

 3\. Nombre la regla *AWS Inbound*

4\. Haga click en *Set Source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC108.jpg)

1\. Seleccione *Connected VPC Prefixes*

2\. Haga click en *SAVE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC109.jpg)

1\. Haga click en *Set Destination*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC110.jpg)

1\. Seleccione *Any*

2\. Haga click en *SAVE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC111.jpg)

1\. Haga click en *Set Service*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC112.jpg)

1\. Seleccione *Any*

2\. Haga click en *SAVE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC113.jpg)

1\. Haga click en *PUBLISH*

#### Regla de Firewall AWS Entrante

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC10.jpg)

Cree una Regla de Firewall AWS entrante siguiendo las siguientes instrucciones:

4\. Name - **AWS Inbound**

5\. Action - **Allow**

6\. Source - **All connected Amazon VPC**

7\. Destination - **192.168.#.0/24** (Donde # es su número de estudiante)

8\. Service - **ANY**

9\. Haga click en el botón *SAVE*

#### Regla de Firewall AWS Saliente

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC11.jpg)

Siga el mismo proceso que en la sección anterior y cree una Regla de Firewall AWS Saliente:

1\. Name - **AWSOutbound**

2\. Action - **Allow**

3\. Source - **192.168.#.0/24** (Donde # es su número de estudiante)

4\. Destination - **All connected Amazon VPC**

5\. Service - **ANY**

6\. Haga click en el botón *SAVE*

## Ingrese al vCenter de VMware Cloud on AWS

### Pestaña de Ajustes (Settings)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC12.jpg)

1\. Ingrese a su Student # SDDC y haga click en la pestaña *Settings*. La información de conexión será desplegada para el ambiente VMware Cloud on AWS:

2\. Información de la cuenta local para el vCenter de VMware Cloud on AWS.

3\. URL del vCenter Server

4\. URL del vCenter Server API Explorer

5\. Cadena de conexión PowerCLI Connect para conectarse via PowerCLI al vCenter Server de VMware Cloud on AWS

6\. Nombre de dominio completo (FQDN) de vCenter

7\. Información de IP pública de vCenter

8\. IP pública actual de vCenter

9\. IP privada asignada a vCenter

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC13.jpg)

Haga click en la URL del vSphere Client (\#3 mencionado arriba), ingrese con el usuario cloudadmin@vmc.local y copie la contraseña al portapapeles del computador y péguelo en el campo Password.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC14.jpg)

Ahora se encuentra dentro del vCenter Server de VMware Cloud on AWS como el usuario cloudadmin@vmc.local.

## Creación de un Content Library

Las bibliotecas de contenido son contenedores para objetos como plantillas de VM, plantillas de vApp, y otros tipos de archivos como imágenes ISO.

Es posible crear bibliotecas de contenido usando vSphere Web Client, y agregarles plantillas, con las cuales es posible desplegar máquinas virtuales o vApps en su ambiente VMware Cloud on AWS si ya tiene una biblioteca de contenido en su centro de datos el contenido puede ser importado a su SDDC.

Puede crear dos tipos de bibliotecas de contenido: contenido local o suscrita.

### Bibliotecas Locales

Puede usar las bibliotecas locales para almacenar objetos en una sola instacia de vCenter Server. Puede publicar la biblioteca local para que usuarios en otros sistemas vCenter Server se suscriban a ella. Cuando se publica una biblioteca de contenido externamente, es posible configurar una contraseña como medio de autenticación.

Las plantillas de VM y las plantillas de vApp son almacenadas como archivos OVF en la biblioteca de contenido. También se pueden cargar otros tipos de archivos como imágenes ISO, archivos de texto y demás.

### Bibliotecas Suscritas

La suscripción a una biblioteca publicada se realiza al crear una biblioteca suscrita. Es posible crear una biblioteca suscrita en la misma instancia de vCenter Server donde la biblioteca esta publicada o en un vCenter Server diferente. En el asistente de creación de la biblioteca existe la opción para descargar todo el contenido de la biblioteca publicada inmediatamente luego de que la biblioteca suscrita haya sido creada o solamente descargar la metadata de los objetos de la biblioteca pubilcada y luego descargar el contenido de los objetos que se desean utilizar.

Para asegurar que el contenido de la biblioteca suscrita esté actualizado, la biblioteca suscrita se sincroniza automáticamente con la biblioteca publicada de origen en intervalos regulares. También es posible sincronizar la biblioteca manualmente.

Puede usar la opción de descarga de contenido de la biblioteca publicada inmediatamente o solamente cuando sea necesario para administrar el almacenamiento.

La sincronización de una biblioteca suscrita que tiene la opción de descargar todo el contenido de la biblioteca publicada inmediatamente, sincronizar tanto como la metadata de los objetos así como ellos. Durante la sincronización de la biblioteca los nuevos objetos son completamente descargados a la ubicación configurada en la biblioteca suscrita.

La sincronización en una biblioteca suscrita que tiene la opción de descargar el contenido solo cuando es necesario, sincronizar solamente la metadata de los objetos de la biblioteca publicada y no descargar los objetos. Esto ahorra espacio. Si un objeto de la biblioteca debe ser utilizado es necesario sincronizar el objeto. Luego de usarlo es posible eliminar el objeto para liberar espacio.

Si utiliza una biblioteca suscrita, solo puede utilizar el contenido, pero no contribuir con nuevos elementos. Solo el administrador de la biblioteca publicada puede administrar los archivos y plantillas.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC15.jpg)

1\. Haga click en *Menu*

 2\. Haga click en *Content Libraries*

### Suscripción a una Biblioteca de Contenido existente

Es posible que tenga Bibliotecas de Contenido en su centro de datos y es posible usar Bibliotecas de Contenido como fuente para importar contenido a su SDDC.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC16.jpg)

1\. En la ventana Content Library, haga click en el simbolo *+* para agregar una Biblioteca de Contenido.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC17.jpg)

2\. Dele el nombre *Student#* a su Biblioteca de Contenido donde # es el número asignado

3\. (Opcional) Agregue algunas notas a su Biblioteca de Contenido 

4\. Haga click en el botón *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC18.jpg)

5\. Seleccione *Subscribed content library*

6\. En el campo *Subscription URL* entre lo siguiente: *https://vcenter.sddc-34-216-241-49.vmc.vmware.com:443/cls/vcsp/lib/4aa185b4-3d6e-45b4-90ca-cd3a845d4502/lib.json*

      POR FAVOR TENGA EN CUENTA QUE PUEDEN HABER PROBLEMAS AL COPIAR Y PEGAR CARACTERES DE LA URL CUANDO SE COPIA Y PEGA DESDE EL MANUAL. LA URL SE ENCUENTRA EN UN ARCHIVO DE TEXTO EN LA UNIDAD Z:\ DEL ESCRITORIO ASIGNADO. SOLICITE INSTRUCCIONES AL INTRUCTOR SI NO PUEDE ENCONTRAR EL ARCHIVO.

7\. Haga click la caja de verificación *Enable authentication*

 8\. Para el campo *Password* entre: **VMware1!**

 9\. Asegúrese que la opción *Download content* se encuentre en *immediately*

10\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC19.jpg)

11\. Elija *WorkloadDatastore* como la ubicación de almacenamiento

12\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC20.jpg)

13\. Haga click en el botón *Finish*. La biblioteca de contenido toma aproximadamente 20 minutos en completar su sincronización.

### Crear una Biblioteca de Contenido Local

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC21.jpg)

1\. Haga click en el signo *+* para crear una Biblioteca de Contenido

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC22.jpg)

2\. Dele el nombre : **LocalContentLibrary#** a su Biblioteca de Contenido donde # corresponde su número de estudiante

3\. (Opcional) Agregue algunas notas a su Biblioteca de Contenido 

4\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC23.jpg)

5\. Asegúrese que *Local content library* este seleccionado

6\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC24.jpg)

7\. Seleccione *WorkloadDatastore* como la ubicación de almacenamiento

8\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC25.jpg)

9\. Revise la información y haga click en el botón *Finish*

Felicitaciones, ha creado una Biblioteca Local de Contenido.

## Creación de una Red Lógica

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC26.jpg)

1\. Una vez haya ingresado a vCenter Server haga click in *Menu*

2\. Seleccione *Global Inventory Lists* del menú desplegable, la siguiente imagen deberá aparecer

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC27.jpg)

3\. Haga click en *Logical Networks* en el panel izquierdo

4\. Haga click en el botón *+ Add*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC28.jpg)

5\. Dele el nombre *Student#-LN* a la nueva Red Lógica donde # es su número de estudiante

6\. Asegúrese de haber seleccionado *Routed Network*

7\. Para el bloque CIDR entre **192.168.###.0/24** usando su número de estudiante en lugar de \#

    • Si su número de estudiante asignado esta entre 1 y 9, su bloque CIDR debería parecerse a: *192.168.1.0/24* - En este ejemplo se muestra el número de estudiante 1

    • Para los números de estudiante entre 10 y 20 debería parecerse a: *192.168.10.0/24* - En este ejemplo se muestra el número de estudiante 10

8\. Entre **192.168.###.1** como Default Gateway IP - Ejemplo: 192.168.1.1 

9\. Asegúrese que la opción DHCP este habilitada dandole click a la caja de verificación

10\. Entre **192.168.###.100-192.168.###.200** en el campo IP Range

 11\. Digite **corp.local** en el campo DNS Domain Name

 12\. Haga click en *OK* para crear una nueva Red Lógica

## Creación de una Especificación de Personalización para Linux

Cuando se clona una máquina virtual o se despliega desde una plantilla, es posible personalizar el sistema operativo de la VM para cambiar propiedades como el nombre del red, la configuración de red o licenciamiento.

Personalizar los sistemas operativos de las VMs ayuda a prevenir conflictos que pueden resultar en VMs desplegadas con configuraciones idénticas como por ejemplo nombres de red iguales.

Se pueden especificar características personalizadas abriendo el asistente de Guest Customization durante los procesos de clonación o despliegue. Alternativamente, se pueden crear especificaciones de personalización, que son configuraciones de personalización almacenadas en la base de datos de vCenter. Durante los procesos de clonación o despliegue, es posible seleccionar una especificación de personalización para aplicar a esta nueva máquina virtual.


Use el Customization Specification Manager para administrar las especificaciones de personalización que son creadas usando el asistente de Guest Customization.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC29.jpg)

1\. Haga click en *Menu*

 2\. Haga click en *Policies and Profiles*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC30.jpg)

3\. Haga click en *+ New* para agregar una nueva Especificación de Personalización para Linux

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC31.jpg)

4\. Dele un nombre a la Especificación de Configuración de la VM

5\. Entre una descripción (Opcional)

6\. Asegúrese de seleccionar *Linux*

7\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC32.jpg)

8\. Haga click en el botón *Enter a name*

 9\. Dele un nombre a la VM linux

 10\. Haga click en la caja de verificación *Append a numeric value*

11\. Entre **corp.local** para el campo *Domain Name*

 12\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC33.jpg)

13\. Seleccione *US* en el campo Area

14\. Seleccione *Eastern* en el Location

15\. Seleccione *Local time*

 16\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC34.jpg)

17\. Deje los valores por defecto en la pantalla *Network* y haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC35.jpg)

18\. En el campo Primary DNS Server entre **10.46.159.10**

19\. Escriba **corp.local** en el campo DNS Search Paths

 20\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC36.jpg)

21\. Revise la información y haga click en el botón *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC37.jpg)

Felicitaciones! Ha creado exitosamente una Especificación de Personalización para maquinas virtuales Linux. Tambien es posible Duplicar, Editar, Importar y Exportar una Especificación de Configuración para VM.

## Implementación de una Máquina Virtual

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC38.jpg)

1\. En Bibliotecas de Contenido *(Menu -> Content Libraries)*, seleccione *Student#* y seleccione la pestaña *Templates*

2\. Haga click derecho en la plantilla *centos01-web* y seleccione *New VM from This Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC39.jpg)

3\. Dele el nombre *StudentVM#* a la Máquina Virtual donde # es el número de estudiante

4\. Expanda el area de ubicación hasta que *Workloads* sea visible y selecciónela

5\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC40.jpg)

6\. Expanda el area hasta encontrar *Compute-ResourcePool*, seleccione esta opción

7\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC41.jpg)

8\. Haga click en el botón *Next* y revise la información en la pantalla de detalles

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC42.jpg)

9\. En el paso *Select storage*, seleccione *WorkloadDatastore*

10\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC43.jpg)

11\. En el paso *Select networks*, despliegue y seleccione en Destination Network (podría ser necesario hacer click en Browse para ver más redes) y seleccione la red *Student#-LN* que fue creada previamente

12\. Haga click en el botón *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC44.jpg)

13\. En la sección *Ready to complete*, asegúrese que todas las secciones tienen la información correcta y haga click en el botón *Finish*

## Conversión de una Máquina Virtual a Plantilla

En este sección se clonará una Máquina Virtual recién creada a una Plantilla para usarla luego en la sección de vRealize Automation.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC45.jpg)

1\. Asegúrese que la implementación de la VM de la sección anterior haya terminado

2\. Haga click en *Menu*

 3\. Seleccione *VMs and Templates*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC46.jpg)

4\. Seleccione la VM recientemente creada *Student#* donde # es su numero de estudiante

5\. Haga click en *Template*

 6\. Seleccione *Convert to Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/working-with-sddc-lab/SDDC47.jpg)

7. Haga click en el boton *Yes* en el cuadro de dialogo de con_rmación Ha terminado este paso.

Ha completado esta sección.

Entre comentarios en el àrea de abajo si le gustaría dejar sugerencias sobre este ejercicio.
