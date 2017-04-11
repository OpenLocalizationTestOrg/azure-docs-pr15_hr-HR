<properties
   pageTitle="Praćenje programa klaster servisa Azure kontejner s Datadog | Microsoft Azure"
   description="Praćenje programa klaster servisa Azure kontejner s Datadog. Korištenje Kontroler/OS web korisničkog Sučelja za implementaciju agenata Datadog za svoj klaster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Spremnika, Kontroler/OS, Docker Swarm Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Praćenje programa klaster servisa Azure kontejner s Datadog

U ovom članku smo će uvesti agenata Datadog sve agent čvorove svoj klaster servisa Azure spremnik. Račun s Datadog ćete za tu konfiguraciju. 

## <a name="prerequisites"></a>Preduvjeti 

[Uvođenje](container-service-deployment.md) i [Povezivanje](container-service-connect.md) klaster konfigurirao servisa Azure kontejner. Upoznavanje s [Marathon korisničkog Sučelja](container-service-mesos-marathon-ui.md). Idite na [http://datadoghq.com](http://datadoghq.com) da biste postavili Datadog račun. 

## <a name="datadog"></a>Datadog 

Datadog je servis za nadzor koji prikuplja nadzora podatke iz svoje spremnika u svoj klaster servisa Azure kontejner. Datadog ima Docker Integracija nadzorne ploče komponente gdje možete vidjeti određene metrike unutar vaše spremnika. Metriku prikupili iz vaše spremnika organiziranih prema procesora, memorije, mreže i/i. Datadog dijeli metriku spremnika i slike. Primjer kako korisničko Sučelje izgleda za korištenje središnjih procesora je ispod.

![Datadog korisničkog Sučelja](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Datadog implementaciju konfigurirali Marathon

Korake u nastavku vode kroz možete konfigurirati i implementaciju aplikacija Datadog svoj klaster s Marathon. 

Pristup Kontroler/OS korisničko Sučelje preko [http://localhost:80 /](http://localhost:80/). Jednom u korisničkom Sučelju Kontroler/OS dođite do s "Universe" koja je u donjem lijevom kutu pa potražite "Datadog" i kliknite "Instaliraj".

![Paket Datadog unutar Universe Kontroler/OS](./media/container-service-monitoring/datadog1.png)

Sada da biste dovršili konfiguraciju ćete Datadog računa ili pomoću računa za probno razdoblje. Kada ste prijavljeni Datadog izgleda web-mjesta s lijeve strane i idite na integracije -> zatim API. 

![Ključ za Datadog API](./media/container-service-monitoring/datadog2.png)

Zatim unesite ključ za API-JA u konfiguraciji Datadog unutar Universe Kontroler/OS. 

![Konfiguracija Datadog u Universe Kontroler/OS](./media/container-service-monitoring/datadog3.png) 

U gornjem konfiguraciji instance postavljene na 10000000 tako da svaki put kada se novi čvor dodaje se klaster Datadog će automatski implementacija agent taj čvor. To je privremeno rješenje. Kada instalirate paket treba vratite se na web-mjestu Datadog i pronaći "Nadzornih ploča". Iz nje vidjet ćete prilagođeni i integracija nadzorne ploče. Nadzorna ploča za integraciju Docker će imati sva mjerenja spremnik vam je potrebno za nadzor svoj klaster. 
