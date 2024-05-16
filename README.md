# proyecto2 tópicos de telemática 

### configuración de las instancias 
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
