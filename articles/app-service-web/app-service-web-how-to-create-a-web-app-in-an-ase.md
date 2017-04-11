<properties
    pageTitle="Stvaranje web-aplikacijama u okruženju servisa aplikacija"
    description="Saznajte kako stvoriti web-aplikacije i aplikacije servisa tarife u okruženju servisa aplikacija"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Stvaranje web-aplikacijama u okruženju servisa aplikacija

## <a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako stvoriti web-aplikacije i aplikacije servisa za tarife u [Aplikaciju servisa okruženje](app-service-app-service-environment-intro.md) (elika i mala slova). 

> [AZURE.NOTE] Ako želite saznati kako stvoriti web-aplikacijama, ali ne morate učiniti u okruženju servisa aplikacije, potražite u članku [Stvaranje web-aplikacijama .NET](web-sites-dotnet-get-started.md) ili jednu od povezanih vodiče za druge jezike i okviri.

## <a name="prerequisites"></a>Preduvjeti

Pomoću ovog praktičnog vodiča pretpostavlja da ste stvorili okruženju aplikacije servisa. Ako dosad niste koji još, potražite u članku [Stvaranje okruženja aplikacije servisa](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Stvaranje web-aplikacijama

1. [Portal za Azure](https://portal.azure.com/)kliknite **Novo > Web + Mobile > web-aplikacije**. 

    ![][1]

2. Odaberite pretplatu.  

    Ako imate više pretplata Imajte na umu da da biste stvorili aplikacije u okruženju sustava aplikacije servisa, morate koristiti istu pretplatu koju ste koristili prilikom stvaranja okruženja. 

3. Odaberite ili stvorite grupu resursa.

    *Grupa resursa* omogućuju upravljanje povezane Azure resurse kao jedinica i su korisne prilikom uspostave *Kontrola pristupa na temelju uloga* (RBAC) pravila za aplikacije. Dodatne informacije potražite u članku [Upravljanje Azure resursa][ResourceGroups]. 

4. Odaberite ili stvorite plan sustava aplikacije servisa.

    *Aplikacije servisa za tarife* upravlja skupova web-aplikacije.  Obično kad odaberete cijene, cijena naplatiti primjenjuje se na željenu tarifu aplikacije servisa, a ne u pojedinačne aplikacije. U programa elika i mala slova plaćate instanci računalnim dodijeliti u elika i mala slova umjesto naveden s vašeg ASP.  Skaliranja broj instanci web-aplikacijama proširenja instanci aplikacije servisa plan, i to utječe na sve web-aplikacije u tom plan.  Neke značajke kao što su slobodnih web-mjesta ili VNET Integracija imati i ograničenja količina unutar plan.  Dodatne informacije potražite u članku [Pregled tarife aplikacije servisa za Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    Tarife za aplikacije servisa u vašem elika i mala slova možete prepoznati tako da pogledate mjesto na koje je navedeno u odjeljku naziv plana.  

    ![][5]

    Ako želite koristiti tarifu aplikacije servisa za koja već postoji u svom okruženju aplikacije servisa, odaberite ga. Ako želite da biste stvorili novu tarifu aplikacije servisa, u sljedećem odjeljku ovog praktičnog vodiča, [Stvaranje tarifu aplikacije servisa u okruženju aplikacije servisa](#createplan).

5. Unesite naziv za web-aplikacije, a zatim kliknite **Stvori**. 

    Je li vaš elika i mala slova koristi vanjski VIP URL aplikacije u programa elika i mala slova: [*NazivWebMjesta*]. [*naziv vaše okruženje servisa aplikacija*]. p.azurewebsites.net umjesto [*NazivWebMjesta*]. azurewebsites.net
    
    Ako vaš elika i mala slova koristi interni VIP, a zatim URL aplikacije u tom je elika i mala slova: [*NazivWebMjesta*]. [*poddomene navedeni tijekom stvaranja elika i mala slova*]   
    Kada odaberete vaše ASP tijekom stvaranja elika i mala slova vidjet ćete poddomene ažuriranje ispod **naziva**

## <a name="createplan"></a>Stvaranje tarifu aplikacije servisa

Kada stvorite plan sustava aplikacije servisa u okruženju aplikacije servisa, izbore radnih razlikuju se kao postoje bez zaposlenici zaduženi za zajedničke programa elika i mala slova.  Zaposlenici zaduženi, morate koristiti se oni koji su u administrator dodijelio da biste na elika i mala slova  To znači da da biste stvorili novu tarifu, morate koristiti više zaposlenici zaduženi za dodijeliti vaše elika i mala slova radnih grupa aplikacija od ukupan broj instanci preko svih planove već u tom tempiranja.  Ako nemate dovoljno zaposlenici zaduženi za vaše elika i mala slova radnih grupa aplikacija da biste stvorili tarifa, morate raditi s elika i mala slova administratora da biste ih dodali.

Drugi razlika aplikacije servisa za tarife koje hostira tvrtka okruženju servisa aplikacija je nedostatak cijene odabira.  Kada imate okruženju aplikacije servisa platili se za računalnim resursa koji se koriste za sustav i nemaju dodane naknade za planovi u tom okruženju.  Obično prilikom stvaranja tarifu aplikacije servisa odaberite plan cijene koji određuje naplatom.  Okruženju servisa aplikacija je zapravo privatne mjesto gdje možete stvarati sadržaj.  Plaćate za okruženje, a ne hostira vaš sadržaj.

Sljedeće upute pokazati kako stvoriti tarifu aplikacije servisa za dok stvarate web-aplikacijama kao što je opisano u prethodnom odjeljku vodič.

1. Kliknite **Stvori novu** tarifu odabira korisničkog Sučelja i navedite naziv tarifa kao što to obično činite izvan programa elika i mala slova.

2. Odaberite elika i mala slova koji želite koristiti iz alata za odabir mjesta.

    Jer okruženju servisa aplikacija je zapravo mjesto privatne implementaciju, prikazuje se u odjeljku lokacije. 

    ![][2]

    Nakon odabira se elika i mala slova u alatu za odabir mjesta, stvaranje plana aplikacije servisa korisničkog Sučelja ažurira.  Mjesto sada prikazuje naziv sustava elika i mala slova i područje je u i cijene birač plan je zamijenjena funkcijom radnih grupa aplikacija za odabir.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Odabir radnih skup

Obično u aplikacije servisa za Azure i izvan okruženju servisa aplikacija ne postoje 3 računalnim veličine koje su dostupne s odabranim plan namjenski cijena.  Na sličan način za programa elika i mala slova možete definirati do 3 grupe zaposlenici zaduženi za i odredite veličinu računalnim koji se koristi za tu grupu tempiranja.  Što to znači da za klijenata na elika i mala slova umjesto da je odabira cijene plan s računalnim veličine za plan aplikacije servisa, odaberite što se naziva *radnih grupa aplikacija*.  

Odabir grupe aplikacija radnih korisničkog Sučelja prikazuje veličinu računalnim koristi za tu grupu radnih ispod naziva.  Količina dostupne se odnosi na izračunati koliko instanci su dostupni za upotrebu u tu grupu.  Ukupna skup mogu biti dostupne dodatne instance od taj broj, ali tu vrijednost se odnosi na jednostavno koliko se ne koriste.  Ako vam je potrebna da biste prilagodili okruženja aplikacije servisa da biste dodali više izračunati resursa potražite u članku [Konfiguriranje okruženja aplikacije servisa](app-service-web-configure-an-app-service-environment.md).

![][4]

U ovom primjeru vidjeti samo dva tempiranja grupe dostupne. To je zato što administrator elika i mala slova samo dodijeliti domaćini u ta dva tempiranja grupe.  Treći bi prikazivati kada postoje VMs dodijeliti u nju.  

## <a name="after-web-app-creation"></a>Nakon stvaranja web app

Postoji nekoliko zahtjevi za pokretanje web-aplikacije i upravljanje planove aplikacije servisa za elika i mala slova koje treba uzeti u obzir.  

Kao što je naznačeno ranije, vlasnik u elika i mala slova je odgovoran za veličinu sustava i kao rezultat su i odgovoran za osiguravanje ima li dovoljno kapaciteta za hostiranje željeni aplikacije servisa za tarife. Ako postoje bez zaposlenici zaduženi za dostupna, nećete moći stvoriti plan aplikacije servisa.  To je TRUE skaliranje gore web-aplikaciju programa.  Ako vam je potrebna više instanci zatim promijenile da biste dobili administratoru okruženja aplikacije servisa da biste dodali zaposlenici zaduženi za više.

Nakon stvaranja web-aplikacije i aplikacije servisa za planiranje je dobro mjerilo prema gore.  U programa elika i mala slova uvijek morate imati barem 2 instanci aplikacije servisa za tarife nudi kvara odstupanje za aplikacije.  Promjena veličine tarifu aplikacije servisa u programa elika i mala slova je isti kao običnu putem aplikacije servisa za planiranje korisničkog Sučelja.  Dodatne informacije o skaliranje, [upute za promjenu veličine web app u okruženju servisa aplikacija](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
