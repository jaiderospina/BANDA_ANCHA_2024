#GUIA GNS3
Para crear un nuevo proyecto se clickea en File►New blank proyect. Se nombra el proyecto como uno desee.

La siguiente imagen muestra la interfaz gráfica de usuario GNS, la cual se divide en varias partes.

!(Imagenes/InterfazUsuarioGNS3.jpg)

El espacio de trabajo es ese espacio grande del centro de la pantalla, donde se crean las tipologías, agregando los dispositivos y conexiones necesarios.

!(Imagenes/EspacioDeTrabajo.jpg)

La barra de herramientas está en el espacio de arriba y facilita las cosas.

!(Imagenes/BarraDeHerramientas.jpg)

La barra de herramientas de dispositivos está ubicada en la parte izquierda de la pantalla, permitiendo agregar dispositivos al espacio de trabajo, siendo routers, switches, dispositivos finales, dispositivos de seguridad, conexiones y una pestaña con todos los dispositivos que estén en la barra de herramientas de dispositivos.

!(Imagenes/BarraDeHerramientasDispositivos.jpg)

El panel de resumen de topología y el de servidores están ubicados en la parte derecha de la pantalla, mostrando los dispositivos, sus estados e interfaces de conexión entre los dispositivos.

!(Imagenes/PanelResumenTopología.jpg)

!(Imagenes/PanelResumenServer.jpg)

Por ultimo esta la consola la cual está ubicada en la parte de abajo y su función es la de ejecutar códigos y avisar de errores que se hallan presentado.

!(Imagenes/ConsolaGNS3.jpg)

##CREAR TOPOLOGIAS

Para agregar dispositivos en el espacio de trabajo se arrastran y sueltan con el mouse, gracias a la barra de herramientas de dispositivos.

!(Imagenes/Topo1.jpg)

!(Imagenes/Topo2.jpg)

Para conectar los dispositivos se selecciona el botón de conexiones de la barra de herramientas de dispositivos, se clickea el dispositivo que se quiera seleccionar, escogiendo un puerto, se repite el proceso anterior para conectar el dispositivo con otro.

!(Imagenes/Topo3.jpg)

!(Imagenes/Topo4.jpg)

Se pueden apreciar los estados en las conexiones de los dispositivos en el espacio de trabajo, los cuales también aparecen en el panel de resumen de topología como se dijo anteriormente. También podemos agregar notas gracias al agregar texto y etiquetas de interfaces, los cuales se encuentran en la barra de herramientas, estas notas nos pueden facilitar la comprensión de la topología.

En la barra de herramientas se encuentran 3 botones muy importantes, el botón de play que enciende todos los dispositivos, el botón de pausa que los suspende y el botón de stop que los apaga.

!(Imagenes/Topo5.jpg)

!(Imagenes/Topo6.jpg)

Una función muy importante de la barra de herramientas es el botón de “Console connect to all devices” el cual permite configurar los dispositivos usando por defecto Solar-PuTTY en Windows, el cual se puede para cambiar por otros.

!(Imagenes/Topo7.jpg)

Usando los comandos se puede asignar una ip a nuestros dispositivos finales, pudiendo hacer ping entre ellos, si se agrega otro dispositivo después de clickear el botón de play se puede encender y realizar las funciones previamente mencionadas dándole click derecho.

!(Imagenes/Topo8.jpg)

!(Imagenes/Topo9.jpg)

!(Imagenes/Topo10.jpg)

!(Imagenes/Topo11.jpg)

!(Imagenes/Topo12.jpg)

!(Imagenes/Topo13.jpg)

!(Imagenes/Topo14.jpg)

!(Imagenes/Topo15.jpg)

!(Imagenes/Topo16.jpg)

!(Imagenes/Topo17.jpg)

Por último, es importante guardar el proyecto manualmente, pues GNS3 no lo hace por defecto.

!(Imagenes/Topo18.jpg)

!(Imagenes/Topo19.jpg)

!(Imagenes/Topo20.jpg)

##CREAR TOPOLOGIAS CISCO

Antes de explicar cabe aclarar que hay que agregar una imagen de un Cisco IOS Router, la referencia de esta imagen depende de cada uno.

En el botón de Routers que está en la barra de herramientas de dispositivos estará la imagen que hayas importado, ahí podrás arrastrar y soltar el Router en el espacio de trabajo, repite el proceso para sacar otro Router.

!(Imagenes/CiscoTopo1.jpg)

Para realizar la conexión entre los 2 Routers se aplica el procedimiento anteriormente dicho, solo que para efectos de este ejemplo usaremos los puertos de FastEthernet0/0. También agregamos texto y etiquetas para facilitar el entendimiento de la topología.

!(Imagenes/CiscoTopo2.jpg)

!(Imagenes/CiscoTopo3.jpg)

Clikearemos primero en el botón start y depues en el “Console connect to all devices” para poder configurar a los Routers, podemos ver las interfaces con el comando “sh ip int br” desde el modo EXEC privilegiado. Primero toca habilitar las interfaces que utilizaremos, la cual será una física y una virtual que se llamará “loopback0”, para configurarlos entraremos a la interfaz de fa0/0 desde el modo de configuración global con el siguiente comando “int fa0/0” y después pondremos este comando “ip add 10.1.1.1 255.255.255.0”, después crearemos la interfaz virtual con el siguiente comando “int loop 0” y despues con este comando “ip add 1.1.1.1 255.255.255.255”. Por último, verificamos que hicimos bien el proceso con el comando “sh ip int br”. Por último, entramos a la interfaz de fa0/0 y escribimos este comando “no shut” para abrir el puerto. Repetiremos el proceso anterior con el segundo Router solo que para el fa0/0 sera “ip add 10.1.1.2 255.255.255.0” y para el loopback0 “ip add 2.2.2.2 255.255.255.255”.

!(Imagenes/CiscoTopo4.jpg)

!(Imagenes/CiscoTopo5.jpg)

!(Imagenes/CiscoTopo6.jpg)

!(Imagenes/CiscoTopo7.jpg)

!(Imagenes/CiscoTopo8.jpg)

!(Imagenes/CiscoTopo9.jpg)

Adicionalmente con el comando “sh ver” desde el modo EXEC privilegiado podremos ver que imagen estamos utilizando.

!(Imagenes/CiscoTopo10.jpg)

Se testeará la conexión con el comando ping, podiendo la ip de fa0/0 del otro Router.

!(Imagenes/CiscoTopo11.jpg)

!(Imagenes/CiscoTopo12.jpg)

Se usará un protocolo de enrutamiento para las interfaces de loopback0, pues no hay rutas a esas interfaces. El comando es el siguiente “router ospf 1” el cual se podrá poner en el modo de configuración global, después pondremos este comando “router-id 1.1.1.1” “network 0.0.0.0 255.255.255.255 area 0”. Repetiremos el proceso con el otro router usando el comando “router-id 2.2.2.2” y “network 0.0.0.0 255.255.255.255 area 0”

!(Imagenes/CiscoTopo13.jpg)

!(Imagenes/CiscoTopo14.jpg)

Con este comando “sh ip ospf neigh” comprobaremos que hay conexión entre los routers y el estado. Con este otro comando veremos una tabla de enrutamiento “sh ip route”.

!(Imagenes/CiscoTopo15.jpg)

!(Imagenes/CiscoTopo16.jpg)

Ahora ya se puede hacer ping entre las interfaces loopback0.

!(Imagenes/CiscoTopo17.jpg)

!(Imagenes/CiscoTopo18.jpg)

Después guardaremos toda esta configuración anterior con el mondando “copy running-config startup-config” o “wr”

!(Imagenes/CiscoTopo19.jpg)