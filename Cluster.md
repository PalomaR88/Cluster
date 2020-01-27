# Cluster

Un cluster es un conjunto de equipos independientes que realizan una terea común en la que se comportan como un solo equipo. 

Ofrece uno o varios de los siguientes servicios:
- Alto rendimiento
- Alta disponibilidad
- Balance de carga
- Escabilidad

Surge como alternativa al crecimiento vertical de los ordenadores: al aumentar los requisistos computacionales, en vez de sustituir el equipo por uno nuevo más potente, se añade una máquina más y se crea un clúster entre ambas.

**Máquinas físicas**
**Máquinas virtuales**
**Instancias de cloud**

Y combinaciones de ellas.

### Cluster HCP
Se suelen usar máquinas físicas. PAra cálculos complejos. Se utilizan en centros de investigación, ingeniería y otras actividades que requiren mucho cómputo.

### Clúster de alta disponibilidad HA
Implementa tolerancia a fallos para garantizar el servicio en caso de que ocurra algún fallo. Utiliza nodos redundantes que sustituyen o complementan a los existentes. Se suelen implementar con balanceo de carga, pero no tiene por qué. Ampliamente utilizado en servidios de Internet. 

### Cluster de balanceo
Es una propiedad que permite repartir el trabajo entre varios nodos del cluster. Se utiliza algún algoritmo para el reparto de tarea entre los nodos del clúster. 

### Almacenamiento
Los sistemas de ficheros tradicionales solo pueden montarse en un equipo. Si hay dos servidores que deben tener el mismo sistema de fichero se peude solucionar con:
- NAS: con un servicio que controla la concurrencia. Así se tendría un directorio con acceso a través de NAS. Pero en ocasiones esto no es suficiente, porque se necesita el disco, no el acceso al sistema de ficheros.
- SAN: una red de almacenamiento, dedicada. Similar al anterior, pero se conecta el dispositivo de bloque, no el sistema de fichero. Para ello, hay que utilizar un sistema de fichero compatible con cluster. 

#### Sistemas de dispositivos bloques compartidos
##### DRBD
RAID 1 en red. No es compatible con el balanceo de carga. 

##### OCFS2
Si acepta el balanceo de carga. Para Debian. Es un sistema de fichero con demonio, con un puerto abierto desde donde se comunican para que no haya concurrencias. Se suele utilizar con iSCSI.

##### GFS2
Si acepta el balanceo de carga. Para RedHat.

#### Sistemas de ficheros distribuidos
Mecanismo que no utiliza nada del anterior pero opporta la solución similar.

##### GlusterFS
Se utilizaría en una red de almacenamiento o similar, con un nodo de almacenamiento donde se instala el software gluster, sin iSCSI. En un disco se crean unos dispositivos de bloques dentro, que se llaman bricks, que son los que usarán los otros nodos. Y en los nodos se isntala también gluster. El nodo principal levanta un servicio en el puerto 24007 y los clientes, por TCP/IP montan los bridge. Y, a su vez, los nodos de almacenamiento también debe tener una réplica, gluster, porque si se va el nodo de almacenamiento se estropea todo. 

Es para máquinas Linux. 

##### CephFS
Hay múltiples nodos, cada uno de ellos con un disco, sin raid y sin nada, y la redundancia se garantiza porque todos se comunican con todos y se pone de acuerdo con Ceph para que se repliquen entre ellos. 

Además, lo que proporciona fuera del esquema de nodos replicados, lo que ofrece es un volumen, un disco, que se puede formatear como se quiera que es Ceph RBD (que sería tecnología iSCSI), CephFS que es la alternativa de cluster y almacenamiento de objetos Ceph Rados GW (para contenedores).

##### Windows DFS
Soporte nativo de Windows Server. No utiliza una capa por debajo sino que entre los nodos se usa un mismo volumen y se sincroniza. 

##### Lustre
En este caso es importatne la capidad computacional... 
