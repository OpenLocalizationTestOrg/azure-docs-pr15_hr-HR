<properties 
    pageTitle="Poveznici za tvrtke Business i API aplikacije u aplikacijama logike | Microsoft Azure" 
    description="Upute za stvaranje i konfiguriranje poveznici za uređivanje, EDIFACT, AS2 i TPM; microservices arhitekture" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>Poveznici za tvrtke Business i aplikacije API-JA

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Logika aplikacije sadrži mnogo BizTalk API aplikacijama koje su ključne za integraciju okruženja. Ove aplikacije API temelje se na koncepata i alata koji se koriste u BizTalk Server, ali Zasad su dostupni kao dio logike aplikacije. 

Jedna kategorija te aplikacije API su aplikacije API Business-to-Business (B2B). Pomoću ove aplikacije B2B API-JA, možete jednostavno dodavanje partnera, stvaranje ugovore pa učinite sve kako bi lokalnog pomoću uređivanje, AS2 i EDIFACT.  

Ove aplikacije API B2B nude mogućnosti "Okidača" i "Akcije". Okidač pokreće novu instancu ovisno o određenim događajima, kao što su dolazak do X12 poruke od partnera. Akciju je rezultat, kao što su nakon primanja programa X12 poruku, a zatim pošaljite poruku koristeći AS2.


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>Što je poveznik za tvrtke Business i aplikacija za API-JA
Značajka Business-to-Business (B2B) obuhvaća postojeće, ugrađene aplikacije API koje omogućuju različita poduzeća, odjelima, aplikacije i tako dalje za komunikaciju putem AS2, uređivanje i EDIFACT. 

API aplikacije B2B obuhvaćaju sljedeće: 

Poveznik ili aplikacije API-JA | Opis
--- | ---
Upravljanje partnera trgovina BizTalk | Aplikacija API stvara business-to-business (B2B) odnosa pomoću partnera i ugovore. Korištenje te odnose na AS2, EDIFACT i X12 protokol.<br/><br/>Aplikacija za API TPM je obavezne osnovni poveznik AS2 i u X12 ili EDIFACT API aplikacije. 
Poveznik za AS2 | Poveznik koji prima i šalje poruke pomoću AS2 prijenosa. Poveznik prijenosi podataka sigurno i pouzdano putem Interneta.
BizTalk EDIFACT | Aplikaciju API koja prima i šalje poruke pomoću EDIFACT. EDIFACT se obično naziva Poništavanje/EDIFACT (Sjedinjenih Američkih Država države/elektroničke podataka za razmjenu za administraciju, trgovine i prijenosa) i široko koristi preko delatnosti.
BizTalk X12 | Aplikaciju API koja prima i šalje poruke pomoću na X12 protokol. X12 se obično naziva ASC X 12 (Accredited standarde Committee X12) i široko koristi preko delatnosti. 


Pomoću tih API aplikacija, nego što dovršite različite uređivanje poruka zadatke. Na primjer, pomoću poveznika AS2, možete sigurno primati i slati različite vrste poruka (Uređivanje, XML, ravnomjerne datoteke, i tako dalje) klijentu, odjel unutar tvrtke kao što su ljudskih resursa ili svima koji koristi AS2. 

Možete stvoriti proizvoljan broj aplikacije API kao želite te ih jednostavno stvaranje. Možete koristiti i jednostruko API aplikacije unutar više scenariji ili tijekovi rada.

To možete učiniti bez pisanja kod. Započnimo. 


## <a name="requirements-to-get-started"></a>Preduvjeti za početak rada
Prilikom stvaranja aplikacije B2B API-JA postoje neki obavezni resursi. Stavke moraju se stvoriti koje ste prije mogu se koristiti u drugim aplikacijama API-JA. Tim preduvjetima obuhvaćaju sljedeće: 

Preduvjet | Opis
--- | ---
Baze podataka Azure SQL | Sprema B2B stavke obuhvaća partnera, sheme, potvrde i agreeements. Svake od aplikacija API B2B zahtijeva vlastitu baze podataka SQL Azure. <br/><br/>**Napomena** Kopirajte niz za povezivanje s bazom podataka.<br/><br/>[Stvaranje baze podataka Azure SQL](../sql-database/sql-database-get-started.md)
Azure spremnik spremište blobova platforme | Služi za pohranu poruka svojstava kada je omogućen AS2 arhiviranje. Ako ne trebate AS2 poruke arhiviranje, spremnik za pohranu nije potreban. <br/><br/>**Napomena** Ako ste omogućili korištenje arhiviranje, kopirajte niz za povezivanje ovo spremište blobova platforme.<br/><br/>[O računima Azure prostora za pohranu](../storage/storage-create-storage-account.md)
Polje naziva Bus servisa i vrijednosti ključa | Sprema X12 i EDIFACT grupnog slanja promjena podataka. Ako ne trebate grupnog slanja promjena prostora za naziv Bus servisa nije potreban.<br/><br/>**Napomena** Ako ste omogućili korištenje grupnog slanja promjena, kopirajte ove vrijednosti.<br/><br/>[Stvaranje servisa Bus prostor naziva](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM instanci | Da biste stvorili poveznik za AS2 i X12 ili EDIFACT API aplikacije potreban je instanca komponente BizTalk trgovina partnera Management (TPM). Prilikom stvaranja aplikacije za API TPM, stvarate instancu TPM. <br/><br/>**Napomena** Znate naziv aplikacije TPM API-JA. 


## <a name="create-the-api-apps"></a>Stvaranje aplikacije API-JA
B2B API aplikacije mogu se kreirati pomoću portala za Azure ili pomoću REST API-ji. 


### <a name="create-the-api-apps-using-rest-apis"></a>Stvaranje aplikacije API pomoću REST API-ji
[Potražite u dokumentaciji o korištenju REST API-ji.](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>Stvaranje aplikacije API B2B na portalu za Azure
Na portalu Azure možete stvoriti B2B API aplikacije prilikom stvaranja aplikacije logike, web-aplikacijama ili mobilne aplikacije. Ili možete stvoriti pomoću vlastitu plohu. Oba načina su jednostavni tako što ovisi o vaše potrebe ili Preference. Neki korisnici radije da prvo stvorite sve aplikacije API B2B sa svojim konkretnih svojstava. Zatim stvorite logike aplikacije/web-aplikacijama Mobile aplikacije i dodavanje aplikacija API B2B koju ste stvorili.  

Sljedeći koraci stvorite aplikacije za API na B2B pomoću plohu API aplikacije.


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>Stvaranje BizTalk trgovina partnera Management (TPM) API aplikacije

> [AZURE.NOTE] Da biste stvorili poveznik za AS2 i X12 ili EDIFACT API aplikacije potreban je instanca komponente BizTalk trgovina partnera Management (TPM). Prilikom stvaranja aplikacije za API TPM, stvarate instancu TPM.

Na sljedeći način stvoriti instancu TPM:

1. Azure portalu Startboard (Početna stranica) odaberite **trgovine**. **API aplikacije** navedene sve postojeće aplikacije API-JA i poveznika. Možete i **pretraživanje** za određene aplikacije API B2B.
2. Odaberite **BizTalk trgovina upravljanje partnera**. U novi plohu odaberite **Stvori**. 
3. Unesite svojstva: 

    Svojstvo | Opis
--- | ---
Ime | Unesite bilo koji naziv TPM instance. Na primjer, možete ga nazvati *AccountsPayableTPM*.
Postavke paketa | Unesite ADO.NET **Baze podataka niz za povezivanje** s bazom podataka SQL Azure koju ste stvorili. <br/><br/>Kada kopirate niz za povezivanje, lozinka ne dodaje se niz za povezivanje. Pripazite da unesete lozinku u nizu za povezivanje.
Aplikacije servisa za planiranje | Popis tarifa plaćanja. Možete promijeniti ako je potrebno više ili manje resursa.
Cijene sloju | Svojstvo samo za čitanje s popisom cijene kategoriju unutar Azure pretplatu. 
Grupa resursa | Stvorite novu ili postojeću grupu. Sve aplikacije API-JA i poveznici za logike aplikacije, web-aplikacije i mobilne aplikacije mora biti u istoj grupi resursa. <br/><br/>[Pomoću grupa resursa](../azure-resource-manager/resource-group-overview.md) objašnjava to svojstvo. 
Pretplate | Svojstvo samo za čitanje s popisom trenutne pretplate.
Mjesto | Zemljopisnu lokaciju koja hostira Azure service. 
Dodavanje Startboard | Potvrdite ovaj okvir da biste dodali aplikaciju API B2B Starboard (Početna stranica).

4. Odaberite **Stvori**. 

Nakon stvaranja API Aplikaciju TPM (TPM Instance), zatim možete stvoriti poveznik AS2 i/ili na X12 ili EDIFACT API aplikacije. 


#### <a name="create-the-as2-connector"></a>Stvaranje AS2 poveznika

1. Azure portalu Startboard (Početna stranica) odaberite **trgovine**. **API aplikacije** navedene sve postojeće aplikacije API-JA i poveznike. Možete i **pretraživanje** za određene aplikacije API B2B.
2. Odaberite **AS2 poveznik**. U novi plohu odaberite **Stvori**. 
3. Unesite svojstva: 

    Svojstvo | Opis
--- | ---
Ime | Unesite naziv neke poveznik AS2. Na primjer, možete ga nazvati *AS2Connector*.
Postavke paketa | Unesite posebne postavke na tu aplikaciju za API-JA, kao što su naziv TPM Instance. <br/><br/>Potražite u članku [Dodavanje postavke paketa AS2](#AddAS2Conn) u ovoj temi za konkretnih svojstava. 
Aplikacije servisa za planiranje | Popis tarifa plaćanja. Možete promijeniti ako je potrebno više ili manje resursa.
Cijene sloju | Svojstvo samo za čitanje s popisom cijene kategoriju unutar Azure pretplatu. 
Grupa resursa | Stvorite novu ili postojeću grupu. [Pomoću grupa resursa](../azure-resource-manager/resource-group-overview.md) objašnjava to svojstvo. 
Pretplate | Svojstvo samo za čitanje s popisom trenutne pretplate.
Mjesto | Zemljopisnu lokaciju koja hostira Azure service. 
Dodavanje Startboard | Potvrdite ovaj okvir da biste dodali aplikaciju API B2B Starboard (Početna stranica).

    **<a name="AddAS2Conn"></a>Poveznik za AS2 postavke paketa**

    Svojstvo | Opis
--- | --- 
Niz za povezivanje baze podataka | Unesite ADO.NET niz za povezivanje s bazom podataka SQL Azure koju ste stvorili. Kada kopirate niz za povezivanje, lozinka ne dodaje se niz za povezivanje. Pripazite da unesete lozinku u nizu za povezivanje prije lijepljenja.
Omogućivanje arhiviranja za dolazne poruke | Neobavezno. Omogućivanje tog svojstva za pohranu svojstva poruke dolazne poruke AS2 primljenih od partnera. 
Niz za povezivanje spremište blobova platforme Azure  | Unesite niz za povezivanje spremište blobova platforme Azure spremniku koji ste stvorili. Kada je omogućen arhiviranje, kodiranih i dešifrirani porukama spremaju se u ovom spremniku prostora za pohranu.
Naziv Instance TPM | Unesite naziv za **Upravljanje partnera trgovina BizTalk** API aplikacije koje ste prethodno stvorili. Kada stvorite AS2 poveznika, ovaj poveznik izvršava samo ugovore AS2 unutar tu instancu TPM.

4. Odaberite **Stvori**. 


#### <a name="create-the-x12-or-edifact-api-apps"></a>Stvaranje aplikacije API X12 ili EDIFACT

1. Azure portalu Startboard (Početna stranica) odaberite **trgovine**. **API aplikacije** navedene sve postojeće aplikacije API-JA i poveznika. Možete i **pretraživanje** za određene aplikacije API B2B.
2. Odaberite **BizTalk X 12** ili **BizTalk EDIFACT**. U novi plohu odaberite **Stvori**. 
3. Unesite svojstva: 

    Svojstvo | Opis
--- | ---
Ime | Unesite naziv bilo koju aplikaciju B2B API-JA. Na primjer, možete ga nazvati *EDI850APIApp*.
Postavke paketa | Unesite posebne postavke na tu aplikaciju za API-JA, kao što su naziv TPM Instance. <br/><br/>Potražite u članku [X12 ili postavke paketa EDIFACT](#AddX12) u ovoj temi za konkretnih svojstava. 
Aplikacije servisa za planiranje | Popis plan plaćanja. Možete promijeniti ako je potrebno više ili manje resursa.
Cijene sloju | Svojstvo samo za čitanje s popisom cijene kategoriju unutar Azure pretplatu. 
Grupa resursa | Stvorite novu ili postojeću grupu. [Pomoću grupa resursa](../azure-resource-manager/resource-group-overview.md) objašnjava to svojstvo. 
Pretplate | Svojstvo samo za čitanje s popisom trenutne pretplate.
Mjesto | Zemljopisnu lokaciju koja hostira Azure service. 
Dodavanje Startboard | Potvrdite ovaj okvir da biste dodali aplikaciju API B2B Starboard (Početna stranica).

    **<a name="AddX12"></a>X12 i EDIFACT postavke paketa aplikacije API-JA**  

    Svojstvo | Opis
--- | --- 
Niz za povezivanje baze podataka | Unesite ADO.NET niz za povezivanje s bazom podataka SQL Azure koju ste stvorili. Kada kopirate niz za povezivanje, lozinka ne dodaje se niz za povezivanje. Pripazite da unesete lozinku u nizu za povezivanje prije lijepljenja.
Polje naziva Bus servisa | Unesite mjesto za naziv Bus servis koji ste stvorili. Potreban je samo kada grupnog slanja promjena omogućena. 
Naziv servisa Bus prostor naziva zajedničko korištenje tipki pristupa | Unesite servisa Bus prostora za naziv tipkovni prečac koji ste stvorili. Potreban je samo kada grupnog slanja promjena omogućena. 
Vrijednost servis Bus prostor naziva zajedničko korištenje tipki pristupa | Unesite Bus servisa naziva tipki pristupa vrijednost koju ste stvorili. Potreban je samo kada grupnog slanja promjena omogućena. 
Naziv Instance TPM | Unesite naziv za **Upravljanje partnera trgovina BizTalk** API aplikacije koje ste prethodno stvorili. Kada stvorite na X12 ili EDIFACT API aplikacije, aplikacija API izvršava samo ugovore X12/EDFIACT unutar tu instancu TPM.

4. Odaberite **Stvori**. 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>Dodavanje partnera, ugovore, potvrde i sheme 
Na portalu Azure otvorite aplikaciju TPM API-JA. U odjeljku **komponente** dodajte partnera, ugovore, potvrde i sheme. 

Također možete dodati ugovore AS2 poveznika, X12 API aplikacije i EDIFACT API aplikacija. 


## <a name="monitor-your-api-apps"></a>Praćenje aplikacija u API-JA
Na portalu Azure otvorite aplikaciju TPM API-JA. U odjeljku **operacija** možete vidjeti različite upravljanje operacije. Na primjer, možete:

- Prikaz Informational i pogreškama događaja
- Prikaz memorije korištenja i niti broj procesu (w3wp)
- U zapisnicima računala i web-poslužitelj

Na više [monitora logike aplikacije](app-service-logic-monitor-your-logic-apps.md).


## <a name="add-the-api-apps-to-your-application"></a>Dodavanje aplikacija API-JA u aplikaciji 
Servis za aplikaciju Microsoft Azure izlaže vrste drugu aplikaciju koje možete koristiti te aplikacije B2B API-JA. Možete stvoriti novu ili Dodavanje postojećih aplikacija API B2B logike aplikacije i mobilne aplikacije ili u web-aplikacije. 

U aplikaciji, jednostavno odaberete API aplikacijama B2B iz galerije automatski dodaje je u aplikaciju.  

> [AZURE.IMPORTANT] Da biste dodali poveznika i API aplikacije koje ste prethodno stvorili, stvorite logike aplikacije, mobilne aplikacije ili web-aplikacije u istoj grupi resursa. 

Sljedeći koraci dodavanje aplikacija API B2B logike aplikacije, mobilne aplikacije ili web-aplikacije: 

1. Azure portalu Startboard (Početna stranica) idite na **tržištu**i pretraživanje za svoje logike, Mobile ili web-aplikacije. 

    Ako stvarate novu aplikaciju pretraživanja logiku aplikacije, mobilne aplikacije ili web-aplikacije. Odaberite aplikaciju, a novi plohu odaberite **Stvori**. [Stvaranje aplikacije za logiku](app-service-logic-create-a-logic-app.md) navedeni koraci. 

2. Otvorite aplikaciju i odaberite **okidača i akcija**. 

3. Iz **galerije**odaberite aplikaciju za API B2B koji se automatski dodaje u aplikaciju. Možete stvoriti i novu aplikaciju B2B API-JA.

    > [AZURE.IMPORTANT] Poveznik za AS2 i X12, EDIFACT API aplikacije potreban je TPM Instance. Da ako stvarate novih aplikacija B2B API-JA, prvo stvorite aplikaciju TPM API-JA, a zatim stvorite AS2 poveznik, X12 API aplikacije ili EDIFACT API aplikacije. 

4. Odaberite **u redu** da biste spremili promjene. 

>[AZURE.NOTE] Da biste započeli s aplikacijama logike Azure prije registracije za Azure račun, [Pokušajte logike aplikacije](https://tryappservice.azure.com/?appservice=logic). Odmah možete stvoriti aplikaciju za logiku short-lived starter. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="more-b2b-resources"></a>Dodatni resursi B2B

[Stvaranje B2B postupka](app-service-logic-create-a-b2b-process.md)<br/>
[Stvaranje poslovnih partnerom](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[Što su poveznika i aplikacije BizTalk API-JA](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>Saznajte više o logike aplikacije i web-aplikacije
[Što su logike aplikacije?](app-service-logic-what-are-logic-apps.md)<br/>
[Web-mjesta i Web Apps u aplikacije servisa za Azure](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>Dodatne poveznika

[Popis aplikacija za API-JA i poveznika](app-service-logic-connectors-list.md)<br/><br/>
[Što su poveznika i aplikacije BizTalk API-JA](app-service-logic-what-are-biztalk-api-apps.md) 
