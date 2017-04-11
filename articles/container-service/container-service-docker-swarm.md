<properties
   pageTitle="Azure Upravljanje servisom spremnik kontejner s Docker Swarm | Microsoft Azure"
   description="Implementacija spremnika Docker Swarm u servisu Azure spremnik"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, spremnika, Micro-servisima, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-with-docker-swarm"></a>Upravljanje kontejner s Docker Swarm

Docker Swarm nudi okruženja za implementaciju containerized radnih opterećenja u skupu rezultata zajedničke Docker domaćini. Docker Swarm koristi nativni Docker API-JA. Tijek rada za upravljanje spremnika na Docker Swarm je gotovo jednako što bi na jedan spremnik glavno računalo. Ovaj dokument sadrži jednostavnog primjera implementacija containerized radnih opterećenja u instanca servisa Azure spremnik Docker Swarm. Detaljnije dokumentaciju na Docker Swarm potražite u članku [Docker Swarm na Docker.com](https://docs.docker.com/swarm/).

Preduvjeti za vježbe u ovom dokumentu:

[Stvaranje Swarm klaster u servisu Azure spremnik](container-service-deployment.md)

[Povezivanje s klaster Swarm u servisu Azure spremnik](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Implementacija novi spremnik

Da biste stvorili novi spremnik u Docker Swarm, koristite na `docker run` naredba (osiguravanje otvorene programa tunelom SSH s matricama po preduvjeti gore). U ovom se primjeru stvara kontejnera s na `yeasy/simple-web` slike:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Nakon stvaranja spremnik pomoću `docker ps` da biste se vratili informacije o spremnik. Obratite pozornost na to ovdje Swarm agent koji se nalaze spremnik nalazi:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Sada možete pristupiti u aplikaciji koja se izvodi u ovom spremniku putem javne DNS naziva Swarm agent opterećenja. Ove informacije možete pronaći na portalu za Azure:  


![Realni posjetiti rezultata](media/real-visit.jpg)  

Prema zadanim postavkama raspoređivača opterećenja sadrži priključke 80, 8080 i 443 Otvori. Ako se želite povezati na neki drugi priključak morat ćete otvoriti tu priključak na opterećenja Azure za skup Agent.

## <a name="deploy-multiple-containers"></a>Implementacija više spremnika

Kao što je više spremnika pokrenute, izvršavanjem "docker cilja" više puta, možete koristiti u `docker ps` naredbu da biste vidjeli koji hostira spremnike su pokrenuti na. U primjeru u nastavku tri spremnika širi ravnomjerno preko tri agenata Swarm:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Implementacija spremnika pomoću sastavite Docker

Sastavite Docker možete koristiti da biste automatizirali implementaciji i konfiguraciji više spremnika. Da biste to učinili, provjerite sigurne ljuske (SSH) tunelom je stvorena te da varijabla DOCKER_HOST postavljen (vidi iznad stara requisites).

Stvaranje datoteke docker compose.yml na lokalnom sustavu. Da biste to učinili, koristite ovaj [uzorka](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Pokrenite `docker-compose up -d` da biste pokrenuli implementacijama spremnik:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Na kraju, na popisu izvršenja spremnika vratit će se. Ovaj popis odražava spremnika koje su uvedene pomoću Docker sastavite:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Prirodno, možete koristiti `docker-compose ps` da biste pregledali samo spremnika definirano u svoje `compose.yml` datoteku.

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o Docker Swarm](https://docs.docker.com/swarm/)
