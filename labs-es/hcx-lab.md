---
layout: single
title: "Hybrid Cloud Extension (HCX) Lab Manual"
categories: labs
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
---
# Introducción

En este ejercicio aprenderá acerca de Hybrid Cloud Extension (HCX), principalmente esta es una herramienta, incluida con VMware Cloud on AWS, que le permitirá migrar cargas de trabajo a VMware Cloud on AWS y reducir significativamente el tiempo y la complejidad de la migración de cargas de trabajo en la esfera de la nube pública.

## ¿Qué es Hybrid Cloud Extension (HCX)?

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX1.jpg)

Hybrid Cloud Extension abstrae los recursos on-premises y los ubicados en la nube presentándolos a las aplicaciones como una nube híbrida continua. Hybrid Cloud Extension provee interconexiones a múltiples sitios, de alto rendimiento, seguras y optimizadas. La hibridación se da gracias a la abstracción y a las interconexiones. Sobre esta hibridación, Hybrid Cloud Extension facilita movilidad de aplicaciones segura y simple a través de las plataformas vSphere en el centro de datos y las Nubes VMware. Hybrid Cloud Extension es un servicio multi-sitio, multi-nube que facilita la operación de una verdadera nube híbrida.

### Características de Hybrid Cloud Extension

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX2.jpg)

Any-to-Any vSphere Cloud App Mobility

• Elimina la necesidad de una evaluación de preparación de nube y un diagnostico de dependencia de aplicaciones

• Movimiento rápido de cargas de trabajo existentes desde una plataforma vSphere al SDDC

• Reducir el tiempo de planeación inicial para costos y análisis de recursos

• Acelerar la adopción de nube y evitar cambios importantes en el ambiente on-premise

**Continuidad de Negocio con un Menor TCO**

• Reconfiguración de direcciones IP y MAC no es necesario

• No es necesario modificar las VMs existentes en el ambiente

• Hybrid Cloud Extension brinda migración warm y cold en lote y bidireccional

• Hybrid Cloud Extension simplifica su modelo operacional

**Diseñado para la Seguridad**

• Asegure un control altamente seguro para las nubes privadas y publicas

• Proteja recursos con capacidades de recuperación de desastre

• La DMZ híbrida de Hybrid Cloud Extension permite portabilidad de las redes empresariales y
las prácticas seguras en la nube

• Las políticas de seguridad migran junto con las aplicaciones

**Hibridación de Infraestructura de Alto Rendimiento**

• Optimización WAN incluida que se adapta a las necesidades de los casos de uso híbridos

• Hybrid Cloud Extension brinda enrutamiento ágil e inteligente

• Balanceo de tráfico garantizado por políticas

• Múltiples modelos de migración de VM (incluyendo vMotion, warm y cold) facilita la
migración a y desde la nube sin ningún cambio

## HCX - Migración usando vMotion

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX3.jpg)

1\. Abra Chrome y haga click en el acceso directo *HCX-vMotion*

2\. Haga click en *X* en el panel derecho y aumente el tamaño de la pantalla principal

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX4.jpg)

La primera pestaña en el browser muestra un vCenter Server on-premise.

3\. Haga click en la segunda pestaña, esta muestra un segundo centro de datos (Tambien podría mostrar un VMware Cloud on AWS vCenter)

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX5.jpg)

4\. Haga click en la primera pestaña del browser y regrese al vCenter on-premise.

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX6.jpg)

5\. Haga click en la VM con el nombre *Mission Critical Workload 1*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX7.jpg)

6\. Haga click en la Consola

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX8.jpg)

Una consola se abrirá para la VM *Mission Critical Workload 1*, trate de hacer ping a dirección IP 10.159.137.212 la cual corresponde a una VM en el sitio secundario.

7\. Haga click en la pestaña correspondiente al sitio secundario

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX9.jpg)

8\. Haga click en la VM con el nombre *TargetSite-TestVM*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX10.jpg)

9\. Anote la dirección IP correspondiente a esta VM que corresponde a la VM *Mission Critical Workload 1* en el sitio primario e intente hacer ping

10\. Haga click en la pestaña *Mission Critical Workload 1*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX11.jpg)

• Presione *enter* luego del comando ping

• Haga *Control-C* para detener el ping

• Escriba **ping 172.16..4.2**, esta la dirección IP de la VM

11\. Haga click en la *X* de esta pestaña en su browser

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX12.jpg)

12\. Haga click en la primera pestaña y regrese al vCenter on-premise

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX13.jpg)

13\. Haga click en el botón *Actions*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX14.jpg)

14\. Haga click en *Hybridity Actions*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX15.jpg)

15\. Haga click en *Migrate to the Cloud*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX16.jpg)

16\. Haga click en *(Specify Destination Container)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX17.jpg)

17\. Seleccione *RedwoodCluster*

18\. Haga click en el botón *Select Destination*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX18.jpg)

19\. Haga click en el botón *(Select Storage)*

20\. Seleccione *cloudStorage*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX19.jpg)

21\. Haga click en *(Select Virtual Disk Format)*

22\. Seleccione *Same format as source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX20.jpg)

23\. Haga click en *(Select Migration Type)*

24\. Seleccione *vMotion*

25\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX21.jpg)

26\. Espere el mesaje *Validation is Successful*

27\. Haga click en el botón *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX22.jpg)

28\. Haga click en el botón *Home*

29\. Haga click *HCX* en el panel izquierdo

30\. Haga click en la pestaña *Migration*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX23.jpg)

31\. Revise el progreso de la migracion via vMotion

32\. Haga click en el boton *Refresh* para actualizar el estado

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX24.jpg)

33\. Una vez que la migración haya terminado, haga click en la segunda pestaña del browser para abrir el vCenter del sitio de destino

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX25.jpg)

34\. Ahora verá que la VM *Mission Critical Workload 1* se ha migrado exitosamente al Sitio de Destino, haga click en su nombre

35\. Haga click en la consola

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX26.jpg)

• Puede ver el ping que dejo ejecutándose en el paso previo nunca paro y que la VM durante el proceso de vMotion no tuvo caídas

• Presione *Control-C* para detener el ping

36\. Haga click en la *X* de la pestaña del browser para cerrar la pestaña de la consola

**Migración Reversa**

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX27.jpg)

37\. Haga click en la primera pestaña del browser

38\. Haga click en el botón *Migrate Virtual Machines*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX28.jpg)

39\. Haga click en la caja de verificación *Reverse Migration*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX29.jpg)

40\. Haga click en la caja de veri cación *Mission Critical Workload 1*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX30.jpg)

41\. Haga click en *(Specify Destination Container)*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX31.jpg)

42\. Seleccione *Tier 0 Workloads*

43\. Haga click en el botón *Select Destination*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX32.jpg)

44\. Haga click en *(Select Storage)*

45\. Seleccione *onpremStorage*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX33.jpg)

46\. Haga click en *(Select Virtual Disk Format)*

47\. Seleccione *Same format as source*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX34.jpg)

48\. Haga click en *(Select Migration Type)*

49\. Seleccione *vMotion*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX35.jpg)

50\. Haga click en *Next*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX36.jpg)

51\. Una vez que el mensaje *Validation is Successful* aparezca

52\. Haga click en el botón *Finish*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX37.jpg)

53\. Revise el progreso del proceso de verificación

54\. Haga click en el botón *Refresh*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX38.jpg)

55\. Haga click en el botón Home

56\. Haga click en el botón *Hosts and Clusters*

![](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/HCX/HCX39.jpg)

57\. Haga click en la VM *Mission Critical Workload 1*, puede ver que el proceso de migración reversa al vCenter on-premise fue exitoso

## HCX - Bulk Migration

1. In your Chrome browser click the **HCX-Bulk** bookmark
2. Click on the **X** to enlarge the main screen
3. As with the vMotion module, in this example we also have a source site (on-premises) vCenter
4. Click on the second tab in the browser to view the Target site vCenter
5. Click on the tab for the on-premises vCenter
6. Click on the **Home** button
7. Click **HCX**
8. Click on the **Migration** tab
9. Click the **Migrate Virtual Machines** button
10. Select **Tier 1 Workloads** in the left pane
11. Click the **checkbox** to select all VM's
12. Click **(Specify Destination Container)**
13. Select **RedwoodCluster** as the Destination Container
14. Click the **Select Destination** button
15. Click **(Select Storage)**
16. Select **cloudStorage**
17. Click **(Select Virtual Disk Format)**
18. Select **Same format as source**
19. Click **(Select Migration Type)**
20. Select **Bulk Migration**
21. Click on **HelpDesk Workload 1** to see the options for this workload
22. Click on **HelpDesk Workload 2** to see the options for this workload
23. Click on the **sidebar** to view all the options
24. Click the **Next** button
25. Wait for **Validation is Successful** message
26. Click **Finish** button
27. Check Progress of the migrations
28. Click **Refresh** button
29. Once migrations complete, click the **Hosts and Clusters** button
30. Click on the second tab to view the cloud vCenter
31. You can see that both workloads have successfully migrated to the Target vCenter
