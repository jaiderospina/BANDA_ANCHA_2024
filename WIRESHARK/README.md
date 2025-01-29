**Manual Básico de Wireshark**

**¿Qué es Wireshark?**

Wireshark es un analizador de protocolos de red de código abierto ampliamente utilizado. Permite capturar y examinar el tráfico de red en tiempo real, lo que lo convierte en una herramienta invaluable para:

* **Administradores de redes:** Diagnosticar problemas de red, identificar cuellos de botella y analizar el rendimiento.
* **Profesionales de seguridad:** Detectar intrusiones, analizar malware y realizar pruebas de penetración.
* **Desarrolladores:** Depurar aplicaciones de red y comprender el comportamiento de los protocolos.
* **Estudiantes:** Aprender sobre protocolos de red y cómo funciona Internet.

**Funcionalidades Principales**

1. **Captura de paquetes:** Wireshark puede capturar paquetes de red directamente desde una interfaz de red o leer archivos de captura previamente guardados.

2. **Análisis de protocolos:** Wireshark puede decodificar y mostrar la información contenida en los paquetes de red, incluyendo encabezados de protocolos y datos de la aplicación.

3. **Filtrado de paquetes:** Wireshark permite filtrar los paquetes capturados para mostrar solo aquellos que cumplen con ciertos criterios, lo que facilita el análisis de tráfico específico.

4. **Seguimiento de conversaciones:** Wireshark puede seguir conversaciones TCP completas, mostrando los paquetes en orden cronológico y facilitando la comprensión de la comunicación entre hosts.

5. **Visualización de estadísticas:** Wireshark puede generar estadísticas sobre el tráfico de red, como el número de paquetes por protocolo, el ancho de banda utilizado y los tiempos de respuesta.

**Filtros en Wireshark**

Los filtros son una de las características más potentes de Wireshark. Permiten seleccionar y mostrar solo los paquetes que cumplen con ciertos criterios, lo que facilita el análisis de tráfico específico. Los filtros se basan en expresiones que combinan campos de protocolos, operadores lógicos y valores.

**Ejemplos de filtros:**

1. **Filtrar por dirección IP:**

   * `ip.addr == 192.168.1.100`: Muestra todos los paquetes que tienen la dirección IP 192.168.1.100 como origen o destino.
   * `ip.src == 192.168.1.100`: Muestra todos los paquetes que tienen la dirección IP 192.168.1.100 como origen.
   * `ip.dst == 192.168.1.100`: Muestra todos los paquetes que tienen la dirección IP 192.168.1.100 como destino.

2. **Filtrar por puerto:**

   * `tcp.port == 80`: Muestra todos los paquetes TCP que utilizan el puerto 80 (HTTP).
   * `udp.port == 53`: Muestra todos los paquetes UDP que utilizan el puerto 53 (DNS).

3. **Filtrar por protocolo:**

   * `http`: Muestra todos los paquetes HTTP.
   * `tcp`: Muestra todos los paquetes TCP.
   * `udp`: Muestra todos los paquetes UDP.

4. **Filtrar por tipo de paquete:**

   * `icmp`: Muestra todos los paquetes ICMP (utilizados para ping).
   * `arp`: Muestra todos los paquetes ARP (utilizados para resolver direcciones IP a direcciones MAC).

5. **Combinar filtros:**

   * `ip.src == 192.168.1.100 && tcp.port == 80`: Muestra todos los paquetes TCP que tienen la dirección IP 192.168.1.100 como origen y utilizan el puerto 80.
   * `http || dns`: Muestra todos los paquetes HTTP o DNS.

6. **Filtrar por contenido:**

   * `http contains "User-Agent"`: Muestra todos los paquetes HTTP que contienen la cadena "User-Agent" en su contenido.
   * `tcp contains "password"`: Muestra todos los paquetes TCP que contienen la cadena "password" en su contenido (¡cuidado con esto en redes reales!).

**Recomendaciones Adicionales**

* **Practica:** La mejor forma de aprender a usar Wireshark es practicar capturando y analizando tráfico de red real.
* **Documentación:** Consulta la documentación oficial de Wireshark para obtener información más detallada sobre sus funcionalidades y opciones.
* **Comunidad:** Únete a la comunidad de usuarios de Wireshark para obtener ayuda y compartir conocimientos.


