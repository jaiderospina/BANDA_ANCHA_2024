# Manual: VLAN en GNS3 usando Cisco 3725 + NM-16ESW (FASTETHERNET)

## 1\. Armado de laboratorio

- **Imagen IOS:** `c3725-adventerprisek9-mz.124-15.T.img`
- **Módulo NM-16ESW** en un slot del 3725
- **Router 3640** (o 3725) con NM-1FE-TX (mínimo una FastEthernet para trunk)
- **3 x VPCS**
- **Conexiones:**  
  - PC1 a **fa1/1**  
  - PC2 a **fa1/2**  
  - PC3 a **fa1/3**  
  - **fa1/0** (switch, trunk) a **fa0/0** (router, trunk)

***

## 2\. Creación de VLANs (prompt y comandos correctos)

**Desde prompt privilegiado:**
```
Router# vlan database
Router(vlan)# vlan 10 name ADMIN
Router(vlan)# vlan 20 name USERS
Router(vlan)# exit
Router#
```

***

## 3\. Asignación de puertos y trunk

**Desde configuración global:**
```
Router# configure terminal

Router(config)# interface fastethernet1/1
Router(config-if)# switchport mode access
Router(config-if)# switchport access vlan 10
Router(config-if)# exit

Router(config)# interface fastethernet1/2
Router(config-if)# switchport mode access
Router(config-if)# switchport access vlan 10
Router(config-if)# exit

Router(config)# interface fastethernet1/3
Router(config-if)# switchport mode access
Router(config-if)# switchport access vlan 20
Router(config-if)# exit

Router(config)# interface fastethernet1/0
Router(config-if)# switchport trunk encapsulation dot1q
Router(config-if)# switchport mode trunk
Router(config-if)# exit

Router(config)# exit
Router#
```

***

## 4\. Configuración del router de ruteo inter-VLAN (ejemplo 3640)

```
Router# configure terminal
Router(config)# interface fastethernet0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface fastethernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# no shutdown
Router(config-subif)# exit

Router(config)# interface fastethernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# no shutdown
Router(config-subif)# exit

Router(config)# exit
Router#
```

***

## 5\. Configuración de PCs (VPCS)

**Usa:**
```
VPCS> ip 192.168.10.2/24 192.168.10.1
VPCS> ip 192.168.10.3/24 192.168.10.1
VPCS> ip 192.168.20.2/24 192.168.20.1
```

***

## 6\. Verificación

**En el “switch” (3725):**
```
Router# show vlan-switch
Router# show running-config
```
**En el router:**
```
Router# show ip interface brief
Router# show ip route
```
**En VPCS:**
```
VPCS> ping [IP destino]
```

***

## 7\. Para guardar configuración

```
Router# write memory
Router# copy running-config startup-config
```

***

**Resumen:**
- El NM-16ESW siempre crea interfaces **FastEthernet1/x**
- Siempre utiliza el nombre correcto para tus comandos.




**Esquema de conexión** para el laboratorio propuesto con el Cisco 3725 + NM-16ESW y router 3640 para inter-VLAN routing:

***

## Esquema de conexión

```
            [PC1]             [PC2]              [PC3]
             |                 |                   |
        fa1/1 (VLAN10)    fa1/2 (VLAN10)    fa1/3 (VLAN20)
             \                 |                 /
              \                |                /
               \               |               /
                \              |              /
   =========================================================
   |              Cisco 3725 (NM-16ESW) "switch"           |
   |-------------------------------------------------------|
   | fa1/0 (trunk, encapsulación dot1q)                    |
   =========================================================
                         |
                         |
               fa0/0 (router 3640 trunk)
                         |
 ----------------------------------------------------------
 |           Cisco 3640 (router, subinterfaces)           |
 ----------------------------------------------------------
 | sub-interface fa0/0.10  <- VLAN10 gateway (192.168.10.1)|
 | sub-interface fa0/0.20  <- VLAN20 gateway (192.168.20.1)|
 ----------------------------------------------------------
```

- **VPCS/PC1:** Conectado al puerto fa1/1 del 3725 (VLAN 10)
- **VPCS/PC2:** Conectado al puerto fa1/2 del 3725 (VLAN 10)
- **VPCS/PC3:** Conectado al puerto fa1/3 del 3725 (VLAN 20)
- **fa1/0 del 3725:** Puerto trunk, conectado al fa0/0 del router 3640
- **fa0/0 del router 3640:** Trunk con subinterfaces dot1Q para VLAN10 y VLAN20

***

Cada línea conecta un PC simulado (VPCS) a un puerto “FastEthernet” del switch (3725+NM16ESW).  
El “trunk” entre fa1/0 del switch y fa0/0 del router transporta las VLANs hacia el router, que realiza el **ruteo inter-VLAN** por subinterfaces.

***

**Visual en GNS3:**
- Arrastra 3 VPCS y conéctalos a fa1/1, fa1/2, fa1/3 del 3725.
- Conecta fa1/0 del 3725 al fa0/0 del router.
- Configura los PCs, el switch y el router según el manual.


# Protocolo de pruebas para laboratorio VLAN GNS3 (Cisco 3725 + NM-16ESW + router 3640)

Este protocolo ayuda a **verificar paso a paso** la funcionalidad correcta dEL laboratorio, asegurando el aislamiento por VLAN, el ruteo inter-VLAN, y la conectividad adecuada.

***

## **1. Verificación de VLANs en el switch (Cisco 3725)**

**Comando:**
```
Router# show vlan-switch
```
**Resultado esperado:**
- Debes ver listadas al menos:
  - VLAN 1 (default)
  - VLAN 10 (ADMIN)
  - VLAN 20 (USERS)
- Los puertos fa1/1 y fa1/2 deben salir asignados a VLAN 10, fa1/3 a VLAN 20.

***

## **2. Verificar asignación de interfaces y modo trunk**

**Comando:**
```
Router# show interfaces trunk
```
**Resultado esperado:**
- El puerto **fa1/0** debe estar en modo trunk.
- Debe indicar que transporta VLANs 10 y 20.

***

## **3. Verificar subinterfaces y estado en el router (3640)**

**Comando:**
```
Router# show ip interface brief
```
**Resultado esperado:**
- Interfaces **fa0/0.10** y **fa0/0.20** están "up" y con IP configurada (192.168.10.1 y 192.168.20.1).
- Fa0/0 "up".

***

## **4. Prueba de conectividad intra-VLAN**

**Comando en VPCS de PC1 y PC2 (misma VLAN):**
```
VPCS> ping 192.168.10.3
```
**Resultado esperado:**
- Respuesta exitosa (reply) entre ambos hosts de VLAN 10.

***

## **5. Prueba de conectividad inter-VLAN**

**Comando en VPCS de PC3 (VLAN 20):**
```
VPCS> ping 192.168.10.2
```
**Resultado esperado:**
- **Reply exitoso** si el ruteo inter-VLAN está bien configurado.
- Si hay reply, el router 3640 está realizando ruteo correctamente.
- Si no hay reply, revisar configuración/subinterfaces, gateway o reglas ACL.

***

## **6. Prueba de acceso a Gateway**

**Comando en todos los VPCS:**
```
VPCS> ping [Gateway correspondiente]
```
Ejemplo:
- PC1/PC2: `ping 192.168.10.1`
- PC3: `ping 192.168.20.1`

**Resultado esperado:**
- Todos los PCs reciben reply del gateway correspondiente.

***

## **7. Verificar tabla de rutas en el router**

**Comando:**
```
Router# show ip route
```
**Resultado esperado:**
- Debe mostrar rutas conectadas para:
  - 192.168.10.0/24 (VLAN 10)
  - 192.168.20.0/24 (VLAN 20)

***

## **8. Probar aislamiento de VLANs**

**Desconecta** el trunk entre switch y router.
**Comando en VPCS:**
```
VPCS> ping [PC en otra VLAN]
```
**Resultado esperado:**
- Sin trunk, los paquetes entre VLANs no pasan; solo hay conectividad entre PCs de la misma VLAN.

***

## **9. Protocolo final de guardado**

**Comando en ambos routers:**
```
Router# write memory
```
**Resultado esperado:**
- Confirmación “Building configuration…[OK]”, indicando que las configuraciones han sido guardadas.

***

## **Resumido (tabla de chequeo):**

| Prueba                     | Comando                       | Resultado esperado         |
|----------------------------|-------------------------------|---------------------------|
| Listar VLANs               | show vlan-switch              | VLANs 10/20 creadas       |
| Ver trunk                  | show interfaces trunk         | fa1/0 en trunk            |
| SubInterfaces router       | show ip interface brief       | up/up con IP              |
| Ping intra-VLAN            | ping 192.168.10.x             | Reply OK                  |
| Ping inter-VLAN            | ping 192.168.20.x             | Reply OK (si ruteo activo)|
| Ping gateway               | ping [gateway]                | Reply OK                  |
| Rutas router               | show ip route                 | Conn. a ambas VLANs       |
| Guardar config             | write memory                  | [OK]                      |

***

