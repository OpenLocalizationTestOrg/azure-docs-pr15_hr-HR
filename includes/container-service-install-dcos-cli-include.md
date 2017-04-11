<properties
   pageTitle="Instalirati operacijski sustav Kontroler/EŽA | Microsoft Azure"
   description="Instalirajte Kontroler/OS EŽA."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Spremnika, Micro services Kontroler/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Ovo je za rad s klastere ACS Kontroler/OS-poštu. Nema potrebe da biste to učinili za klastere utemeljen na Swarm ACS.

Prvo [Povezivanje s svoj klaster Kontroler/OS sustavom ACS](../articles/container-service/container-service-connect.md). Kada to učinite, možete instalirati EŽA Kontroler/OS na klijentskom računalu s naredbama u nastavku:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Ako koristite stariju verziju programa Python, možda ćete primijetiti neke "InsecurePlatformWarnings". Sigurno možete zanemariti te.

Da biste početak bez ponovnog pokretanja sustava ljuske pokrenuti:

```bash
source ~/.bashrc
```

Ovaj korak nije bit će potrebno kada započnete novi ljuske.

Sada možete provjeriti je li instaliran na EŽA:

```bash
dcos --help
```