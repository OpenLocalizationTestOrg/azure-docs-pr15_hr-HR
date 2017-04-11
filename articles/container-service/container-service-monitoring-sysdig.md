<properties
   pageTitle="Praćenje programa klaster servisa Azure kontejner s Sysdig | Microsoft Azure"
   description="Praćenje programa klaster servisa Azure kontejner s Sysdig."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Azure spremnika, Kontroler/OS"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Praćenje programa klaster servisa Azure kontejner s Sysdig

U ovom se članku smo će uvesti agenata Sysdig sve agent čvorove svoj klaster servisa Azure kontejner. Morate imati račun kod Sysdig za tu konfiguraciju. 

## <a name="prerequisites"></a>Preduvjeti 

[Uvođenje](container-service-deployment.md) i [Povezivanje](container-service-connect.md) klaster konfigurirao servisa Azure kontejner. Upoznavanje s [Marathon korisničkog Sučelja](container-service-mesos-marathon-ui.md). Idite na [http://app.sysdigcloud.com](http://app.sysdigcloud.com) da biste postavili račun za oblak Sysdig. 

## <a name="sysdig"></a>Sysdig

Sysdig je servis nadzora koja omogućuje praćenje vaše spremnika u svoj klaster. Sysdig poznato je da biste olakšali rješavanje problema, ali ima i osnovni nadzora mjernih podataka za procesora, povezivanje s mrežom, memorije i/i. Sysdig olakšava da biste vidjeli spremnike rade na hardest ili zapravo pomoću najviše memorije i procesora. Taj se prikaz nalazi u odjeljku "Pregled", koja je trenutno u beta. 

![Sysdig korisničkog Sučelja](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Sysdig implementaciju konfigurirali Marathon

Korake u nastavku vode kroz možete konfigurirati i implementaciju aplikacija Sysdig svoj klaster s Marathon. 

Pristup Kontroler/OS korisničko Sučelje preko [http://localhost:80 /](http://localhost:80/) jednom u korisničkom Sučelju Kontroler/OS dođite do s "Universe", koja je u donjem lijevom kutu, a zatim potražite "Sysdig."

![Sysdig u Universe Kontroler/OS](./media/container-service-monitoring-sysdig/sysdig1.png)

Sada da biste dovršili konfiguraciju potreban račun za oblak Sysdig ili besplatnu probnu računa. Kada ste prijavljeni na web-mjesto za oblak Sysdig, kliknite korisničko ime, a na stranici trebali biste vidjeti "Pristup ključa." 

![Ključ za Sysdig API](./media/container-service-monitoring-sysdig/sysdig2.png) 

Zatim unesite tipka za pristup u konfiguraciji Sysdig unutar Universe Kontroler/OS. 

![Konfiguracija Sysdig u Universe Kontroler/OS](./media/container-service-monitoring-sysdig/sysdig3.png)

Postavljanje sada instanci 10000000 tako da svaki put kada se dodaje novi čvor klaster Sysdig će se automatski implementirati agent za taj novi čvor. To je privremeno rješenje da biste bili sigurni da će sve nove agenata unutar klaster implementacija Sysdig. 

![Konfiguracija Sysdig u Kontroler/OS Universe-instanci](./media/container-service-monitoring-sysdig/sysdig4.png)

Kada instalirate paket idite natrag Sysdig korisničkog Sučelja i ćete moći Istraživanje metriku različite korištenje spremnika u svoj klaster. 