# ST0256 Tópicos Especiales en Telemática

## Estudiante(s):
- David Fonseca Lara (dfonsecal@eafit.edu.co)
- Julian Andres Romero Hinestroza (jaromeroh@eafit.edu.co)
- Juan Esteban Avendaño Castaño (jeavendanc@eafit.edu.co)

## Profesor: 
- *Nombre:* Álvaro Ospina
- *Correo:* aeospinas@eafit.edu.co

# Proyecto 2 - Cluster Kubernetes

## 1. Breve descripción de la actividad
### 1.1. Que aspectos cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)
### 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

## 2. información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.

## 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.
### como se compila y ejecuta.
### detalles del desarrollo.
### detalles técnicos
### descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)
### opcional - detalles de la organización del código por carpetas o descripción de algún archivo. (ESTRUCTURA DE DIRECTORIOS Y ARCHIVOS IMPORTANTE DEL PROYECTO, comando 'tree' de linux)
### opcionalmente - si quiere mostrar resultados o pantallazos 

## 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.
## IP o nombres de dominio en nube o en la máquina servidor.
### descripción y como se configura los parámetros del proyecto (ej: ip, puertos, conexión a bases de datos, variables de ambiente, parámetros, etc)
### como se lanza el servidor.
### una mini guia de como un usuario utilizaría el software o la aplicación
### opcionalmente - si quiere mostrar resultados o pantallazos 

## 5. Configuración del Cluster y despliegue de la aplicación
### Creación de las instancias
Para las instancias usadas para los nodos del cluster se utilizó el servicio de GCP, y se crearon 3 instancias con las siguientes características:

> Se usa una instancia `E2 small` para que soporten microk8s
> ![Instancias](./images/vm-creation-1.png)

> Activar las reglas de firewall
> ![Instancias](./images/vm-creation-2.png)

### Instalación de MicroK8s
Para la instalación de MicroK8s se siguió la guía oficial de la página de MicroK8s, la cual se encuentra en el siguiente [link](https://microk8s.io/docs/getting-started).

### Creación del cluster
Para la creación del cluster debemos dirijirnos al nodo maestro y ejecutar el siguiente comando:

```bash
$ microk8s add-node
From the node you wish to join to this cluster, run the following:
microk8s join 10.128.0.7:25000/4ba95021d5567941c329d2500bc8f023/105d18a7bb28

Use the '--worker' flag to join a node as a worker not running the control plane, eg:
microk8s join 10.128.0.7:25000/4ba95021d5567941c329d2500bc8f023/105d18a7bb28 --worker

If the node you are adding is not reachable through the default interface you can use one of the following:
microk8s join 10.128.0.7:25000/4ba95021d5567941c329d2500bc8f023/105d18a7bb28
```

Con este comando obtenemos el comando que debemos ejecutar en los nodos workers para unirlos al cluster, en nuestro caso ejecutamos el siguiente comando en los nodos workers:

```bash
$ microk8s⋅join⋅10.128.0.7:25000/4ba95021d5567941c329d2500bc8f023/105d18a7bb28
```
> [!IMPORTANT]
> El comando `microk8s add-node` se ejecuta cada vez que se quiera agregar un nuevo nodo al cluster.


##### PRIMERO: 
instalar snap:  sudo apt install snap -y && sudo apt install snapd -y

##### SEGUNDO: 
instalar microk8s: sudo snap install microk8s --classic

##### TERCERO 
crear el nodo maestro y conectarle los workes 

### Configuración del NFS 
##### PRIMERO:
Step 1: Installing NFS Kernel Server
To set up the NFS server, you first need to install the NFS Kernel server package on your Ubuntu system. Open your terminal and execute the following command:

sudo apt update
sudo apt install nfs-kernel-server
Step 2: Creating the Export Directory
After installing the necessary package, the next step is to create a directory that you wish to share with your clients. For this guide, we will use /var/nfs/general (though you can choose any directory as per your requirements).

sudo mkdir -p /var/nfs/general
sudo chown nobody:nogroup /var/nfs/general
Step 3: Configuring NFS Exports
Once the directory is ready, you must edit the /etc/exports file to make it available to clients. Open the file with your preferred text editor:

sudo nano /etc/exports
Add the following line to the file, which specifies the directory to share, the client, and the sharing options:

/var/nfs/general    client_ip(rw,sync,no_subtree_check)
Replace client_ip with the IP address of the client machine. If you want to specify multiple clients, add additional lines or use wildcards and/or subnets.

Step 4: Exporting the Shared Directory
After configuring the exports, apply the changes by running the exportfs command:

sudo exportfs -a
Step 5: Starting and Enabling the NFS Server
Start the NFS server and ensure that it will start automatically on boot:

sudo systemctl start nfs-kernel-server
sudo systemctl enable nfs-kernel-server

##### SEGUNDO:
Create a Persistent Volume (PV) 
Create a Persistent Volume Claim (PVC)### configuración de las instancias 
##### PRIMERO: 
instalar snap:  sudo apt install snap -y && sudo apt install snapd -y

##### SEGUNDO: 
instalar microk8s: sudo snap install microk8s --classic

##### TERCERO 
crear el nodo maestro y conectarle los workes 

### Configuración del NFS 
##### PRIMERO:
Step 1: Installing NFS Kernel Server
To set up the NFS server, you first need to install the NFS Kernel server package on your Ubuntu system. Open your terminal and execute the following command:

sudo apt update
sudo apt install nfs-kernel-server
Step 2: Creating the Export Directory
After installing the necessary package, the next step is to create a directory that you wish to share with your clients. For this guide, we will use /var/nfs/general (though you can choose any directory as per your requirements).

sudo mkdir -p /var/nfs/general
sudo chown nobody:nogroup /var/nfs/general
Step 3: Configuring NFS Exports
Once the directory is ready, you must edit the /etc/exports file to make it available to clients. Open the file with your preferred text editor:

sudo nano /etc/exports
Add the following line to the file, which specifies the directory to share, the client, and the sharing options:

/var/nfs/general    client_ip(rw,sync,no_subtree_check)
Replace client_ip with the IP address of the client machine. If you want to specify multiple clients, add additional lines or use wildcards and/or subnets.

Step 4: Exporting the Shared Directory
After configuring the exports, apply the changes by running the exportfs command:

sudo exportfs -a
Step 5: Starting and Enabling the NFS Server
Start the NFS server and ensure that it will start automatically on boot:

sudo systemctl start nfs-kernel-server
sudo systemctl enable nfs-kernel-server

##### SEGUNDO:
Create a Persistent Volume (PV) 
Create a Persistent Volume Claim (PVC)

## referencias:
- [Instalación y configuración de MicroK8s](https://microk8s.io/docs/getting-started)
- [Configuración cluster de kubernetes](https://microk8s.io/docs/clustering)
