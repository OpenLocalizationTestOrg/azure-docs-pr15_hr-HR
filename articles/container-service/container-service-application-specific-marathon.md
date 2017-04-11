<properties
   pageTitle="Aplikaciju ili servis Marathon specifične za korisnika | Microsoft Azure"
   description="Stvaranje aplikacije ili servisa Marathon specifične za korisnika"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Spremnika, Marathon, Micro-servisima, Kontroler-OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Stvaranje aplikacije ili servisa Marathon specifične za korisnika

Azure Service spremnik nudi skup glavnog poslužitelja na kojem smo prethodno konfigurirati Apache Mesos i Marathon. To se može koristiti za orkestrirali aplikacija na klaster, ali preporučuje se da nećete koristiti poslužitelje sustava osnovne u tu svrhu. Na primjer, tweaking konfiguracije Marathon potreban zapisivanje u glavni poslužitelje same i mijenjanje – to potiče jedinstveni osnovne poslužitelje koji su nešto drukčiji iz standardni i morate cared za i upravlja neovisno. Uz to, konfiguracije potrebnih jednog tima možda neće biti optimalnih konfiguracija za drugom timu.

U ovom se članku smo ćete objašnjavaju kako dodati aplikaciju ili servis Marathon specifične za korisnika.

Budući da jedan korisnik ili timu će pripadati taj servis, su besplatno da biste konfigurirali na način koji su žele. Osim toga, servisa Azure spremnik li servis nastavljaju se za pokretanje. Ako servis ne uspije, servisa Azure spremnik je će pokrenuti umjesto vas. U većini slučajeva koje nećete čak i obratite pozornost na to je imala isključiti.

## <a name="prerequisites"></a>Preduvjeti

[Uvođenje instanca servisa Azure kontejner](container-service-deployment.md) s vrstom orchestrator Kontroler/OS i [Provjerite je li vaš klijent možete povezati svoj klaster](container-service-connect.md). Osim toga, učinite sljedeće korake.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Stvaranje aplikacije ili servisa Marathon specifične za korisnika

Započnite stvaranjem JSON konfiguracijska datoteka koja definira naziv aplikacije servisa koji želite stvoriti. Ovdje koristimo `marathon-alice` nazivu framework. Spremite datoteku u obliku nešto poput `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Nakon toga pomoću EŽA Kontroler/OS da biste instalirali instancu Marathon s mogućnostima koji se postavljaju u konfiguracijskoj datoteci:

```bash
dcos package install --options=marathon-alice.json marathon
```

Sada trebali biste vidjeti vaše `marathon-alice` usluge pokrenut na kartici usluge vaše korisničko Sučelje Kontroler/OS. Bit će korisničko Sučelje `http://<hostname>/service/marathon-alice/` ako želite da pristupaju izravno.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Postavljanje EŽA Kontroler/OS za pristup servisu

Po želji možete konfigurirati na Kontroler/OS EŽA da biste pristupili taj novi servis tako da postavite na `marathon.url` da postavite pokazivač na svojstvo na `marathon-alice` instancu na sljedeći način:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Možete provjeriti koji pojavljivanja Marathon koji vaše EŽA radi odnosu s na `dcos config show` naredbe. Vratite se na osnovne uslugu Marathon pomoću naredbe `dcos config unset marathon.url`.
