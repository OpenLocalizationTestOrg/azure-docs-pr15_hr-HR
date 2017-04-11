<properties
    pageTitle="Uređivač za JavaScript DocumentDB skripte Exploreru | Microsoft Azure"
    description="Saznajte više o DocumentDB Explorer skripte Azure Portal alat za upravljanje poslužiteljskim programiranje artefakte DocumentDB obuhvaća pohranjene procedure, okidača i korisnički definirane funkcije."
    keywords="Uređivač za JavaScript"
    services="documentdb"
    authors="kirillg"
    manager="jhubbard"
    editor="monicar"
    documentationCenter=""/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="kirillg"/>

# <a name="create-and-run-stored-procedures-triggers-and-user-defined-functions-using-the-documentdb-script-explorer"></a>Stvaranje i pokretanje pohranjene procedure, okidača i korisnički definirane funkcije pomoću programa Explorer za skripte DocumentDB

Ovaj članak sadrži pregled programa Explorer [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) skriptu koja je uređivač za JavaScript na portalu za Azure koja omogućuje prikaz i izvršiti DocumentDB poslužiteljsko programiranje artefakte obuhvaća pohranjene procedure, okidača i korisnički definirane funkcije. Dodatne informacije potražite u o DocumentDB poslužiteljsko programiranja u članku [procedure spremljene, okidača baze podataka i UDF-ove](documentdb-programming.md) .

## <a name="launch-script-explorer"></a>Pokretanje programa Explorer skripte

1. Na portalu Azure u Jumpbar, kliknite **DocumentDB (NoSQL)**. Ako **DocumentDB računa** nije vidljivo, kliknite **Više servisa** , a zatim kliknite **DocumentDB (NoSQL)**.

2. Na izborniku resurse kliknite **Skripta Explorer**.

    ![Snimka zaslona s naredbom Explorer skripte](./media/documentdb-view-scripts/scriptexplorercommand.png)
 
    U **bazu podataka** i **Zbirka** padajućeg popisa unaprijed popunjavaju ovisno o kontekstu pokrenuti skriptu Explorer.  Ako, na primjer, ako pokretanje iz baze podataka plohu zatim Trenutna baza podataka je prethodno popunjen.  Pokretanje iz zbirke plohu tada trenutne zbirke je prethodno popunjen.

4.  Pomoću okvira padajućeg popisa **baze podataka** i **Zbirka** jednostavno promijeniti zbirke iz kojeg skripte su trenutno gleda bez potrebe za zatvorite i ponovno pokrenite skripte Explorer.  

5. Skripta Explorer podržava i filtriranje trenutno učitani skup skripte njihove svojstvo ID-a.  Jednostavno upišite u okvir filtar i rezultata na popisu Explorer skripte su filtrirani prema navedenom kriterije.

    ![Snimka zaslona programa skripte Explorer s rezultatima filtriranja](./media/documentdb-view-scripts/scriptexplorerfilterresults.png)


    > [AZURE.IMPORTANT] Skripta Explorer filtriranje funkcionalnost samo filtre iz skupa ***trenutno*** učitati skripte i ne automatsko osvježavanje trenutno odabranu zbirku.

5. Da biste osvježili popis skripte učitati Explorer skripte, jednostavno kliknite naredbu **Osvježi** pri vrhu na plohu.

    ![Snimka zaslona Explorer za skripte naredbe Osvježi](./media/documentdb-view-scripts/scriptexplorerrefresh.png)


## <a name="create-view-and-edit-stored-procedures-triggers-and-user-defined-functions"></a>Stvaranje, prikaz i uređivanje pohranjene procedure, okidača i korisnički definirane funkcije

Skripta Explorer omogućuje jednostavno izvođenje operacije CRUD DocumentDB poslužiteljsko programiranje artefakte.  

- Da biste stvorili skriptu, jednostavno kliknite na odgovarajuće stvorite naredbe u programu explorer skripte, unijeti ID-ove, unesite sadržaj skripte i kliknite **Spremi**.

    ![Snimka zaslona Explorer za skripte mogućnost Stvori, prikazuje JavaScript uređivača](./media/documentdb-view-scripts/scriptexplorercreatecommand.png)

- Prilikom stvaranja okidač, morate navesti okidača operacija vrsta i okidača

    ![Snimka zaslona Explorer za skripte stvorili mogućnost okidača](./media/documentdb-view-scripts/scriptexplorercreatetrigger.png)

- Da biste pogledali skriptu, jednostavno kliknite skripte u kojem koji vas zanima.

    ![Snimka zaslona skripte Explorer prikaz skripte sučelje](./media/documentdb-view-scripts/scriptexplorerviewscript.png)

- Da biste uredili skriptu, jednostavno unesite željene promjene u JavaScript uređivač i kliknite **Spremi**.

    ![Snimka zaslona skripte Explorer prikaz skripte sučelje](./media/documentdb-view-scripts/scriptexplorereditscript.png)

- Da biste odbacili sve promjene na čekanju u skriptu, jednostavno kliknite naredbu **Odbaci** .

    ![Snimka zaslona Explorer za skripte Odbaci promjene sučelja](./media/documentdb-view-scripts/scriptexplorerdiscardchanges.png)

- Skripta Explorer omogućuje i jednostavno prikazati svojstva sustava trenutno učitani skripte klikom na naredbu **Svojstva** .

    ![Prikaz svojstava skripte snimka Explorer za skripte](./media/documentdb-view-scripts/scriptproperties.png)

    > [AZURE.NOTE] Svojstvo vremenske oznake (_ts) interno predstavljen kao epoch vremena, ali skripte Explorer prikazuje vrijednost u obliku Ljudski čitljiv GMT.

- Da biste izbrisali skriptu, odaberite ga u programu Explorer skripte i kliknite naredbu za **Brisanje** .

    ![Snimka zaslona Explorer za skripte naredba Izbriši](./media/documentdb-view-scripts/scriptexplorerdeletescript1.png)

- Potvrdite akciju brisanja tako **da** kliknete ili poništili akciju brisanja tako da kliknete **bez**.

    ![Snimka zaslona Explorer za skripte naredba Izbriši](./media/documentdb-view-scripts/scriptexplorerdeletescript2.png)

## <a name="execute-a-stored-procedure"></a>Izvršavanje pohranjena procedura

> [AZURE.WARNING] Izvođenje pohranjene procedure u programu Explorer skripte još nije podržana za poslužitelj strani particije zbirke. Dodatne informacije potražite u članku [Partitioning i promjena veličine u DocumentDB](documentdb-partition-data.md).

Skripta Explorer omogućuje izvršavanje poslužiteljsko pohranjene procedure s portala za Azure.

- Prilikom otvaranja nove plohu postupak stvaranja pohranjene zadanu skriptu (*prefiks*) se prethodno navesti. Da biste pokrenuli *prefiks* skripte ili vlastite skripte, dodajte *id* i *unosa*. Pohranjene procedure koji koriste više parametre, sve unose mora biti u polju (npr. *["odnožje", "traka"]*).

    ![Snimka zaslona od skripte Explorer pohranjene procedure plohu da biste dodali unos i izvršavanje pohranjena procedura](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure-input.png)

- Izvršavanje pohranjena procedura, jednostavno kliknite naredbu **Spremi i izvršiti** unutar okna script editor.

    > [AZURE.NOTE] Naredba za **Spremanje i izvršavanje** će spremiti vaše pohranjena procedura prije izvršavanja, što znači prebrisat će prethodno spremljene verzije pohranjena procedura.

- Uspješan pohranjena procedura izvršavanja sadržavat će *uspješno spremiti i izvršava pohranjena procedura* status, a opseg vraćenih rezultata će biti ispunjena u oknu *rezultata* .

    ![Snimka zaslona od skripte Explorer pohranjene procedure plohu, izvršiti pohranjena procedura](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure.png)

- Ako izvršavanja naiđe na pogrešku, pogreška će unose u oknu s *rezultatima* .

    ![Prikaz svojstava skripte snimka Explorer za skripte. Izvršavanje pohranjena procedura s pogreškama](./media/documentdb-view-scripts/documentdb-execute-a-stored-procedure-error.png)

## <a name="work-with-scripts-outside-the-portal"></a>Rad s skripte izvan portala

Explorer skripte na portalu za Azure je samo jedan od načina za rad s pohranjenim procedurama, okidača i korisnički definiranih funkcija u DocumentDB. Možete i u suradnji s skripte koje koriste u [klijent SDK-ovi](documentdb-sdk-dotnet.md)i REST API-JA. Dokumentacija REST API-JA sadrži uzorke za rad s [pohranjene procedure pomoću REST](https://msdn.microsoft.com/library/azure/mt489092.aspx), [korisnički definirane funkcije pomoću REST](https://msdn.microsoft.com/library/azure/dn781481.aspx)i [pomoću okidača REST](https://msdn.microsoft.com/library/azure/mt489116.aspx). Uzorci također su dostupne prikazuje kako [raditi s skripti pomoću C#](documentdb-dotnet-samples.md#server-side-programming-examples) i [Rad s skripti pomoću Node.js](documentdb-nodejs-samples.md#server-side-programming-examples).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o DocumentDB poslužiteljsko programiranja u članku [procedure spremljene, okidača baze podataka i UDF-ove](documentdb-programming.md) .

[Tečaj](https://azure.microsoft.com/documentation/learning-paths/documentdb/) i koristan je resurs će vas voditi kroz dok učite više o DocumentDB.  
