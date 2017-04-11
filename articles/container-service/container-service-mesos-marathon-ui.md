<properties
   pageTitle="Azure Upravljanje servisom spremnik spremnik putem korisničkog Sučelja web | Microsoft Azure"
   description="Implementacija spremnika sa servisom Azure spremnik servisa klaster korištenjem web Marathon korisničkog Sučelja."
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
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Upravljanje spremnik putem weba korisničkog Sučelja

Kontroler/OS nudi okruženja za implementaciju i skaliranje grupirani radnih opterećenja tijekom abstracting podlozi hardvera. Pri vrhu Kontroler/OS, postoji framework koja upravlja planiranje i izvršavanja radnih opterećenja računalnim.

Dok okviri su dostupne za mnoge popularne radnih opterećenja, ovaj dokument će se opisuju kako možete stvoriti i skaliranje implementacijama kontejner s Marathon. Prije radu putem u ovim se primjerima, trebat će vam Kontroler/OS klaster koji je konfiguriran u servisu Azure kontejner. Morate koristiti remote connectivity u ovoj grupi. Dodatne informacije o tih stavki potražite u sljedećim člancima:

- [Implementacija programa servisa Azure spremnik klaster](container-service-deployment.md)
- [Povezivanje programa servisa Azure spremnik klaster](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Istražite Kontroler/OS korisničkog Sučelja

S tunelom sigurne ljuske (SSH) koja se uspostaviti, pronađite http://localhost/. Time se učitava web Kontroler/OS korisničkog Sučelja i prikazuje informacije o klaster, kao što su korištenih resursa, aktivni agenata i pokrenuti servisi.

![KONTROLER/OS KORISNIČKOG SUČELJA](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Upoznavanje s Marathon korisničkog Sučelja

Da biste vidjeli Marathon korisničko Sučelje, pronađite http://localhost/Marathon. S ovog zaslona možete započeti novi spremnik ili neku drugu aplikaciju klaster Azure Kontroler u servis spremnik/OS. Možete vidjeti i informacije o pokretanju spremnika i aplikacije.  

![Marathon korisničkog Sučelja](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Implementacija spremniku Docker oblikovani

Da biste implementirali novi spremnik pomoću Marathon, kliknite gumb **Stvori aplikacija** , a zatim unesite sljedeće podatke u obrazac:

Polje           | Vrijednost
----------------|-----------
ID-A              | nginx
Slika           | nginx
Mreža         | Njegove
Priključak glavnog računala       | 80
Protokol        | TCP

![Novu aplikaciju korisničkog Sučelja – Općenito](media/dcos/dcos4.png)

![Novu aplikaciju korisničkog Sučelja – spremnika Docker](media/dcos/dcos5.png)

![Novu aplikaciju korisničkog Sučelja – priključci i otkrivanje servisa](media/dcos/dcos6.png)

Ako želite statički mapiranje priključak kontejner s priključkom na agenta, morate je korištenje JSON načina. Da biste to učinili, promijenite čarobnjaka za novu aplikaciju u **Način rada JSON** pomoću tog prekidača. Unesite sljedeće u odjeljku s `portMappings` dio definiciju aplikacije. U ovom se primjeru povezuje priključak 80 spremnika priključak 80 agent Kontroler/OS. Nakon što napravite ove promjene koje možete prijeći čarobnjaka izašli iz načina JSON.

```none
"hostPort": 80,
```

![Novu aplikaciju korisničkog Sučelja – priključak 80 primjer](media/dcos/dcos13.png)

Klaster Kontroler/OS uvode se sa skupom privatni i javni agenata. Za klaster omogućiti pristup aplikacijama putem Interneta, morate implementacija aplikacije da biste javno agent. Da biste to učinili, odaberite karticu **Dodatno** čarobnjaka za novu aplikaciju, a zatim unesite **slave_public** za **Prihvaćen uloge resursa**.

![Novu aplikaciju korisničkog Sučelja – javno agent za postavljanje](media/dcos/dcos14.png)

Natrag na glavnoj stranici Marathon, možete vidjeti status implementacije spremnik.

![Marathon glavne stranice korisničkog Sučelja – spremnika status uvođenja](media/dcos/dcos7.png)

Kada se prebacite natrag na webu Kontroler/OS korisničkog Sučelja (http://localhost/), vidjet ćete da na klaster Kontroler/OS nije ostalo zadatka (u ovom slučaju spremniku Docker oblikovani).

![Kontroler/OS web-Sučelja – zadataka koji se izvode na klaster](media/dcos/dcos8.png)

Možete pogledati i čvor klaster zadatak na kojoj se izvršava.

![Kontroler/OS web-Sučelja – čvor klaster zadatka](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Promjena veličine na spremnika

Da biste skalirali broj instanci spremniku, poslužite se Marathon korisničkog Sučelja. Da biste to učinili, idite na stranicu **Marathon** , odaberite kontejner u koju želite za promjenu veličine i kliknite gumb **Promjena veličine** . U dijaloškom okviru **Aplikacije mjerilo** unesite broj instanci spremnik koje želite pa odaberite **Skaliranje aplikacije**.

![Marathon korisničkog Sučelja – dijaloški okvir skaliranje aplikacije](media/dcos/dcos10.png)

Kada završi postupak mjerilo, vidjet ćete više instanci isti zadatak dodijeliti većem agenata Kontroler/OS.

![Nadzorne ploče korisničkog Sučelja web Kontroler/OS – zadatka Podjela preko agenata](media/dcos/dcos11.png)

![Korisničko Sučelje – čvorove web-Kontroler/OS](media/dcos/dcos12.png)

## <a name="next-steps"></a>Daljnji koraci

- [Rad s Kontroler/OS i Marathon API-JA](container-service-mesos-marathon-rest.md)

Precizno dive servis spremnik za Azure s Mesos

> [AZURE. Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos VIDEOZAPIS]]
