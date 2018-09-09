---
layout: single
title: "AWS Integration Workshop Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---
# Introducción

Una de las razones más convincentes para adoptar VMware Cloud on AWS es integrar sus sistemas existentes que se encuentran en su entorno de nube VMware, con plataformas de aplicaciones que residen en su entorno de nube privada virtual (VPC) de AWS. La integración que VMware y AWS han creado permite que estos servicios se comuniquen, de forma gratuita, a través de un espacio de direcciones de red privada para servicios como las instancias EC2, que se conectan a subredes dentro de una AWS VPC nativa o con servicios de plataforma que tienen la capacidad de conectarse a un punto final de VPC, como almacenamiento S3.

En este módulo, trabajaremos en algunas integraciones básicas comunes que puede utilizar en su plataforma VMware Cloud on AWS.

*Nota:* hay un requisito en este laboratorio de haber completado los pasos en el módulo [Trabajando con un SDDC](https://vmc-field-team.github.io/labs-es/working-with-sddc-lab/) en relación a la creación de un  Content Library, Redes, y las reglas de Firewall.

## Integración con AWS Relational Database Service (RDS)

### Implementar la VM Photo

Como primer paso para configurar nuestra integración entre la plataforma VMware Cloud on AWS y los servicios nativos de AWS, implementaremos una máquina virtual que utilizaremos para esta demostración. Esta VM se denominará "Photo VM". Por favor, siga las siguientes instrucciones.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS1.jpg)

1\. Si todavía o ha ingresado, abra el vCenter de VMware Cloud on AWS y haga click en *Menu*

2\. Seleccione *Content Libraries*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS2.jpg)

3\. Haga click en la Biblioteca de Contenido previamente creada con el nombre **Student#** donde # es su número de estudiante

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS3.jpg)

4\. Si todavía no esta ubicado ahi, asegúrese de hacer click en la pestaña *Template*

5\. Haga click derecho en la Plantilla *photo app*

6\. Seleccione *New VM from This Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS4.jpg)

7\. Dele el nombre a la máquina virtual de **PhotoApp#** donde # es su número de estudiante

8\. Expanda la ubicación y seleccione *Workloads*

9\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS5.jpg)

10\. Expanda el destino hasta encontrar *Compute-ResourcePool* como recurso de cómputo

11\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS6.jpg)

12\. En el paso **Review details** haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS7.jpg)

13\. En el paso **Select storage**, seleccione *WorkloadDatastore*

14\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS8.jpg)

15\. Seleccione la red que se creó en un paso anterior para la VM

16\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS9.jpg)

17\. Haga click en *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS10.jpg)

18\. Siga la implementación de la VM hasta que se haya completado

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS11.jpg)

19\. Revise la implementación de la VM

20\. Haga click en *Menu*

21\. Seleccione *VMs and Templates*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS12.jpg)

22\. Asegúrese que la VM esté encendida. Si no lo está, haga click derecho en la VM

23\. Seleccione *Power -> Power On*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS13.jpg)

24\. Asegúrese que una IP ha sido asignada a la VM (podría ser necesario esperar un minuto o dos). Anote esta dirección IP para un paso posterior.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS14.jpg)

25\. Regrese al portal de VMware Cloud on AWS y haga click en la pestaña *Network* para solicitar una dirección IP Pública

26\. Bajo *Compute Gateway* haga click y expanda *Public IPs*

27\. Haga click en *REQUEST PUBLIC IP*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS15.jpg)

28\. (Opcional) Escriba una nota para esta IP

29\. Haga click en *Request*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS16.jpg)

30\. Debería aparecer una notificación similar a la anterior

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS17.jpg)

31\. Anote la nueva dirección IP Publica adquirida

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS18.jpg)

32\. Ahora se crea una regla de NAT desde la nueva IP Pubica adquirida que fue anotada en el paso anterior a la dirección IP interna de la VM creada. Haga click en *NAT* para expander

33\. Haga click en *ADD NAT RULE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS19.jpg)

34\. Dele un nombre a la regla de NAT

35\. La nueva dirección IP Publica debería estar presente, sino es necesario escribirla

36\. Bajo *Service* seleccione *Any (All Traffic)*

37\. Escriba la dirección IP de la VM

38\. Haga click en el botón *SAVE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS20.jpg)

39\. Una notificación de *NAT rule successfully created* debería aparecer

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS21.jpg)

40\. Expanda *Firewall Rules*

41\. Haga click en *ADD RULE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS22.jpg)

42\. Dele un nombre a la Regla de Firewall

43\. Seleccione *All Internet and VPN* como Source

44\. Escriba la Dirección IP Publica que anotó anteriormente en *Destination*

45\. Seleccione *Any (All Traffic)* para la opción *Service*

46\. Haga click en el botón *SAVE*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS23.jpg)

47\. Deberá aparecer una notificación de *Firewall rule successfully created* similar a la anterior

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS24.jpg)

En el browser, abra una nueva pestaña y visite https://vmcworkshop.signin.aws.amazon.com/console

48\. Account ID o alias - **vmcworkshop**

49\. IAM user name - **Student#** donde # es el número que le fue asignado

50\. Password - **VMCworkshop1211**

51\. Haga click en *Sign In*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS25.jpg)

52\. Dentro de la consola de AWS. Asegúrese de seleccionar la region **Oregon**

53\. Haga click en servicio *RDS*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS26.jpg)

54\. En el panel izquierdo haga click en *Instances*

55\. Haga click en la instancia de RDS que corresponda a su Número de Estudiante

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS27.jpg)

56\. Baje hasta el area *Details* y bajo *Security and network* note que la instancia de RDS no es accesible publicamente, lo que significa que esta instacia solo puede ser accedida desde el interior de AWS

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS28.jpg)

57\. Regrese a la página principal *Services* en la consola AWS dando click en el enlace *Services*

58\. Baje hasta *Networking & Content Delivery* y haga click en *VPC*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS29.jpg)

59\. Haga click en *Security Groups* en el panel izquierdo

60\. Escoja el RDS Security Group **rds-launch-wizard-#** que corresponde al número de estudiante

61\. Luego de seleccionar el grupo de seguridad apropiado haga click en la pestaña *Inbound Rules* en la sección inferior

VMware Cloud on AWS establece el enrutamiento en el VPC Security Group por defecto, solamente RDS puede utilizar esto o crear esta configuración

62\. Note que el rango del bloque CIDR de la Red Logica Student#-LN Logical Network que fue creada en VMware Cloud on AWS esta autorizada para MySQL en el puerto 3306. Esto fue preparado con anterioridad

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS30.jpg)

AWS Relational Database Service (RDS), también crea su propia Elastic Network Interface (ENI) para acceso la cúal esta seprada de la ENI creada por VMware Cloud on AWS.

63\. Haga click en *Services* y regrese a Consola Principal

64\. Haga click en *EC2*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS31.jpg)

65\. En el EC2 Dashboard haga click en *Network Interfaces* en el panel izquierdo

66\. Todos los ambientes de los estudiantes pertenecen a la misma cuenta de AWS, por esta razón, cientos de ENIs podrían existir. Para minimizar la vista, escriba *RDS* en el area de búsqueda y presione Intro para agregar un filtro

67\. Seleccione **rds-launch-wizard-#** que corresponde a su número de estudiante

68\. Anote la dirección *Primary private IPv4 IP* para el próximo paso

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/RDS32.jpg)

69\. Abra una pestaña adicional en el browser y escriba la dirección IP pública que fue solicitada en el portal VMware Cloud on AWS en la barra de dirección seguida por /Lychee (sensible a mayúsculas) ie: x.x.x.x/Lychee

70\. Escriba la información de conexión a la base de datos descrita abajo (sensible a mayúsculas), usando la dirección IP del RDS ENI que anotó en el paso previo:
      Database Host: **x.x.x.x:3306**
      Database Username: **student#**
      Database Password: **VMware1!**

71\. Haga click en *Connect*

Hemos creado con éxito una aplicación híbrida que utiliza componentes tanto en su entorno de VMware Cloud on AWS SDDC como en sus servicios nativos de AWS.

Esta funcionalidad ofrece a los clientes ahora opciones sobre cómo se migran las aplicaciones a la nube. Ahora puede dividir su aplicación entre plataformas y consumir servicios en vSphere y AWS. Este nivel de elección realmente puede ayudar a aquellos que buscan migrar a la nube, acelerar ese proceso al no "empantanarse" en la refactorización de partes de una aplicación que son difíciles de trasladar a la nube de hiperescala.

## Integración con AWS Elastic File System (EFS)

### Creación de una VM para integrar con EFS

En nuestra próxima sección sobre integración de servicios de AWS con VMware Cloud on AWS, utilizaremos AWS Elastic File System (EFS), que podemos usar para montar recursos compartidos de NFS en nuestras máquinas virtuales alojadas en VMware Cloud on AWS.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS1.jpg)

1\. Navegue en la Biblioteca de Contenido, haga click en *Menu* en el vCenter Server de VMware Cloud on AWS

2\. Seleccione *Content Libraries*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS2.jpg)

3\. Haga click en la Biblioteca de Contenido *Student#*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS3.jpg)

4\. Asegúrese que la pestaña *Templates* esté seleccionada

5\. Haga click derecho en la plantilla *efs*

6\. Seleccione *New VM from This Template*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS4.jpg)

7\. Dele el nombre **EFSVM#** a la VM donde # sea el número de estudiante asignado

8\. Seleccione *Workloads* como la ubicación de la VM

9\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS5.jpg)

10\. Seleccione *Compute-ResourcePool* como el destino de su VM

11\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS6.jpg)

12\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS7.jpg)

13\. Seleccione *WorkloadDatastore* como almacenamiento

14\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS8.jpg)

15\. Seleccione la red en *Destination Network*

16\. Haga click en *NEXT*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS9.jpg)

17\. Haga click en *FINISH*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS10.jpg)

18\. Asegúrese de encender la VM y asegúrese que tenga una dirección IP asignada

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS11.jpg)

En el browser, abra una nueva pestaña y visite: https://vmcworkshop.signin.aws.amazon.com/console

19\. Account ID or alias - **vmcworkshop**

20\. IAM user name - **Student#** donde # es el número asignado

21\. Password - **VMCworkshop1211**

22\. Haga click en *Sign In*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS12.jpg)

23\. Ahora ha ingresado a la consola de AWS. Asegúrese que la región seleccionada sea *Oregon*

24\. Haga click en el servicio *EFS*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS13.jpg)

25\. Seleccione el NFS Student #

26\. Anote la dirección IP

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/aws-integrations/EFS14.jpg)

27\. Regrese a la pestaña vCenter Server, haga click en *Launch Web Console* de la VM EFS (es posible que necesite permitir las ventanas emergentes en el browser). Ingrese usando las siguientes credenciales:
    User: **root**
    Password: **VMware1!**

Escriba los siguientes comandos en la terminal:

• **cd /**

• **mkdir efs**

• **sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2
MOUNT_TARGET_IP:/ efs**
    Dónde *MOUNT_TARGET_IP* es la IP que anotó en un paso anterior para su EFS

• **cd efs**

• **touch hello.world**

• **ls**

Hemos integrado AWS EFS don una VM en VMware Cloud on AWS.
