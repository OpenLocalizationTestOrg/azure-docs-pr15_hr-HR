<properties 
   pageTitle="Bilješke o izdanju Azure SDK za .NET 2.5.1" 
   description="Bilješke o izdanju Azure SDK za .NET 2.5.1" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Bilješke o izdanju Azure SDK za .NET 2.5.1

Ovaj dokument sadrži napomene u za Azure SDK za .NET 2.5.1 izdanja. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK za .NET 2.5.1 napomene

U nastavku su nove značajke i ažuriranja u Azure SDK za .NET 2.5.1.

- Novi features\scenarios vezane uz **Extensions alata za Web**. 

    - Azure web-mjesta preimenovana je u aplikacije servisa za Azure. Dodatne informacije potražite u člancima, [aplikacije servisa za Azure i postojeće Azure usluge](app-service-changes-existing-services.md).
    - Podrška za Azure API aplikacije (pretpregled) dodan je tako da klijenti možete objaviti ASP.NET projekte kao API aplikacije, a zatim pomoću dodavanje > gesta Azure API aplikacije klijenta u C# projekata generirati kod koji se temelji na strukturu distribuiranih aplikacije API. 
    - Uspije čvor aplikacije servisa za Azure koja sadrži podršku za resursa u grupne grupiranje Azure API aplikacije, mobilne aplikacije i web-aplikacijama zastarjela čvor web-mjesta u programu Explorer poslužitelja.
    - Podrška za Azure aplikacije Mobile (pretpregled) dodana je tako da korisnici mogu stvaranje novih projekata mobilne aplikacije, dodavanje kontrolera mobilne aplikacije, objavite projekata i daljinski ispravljanje pogrešaka aplikacije.
    - Dodavanje > Azure API aplikacije klijent gesta sada podržava lokalne datoteke Swagger JSON, pa razvojnim inženjerima Web API pomoću NuGets drugih proizvođača, kao što su Swashbuckle možete generirati Swagger ili ga ručno autor. Na taj način razvojnim inženjerima klijenta možete koristiti značajke kod generacije trošiti krajnjoj točki sve Swagger C# projekata. 
    - Web App i objavljivanje u dijaloškim okvirima API aplikacije poboljšani su za podršku pojam Azure Portal resursa grupiranja i odabira/stvaranja grupe resursa Azure i tarife za aplikaciju servisa prikazane su u nove Web-aplikacije i API dodjele resursa dijaloškom okviru. 
    - Azure API aplikacije poslužitelja Explorer čvorove nudimo veze na vezu precizno API aplikacije u Portal za Azure, kao i druge značajke kao što su strujanje zapisnika i daljinsko uklanjanje programskih pogrešaka.

    Poznati problemi i trenutnog ograničenja Azure SDK .NET 2.5.1 [ovaj](app-service-release-notes.md#known_issues_2_5_1) odjeljak u nastavku.


- Novi features\scenarios vezane uz **HDInsight Alati** u Visual Studio omogućeni u ovom izdanju. 
    - Lokalni provjere valjanosti grozd skripti. Kliknite gumb Provjeri valjanost skripte na alatnoj traci da biste vidjeli postoje li neke pogreške u skriptu. 
    - Poboljšani ispravljanje pogrešaka grozd zadataka. Možete sada ispraviti pogreške grozd zadatke tako da pristupite Yarn zapisnika u Visual Studio. Ako aplikacija ima probleme s performansama, istražuje YARN zapisnika pronaći ćete korisne informacije...
    - (Javno pretpregled) Podrška za grozd ključnu riječ-Samodovršetak i IntelliSense. Da biste lakše autor skripte grozd, HDInsight alate za Visual Studio dodali ključnu riječ-Samodovršetak i IntelliSense podrške za grozd.
    - Podrška za oluja. Sada možete koristiti alate za HDInsight za Visual Studio za razvoj oluja topologija/Spouts/Vijci u C#. Možete poslati razvijen topologije oluja klaster i vidjeti stanje topologije/munje/spout. Zapisnici klijenta i zapisnika sustava možete koristiti za otklanjanje poteškoća s vašeg oluja topologija/Vijci/Spouts. Postojeće JAVA sredstvima možete koristiti i u oluja na HDInsight.
    
    Dodatne informacije potražite u članku [Prvi koraci pri korištenju HDInsight Hadoop alate za Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).



##<a id="known_issues_2_5_1"></a>Azure SDK za .NET 2.5.1 Poznati problemi i ograničenja

- Azure API aplikacija je vidljiv kao cilj implementacije za mobilne aplikacije. Web Apps mora biti odredište samo za mobilne aplikacije do sljedeće izdanje. 
- Dodjeljivanje API aplikacije za Azure rezultiraju uspjeh mogu, ali povremeno se neće ažurirati napredak u prozoru Azure aplikacije servisa aktivnosti. Zaobilazno rješenje jest da biste provjerili status novu aplikaciju API Azure na portalu za Azure. 
- Datoteka > novi projekt > aplikacije API > doživljaj F5 rezultira pogreškom HTTP jer nema default/index.html. Zaobilazno rješenje jest ručno pronađite /api/values URL-a. 
- Povremeno, poslužitelj Explorer ikona plošnu. Dodavanje veze za VANJSKIH ponovnim rješava taj problem. 
- Ako je izbačena tijekom Web App ili aplikacije API dodjeljivanja (kao što su prekoračena kvota za pogreške ili duplikat naziva pristupnika Azure API aplikaciju), pogreške Pokaži neke neobrađenog teksta JSON. 
- Problemi Povremeni stvaranjem projekta kada aplikacija uvida u vrijeme stvaranja projekta.
- Ponekad generirani kod Azure API aplikacije klijent nema prostora naziva, moraju se ručno uključiti (ili automatski uvezeni putem Visual Studio signala) za kodove prikupiti. 
- Projekti mobilnu aplikaciju će biti objavljena na web-aplikacije, ali morate odabrati web-mjesta koji ste stvorili kao mobilnu aplikaciju na portalu za Azure jer mobilnu aplikaciju projekata potrebna je baza podataka. 
- Početna stranica za mobilne aplikacije koristi izraz "mobilne usluge" umjesto "mobilne aplikacije" 
- Stvaranje projekta za mobilnu aplikaciju može potrajati nekoliko minuta da biste stvorili. 
- Tijekom API aplikacije dodjele resursa (u nekim slučajevima) pogreška potječe natrag o Azure API da dozvole nije moguće postaviti ispravno, dok aplikaciju API ispravno dodjeli i spreman je za korištenje. Možete ručno postaviti dozvole pomoću portala za Azure.
- Aplikacija uvida nije podržana na aplikaciju API predloške i predloške za mobilnu aplikaciju.
- Projekti API aplikacija ne mogu se koristiti u kombinaciji s projektima u Oblaku.
- Predlošci projekta API aplikacija su dostupne samo u C#.
- API aplikacije potrošnje putem kontekstnog izbornika "Dodavanje Azure API aplikacije klijent" podržano je samo u C#.

 
