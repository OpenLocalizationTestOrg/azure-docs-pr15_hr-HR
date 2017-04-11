<properties
    pageTitle="Stvaranje Docker hosts u Azure s računala Docker | Microsoft Azure"
    description="U članku se opisuje korištenje Docker računala da biste stvorili docker domaćini u Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Pomoću računala Docker upravljački program za Azure

[Docker](https://www.docker.com/) je jedan od najpopularnijih virtualizacije pristupa koja koristi Linux spremnika umjesto virtualnim strojevima kao način izoliranja podataka aplikacije i računalstvo zajedničkog resursa. U ovoj se temi opisuju kada i kako se koristi [Računalo Docker](https://docs.docker.com/machine/) (u `docker-machine` naredba) da biste stvorili novi Linux VMs u Azure omogućen kao docker glavno računalo za vaše spremnika Linux.


## <a name="create-vms-with-docker-machine"></a>Stvaranje VMs s Docker računala

Stvaranje docker VMs glavno računalo u Azure s na `docker-machine create` naredba pomoću na `azure` argument upravljački program za mogućnost upravljački program (`-d`) i sve argumente. 

U sljedećem primjeru oslanja nakon zadane vrijednosti, ali ga otvoriti priključak 80 na VM s Internetom da biste testirali s spremnik nginx, čini `ops` korisnička prijava za SSH i pozive novi VM `machine`. 

Vrsta `docker-machine create --driver azure` da biste vidjeli mogućnosti i njihove zadane vrijednosti; možete čitati i u [dokumentaciji upravljački program za Azure Docker](https://docs.docker.com/machine/drivers/azure/). (Imajte na umu da imate li omogućena je provjera autentičnosti dvofaktorska analiza varijance, zatražit će se provjeriti autentičnost drugi faktor.)

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Rezultat izgleda otprilike ovako, ovisno o tome imati dvostruka provjera autentičnosti konfigurirana u vašem računu.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Konfiguriranje ljuske za docker

Sada upišite `docker-machine env <VM name>` da biste vidjeli što vam je potrebno učiniti da biste konfigurirali ljuske. 

```bash
docker-machine env machine
```

Koji se ispisuje informacije o okruženju, koja izgleda otprilike ovako. Imajte na umu IP adresu vam je dodijeljen, koje je potrebno provjeriti na VM.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Pokretati naredbu predložene konfiguraciju ili možete postaviti varijable okruženja sami. 

## <a name="run-a-container"></a>Pokretanje spremnik

Sada možete pokrenuti jednostavni web-poslužitelju da biste testirali li sve funkcionira ispravno. Ovdje smo koristi standardnu nginx sliku, odrediti treba li preslušali na priključak 80 i da ako se pokreće na VM spremnik potrebno ponovno pokrenuti kao i (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Rezultat izgleda otprilike ovako:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testiranje spremnik

Pregledajte izvodi spremnika pomoću `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

I provjerite izvodi spremnik upišite `docker-machine ip <VM name>` da biste pronašli IP adrese (Ako ste zaboravili iz na `env` naredba):

![Tekući ngnix spremnik](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Daljnji koraci

Ako želite, isprobajte Azure [Docker VM proširenje](virtual-machines-linux-dockerextension.md) u istom operacije pomoću EŽA Azure ili predlošci upravitelja Azure resursa. 

Više primjera rad s Docker potražite u članku [Rad s Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) iz povezivanje [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [pokazni videozapis](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Dodatne početak rada s HealthClinic.biz videozapis, potražite u članku [Početak rada za alate za razvojne inženjere za Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

