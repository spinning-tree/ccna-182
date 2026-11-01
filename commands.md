# comandos básicos

| **comando**               | **notas / explicación**                                               |
| ------------------------- | --------------------------------------------------------------------- |
| **?**                     | muestra las posibles opciones del comando                             |
| **enable**                | pasa al **privileged exec mode**                                      |
| **configure terminal**    | entra al **global configuration mode**                                |
| **no** _command_          | deshace o elimina la configuración especificada                       |
| **do** _command_          | ejecuta comandos de modo exec desde el modo de configuración          |
| **disable**               | sale del modo privilegiado y vuelve al **user exec mode** (prompt >)  |
| **exit**                  | retrocede **un solo nivel** en la jerarquía de menús de configuración |
| **end**                   | sale al **privileged exec mode** (#) desde cualquier nivel de config  |
| **erase startup-config**  | borra la configuración de inicio en la nvram. requiere **reload**     |
| **delete flash:vlan.dat** | elimina la base de datos de vlans de la memoria flash                 |
| **reload**                | reinicia físicamente el dispositivo                                   |
# include

| **comando (filtro)** | **qué hace exactamente**                                                                                                                  |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **`\| include`**     | muestra **solo** las líneas que contienen la palabra. ejemplo: `show run \| include ip address` (ver todas las ips rápido)                |
| **`\| exclude`**     | esconde las líneas que contienen la palabra. ejemplo:`show ip int br \| exclude unassigned` (ver solo interfaces con ip)                  |
| **`\| begin`**       | salta todo el texto y empieza a mostrar **desde** esa palabra. ejemplo: `show run \| begin router ospf` (ir directo a la sección de ospf) |
# consulta básica

| **comando**                                    | **acción / qué verificar**                                                                         |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **show ip interface brief**                    | estado rápido de **capa 3:** muestra ips, _status_ (l1/físico) y _protocol_ (l2/enlace).           |
| **show interfaces status**                     | estado rápido de **capa 2 y físico:** muestra vlan assignada, dúplex, velocidad y tipo de puerto.  |
| **show interfaces**                            | estadísticas detalladas de **todas** las interfaces (paquetes, mtu, ancho de banda y errores).     |
| **show interfaces** _interface_                | estadísticas detalladas de una interfaz específica (útil para revisar contadores de errores).      |
| **show interfaces description**                | listado rápido de todas las interfaces junto a su descripción asignada, _status_ y _protocol_.     |
| **show running-config**                        | muestra la configuración activa en este momento en la **ram**.                                     |
| **show startup-config**                        | muestra la configuración de respaldo guardada en la **nvram** que se cargará al reiniciar.         |
| **show ip route**                              | muestra la **tabla de enrutamiento** (redes conocidas, códigos de origen, métricas y _next hop_).  |
| **show interfaces** _interface_ **switchport** | estado detallado de **capa 2** del puerto (modo operativo, vlan nativa, estado de dtp y trunking). |
| **show version**                               | muestra detalles del vios. versión, etc.                                                           |
| **show file systems**                          | muestra todos los ifs y sus prefijos                                                               |
| **show flash**                                 | muestra el contenido de la flash (.bin)                                                            |
# config t 

| **comando**                                             | **acción / notas de configuración**                                                                            |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **hostname** _name_                                     | cambia el nombre identificativo del dispositivo.                                                               |
| **interface** _interface_                               | entra al modo de configuración de una interfaz específica.                                                     |
| **interface range** _interface_ **-** _interface_       | configura múltiples interfaces consecutivas en bloque                                                          |
| **interface range** _interface_**,** _interface_        | configura múltiples interfaces separadas por comas                                                             |
| **speed** { **10** \| **100** \| **1000** \| **auto** } | fuerza la velocidad del puerto. desactivar el **auto** en un extremo puede causar fallo de dúplex.             |
| **duplex** { **auto** \| **half** \| **full** }         | configura el modo dúplex. si hay discordancia (_mismatch_), el puerto sufre colisiones y lentitud.             |
| **ip address** _address_ _mask_                         | asigna una dirección ip estática y su máscara de subred a la interfaz.                                         |
| **description** _text_                                  | añade una etiqueta descriptiva al puerto (máximo 240 caracteres) para identificar qué hay conectado.           |
| **copy running-config startup-config**                  | guarda la configuración activa de la ram a la nvram (equivalente a **write** o **write memory**).              |
| **default interface** _interface_                       | revierte por completo la configuración de un puerto a sus valores de fábrica.                                  |
| **banner motd** _d_ _message_ _d_                       | configura el mensaje de advertencia del día entre delimitadores coincidentes                                   |
| **ip mtu** _68-1500_                                    | modifica el tamaño máximo de la unidad de transmisión de capa 3 en bytes para esa interfaz.                    |
| **password** _clave_                                    | define la contraseña de la línea actual (consola/vty). **requiere** ejecutar **login** después para activarla. |
# mac addres-table

| **comando**                                               | **acción / qué verifica**                                                                         |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **show mac address-table**                                | muestra la tabla cam completa (direcciones mac, vlans asociadas y puertos físicos).               |
| **show mac address-table dynamic**                        | filtra la tabla para mostrar **solo** las direcciones aprendidas de forma automática.             |
| **show mac address-table interface** _interface_          | muestra únicamente las direcciones mac registradas detrás de un puerto específico.                |
| **show mac address-table address** _mac-address_          | rastrea y localiza el puerto exacto al que está asociada una dirección mac concreta.              |
| **clear mac address-table dynamic**                       | vacía las entradas dinámicas de la tabla cam, forzando al switch a re-aprender el tráfico.        |
| **clear mac address-table dynamic interface** _interface_ | borra las macs dinámicas registradas **solo** en esa interfaz en particular.                      |
| **mac address-table aging-time** _seconds_                | modifica el tiempo de expiración de las entradas dinámicas (por defecto dura **300** segundos).   |
| **show mac address-table count**                          | muestra el número total de direcciones mac activas por vlan y la capacidad disponible del switch. |
| **show arp**                                              | muestra la arp caché de un router (no tienen tabla mac)                                           |
# vlan

### configuración y verificación de vlans

| **comando**                          | **acción / notas de configuración**                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| **show vlan brief**                  | muestra todas las vlans existentes en el switch, su estado y los puertos de acceso asignados.                |
| **vlan** _vlan-id_                   | accede al modo de configuración de la vlan especificada (la crea automáticamente si no existe).              |
| **name** _vlan-name_                 | asigna un nombre identificativo a la vlan dentro de su modo de configuración.                                |
| **switchport mode access**           | configura el puerto para operar exclusivamente en modo de acceso (capa 2 para un único host).                |
| **switchport access vlan** _vlan-id_ | asocia el puerto de acceso a una vlan específica.                                                            |
| **interface vlan** _vlan-id_         | entra al modo de configuración de la interfaz virtual del switch (**svi**) para asignarle una ip de gestión. |
| **shutdown**                         | apaga administrativamente la vlan o svi seleccionada (desactiva el tráfico en ella).                         |
### configuración y verificación de trunking (enlaces troncales)

| **comando**                                                                                         | **acción / notas de configuración**                                                                                                     |
| --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **switchport mode trunk**                                                                           | configura la interfaz en modo troncal para transportar múltiples vlans concurrentemente.                                                |
| **switchport trunk encapsulation dot1q**                                                            | define el estándar de encapsulación **802.1q**. obligatorio en switches antiguos antes de activar el modo trunk.                        |
| **show interfaces trunk**                                                                           | muestra los troncales activos, su modo de negociación, encapsulación, vlan nativa y vlans permitidas.                                   |
| **switchport trunk allowed vlan** { _vlan-list_ \| **all** \| **add** \| **remove** \| **except** } | filtra qué vlans pueden pasar por el trunk. **nota:** usa **add** o **remove** para no machacar la lista existente (ej: **add 10,20**). |
| **switchport trunk native vlan** _vlan-id_                                                          | cambia la vlan nativa del troncal (tráfico sin etiquetar). **debe coincidir en ambos extremos** para evitar _mismatches_.               |
### router-on-a-stick

| **comando**                                         | **acción / notas de configuración**                                                                                                                |
| --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **interface** _interface_**.**_subinterface-number_ | crea o accede a una **subinterfaz** lógica (ej: **g0/0.10**). la interfaz física base debe estar encendida con **no shutdown** y sin dirección ip. |
| **encapsulation dot1q** _vlan-id_                   | define el etiquetado estándar **802.1q** y asocia la subinterfaz a esa vlan específica. **obligatorio** antes de poder asignar la ip.              |
| **ip address** _ip-address_ _mask_                  | asigna la dirección ip y máscara de subred a la subinterfaz. actuará como el _default gateway_ de los hosts en esa vlan.                           |
| **encapsulation dot1q** _vlan-id_ **native**        | especifica qué vlan procesará el tráfico sin etiquetar (_native vlan_) en el router. debe coincidir con la configuración del switch.               |
|                                                     |                                                                                                                                                    |
## svi

### svi y gestión en switches capa 2

| **comando**                             | **acción / notas de configuración**                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **interface vlan** _vlan-id_            | **modo de configuración global.** selecciona la interfaz virtual de conmutador (svi) destinada a la gestión ip del dispositivo de capa 2. por defecto de fábrica se utiliza la interfaz vlan 1. la vlan correspondiente debe existir en la base de datos de vlans y debe tener al menos un puerto físico activo (en modo de acceso o tronco) para que la interfaz cambie a estado operativo (_up/up_). viene desactivada por defecto de forma administrativa (_shutdown_). |
| **ip address** _ip-address subnet-mask_ | **modo de configuración de interfaz.** asigna una dirección ip y su correspondiente máscara de subred a la interfaz svi seleccionada para dotar al switch de identidad en la red. requiere el comando `no shutdown` para habilitarla administrativamente.                                                                                                                                                                                                                  |
| **ip default-gateway** _ip-address_     | **modo de configuración global.** define la dirección ip del router de próximo salto para el switch. comando indispensable en switches de capa 2 para permitir que el tráfico local generado por el propio dispositivo (como respuestas ssh, peticiones ntp o syslog) pueda salir hacia redes o subredes remotas. **no se configura dentro del submodo de interfaz.**                                                                                                      |
### svi y enrutamiento en switches capa 3 (multicapa)

| **comando**                  | **acción / notas de configuración**                                                                                                                                                                                                                                                                                                                              |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ip routing**               | **modo de configuración global.** habilita el enrutamiento de paquetes ip de capa 3 por hardware en el switch multicapa. sin este comando, el dispositivo funciona únicamente como un switch de capa 2 clásico y no procesará el tráfico entre diferentes subredes.                                                                                              |
| **interface vlan** _vlan-id_ | **modo de configuración global.** configura la interfaz svi que actuará como la puerta de enlace predeterminada (_default gateway_) de los hosts finales asignados a esa vlan específica, permitiendo el enrutamiento inter-vlan. requiere la asignación de una dirección ip mediante el comando `ip address` y que la vlan esté asociada a algún puerto activo. |
| **no switchport**            | **modo de configuración de interfaz.** desactiva las funciones de capa 2 en una interfaz física de un switch multicapa, convirtiéndola en un puerto enrutado (_routed port_). Se aplica en las interfaces antes de agruparlas en port-channel                                                                                                                    |
# vtp

| **comando**                                                             | **acción / notas de configuración**                                                                                                                                                                    |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **show vtp status**                                                     | comando clave de verificación. muestra la versión, el dominio, el modo operativo y el **número de revisión de configuración** (crucial para evitar desastres).                                         |
| **vtp domain** _domain-name_                                            | define el nombre del dominio vtp. los switches deben compartir el mismo dominio para sincronizarse.                                                                                                    |
| **vtp password** _password_                                             | configura la contraseña de autenticación para el dominio vtp.                                                                                                                                          |
| **vtp mode** { **server** \| **client** \| **transparent** \| **off** } | configura el modo operativo del switch: **server** (crea/modifica vlans), **client** (solo aprende), **transparent** (solo pasa tráfico vtp) u **off** (desactivado por completo, exclusivo de vtpv3). |
| **vtp version** { **1** \| **2** \| **3** }                             | cambia la versión global de vtp en el dispositivo. por defecto suele operar en la versión 1 o 2.                                                                                                       |
# dtp

| **comando**                                    | **acción / notas de configuración**                                                                                                                                         |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show interfaces** _interface_ **switchport** | comando clave para verificar el estado de dtp (modo administrativo, modo operativo y estado de la negociación).                                                             |
| **switchport mode dynamic desirable**          | **negociación activa:** el puerto inicia activamente mensajes dtp para intentar convertirse en un enlace troncal (_trunk_).                                                 |
| **switchport mode dynamic auto**               | **negociación pasiva:** el puerto escucha mensajes dtp; solo se convierte en troncal si el otro extremo se lo pide.                                                         |
| **switchport mode trunk**                      | fuerza al puerto a actuar como enlace troncal, pero **sigue enviando tramas dtp** al extremo opuesto.                                                                       |
| **switchport mode access**                     | fuerza al puerto a actuar como acceso, pero **sigue enviando tramas dtp** al extremo opuesto.                                                                               |
| **switchport nonegotiate**                     | **desactiva dtp por completo.** evita que el puerto envíe o procese tramas dtp. requiere configurar previamente el puerto de forma estática en modo **trunk** o **access**. |
# stp

### verificación de stp

| **comando**                                             | **acción / qué verifica**                                                                                                                            |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show spanning-tree**                                  | muestra el estado global de stp: prioridad y dirección mac del root bridge, coste total y rol de cada interfaz local (**desg**, **root**, **altn**). |
| **show spanning-tree summary**                          | muestra un resumen rápido con el conteo de puertos locales por estado (**blocking**, **listening**, **learning**, **forwarding**).                   |
| **show spanning-tree detail**                           | muestra información detallada de topología, incluyendo temporizadores (_hello_, _max age_, _forward delay_) y cambios de estado.                     |
| **show spanning-tree interface** _interface_ **detail** | muestra contadores detallados de tramas bpdu enviadas/recibidas y transiciones específicas de un puerto.                                             |
### configuración de stp

| **comando**                                                      | **acción / notas de configuración**                                                                                                                            |
| ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **spanning-tree mode** { **mst** \| **pvst** \| **rapid-pvst** } | define el sabor de stp a ejecutar globalmente. **rapid-pvst+** es el recomendado para convergencia rápida en cisco.                                            |
| **spanning-tree vlan** _vlan-id_ **priority** _0-61440_          | configura manualmente la prioridad del bridge id (bid). **solo acepta múltiplos de 4096** (ej: 4096, 8192, 32768).                                             |
| **spanning-tree vlan** _vlan-id_ **root primary**                | macro de cisco: reduce de golpe la prioridad del switch a **24576** (o 4096 menos que el root bridge actual) para forzarlo a ser la raíz.                      |
| **spanning-tree vlan** _vlan-id_ **root secondary**              | macro de cisco: fija la prioridad en **28672** para designar este equipo como respaldo en caso de que falle el root bridge primario.                           |
| **spanning-tree vlan** _vlan-id_ **cost** _1-200000000_          | modifica manualmente el coste del puerto para esa vlan, alterando la elección del root port.                                                                   |
| **spanning-tree vlan** _vlan-id_ **port-priority** _0-240_       | en interfaz, cambia la prioridad del puerto para desempatar interfaces paralelas que conectan al mismo switch vecino. **solo múltiplos de 16** (defecto: 128). |
## portfast

| **comando**                        | **acción / notas de configuración**                                                                                                                                                                                  |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **spanning-tree portfast**         | _(modo interfaz)_ activa portfast en un puerto de acceso para saltar los estados intermedios de stp y pasar directo a _forwarding_. **solo usar en puertos conectados a hosts finales.**                             |
| **spanning-tree portfast disable** | _(modo interfaz)_ desactiva portfast explícitamente en una interfaz específica.                                                                                                                                      |
| **spanning-tree portfast default** | _(modo global)_ activa de golpe portfast en **todos** los puertos que operen en modo de acceso en el switch.                                                                                                         |
| **spanning-tree portfast trunk**   | _(modo interfaz)_ fuerza la activación de portfast en un puerto troncal. **¡cuidado!** solo debe usarse si conecta a un router en router-on-a-stick (roas) o a servidores de virtualización, nunca a otros switches. |
## bpdu guard

| **comando**                                  | **acción / notas de configuración**                                                                                                                                      |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **spanning-tree bpduguard enable**           | _(modo interfaz)_ activa bpdu guard de forma explícita. si el puerto recibe una trama bpdu, pasa inmediatamente a estado de error (**err-disabled**) para evitar bucles. |
| **spanning-tree bpduguard disable**          | _(modo interfaz)_ desactiva por completo bpdu guard en esa interfaz específica.                                                                                          |
| **spanning-tree portfast bpduguard default** | _(modo global)_ habilita automáticamente bpdu guard en **todos** los puertos del switch que tengan portfast activo por defecto.                                          |
## bpdu filter

| **comando**                                   | **acción / notas de configuración**                                                                                                                                                                                                                    |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **spanning-tree bpdufilter enable**           | _(modo interfaz)_ activa bpdu filter de forma explícita. el puerto **deja de enviar y de procesar** tramas bpdu por completo, ignorando eficazmente stp en esa interfaz.                                                                               |
| **spanning-tree bpdufilter disable**          | _(modo interfaz)_ desactiva por completo bpdu filter en esa interfaz específica.                                                                                                                                                                       |
| **spanning-tree portfast bpdufilter default** | _(modo global)_ habilita bpdu filter en todos los puertos con portfast activo. **comportamiento clave:** deja de enviar bpdus, pero si llega a recibir una, el puerto pierde el estado portfast, desactiva el filtro y vuelve a operar con stp normal. |
## guard root

| **comando**                  | **acción / notas de configuración**                                                                                                                                                                                  |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **spanning-tree guard root** | _(modo interfaz)_ evita que un switch vecino se convierta en root bridge a través de este puerto. si recibe una bpdu superior, pone el puerto en estado _root-inconsistent_ (bloqueado) hasta que deje de recibirla. |
| **spanning-tree guard none** | _(modo interfaz)_ desactiva explícitamente tanto **root guard** como **loop guard** en la interfaz, volviendo al comportamiento stp por defecto.                                                                     |
## loop guard

| **comando**                         | **acción / notas de configuración**                                                                                                                                                                                                        |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **spanning-tree loopguard default** | _(modo global)_ activa automáticamente loop guard en **todos** los puertos del switch que operen como enlaces punto a punto.                                                                                                               |
| **spanning-tree guard loop**        | _(modo interfaz)_ protege contra enlaces unidireccionales. si un puerto bloqueado (_alternate/backup_) deja de recibir bpdus, en lugar de pasar erróneamente a _forwarding_, loop guard lo mueve a estado _loop-inconsistent_ (bloqueado). |
| **spanning-tree guard none**        | desactiva loop guard y root en una interfaz determinada                                                                                                                                                                                    |
## errdisable recovery

| **comando**                                          | **acción / notas de configuración**                                                                                                                                        |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show errdisable recovery**                         | _(modo exec)_ muestra qué causas de error tienen activada la recuperación automática, los puertos actualmente caídos y el tiempo restante del temporizador.                |
| **errdisable recovery cause** { _cause_ \| **all** } | _(modo global)_ habilita la recuperación automática para un motivo específico (ej: **bpduguard**, **loopback**, **port-security**) o para todas de golpe con **all**.      |
| **errdisable recovery interval** _30-86400_          | _(modo global)_ cambia el tiempo de espera en segundos que aguarda el switch antes de intentar reactivar el puerto de forma automática (por defecto son **300** segundos). |
## rpvst

| **comando**                                | **acción / notas de configuración**                                                                                                                                                                       |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **spanning-tree portfast**                 | _(modo interfaz)_ define el puerto como _edge port_ (puerto de borde) en rstp. su comportamiento es idéntico al portfast tradicional: transiciona de inmediato al estado _forwarding_.                    |
| **spanning-tree link-type point-to-point** | _(modo interfaz)_ fuerza al puerto a operar como un enlace punto a punto. rstp solo realiza su convergencia rápida de milisegundos (propuesta/acuerdo) en interfaces con este tipo de enlace full-duplex. |
# etherchannel

### verificación de etherchannel

| **comando**                        | **acción / qué verifica**                                                                                                                                               |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show etherchannel summary**      | muestra el estado rápido del grupo: protocolo usado, interfaces miembro y las flags clave (**s**/**r** para capa 2/3, y **u**/**p** para "en uso" / "en port-channel"). |
| **show etherchannel port-channel** | muestra estadísticas detalladas del grupo, incluyendo el modo operativo preciso y el tiempo de actividad del enlace.                                                    |
| **show etherchannel load-balance** | muestra el algoritmo actual activo que decide cómo se reparte el tráfico entre los cables del grupo.                                                                    |
### configuración de etherchannel

| **comando**                                                                                              | **acción / notas de configuración**                                                                                                                                                                                                                                                                                     |
| -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **channel-group** _number_ **mode** { **active** \| **passive** \| **desirable** \| **auto** \| **on** } | _(modo interfaz)_ agrupa los puertos físicos. configura **lacp** (**active**/**passive**), **pagp** (**desirable**/**auto**) o estático sin protocolo (**on**).<br><br>  <br><br>**nota capa 3:** ejecuta primero **no switchport** en los puertos físicos antes de agruparlos; luego pon la ip en la interfaz virtual. |
| **interface port-channel** _number_                                                                      | _(modo global)_ entra a configurar la interfaz lógica del grupo. cualquier cambio aquí (como **switchport mode trunk**) se hereda automáticamente en los puertos físicos.                                                                                                                                               |
| **port-channel load-balance** _method_                                                                   | _(modo global)_ cambia el criterio de balanceo de carga basándose en variables como mac o ip (ej: **src-dst-ip**, **src-mac**, **dst-ip**).                                                                                                                                                                             |
| **channel-protocol** { **lacp** \| **pagp** }                                                            | _(modo interfaz)_ fuerza el protocolo. **no se suele usar:** el switch deduce el protocolo automáticamente al configurar el comando **channel-group mode**.                                                                                                                                                             |
# cdp

### verificación de cdp

| **comando**                                 | **acción / qué verifica**                                                                                                                            |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show cdp**                                | muestra el intervalo de envío de tramas (_hello timer_), el tiempo de retención (_holdtime_) y la versión activa globalmente.                        |
| **show cdp traffic**                        | muestra contadores detallados de paquetes cdp enviados, recibidos y errores (paquetes corruptos, etc.).                                              |
| **show cdp interface** [ _interface_ ]      | muestra el estado de cdp, la encapsulación y los temporizadores específicos de las interfaces del equipo.                                            |
| **show cdp entry** { _*_ \| _device-name_ } | muestra exactamente el mismo nivel de detalle que el comando anterior, pero filtrando por un dispositivo específico (o todos usando *****).          |
| **show cdp neighbors**                      | muestra la tabla rápida de vecinos: id del dispositivo, interfaz local, _holdtime_, capacidades (**r**, **s**, **i**), plataforma e interfaz remota. |
| **show cdp neighbors detail**               | muestra toda la información de los vecinos incluyendo direcciones ip, versión de cisco ios, dúplex y la vlan nativa.                                 |
### configuración de cdp

|**comando**|**acción / notas de configuración**|
|---|---|
|**cdp run**|_(modo global)_ activa cdp en todo el dispositivo (habilitado por defecto). usa **no cdp run** para apagarlo por completo.|
|**cdp enable**|_(modo interfaz)_ activa cdp en una interfaz específica. usa **no cdp enable** en puertos orientados a internet o usuarios finales por seguridad.|
|**cdp timer** _5-254_|_(modo global)_ configura la frecuencia en segundos con la que el dispositivo envía actualizaciones cdp (por defecto son **60** segundos).|
|**cdp holdtime** _10-255_|_(modo global)_ configura el tiempo en segundos que el switch vecino mantendrá la información de este equipo antes de borrarla (por defecto **180** segundos).|
|**cdp advertise-v2**|_(modo global)_ fuerza el uso de la versión 2 de cdp (activa por defecto). usa **no cdp advertise-v2** para degradar a la versión 1.|
# lldp

### verificación de lldp

| **comando**                          | **acción / qué verifica**                                                                                                                          |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show lldp**                        | muestra si lldp está activo globalmente, junto con la frecuencia de envío (_timer_), retención (_holdtime_) y tiempo de inicialización (_reinit_). |
| **show lldp traffic**                | muestra el contador total de tramas lldp enviadas, recibidas, descartadas o con errores.                                                           |
| **show lldp interface**  _interface_ | muestra si la transmisión (**tx**) y recepción (**rx**) están habilitadas en cada interfaz de forma individual.                                    |
| **show lldp entry** _device-name_    | muestra la información detallada completa filtrando específicamente por el nombre de un vecino en particular.                                      |
| **show lldp neighbors**              | muestra la tabla rápida de vecinos estándar: id del chasis, interfaz local, _holdtime_, capacidades y el id del puerto remoto.                     |
| **show lldp neighbors detail**       | muestra la información extendida de todos los vecinos: direcciones ip de gestión, descripción del sistema, capacidades de red y vlan nativa.       |
### configuración de lldp

| **comando**                 | **acción / notas de configuración**                                                                                                                                                                    |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **lldp run**                | _(modo global)_ activa lldp en todo el dispositivo. **nota:** a diferencia de cdp, lldp suele venir desactivado por defecto en muchos equipos cisco. usa **no lldp run** para apagarlo.                |
| **lldp transmit**           | _(modo interfaz)_ habilita explícitamente el envío de tramas lldp en esa interfaz.                                                                                                                     |
| **lldp receive**            | _(modo interfaz)_ habilita explícitamente el procesamiento y recepción de tramas lldp en esa interfaz.                                                                                                 |
| **lldp timer** _5-65535_    | _(modo global)_ configura el intervalo de transmisión en segundos para las actualizaciones de lldp (por defecto son **30** segundos).                                                                  |
| **lldp holdtime** _0-65535_ | _(modo global)_ configura el tiempo de retención en segundos que los vecinos guardarán la información de este equipo (por defecto son **120** segundos).                                               |
| **lldp reinit** _1-10_      | _(modo global)_ define el tiempo de espera en segundos para la inicialización de lldp en una interfaz tras un cambio de estado antes de volver a intentar transmitir (por defecto son **2** segundos). |
# routing

## static routing

| **comando**                                                                | **acción / qué hace exactamente**                                                                                             |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **show ip route**                                                          | muestra la **tabla de enrutamiento** (códigos de origen, redes, métricas y _next hop_).                                       |
| **show ip route** [ _code_ ]                                               | filtra la tabla por origen de la ruta (ej: **connected**, **static**, **ospf**, **bgp**).                                     |
| **ip route** _network_ _mask_ { _next-hop_ \| _interface_ } [ _distance_ ] | configura una **ruta estática**. permite definir ip del siguiente salto, interfaz de salida y distancia administrativa (ad).  |
| **ip route 0.0.0.0 0.0.0.0** { _next-hop_ \| _interface_ }                 | configura la **ruta por defecto** (_gateway of last resort_) para enviar el tráfico sin coincidencia específica.              |
| **traceroute** _address_                                                   | rastrea la ruta de los paquetes mediante sondas udp de puerto alto (a diferencia del icmp de windows).                        |
| **traceroute**                                                             | inicia el modo interactivo en la cli para personalizar parámetros del rastreo (ip origen, timeouts, etc.).                    |
| **clear ip route ***                                                       | vacía temporalmente la tabla de enrutamiento dinámico sin romper vecindades ni borrar bases de datos (lsdb).                  |
| **interface loopback** _number_                                            | crea una interfaz lógica virtual. **nota:** en routers, su ip se configura típicamente con máscara **255.255.255.255** (/32). |
| **ip default-gateway** ip_address                                          | gateway para tráfico originado por el dispositivo cuando `ip routing` está deshabilitado.                                     |
## osfp 

### verificación ospf

| **comando**                                | **acción / qué verifica**                                                                                              |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| **show ip protocols**                      | muestra parámetros globales del protocolo activo (id del router, temporizadores, redes y filtros).                     |
| **show ip ospf neighbor**                  | lista los vecinos ospf, su estado de adyacencia (**full**, **2-way**), prioridad y rol (**dr**, **bdr**, **drother**). |
| **show ip ospf interface** [ _interface_ ] | muestra detalles ospf del puerto: tipo de red, costo, intervalos hello/dead y estado del vecino.                       |
| **show ip ospf interface brief**           | vista simplificada de interfaces en ospf: área, costo, estado y número de vecinos.                                     |
| **show ip ospf database**                  | muestra la base de datos de estado de enlace (**lsdb**) con los lsa conocidos en el área.                              |
### configuración ospf global

| **comando**                                                 | **acción / notas de configuración**                                                                                                    |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **router ospf** _process-id_                                | activa el proceso ospf localmente. el id de proceso es de significado local y no requiere coincidir con los vecinos.                   |
| **network** _ip-address_ _wildcard-mask_ **area** _area-id_ | activa ospf en las interfaces que coincidan con ese rango y las anuncia.                                                               |
| **network 0.0.0.0 255.255.255.255 area** _area-id_          | atajo para habilitar ospf en absolutamente todas las interfaces activas del router.                                                    |
| **passive-interface** _interface_                           | evita el envío/recepción de paquetes hello por esa interfaz; no forma vecindades pero sí anuncia la red.                               |
| **passive-interface default**                               | configura todas las interfaces como pasivas por defecto. se revierte de forma individual con **no passive-interface**.                 |
| **default-information originate**                           | inyecta y propaga una ruta por defecto (0.0.0.0/0) en el dominio ospf. convierte al router en **asbr**.                                |
| **maximum-paths** _number_                                  | define el número máximo de rutas con el mismo costo para balanceo de carga (**ecmp**).                                                 |
| **distance** _number_                                       | modifica la distancia administrativa (ad) de ospf (por defecto es **110**) en este equipo.                                             |
| **router-id** _ip-address_                                  | define manualmente el id del router (rid). requiere **clear ip ospf process** para aplicarse.                                          |
| **clear ip ospf process**                                   | reinicia el proceso ospf. fuerza la reconstrucción de adyacencias e intercambio de la lsdb.                                            |
| **auto-cost reference-bandwidth** _megabits-per-second_     | cambia la banda ancha de referencia para el cálculo de costos. obligatorio en redes mayores a 100 mbps (ej: **100000** para 100 gbps). |
### configuración ospf en interfaz

| **comando**                                                 | **acción / notas de configuración**                                                                    |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **ip ospf** _process-id_ **area** _area-id_                 | habilita ospf directamente bajo la interfaz física o subinterfaz.                                      |
| **ip ospf cost** _value_                                    | modifica manualmente el costo ospf de la interfaz, ignorando el cálculo automático por ancho de banda. |
| **ip ospf hello-interval** _seconds_                        | cambia el temporizador de hello. **debe coincidir con el vecino** para formar adyacencia.              |
| **ip ospf dead-interval** _seconds_                         | cambia el temporizador dead (por defecto $4 \times \text{hello}$). **debe coincidir con el vecino**.   |
| **ip ospf network** { **broadcast** \| **point-to-point** } | define el tipo de red ospf. cambia el comportamiento de las elecciones de dr/bdr y los temporizadores. |
| **ip ospf priority** _0-255_                                | configura la prioridad para la elección de dr/bdr. el valor **0** excluye al router de la elección.    |
### configuración ospf de enlaces seriales / p2p legacy

| **comando**                               | **acción / notas de configuración**                                                                             |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **clock rate** _bits-per-second_          | se configura en el extremo **dce** del cable serial para dictar la velocidad de sincronización (ej: **64000**). |
| **encapsulation** { **hdlc** \| **ppp** } | configura el protocolo de enlace de capa 2. debe ser idéntico en ambos extremos. hdlc es el defecto de cisco.   |
| **show controllers**                      | comando de verificación para identificar si la interfaz local actúa como cable **dce** o **dte**.               |
### autenticación ospf

| **comando**                                                | **acción / notas de configuración**                                                                                   |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **area** _area-id_ **authentication**                      | activa la autenticación en texto plano (tipo 1) para toda el área. vulnerable.                                        |
| **ip ospf authentication-key** _password_                  | configura la contraseña en texto plano en la interfaz correspondiente.                                                |
| **area** _area-id_ **authentication message-digest**       | activa la autenticación segura por hash **md5** (tipo 2) a nivel de área global.                                      |
| **ip ospf authentication message-digest**                  | habilita la autenticación por hash md5 localmente en esa interfaz específica.                                         |
| **ip ospf message-digest-key** _key-id_ **md5** _password_ | configura la clave md5 y su contraseña en la interfaz. el _key-id_ y la contraseña deben coincidir en ambos extremos. |
## rip

| **comando**                           | **acción / notas de configuración**                                                                                                                                         |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show ip protocols**                 | el comando clave para verificar de un vistazo los temporizadores de rip, redes declaradas, interfaces pasivas y filtros.                                                    |
| **show ip rip database**              | muestra el mapa completo de redes conocidas por rip, incluyendo aquellas ocultas o con peor métrica que la ruta activa.                                                     |
| **router rip**                        | activa el proceso global del protocolo rip.                                                                                                                                 |
| **version 2**                         | habilita ripv2 para soportar direccionamiento sin clase (**cidr** / **vlsm**), a diferencia de v1 (_classful_).                                                             |
| **no auto-summary**                   | desactiva la sumarización automática en las fronteras de red principal, permitiendo anunciar las subredes reales.                                                           |
| **network** _classful-network-number_ | activa rip en las interfaces que pertenezcan a esa red con clase. **nota:** se ingresa la red clásica (ej: 10.0.0.0), pero v2 enviará la máscara específica de la interfaz. |
| **passive-interface** _interface_     | detiene el envío de actualizaciones periódicas por esa interfaz (ahorra ancho de banda y cpu), pero sigue anunciando su red.                                                |
| **default-information originate**     | genera e inyecta de forma dinámica una ruta por defecto (0.0.0.0/0) hacia los demás routers del dominio rip.                                                                |
| **maximum-paths** _1-32_              | define la cantidad máxima de rutas con igual costo para balanceo de carga (**ecmp**). por defecto son 4.                                                                    |
| **distance** _number_                 | modifica la distancia administrativa (ad) de rip (por defecto es **120**) para este dispositivo.                                                                            |
## eigrp

### verificación eigrp

| **comando**                  | **acción / qué verifica**                                                                                                 |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **show ip protocols**        | verifica los valores de las constantes métricas (**k-values**), el número de sistema autónomo (as), la varianza y redes.  |
| **show ip eigrp interfaces** | muestra en qué puertos específicos está activo el proceso eigrp y el conteo de peers en cada uno.                         |
| **show ip eigrp topology**   | muestra la tabla de topología completa (contiene los _successors_, _feasible successors_ y sus métricas **fd** y **rd**). |
| **show ip eigrp neighbors**  | lista los vecinos con adyacencia establecida. si está vacía, no hay intercambio de rutas.                                 |
### configuración eigrp global

| **comando**                              | **acción / notas de configuración**                                                                                               |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **router eigrp** _as-number_             | activa el proceso eigrp para el sistema autónomo especificado. **debe coincidir en todos los routers** para formar vecindad.      |
| **network** _classful-network-number_    | activa eigrp en las interfaces dentro de esa red con clase. usar **0.0.0.0** lo habilita en todas las interfaces del equipo.      |
| **network** _ip-address_ _wildcard-mask_ | activa eigrp de forma selectiva y precisa delimitando el rango mediante una máscara wildcard.                                     |
| **variance** _1-128_                     | multiplicador para permitir balanceo de carga con distinto costo (**ucmp**). permite meter en la tabla rutas con métricas peores. |
| **redistribute static**                  | inyecta y propaga las rutas estáticas configuradas en este router hacia todo el dominio eigrp. **SOLO CLASSFULS**                 |
| **redistribute static subnets**          | inyecta y propaga las rutas estáticas **NO classfuls**                                                                            |
| **eigrp router-id** _ip-address_         | define manualmente la identidad única de 32 bits para el router dentro del proceso eigrp.                                         |
| **passive-interface** _interface_        | detiene el envío y recepción de paquetes hello. evita formar adyacencias pero sigue anunciando la red del puerto.                 |
| **maximum-paths** _1-32_                 | define el número máximo de rutas paralelas en la tabla para balanceo de carga (por defecto es 4).                                 |
### configuración eigrp en interfaz

| **comando**                                               | **acción / notas de configuración**                                                                            |
| --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **ip hello-interval eigrp** _as-number_ _seconds_         | modifica la frecuencia de envío de los paquetes hello en esa interfaz específica.                              |
| **ip hold-time eigrp** _as-number_ _seconds_              | modifica el tiempo de espera antes de declarar al vecino muerto (por defecto es $3 \times \text{hello}$).      |
| **ip summary-address eigrp** _as-number_ _network_ _mask_ | realiza una sumarización manual de rutas salientes a nivel de interfaz para reducir las tablas de los vecinos. |
## hsrp

| **comando**                                                | **acción / notas de configuración**                                                                                                                                               |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show standby**                                           | _(modo exec)_ vista general completa de hsrp: estado local (**active**, **standby**), versión, ips/macs virtuales, temporizadores y si la premonición (_preemption_) está activa. |
| **standby** _group-number_ **ip** [ _virtual-ip-address_ ] | _(modo interfaz)_ configura el número de grupo hsrp y su dirección ip virtual correspondiente (el _gateway_ de los hosts).                                                        |
| **standby** _group-number_ **preempt**                     | _(modo interfaz)_ fuerza al router a recuperar el rol de **active** de forma inmediata si vuelve a estar en línea y tiene mayor prioridad que el router actual.                   |
| **standby** _group-number_ **priority** _0-255_            | _(modo interfaz)_ define la prioridad del router para la elección (por defecto es **100**). el valor más alto gana el rol activo.                                                 |
| **standby version** { **1** \| **2** }                     | _(modo interfaz)_ cambia la versión de hsrp. la versión 2 es obligatoria para soportar ipv6 y ampliar el rango de grupos de **0 a 4095** (la v1 solo admite de 0 a 255).          |
## vrf
| **comando**                                                                 | **descripción**                                                                                                            |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **show ip vrf**                                                             | muestra todas las configuraciones vrf del router                                                                           |
| **show ip route vrf** *instancia*                                           | muestra la routing table de la instancia vrf. el resto de interfaces no asignadas se quedarán en la routing table general. |
| **ip vrf** *instancia*                                                      | crear una instancia vrf                                                                                                    |
| **ip vrf forwarding** *instancia*                                           | en la interfaz, le asignas la instancia vrf (esto borrará cualquier ip asignada anteriormente)                             |
| **ip address** *ip* *máscara*                                               | aplicada la instancia vrf, asignas una nueva ip a la interfaz                                                              |
| **ping vrf** *instancia* *ip*                                               | hacer ping dentro de una instancia vrf                                                                                     |
| **ip route vrf** *instancia* _network_ _mask_ { _next-hop_ \| _interface_ } | añadir ruta a una instancia vrf                                                                                            |
# acl

### verificación de acls

| **comando**                           | **acción / qué verifica**                                                                                                                                     |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show access-lists**                 | muestra la configuración de **todas** las listas de acceso del sistema (ip, mac, etc.) junto con sus contadores de coincidencia (_matches_).                  |
| **show ip access-lists**              | muestra en detalle únicamente las listas de acceso basadas estrictamente en **ipv4** y sus líneas de coincidencia numéricas.                                  |
| **show ip interface** [ _interface_ ] | muestra los parámetros detallados de capa 3 del puerto, especificando qué acl está aplicada en dirección de entrada (**inbound**) u de salida (**outbound**). |
### standard numbered

| **comando**                                                                   | **acción / notas de configuración**                                                                                                                        |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **access-list** _1-99_ { **deny** \| **permit** } _source-ip_ _wildcard-mask_ | crea una **acl estándar numerada**. evalúa solo la red de origen. rangos válidos: **1-99** y **1300-1999**.                                                |
| **access-list** _1-99_ { **deny** \| **permit** } **host** _source-ip_        | atajo para especificar una única ip origen (equivalente a usar la máscara wildcard **0.0.0.0**).                                                           |
| **access-list** _1-99_ { **deny** \| **permit** } **any**                     | atajo que hace coincidir absolutamente cualquier ip de origen (equivalente a **0.0.0.0 255.255.255.255**).                                                 |
| **access-list** _1-99_ **remark** _text_                                      | añade un comentario descriptivo (máximo 100 caracteres) para documentar el propósito de la regla.                                                          |
| **ip access-list resequence** _1-99_ _starting-sequence-number_ _increment_   | reordena de golpe los números de línea de una acl. define el número inicial y el salto entre secuencias (ej: iniciar en 10 con incremento de 10).          |
| **ip access-list standard** { _1-99_ \| _name_ }                              | crea o edita una acl estándar (por nombre o número). permite insertar o borrar líneas específicas mediante números de secuencia sin rehacer toda la lista. |
| **no access-list** _1-99_                                                     | elimina por completo la lista de acceso numerada especificada.                                                                                             |
| **ip access-group** _1-99_ { **in** \| **out** }                              | _(modo interfaz)_ aplica la acl en una interfaz física o subinterfaz, especificando la dirección del flujo: entrada (**in**) o salida (**out**).           |
| **access-class** *number* { **in \| out** }                                   | aplicas una acl a una línea (console o vty) dentro del modo de configuración de la línea                                                                   |
### standard named 

| **comando**                                                                  | **acción / notas de configuración**                                                                                                                                    |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ip access-list standard** _name_                                           | _(modo global)_ crea o accede a una **acl estándar nombrada**. te introduce en el submodo de configuración (config-std-nacl).                                          |
| [ _sequence-number_ ] { **deny** \| **permit** } _source-ip_ _wildcard-mask_ | _(modo acl)_ define una regla de control de acceso (ace). si omites el número de secuencia, el sistema lo asigna automáticamente en incrementos de 10 (10, 20, 30...). |
| **remark** _text_                                                            | _(modo acl)_ añade una descripción explicativa directamente debajo de la línea de secuencia de la acl.                                                                 |
| **no** _sequence-number_                                                     | _(modo acl)_ elimina una línea específica de la lista utilizando únicamente su número de secuencia.                                                                    |
| **no ip access-list standard** _name_                                        | _(modo global)_ borra por completo la acl nombrada especificada.                                                                                                       |
| **ip access-group** _name_ { **in** \| **out** }                             | _(modo interfaz)_ aplica la acl por nombre en una interfaz, determinando la dirección del tráfico: entrada (**in**) o salida (**out**).                                |
| **ip access-list resequence** _name_ _starting-sequence-number_ _increment_  | _(modo global)_ reorganiza los números de secuencia de la acl nombrada con el valor inicial e incremento indicados.                                                    |
| **access-class** *name* { **in \| out** }                                    | aplicas una acl a una línea (console o vty) dentro del modo de configuración de la línea                                                                               |
### extended acls 

| **comando**                                                                                                                        | **acción / notas de configuración**                                                                                                                                                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **access-list** _100-199_ { **deny** \| **permit** } _protocol_ _source_ [ _operator_ _port_ ] _destination_ [ _operator_ _port_ ] | crea una **acl extendida numerada**. filtra por origen, destino y protocolo (ej: **tcp**, **udp**, **icmp**, **ip**).<br><br>  <br><br>rangos válidos: **100-199** y **2000-2699**. los operadores de puerto (**eq**, **lt**, **gt**, **neq**, **range**) solo aplican para tcp y udp. |
| **ip access-list extended** { _100-199_ \| _name_ }                                                                                | _(modo global)_ crea o edita una acl extendida (por número o nombre). te introduce en el submodo de configuración (config-ext-nacl).                                                                                                                                                   |
| [ _sequence-number_ ] { **deny** \| **permit** } _protocol_ _source_ [ _operator_ _port_ ] _destination_ [ _operator_ _port_ ]     | _(modo acl)_ define una regla de control de acceso (ace) dentro de una lista extendida. si omites el número de secuencia, se asigna automáticamente en incrementos de 10.                                                                                                              |
| **no** _sequence-number_                                                                                                           | _(modo acl)_ elimina una línea específica de la lista de acceso utilizando únicamente su número de secuencia.                                                                                                                                                                          |
| **ip access-group** { _number_ \| _name_ } { **in** \| **out** }                                                                   | _(modo interfaz)_ aplica la acl extendida (sea por nombre o número) en un puerto, determinando el sentido del tráfico: entrada (**in**) o salida (**out**).                                                                                                                            |
| **ip access-list resequence** { _number_ \| _name_ } _start_ _increment_                                                           | _(modo global)_ reorganiza de golpe los números de secuencia de la acl extendida con el valor inicial e incremento especificados.                                                                                                                                                      |
| **access-class** { *number* \| *name* } { **in \| out** }                                                                          | aplicas una acl a una línea (console o vty) dentro del modo de configuración de la línea                                                                                                                                                                                               |

# ntp

### verificación del reloj

|**comando**|**acción / qué verifica**|
|---|---|
|**show clock**|muestra la hora actual del _software clock_. **nota clave:** si la hora va precedida por un asterisco (*), indica que no es de confianza (_no authoritative_) porque no se ha sincronizado con una fuente fiable.|
|**show clock detail**|muestra la hora del sistema detallando la fuente de sincronización (ej: configurado por usuario, ntp o _hardware calendar_).|
|**show calendar**|muestra la hora actual del reloj por hardware interno (_hardware clock / calendar_), el cual mantiene la hora incluso si el equipo se apaga.|
### configuración del reloj (modo privileged exec)

| **comando**                                      | **acción / notas de configuración**                                                                                                      |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **clock set** _hh:mm:ss_ _day_ _month_ _year_    | configura manualmente el reloj por software del sistema. el mes se introduce con sus tres primeras letras en inglés (ej: jan, feb, mar). |
| **calendar set** _hh:mm:ss_ _day_ _month_ _year_ | configura manualmente el reloj por hardware interno (_hardware clock_).                                                                  |
| **clock update-calendar**                        | sincroniza los relojes: hace que el _hardware calendar_ se actualice copiando la hora actual del _software clock_.                       |
| **clock read-calendar**                          | sincroniza los relojes: hace que el _software clock_ lea y adopte la hora almacenada en el _hardware calendar_.                          |
### zonas horarias y horario de verano (modo configuración global)

| **comando**                                                                                         | **acción / notas de configuración**                                                                                                                                                                                                                                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **clock timezone** _zone_ _hours-offset_ [ _minutes-offset_ ]                                       | define la zona horaria del equipo indicando el desfase respecto a utc/gmt (ej: para españa central **clock timezone cet 1**, para retrasar horas usa valores negativos).                                                                                                                                                                                                                                                                                          |
| **clock summer-time** *zone* **recurring** [*week day month hh:mm week day month hh:mm* [*offset*]] | configura el cambio automático al horario de verano (_daylight saving time_). la opción **recurring** aplica las reglas de inicio y fin de forma anual según el calendario (ej: **recurring last sun mar 02:00 last sun oct 03:00**).<br><br>`recurring` solo, sin parámetros, también es válido en IOS, pero utiliza las fechas predeterminadas de Cisco para EUA.<br><br>El `offset` es **opcional** también; si lo omites, IOS usa **60 minutos** por defecto. |
### verificación de ntp

| **comando**               | **acción / qué verifica**                                                                                                                                                                         |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show ntp associations** | muestra la tabla de servidores ntp. prefijos clave: ***** (servidor actual sincronizado) y **+** (candidato válido). muestra además el _stratum_ y la ip/reloj de referencia (ej: .locl., .gps.). |
| **show ntp status**       | muestra de forma rápida si el reloj está sincronizado (**clock is synchronized**), el _stratum_ actual (siempre suma +1 respecto al servidor) y la ip del origen de tiempo.                       |
### configuración de clientes y servidores ntp

| **comando**                                | **acción / notas de configuración**                                                                                                                                                                              |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ntp server** _ip-address_ [ **prefer** ] | _(modo global)_ sincroniza la hora con un servidor externo. el parámetro opcional **prefer** le da prioridad absoluta sobre otros servidores configurados.                                                       |
| **ntp master** [ _1-15_ ]                  | _(modo global)_ convierte al dispositivo en un servidor ntp maestro. por defecto asume _stratum_ **7** (actuando como _stratum_ 8 para sus clientes) y usa .locl. como referencia si pierde la conexión externa. |
| **ntp source** _interface_                 | _(modo global)_ fuerza al dispositivo a enviar todos los paquetes ntp utilizando la dirección ip de la interfaz especificada (altamente recomendado usar una interfaz **loopback**).                             |
| **ntp peer** _ip-address_                  | _(modo global)_ configura una relación de igual a igual (_peer-to-peer_). permite que dos routers compartan y se sincronicen el tiempo de forma bidireccional como respaldo.                                     |
| **ntp update-calendar**                    | _(modo global)_ permite que ntp actualice automáticamente el reloj por hardware (_hardware calendar_) de forma periódica para mantenerlo alineado con el reloj por software.                                     |

### autenticación ntp (modo conf global)

| **comando**                                              | **acción / notas de configuración**                                                                                |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **ntp authenticate**                                     | habilita globalmente la función de autenticación ntp en el dispositivo.                                            |
| **ntp authentication-key** _number_ **md5** _key-string_ | define y asocia un número de índice a una contraseña en texto plano cifrada con md5.                               |
| **ntp trusted-key** _number_                             | autoriza explícitamente al sistema a confiar en el número de clave configurado para la sincronización.             |
| **ntp server** _ip-address_ **key** _number_             | **[en el cliente]** vincula la ip del servidor con el número de clave de confianza requerida para aceptar su hora. |
| **ntp peer** _ip-address_ **key** _number_               | **[entre peers]** vincula la relación de peering exigiendo la clave de autenticación especificada.                 |
# dhcp

### dhcp relay agent

| **comando**                        | **descripción**                                                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **ip helper-address** _ip-address_ | dentro de la interfaz, habilita la función de dhcp relay agent para redirigir las peticiones broadcast por unicast al servidor. |
### dhcp client

| **comando**                                                   | **descripción**                                                                                            |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **ip address dhcp**                                           | configura una interfaz de un router para que adquiera su dirección ip de forma dinámica como cliente dhcp. |
### dhcp server

| **comando**                                                   | **descripción**                                                                                      |
| ------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **show ip dhcp binding**                                      | muestra la lista de direcciones ip asignadas actualmente a los clientes (tabla de bindings).         |
| **show ip dhcp pool**                                         | muestra detalle de los pools                                                                         |
| **ip dhcp excluded-address** _low-address_ [_high-address_]   | excluye una dirección ip o un rango de direcciones para que no sean asignadas dinámicamente.         |
| **ip dhcp pool** _pool-name_                                  | crea un pool de dhcp y entra al modo de configuración del pool.                                      |
| **network** _ip-address_ { _prefix-length_ \| _subnet-mask_ } | define la subred de direcciones ip disponibles en el pool de dhcp.                                   |
| **default-router** _ip-address_                               | configura la dirección ip de la puerta de enlace predeterminada (default gateway) para los clientes. |
| **dns-server** _ip-address_                                   | configura la dirección del servidor dns disponible para los clientes del pool.                       |
| **domain-name** _domain-name_                                 | define el nombre de dominio de la red para los clientes dhcp.                                        |
| **lease** { _days_ [_hours_ [_minutes_]] \| **infinite** }    | establece la duración del tiempo de préstamo o concesión de las direcciones ip.                      |
# dns

| **comando con sintaxis oficial**    | **nota breve**                                                                                                                                           |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show hosts**                      | muestra la tabla de resolución local. indica si la entrada es **perm** (estática) o **temp** (aprendida por dns), junto con su tiempo de vida (**age**). |
| **ip name-server** _ip-address_     | define la ip de un servidor dns externo (como `1.1.1.1`) al que el router consultará si no encuentra el nombre en su tabla local.                        |
| **ip domain lookup**                | activa las capacidades del router para resolver nombres dns (viene encendido por defecto; en ios antiguos se escribía **ip domain-lookup**).             |
| **ip dns server**                   | activa las funciones de servidor dns en el router, permitiéndole responder consultas de los clientes de la red.                                          |
| **ip host** _hostname_ _ip-address_ | crea una entrada estática y permanente en la tabla dns local del router, mapeando un nombre a su dirección ip.                                           |
| **ip domain name** _domain-name_    | define el sufijo de dominio por defecto del router (ej: `empresa.com`). en versiones de ios antiguas se configuraba como **ip domain-name**.             |

# snmp

| **comando**                                                         | **descripción**                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **snmp-server contact** _contact-info_                              | define la información de contacto del administrador del dispositivo (ej. correo electrónico). es opcional.                                                                                                                                                                                                                                                                                                                    |
| **snmp-server location** _text_                                     | define la ubicación física del dispositivo. es información opcional para el inventario.                                                                                                                                                                                                                                                                                                                                       |
| **snmp-server community** _string-name_ **ro**                      | configura una cadena de comunidad (contraseña) con permisos de **solo lectura**. el nms no puede usar mensajes _set_. <br>*NOTA: Aquí cualquier con la community obtendrá los datos, si quieres limitar, usa ACL*                                                                                                                                                                                                             |
| **snmp-server community** _string-name_ **rw**                      | configura una cadena de comunidad con permisos de **lectura y escritura**. el nms puede modificar parámetros con mensajes _set_.<br>*NOTA: Aquí cualquier con la community obtendrá los datos, si quieres limitar, usa ACL*                                                                                                                                                                                                   |
| **snmp-server host** *ip-del-NMS* **version** *versión* *community* | define a qué NMS se enviarán las notificaciones, qué versión SNMP usar y la community.<br><br>Por si solo no hace nada. Hay que habilitar el envío con los comandos siguientes.                                                                                                                                                                                                                                               |
| **snmp-server enable traps**                                        | habilita todo el envío de alertas (_traps_)                                                                                                                                                                                                                                                                                                                                                                                   |
| **snmp-server enable traps** [*tipo-de-trap*] [*subtipo-opcional*]` | Habilita el envío de traps SNMP para determinados eventos del dispositivo.<br><br>Ejemplo: **snmp-server enable traps snmp linkdown linkup** (habilita específicamente el envío de alertas (_traps_) cuando una interfaz de red sube (_linkup_) o baja (_linkdown_).)<br><br>Ejemplo: **snmp-server enable traps config** (habilita el envío de alertas (_traps_) cada vez que se modifica la configuración del dispositivo.) |
# syslog

| **comando**                                                                     | **descripción**                                                                                                                        |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **logging console** { _severity_level_ \| _severity_keyword_ }                  | activas mensajes de logs en la consola del security level especificado hacia arriba                                                    |
| **logging monitor** { _severity_level_ \| _severity_keyword_ }                  | activas mensajes de logs en las líneas vty del security level especificado hacia arriba                                                |
| **logging buffered** [_buffer size_] { _severity_level_ \| _severity_keyword_ } | activas mensajes de logs en la ram del equipo del security level especificado hacia arriba. el tamaño del bufer se especifica en bytes |
| **logging [host]** _ip server address_                                          | activas mensajes de logs en servidor syslog remoto                                                                                     |
| **logging trap** { _severity_level_ \| _severity_keyword_ }                     | defines nivel severity que enviarás al servidor syslog remoto (de ese nivel hacia arriba)                                              |
| **terminal monitor**                                                            | activas el logging en la línea vty. debe activarse en cada sesión y en modo privileged exec                                            |
| **logging synchronous**                                                         | en modo configuración de la línea, indicas que los mensajes no interrumpan lo que escribes.                                            |
| **service timestamps log [datetime \| uptime]**                                 | activas la salida de la fecha o el uptime en los logs                                                                                  |
| **service sequence numbers**                                                    | activas la salida de los seq numbers en los logs                                                                                       |
# ssh/telnet

### configuración de ssh

| **comando**                                  | **descripción**                                                                                              |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **show ip ssh**                              | muestra si ssh está activado y la versión                                                                    |
| **ip domain name** *domain*                  | defines el dominio de la red                                                                                 |
| **crypto key generate rsa**                  | modo 1 para generar las claves. luego debes especificar el tamaño. la clave para sshv2 debe ser mayor de 768 |
| **crypto key generate rsa modulus** *length* | modo 2 para generar las claves. le pasas el tamaño directamente. la clave para sshv2 debe ser mayor de 768   |
| **ip ssh version 2**                         | restringes el uso exclusivo de la versión 2 (recomendado)                                                    |
### configuración de la línea

| **comando**                                                            | **descripción**                                                                                                                |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **enable password** _pass_                                             | define clave de acceso al modo privilegiado en texto plano. **no usar** (vulnerable, tipo 0).                                  |
| **service password-encryption**                                        | encripta débilmente (tipo 7) las contraseñas de texto plano actuales y futuras en el archivo de configuración.                 |
| **enable secret** _pass_                                               | define clave de acceso al modo privilegiado cifrada con un hash seguro (tipo 5 md5 o superior). obligatorio para telnet y ssh. |
| **username** *user* { **secret \| password** } *password*              | en modo config, configuras un par usuario/contraseña local                                                                     |
| **line console 0**                                                     | entras a la configuración de la línea consola                                                                                  |
| **line vty 0 15**                                                      | entras a la configuración de las líneas vty                                                                                    |
| **password** *password*                                                | defines una contraseña de línea para entrar a la cli                                                                           |
| **login**                                                              | configuras la línea para que solicite contraseña de línea. no válido para ssh.                                                 |
| **login local**                                                        | aplicas el usuario/contraseña local                                                                                            |
| **exec-timeout** *minutes* [*seconds*]                                 | configuras en la línea el tiempo de inactividad máximo                                                                         |
| **transport input** { **telnet \| ssh \| telnet ssh \| all \| none** } | configuras en la línea qué conexiones están permitidas                                                                         |
| **access-class** { *number* \| *name* } { **in \| out** }              | aplicas una acl a una línea (console o vty) dentro del modo de configuración de la línea                                       |
### conexión desde host
| **comando**                | **descripción**                |
| -------------------------- | ------------------------------ |
| **telnet** *ip*            | iniciar conexión telnet remota |
| **ssh -l** *username* *ip* | iniciar conexión ssh remota    |
| **ssh** *username@ip*      | iniciar conexión ssh remota    |
# tftp

>[!info] se muestra ejemplo de actualizar el ios desde servidor tftp

| **comando**                     | **descripción**                                                                |
| ------------------------------- | ------------------------------------------------------------------------------ |
| **show version**                | muestra detalles del vios. versión, etc.                                       |
| **show file systems**           | muestra todos los ifs y sus prefijos                                           |
| **show flash**                  | muestra el contenido de la flash (.bin)                                        |
| **copy** *source* *destination* | copia de un ifs o prefijo a otro (ej. `copy tftp: flash:`)                     |
| **boot system** *file path*     | indicas que en el próximo inicio cargue una nueva imagen (flash:nueva_ios.bin) |
| **write memory**                | para que tome la nueva imagen                                                  |
| **reload**                      | reinicias el dispositivo                                                       |
| **delete** *filepath*           | borras el contenido (ej. un .bin antiguo)                                      |
| **rename** *old_name new_name*  | renombrar archivo                                                              |

# ftp

>[!info] se muestra ejemplo de actualizar el ios desde servidor ftp

| **comando**                     | **descripción**                                                                |
| ------------------------------- | ------------------------------------------------------------------------------ |
| **ip ftp username** *username*  | usuario para autenticarse en ftp                                               |
| **ip ftp password** *username*  | contraseña para autenticarse en ftp                                            |
| **show version**                | muestra detalles del vios. versión, etc.                                       |
| **show file systems**           | muestra todos los ifs y sus prefijos                                           |
| **show flash**                  | muestra el contenido de la flash (donde está el .bin)                          |
| **copy** *source* *destination* | copia de un ifs o prefijo a otro (ej. `copy ftp: flash:`)                      |
| **boot system** *file path*     | indicas que en el próximo inicio cargue una nueva imagen (flash:nueva_ios.bin) |
| **write memory**                | para que tome la nueva imagen                                                  |
| **reload**                      | reinicias el dispositivo                                                       |
| **delete** *filepath*           | borras el contenido (ej. un .bin antiguo)                                      |
| **rename** *old_name new_name*  | renombrar archivo                                                              |
# nat

### static nat
| **comando**                                                          | **descripción**                                                              |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **show ip nat translations**                                         | muestra los mapeos actuales configurados (dinámicos y permanentes)           |
| **clear ip nat translation ***                                       | limpia la tabla de entradas dinámicas                                        |
| **show ip nat statistics**                                           | translations activas, total de realizadas, interfaces ouside/inside, etc.    |
| **ip nat inside**                                                    | en modo config de la interfaz, defines que estará conectada a la red interna |
| **ip nat outside**                                                   | en modo config de la interfaz, defines que estará conectada a la red externa |
| **ip nat inside source static** *inside-local-ip* *inside-global-ip* | en modo config global, mapeas la inside local con su inside global           |
## dynamic nat

| **comando**                                                                                                 | **descripción**                                                              |
| ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **show ip nat translations**                                                                                | muestra los mapeos actuales configurados (dinámicos y permanentes)           |
| **clear ip nat translation ***                                                                              | limpia la tabla de entradas dinámicas                                        |
| **show ip nat statistics**                                                                                  | translations activas, total de realizadas, interfaces ouside/inside, etc.    |
| **ip nat inside**                                                                                           | en modo config de la interfaz, defines que estará conectada a la red interna |
| **ip nat outside**                                                                                          | en modo config de la interfaz, defines que estará conectada a la red externa |
| **ip nat pool** *nombre_pool* *ip_inicio* *ip_final* {**prefix-length** *prefijo* \| **netmask** *máscara*} | defines el pool de direcciones inside-global                                 |
| **access-list** *número_acl* **permit** *ip_red* *prefijo*                                                  | defines la red que será nateada                                              |
| **ip nat inside source list** *número_acl* **pool** *nombre_pool*                                           | configuras nat mapeando la acl a la pool                                     |
## pat
| **comando**                                                                                                 | **descripción**                                                              |
| ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **show ip nat translations**                                                                                | muestra los mapeos actuales configurados (dinámicos y permanentes)           |
| **clear ip nat translation ***                                                                              | limpia la tabla de entradas dinámicas                                        |
| **show ip nat statistics**                                                                                  | translations activas, total de realizadas, interfaces ouside/inside, etc.    |
| **ip nat inside**                                                                                           | en modo config de la interfaz, defines que estará conectada a la red interna |
| **ip nat outside**                                                                                          | en modo config de la interfaz, defines que estará conectada a la red externa |
| **access-list** *número_acl* **permit** *ip_red* *prefijo*                                                  | defines la red que será nateada                                              |
| **ip nat inside source list** *número_acl* **interface** *interface* **overload**                           | configuras nat mapeando la acl a la interfaz de salida del router            |
| **ip nat pool** *nombre_pool* *ip_inicio* *ip_final* {**prefix-length** *prefijo* \| **netmask** *máscara*} | defines el pool de direcciones inside-global                                 |
| **ip nat inside source list** *número_acl* **pool** *nombre_pool* **overload**                              | configuras nat mapeando la acl a la pool                                     |
> Puede funcionar de dos formas:
> - Usando **una sola IP pública**, normalmente la IP de la interfaz outside del router.
> - Usando un **pool de IP públicas con `overload`**, donde varios hosts internos pueden compartir incluso la misma IP del pool gracias a los puertos.

# ipv6

### verificación de ipv6

| **comando**                   | **acción / qué verifica**                                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **show ipv6 interface brief** | vista rápida de interfaces ipv6: muestra estados, direcciones de enlace local (**lla**) y globales (**gua**). |
| **show ipv6 neighbor**        | muestra la tabla de vecinos descubiertos por **ndp** (el equivalente a la tabla arp de ipv4).                 |
### configuración de ipv6 global y de interfaz

| **comando**                                                     | **acción / notas de configuración**                                                                                                                                               |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ipv6 unicast-routing**                                        | _(modo global)_ comando indispensable que activa el reenvío de paquetes y enrutamiento ipv6 en todo el equipo.                                                                    |
| **ipv6 enable**                                                 | _(modo interfaz)_ genera automáticamente una dirección de enlace local (_link-local_ **fe80::/10**) basada en eui-64. permite comunicación en el segmento local sin enrutamiento. |
| **ipv6 address** _ipv6-address_**/**_prefix-length_             | _(modo interfaz)_ asigna manualmente una dirección ipv6 estática (ya sea **gua** o **ula**) con su longitud de prefijo.                                                           |
| **ipv6 address** _ipv6-address_ **link-local**                  | _(modo interfaz)_ sobrescribe la dirección de enlace local automática por una dirección personalizada fácil de recordar (ej: **fe80::1**).                                        |
| **ipv6 address** _ipv6-prefix_**/**_prefix-length_ **eui-64**   | _(modo interfaz)_ autocompleta la dirección ipv6 combinando el prefijo de red indicado con la dirección mac del puerto (formato eui-64 de 64 bits).                               |
| **ipv6 address** _ipv6-address_**/**_prefix-length_ **anycast** | _(modo interfaz)_ configura una dirección de tipo _anycast_ compartida entre múltiples servidores o interfaces físicas.                                                           |
| **ipv6 address autoconfig**                                     | _(modo interfaz)_ configura la ip usando **slaac**; el puerto aprende el prefijo local de la red mediante mensajes ndp (_router advertisement_) y genera su propio host id.       |
### enrutamiento estático ipv6

| **comando**                                                                     | **acción / notas de configuración**                                                                                                                               |
| ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ipv6 route** _ipv6-prefix_**/**_prefix-length_ _next-hop-address_             | configura una ruta estática recursiva indicando únicamente la dirección ipv6 del siguiente salto.                                                                 |
| **ipv6 route** _ipv6-prefix_**/**_prefix-length_ _interface_ _next-hop-address_ | configura una ruta estática completamente especificada (_fully specified_). **obligatorio** si usas como siguiente salto una dirección de enlace local (**lla**). |
# qos

### ip phone/voice vlan

| **comando**                                | **descripción**                                                                                                                       |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| **show power inline**                      | muestra estado PoE, potencia usada/disponible y dispositivos alimentados                                                              |
| **switchport voice vlan** *número*         | en un puerto access defines una vlan para el tráfico de voz                                                                           |
| **show power inline police** *interface*   | muestra la configuración de power policing poe de la interfaz.                                                                        |
| **power inline police**                    | activa power policing. si el dispositivo poe supera la potencia permitida, el puerto entra en **err-disabled** y se genera un syslog. |
| **power inline police action err-disable** | equivalente al comando anterior (es la acción por defecto).                                                                           |
| **power inline police action log**         | si el dispositivo poe supera la potencia permitida, se reinicia (no entra en err-disable) y se genera un syslog.                      |

# security
## port-security

| **comando**                                                                    | **descripción**                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **show port-security**                                                         | detalle global de port-security en el dispositivo                                                                                                                                                                                               |
| **show port-security address**                                                 | muestra las mac address grabadas, su tipo e interfaz física donde están grabadas                                                                                                                                                                |
| **show port-security interface** *interface*                                   | muestra detalles de port-security en la interfaz                                                                                                                                                                                                |
| **switchport port-security**                                                   | habilitas port-security en la interfaz **(debe estar en modo access o trunk estático)**. Este comando también sirve para habilitar el modo dinámico: el switch **aprende automáticamente** las primeras MAC que vea, hasta el máximo permitido. |
| **switchport port-security mac-address** *mac-address*                         | habilitas (modo manual) port-security con una mac específica. **primero habilitar port-security**                                                                                                                                               |
| **switchport port-security mac-address sticky**                                | graba las mac addresses dinámicas actuales (y las futuras) en la running-config. **hay que guardar la config** y **primero habilitar port-security**                                                                                            |
| **switchport port-security maximum** *number*                                  | configuras el número de mac addresses permitidas                                                                                                                                                                                                |
| **switchport port-security violation** { **restrict \| protect \| shutdown** } | configuras el violation mode (es shutdown por defecto)                                                                                                                                                                                          |
| **switchport port-security aging time** *minutes*                              | configuras el aging time                                                                                                                                                                                                                        |
| **switchport port-security aging type** { **absolute \| inactivity** }         | configuras el aging type (no funciona con mac addresses estáticas)                                                                                                                                                                              |
| **switchport port-security aging static**                                      | configuras el aging para una mac estática (o sticky)                                                                                                                                                                                            |
| **errdisable recovery cause psecure-violation**                                | habilita la recuperación automática para port-security. ver [[#errdisable recovery]]                                                                                                                                                            |
| **show mac-address table secure**                                              | muestra la tabla mac con las mac-address secure (static = static y sticky, dynamic = dinámicas)                                                                                                                                                 |
| **show interface** *interface* **switchport**                                  | muestra detalles de la interfaz (para saber si está en modo access)                                                                                                                                                                             |
## dhcp snooping
| **comando**                                             | **descripción**                                                                                         |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **show ip dhcp snooping**                               | muestra detalles de dhcp snooping en switch                                                             |
| **show ip dhcp snooping binding**                       | muestra los leasing actuales: mac, ip, interfaz                                                         |
| **ip dhcp snooping**                                    | (global) activas dhcp snooping en el equipo                                                             |
| **ip dhcp snooping vlan** *vlan*                        | (global) no basta con activar dhcp snooping. hay que hacerlo luego en cada vlan                         |
| **ip dhcp snooping trust**                              | (interface) en la interfaz indicada, la configuras como trusted (uplink)                                |
| **ip dhcp snooping limit rate** *pps*                   | (interface) en la interfaz indicada, configuras los paquetes por segundo permitidos                     |
| **no ip dhcp snooping information option**              | (global) desactivas que el switch insert option 82 en los mensajes dhcp. ver [[glossary#dhcp snooping]] |
| **ip dhcp snooping information option allow-untrusted** | (global) aceptas mensajes dhcp con option 82 por puertos untrusted                                      |
| **errdisable recovery cause dhcp-rate-limit**           | (global) activas la recuperación automática para errdisable de interfaz por exceder el rate limit       |
## dynamic arp inspection

| **comando**                                                                | **descripción**                                                                          |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **show ip arp inspection**                                                 | muestra detalles de arp inspection en el equipo                                          |
| **show ip arp inspection interfaces**                                      | muestra el estado de las interfaces, rate y burst                                        |
| **ip arp inspection vlan** *vlan*                                          | activas arp inspection en el switch                                                      |
| **ip arp inspection validate ip src-mac dst-mac**                          | configuras los optional checks en el switch (puedes especificar 1, 2 o los 3)            |
| **ip arp inspection trust**                                                | en la interfaz, configuras como trusted                                                  |
| **ip arp inspection limit rate** *packets* [ **burst interval** *seconds*] | en la interfaz, configuras el rate limit (*x packets in y seconds*)                      |
| **errdisable recovery cause arp-inspection**                               | activas la recuperación automática para errdisable de interfaz por exceder el rate limit |
### arp acl
| **comando**                                                | **descripción**                                                                                              |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **arp access-list** *nombre_acl*                           | creas una acl para permitir arp request de ips estáticas (que no están en la ip dhcp snooping binding table) |
| **permit ip host** *ip_address* **mac host** *mac_address* | defines la ip y la mac address del host                                                                      |
| **ip arp inspection filter** *nombre_acl* **vlan** *vlan*  | aplicas la acl en la vlan determinada                                                                        |
# client os

| **comando**                                           | **nota breve**                                                                                      |
| ----------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **ping** { ip-address \| hostname } [**-n** *number*] | verifica la conectividad de capa 3 enviando paquetes icmp echo request.                             |
| **traceroute** { _ip-address_ \| _hostname_ }         | muestra la ruta exacta y los saltos que da un paquete hasta el destino (en windows es **tracert**). |
| **ipconfig**                                          | muestra la configuración ip básica de todas las interfaces en windows (ip, máscara y gateway).      |
| **ipconfig /all**                                     | muestra la configuración ip detallada de windows, incluyendo direcciones mac y servidores dns.      |
| **ipconfig /release**                                 | libera la dirección ip actual asignada por dhcp, dejando la interfaz sin ip.                        |
| **ipconfig /renew**                                   | solicita una nueva dirección ip (o renueva la actual) al servidor dhcp.                             |
| **ipconfig /displaydns**                              | muestra el contenido de la caché de resolución dns local de windows.                                |
| **ipconfig /flushdns**                                | limpia la caché de resolución dns local de windows.                                                 |
| **nslookup** { _hostname_ \| _ip-address_ }           | realiza consultas dns directas (pasa el dominio) o inversas (pasa la ip automáticamente).           |
| **arp -a**                                            | muestra la tabla caché arp del sistema con las asociaciones de ip y dirección mac.                  |
| **arp -d** [_ip-address_]                             | borra la tabla caché arp completa o una entrada específica si se le indica la ip.                   |
| **netstat -a**                                        | muestra todas las conexiones activas y los puertos que el host tiene en escucha (listening).        |
| **netstat -n**                                        | muestra las direcciones ip y puertos en formato numérico, omitiendo la resolución dns.              |
| **netstat -o**                                        | muestra el pid (identificador de proceso) de la aplicación asociada a cada conexión.                |
| **netstat -r**                                        | muestra la tabla de enrutamiento del host (en windows actúa igual que **route print**).             |
# gre

| **comando**                                    | **nota breve**                                                                                                                                |
| ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **interface tunnel** *number*                  | Defines una interfaz para el túnel. es similar a una loopback interface. simplemente estás creando una interfaz virtual                       |
| **tunnel mode gre ip**                         | Declara GRE sobre IPv4. En muchos routers Cisco es el modo predeterminado de la interfaz Tunnel, pero escribirlo hace explícita la intención. |
| **tunnel source** *interface*                  | (*dentro de la interfaz del túnel) defines la interfaz por donde saldrá el túnel                                                              |
| **tunnel destination** *ip_address*            | (*dentro de la interfaz del túnel*) defines la direccion ip del otro extremo del túnel                                                        |
| **ip address** *ip_address* *netmask*          | (*dentro de la interfaz del túnel*) defines la direccion ip del túnel                                                                         |
| **ip route** *ip_address* *netmask* *next-hop* | defines una ruta hacia la red del otro router. también puedes configurar ospf para que las redes se conozcan.                                 |

