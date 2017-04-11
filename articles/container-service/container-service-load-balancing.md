<properties
   pageTitle="Učitavanje saldo spremnika u klasteru servisa Azure spremnik | Microsoft Azure"
   description="Učitavanje saldo preko više spremnika u skupini programa Azure spremnik servisa."
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
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Učitavanje saldo spremnika u klasteru servisa Azure spremnik

U ovom se članku smo ćete Istraživanje stvaranje internog opterećenja u na Kontroler/OS upravlja servis spremnik Azure pomoću Marathon LB. To će vam omogućiti da biste skalirali aplikacija vodoravno. To će omogućiti da iskoristite prednost agent za javne i privatne klastere potvrđivanjem vaše balancers učitavanja javnim klaster i vaše aplikacije spremnika na privatne klaster.

## <a name="prerequisites"></a>Preduvjeti

[Uvođenje instanca servisa Azure kontejner](container-service-deployment.md) s vrstom orchestrator Kontroler/OS i [Provjerite je li vaš klijent možete povezati svoj klaster](container-service-connect.md). 

## <a name="load-balancing"></a>Za ujednačavanje opterećenja

Postoje dva slojeve za ujednačavanje opterećenja servisa spremnik klasteru bismo će stvoriti: 

  1. Azure opterećenja nudi javnim stavka točke (one koje će kliknite krajnjim korisnicima). Servis spremnik Azure navedeni automatski i po zadanom, konfigurirati da biste otkrili priključak 80 i 443 8080.
  2. Opterećenja Marathon (marathon lb) usmjerava dolazni zahtjevi za spremnik instance koje te zahtjevi za uslugu. Kao što smo skaliranje spremnika pruža naš web-servisa, marathon lb dinamički prilagođavati im. Ovaj opterećenja nije naveden po zadanom na servisu spremnik, ali vrlo je jednostavno da biste instalirali.

## <a name="marathon-load-balancer"></a>Marathon opterećenja

Marathon opterećenja dinamički reconfigures sam spremnika koji ste implementiran na temelju. Preporučuje se i prebacuju do gubitka spremnik ili agent – ako se to dogodi, Apache Mesos jednostavno ponovno pokrenite kontejner s nekog drugog mjesta, a marathon lb će prilagoditi.

Da biste instalirali opterećenja Marathon možete koristiti bilo Kontroler/OS-a web-Sučelja ili naredbenog retka.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Instalacija Marathon LB pomoću korisničkog Sučelja Web Kontroler/OS

  1. Kliknite 'Universe'
  2. Traženje "Marathon LB"
  3. Kliknite "Instaliraj"

![Instaliranje marathon lb putem web-sučelja Kontroler/OS](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Instalacija Marathon LB pomoću EŽA Kontroler/OS

Nakon instalacije EŽA Kontroler/OS i osiguravanje možete se povezati s svoj klaster, pokrenite sljedeću naredbu na klijentskom računalu:

```bash
dcos package install marathon-lb
```

Ta se naredba automatski instalira raspoređivača opterećenja na klasteru javno agenata.

## <a name="deploy-a-load-balanced-web-application"></a>Implementacija opterećenjem raspoređen web-aplikacije

Imamo marathon lb paketa, ne možemo možete implementirati u spremniku aplikacije koji želimo da biste učitali saldo. U ovom primjeru će ćemo pomoću sljedećih konfiguracije implementacija jednostavne web-poslužitelju:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Postavite vrijednost od `HAProxy_0_VHOST` da biste FQDN opterećenja za vaše agenata. Ovo je u obliku `<acsName>agents.<region>.cloudapp.azure.com`. Ako, primjerice, stvaranje spremnika servisa klaster s nazivom `myacs` u regiji `West US`, FQDN bio `myacsagents.westus.cloudapp.azure.com`. To možete pronaći i pomoću tražite opterećenja s "agent" u nazivu kada tražite kroz resurse u grupi resursa koji ste stvorili za spremnik servisa [Azure portal](https://portal.azure.com).
  * Postavite na servicePort priključak > = 10 000. To označava servis koji je pokrenut u ovom spremniku – marathon lb koristi za prepoznavanje servise koje je potrebno saldo preko.
  * Postavljanje na `HAPROXY_GROUP` natpis na "vanjski".
  * Postavljanje `hostPort` 0. To znači da Marathon arbitrarily dodijeliti Slobodan priključak.
  * Postavljanje `instances` broj instanci koju želite stvoriti. Možete uvijek promijenite li ih prema gore i dolje kasnije.

To vrijedi noing po zadanom će Marathon implementacija privatne klaster, to znači da iznad implementacije samo bit će dostupne putem raspoređivača opterećenja koji se obično ponašanje smo žele.

### <a name="deploy-using-the-dcos-web-ui"></a>Implementacija pomoću korisničkog Sučelja Web Kontroler/OS

  1. Posjetite stranicu Marathon na http://localhost/marathon (nakon postavljanja [SSH tunelom](container-service-connect.md) i kliknite`Create Appliction`
  2. U na `New Application` kliknite dijaloški okvir `JSON Mode` u gornjem desnom kutu
  3. Zalijepite iznad JSON u uređivaču
  4. Kliknite`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Implementacija pomoću EŽA Kontroler/OS

Za implementaciju aplikaciju s EŽA Kontroler/OS jednostavno kopirajte iznad JSON u datoteku pod nazivom `hello-web.json`, i pokretanje:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure opterećenja

Prema zadanim postavkama Azure opterećenja izlaže priključke 80, 8080 i 443. Ako koristite neku od ove tri priključaka (kao smo u gornjem primjeru), a zatim je morate učiniti ništa. Trebali biste moći kliknite FQDN vaše agent raspoređivača opterećenja – i svaki put kada osvježite koje ćete kliknite jednu od poslužitelja tri web-kružnog način. Međutim, ako koristite drugi priključak, morate dodati pravilo kružnog a na probni na opterećenja za priključak koji ste koristili. To možete učiniti iz na [EŽA Azure](../xplat-cli-azure-resource-manager.md), s naredbama `azure network lb rule create` i `azure network lb probe create`. To možete učiniti i pomoću portala za Azure.


## <a name="additional-scenarios"></a>Dodatni scenariji

Može imati scenarij u kojem koristite različitih domena da biste vidjeli različite servise. Ako, na primjer:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Da biste to postigli, pogledajte [virtualne domaćini](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), koji omogućuje povezivanje domena na određene marathon lb putove.

Možete nije izložiti različite priključke, a ponovno mapirajte sa servisom točan iza marathon lb. Ako, na primjer:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Daljnji koraci

Potražite dodatne na [marathon lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/)potražite u dokumentaciji Kontroler/OS.
