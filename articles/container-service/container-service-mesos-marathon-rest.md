<properties
   pageTitle="Azure Upravljanje servisom spremnik spremnik kroz REST API-JA | Microsoft Azure"
   description="Implementacija spremnika programa Azure spremnik servisa Mesos klaster pomoću Marathon REST API-JA."
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

# <a name="container-management-through-the-rest-api"></a>Upravljanje spremnik kroz REST API-JA

Kontroler/OS nudi okruženja za implementaciju i skaliranje grupirani radnih opterećenja tijekom abstracting podlozi hardvera. Pri vrhu Kontroler/OS, postoji framework koja upravlja planiranje i izvršavanja radnih opterećenja računalnim.

Iako okviri su dostupni za mnoge popularne radnih opterećenja, ovaj dokument u članku se opisuje kako možete stvoriti i skaliranje implementacijama spremnik pomoću Marathon. Prije radu putem u ovim se primjerima potrebno Kontroler/OS klaster koji je konfiguriran u servisu Azure kontejner. Morate koristiti remote connectivity u ovoj grupi. Dodatne informacije o tih stavki potražite u sljedećim člancima:

- [Implementacija programa servisa Azure spremnik klaster](container-service-deployment.md)
- [Povezivanje programa servisa Azure spremnik klaster](container-service-connect.md)

Kada ste povezani s servisa Azure spremnik klaster, možete pristupiti Kontroler/OS i povezane REST API-ji putem http://localhost:local priključka. Primjeri u ovom dokumentu pretpostavlja da su tuneliranje na priključak 80. Na primjer, krajnju točku Marathon proslijediti poziv `http://localhost/marathon/v2/`. Dodatne informacije o različitim API-ji potražite u članku Mesosphere dokumentaciji [Marathon API -JA](https://mesosphere.github.io/marathon/docs/rest-api.html) i [Chronos API -JA](https://mesos.github.io/chronos/docs/api.html)i [Mesos raspored API](http://mesos.apache.org/documentation/latest/scheduler-http-api/)potražite u dokumentaciji Apache.

## <a name="gather-information-from-dcos-and-marathon"></a>Prikupite informacije iz Kontroler/OS i Marathon

Prije implementacije spremnika klaster Kontroler-OS, prikupite neke informacije o klaster Kontroler-OS, kao što su nazivi i trenutni status agenata Kontroler/OS. Da biste to učinili, upit s `master/slaves` krajnjoj točci Kontroler/OS REST API-JA. Ako je sve dobro prošlo, vidjet ćete popis agenata Kontroler/OS i nekoliko svojstava za svaki unos.

```bash
curl http://localhost/mesos/master/slaves
```

Sada pomoću u Marathon `/apps` krajnju točku da biste provjerili trenutni implementacijama aplikacije klaster Kontroler/OS. Ako je ovo novi klaster, vidjet ćete prazno polje za aplikacije.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Implementacija spremniku Docker oblikovani

Implementacija Docker oblikovani spremnika kroz Marathon pomoću datoteke JSON koji opisuje svrhu implementacije. Sljedeća Ogledna će implementirati spremnik Nginx povezivanja priključak 80 Kontroler/OS agenta za priključak 80 spremnika. Imajte na umu da je svojstvo 'acceptedResourceRoles' postavljeno na "slave_public". To će implementirati spremnik agent u skupu skaliranje agent dostupnom javnosti.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Da biste implementirali spremniku Docker oblikovani, stvorite JSON datoteke ili iskoristite ugrađene ogledne na [servis spremnik Azure pokazni videozapis](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Spremite ga na pristupačno mjesto. Nakon toga za implementaciju spremnik, pokrenite sljedeću naredbu. Navedite naziv datoteke JSON.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Izlaz bit će otprilike ovako:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Sada ako Marathon upit za aplikacije, ova novu aplikaciju prikazat će u izlaz.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Promjena veličine na spremnika

Možete koristiti i Marathon API radi skaliranja izgleda i promjena veličine u implementacijama aplikacije. U prethodnom primjeru uvode se jedna instanca aplikacije. Pogledajmo skaliranje ovaj Izlaz na tri instance programa. Da biste to učinili, stvorite JSON datoteka pomoću sljedećih JSON teksta i spremite ga na pristupačno mjesto.

```json
{ "instances": 3 }
```

Pokrenite sljedeću naredbu da biste skalirali out aplikacije.

>[AZURE.NOTE] URI bit će http://localhost/marathon/v2/apps/, a zatim ID aplikacije za promjenu veličine. Ako koristite uzorak Nginx koji je naveden u nastavku, URI bi http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Na kraju, upit Marathon krajnja točka za aplikacije. Vidjet ćete da sada postoje tri spremnika Nginx.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Korištenje ljuske PowerShell za ovu vježbu: REST API-JA Marathon interakcija sa servisom PowerShell

Ove iste akcije možete izvršiti pomoću naredbe ljuske PowerShell u sustavu Windows.

Da biste Prikupite informacije o klaster Kontroler-OS, kao što su nazivi agent i agent status, pokrenite sljedeću naredbu.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Implementacija Docker oblikovani spremnika kroz Marathon pomoću datoteke JSON koji opisuje svrhu implementacije. Sljedeća Ogledna će implementirati spremnik Nginx povezivanja priključak 80 Kontroler/OS agenta za priključak 80 spremnika.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Stvorite JSON datoteke ili iskoristite ugrađene ogledne na [servis spremnik Azure pokazni videozapis](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Spremite ga na pristupačno mjesto. Nakon toga za implementaciju spremnik, pokrenite sljedeću naredbu. Navedite naziv datoteke JSON.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Možete koristiti i Marathon API radi skaliranja izgleda i promjena veličine u implementacijama aplikacije. U prethodnom primjeru uvode se jedna instanca aplikacije. Pogledajmo skaliranje ovaj Izlaz na tri instance programa. Da biste to učinili, stvorite JSON datoteka pomoću sljedećih JSON teksta i spremite ga na pristupačno mjesto.

```json
{ "instances": 3 }
```

Pokrenite sljedeću naredbu da biste skalirali out aplikacije.

> [AZURE.NOTE] URI bit će http://localhost/marathon/v2/apps/, a zatim ID aplikacije za promjenu veličine. Ako koristite uzorka Nginx navedene u nastavku, URI bi http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Daljnji koraci

- [Dodatne informacije o krajnje točke Mesos HTTP za čitanje]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Dodatne informacije o Marathon REST API -JA za čitanje]( https://mesosphere.github.io/marathon/docs/rest-api.html).
