<properties
    pageTitle="DocumentDB Explorer dokument da biste pogledali JSON | Microsoft Azure"
    description="Saznajte više o DocumentDB Explorer dokument alat za Azure Portal za prikaz JSON, uređivanje, stvaranje i prijenos JSON dokumenata s DocumentDB, NoSQL dokumenta baze podataka."
        keywords="Prikaz json"
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

# <a name="view-edit-create-and-upload-json-documents-using-documentdb-document-explorer"></a>Prikaz, uređivanje, stvaranje i prijenos JSON dokumenata pomoću programa Explorer DocumentDB dokumenta

Ovaj članak sadrži pregled sustava [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Explorer dokument Azure portala alat koji omogućuje prikaz, uređivanje, stvaranje, prijenos i filtrirati JSON dokumenata s DocumentDB. 

Imajte na umu dokument Explorer nije omogućena na računima DocumentDB s podrškom za protokol za MongoDB. Ova stranica će se ažurirati kada je omogućen ovu značajku.

## <a name="launch-document-explorer"></a>Pokretanje programa Explorer dokumenta

1. Na portalu Azure u Jumpbar, kliknite **DocumentDB (NoSQL)**. Ako **DocumentDB (NoSQL)** nije vidljivo, kliknite **Više servisa** , a zatim kliknite **DocumentDB (NoSQL)**.

2. Odaberite naziv računa. 

3. Na izborniku resursa kliknite **Explorer dokumenta**. 
 
    ![Snimka zaslona s naredbom Explorer dokumenta](./media/documentdb-view-JSON-document-explorer/documentexplorercommand.png)

    U plohu **Dokument Explorer** padajućim popisima **baze podataka** i **zbirki** su unaprijed popunjeno ovisno o kontekstu pokretanja Explorer dokumenta. 

## <a name="create-a-document"></a>Stvaranje dokumenta

1. [Pokretanje programa Explorer dokumenta](#launch-document-explorer).

2. U plohu **Explorer dokumenta** kliknite **Stvaranje dokumenta**. 

    Minimalna isječak JSON navedeni su u plohu **dokumenta** .

    ![Snimka zaslona dokument Explorer stvaranje sučelje za dokument, gdje možete pogledati JSON i uređivanje JSON](./media/documentdb-view-JSON-document-explorer/createdocument.png)

2. U plohu **dokument** upišite ili zalijepite u sadržaja dokumenta JSON koju želite stvoriti pa kliknite **Spremi** za primjenu dokumentu prikupljanje naveden u plohu **Explorer dokument** i baze podataka.

    ![Snimka zaslona programa dokument Explorer Spremi naredbe](./media/documentdb-view-JSON-document-explorer/savedocument1.png)

    > [AZURE.NOTE] Ako ne navedete svojstvo programa "id", zatim dokument Explorer automatski dodaje u svojstvo id i generira GUID kao vrijednost ID-a.

    Ako već imate podatke iz JSON datoteke, MongoDB, SQL Server, CSV datoteke, spremište tablica Azure, Amazon DynamoDB, HBase, ili iz drugih zbirki DocumentDB DocumentDB, [alata za migraciju podataka](documentdb-import-data.md) možete koristiti da biste brzo uvoz podataka.

## <a name="edit-a-document"></a>Uređivanje dokumenta

1. [Pokretanje programa Explorer dokumenta](#launch-document-explorer).

2. Da biste uredili postojeći dokument, odaberite ga u plohu **Explorer dokumenta** , uređivanje dokumenta u plohu **dokument** i kliknite **Spremi**.

    ![Snimka zaslona Explorer za dokument Uredi dokument funkcija služi za prikaz JSON](./media/documentdb-view-JSON-document-explorer/editdocument.png)

    Ako uređujete dokument i odlučiti koju želite odbaciti trenutnu skup izmjene, jednostavno kliknite **Odbaci** plohu **dokumenta** , potvrdite akciju Odbaci pa se ponovno učitati prethodno stanje dokumenta.

    ![Snimka zaslona programa Explorer dokument Odbaci naredbe](./media/documentdb-view-JSON-document-explorer/discardedit.png)

## <a name="delete-a-document"></a>Brisanje dokumenta

1. [Pokretanje programa Explorer dokumenta](#launch-document-explorer).

2. Odaberite dokument u **Programu Explorer dokumenta**, kliknite **Izbriši**, a zatim potvrdite brisanje. Nakon potvrde, dokument je uklonjeno s popisa Explorer dokumenta.

    ![Naredba Izbriši Explorer snimka dokumenta](./media/documentdb-view-JSON-document-explorer/deletedocument.png)

## <a name="work-with-json-documents"></a>Rad s dokumentima JSON

Dokument Explorer Provjeri valjanost da svaki novi ili uređivati dokument sadrži valjani JSON.  Možete čak i prikaz JSON pogrešaka tako iznad netočan sekcija da biste vidjeli detalje o pogrešci provjere valjanosti.

![Snimka zaslona programa dokument Explorer istaknute koji nisu valjani JSON](./media/documentdb-view-JSON-document-explorer/invalidjson1.png)

Osim toga, dokument Explorer sprječava spremanje dokumenta sa sadržajem koji nisu valjani JSON.

![Snimka zaslona programa dokument Explorer s koji nisu valjani JSON Spremi pogreške](./media/documentdb-view-JSON-document-explorer/invalidjson2.png)

Na kraju dokumenta Explorer omogućuje jednostavno prikazati svojstva sustava trenutno učitani dokumenta klikom na naredbu **Svojstva** .

![Prikaz svojstava dokumenta Explorer snimka dokumenta](./media/documentdb-view-JSON-document-explorer/documentproperties.png)

> [AZURE.NOTE] Svojstvo vremenske oznake (_ts) interno predstavljen kao epoch vremena, ali dokument Explorer prikazuje vrijednost u obliku Ljudski čitljiv GMT.

## <a name="filter-documents"></a>Filtriranje dokumenata
Dokument Explorer podržava brojne mogućnosti navigacije i napredne postavke.

Prema zadanim postavkama, dokument Explorer učitava prema gore na prvo 100 dokumente na odabranu zbirku po njihove datumu stvaranja od najranijeg do najnoviji.  Odabirom mogućnosti **Učitavanje više** pri dnu plohu Explorer dokumenta možete učitati dodatne dokumente (u serijama od 100). Možete odabrati koje dokumente da biste učitali putem naredbe **Filtar** .

1. [Pokretanje programa Explorer dokumenta](#launch-document-explorer).

2. Pri vrhu plohu **Explorer dokumenta** , kliknite **Filtar**.  

    ![Snimka zaslona s postavkama filtriranja Explorer dokumenta](./media/documentdb-view-JSON-document-explorer/documentexplorerfiltersettings.png)
  
3.  Postavke filtra pojaviti ispod trake s naredbama. U odjeljku postavke filtra pružaju uvjet WHERE i/ili uvjet ORDER BY, a zatim **Filtar**.

    ![Snimka zaslona programa Explorer postavke dokumenta plohu](./media/documentdb-view-JSON-document-explorer/documentexplorerfiltersettings2.png)

    Dokument Explorer osvježava rezultate s dokumentima koji se podudaraju upita filtriranja. Pročitajte više o DocumentDB SQL gramatike u [SQL upita i njegova SQL sintaksa](documentdb-sql-query.md) članak ili ispisati primjerak [SQL upita cheat lista](documentdb-sql-query-cheat-sheet.md).

    Padajući popis okvire **baze podataka** i **Zbirka** mogu se jednostavno promijeniti zbirke iz kojeg dokumenata su trenutno gleda bez potrebe za zatvorite i ponovno pokrenuli Explorer dokumenta.  

    Dokument Explorer podržava i filtriranje trenutno učitani skup dokumenata prema njihovim svojstvo ID-a.  Jednostavno upišite u filtru dokumenata po ID-a.

    ![Snimka zaslona programa dokument Explorer s filtrom istaknuta](./media/documentdb-view-JSON-document-explorer/documentexplorerfilter.png)

    Rezultati u programu Explorer dokumenta filtriraju popisa na temelju navedeni kriterij.

    ![Snimka zaslona programa dokument Explorer s rezultatima filtriranja](./media/documentdb-view-JSON-document-explorer/documentexplorerfilterresults.png)

    > [AZURE.IMPORTANT] Dokument Explorer filtar funkcionalnost samo filtre iz na ***trenutno*** učitati skup dokumenata i izvršavanje upita na temelju trenutno odabranu zbirku.

4. Da biste osvježili popis dokumenata učitati Explorer dokumenta, kliknite **Osvježi** pri vrhu na plohu.

    ![Snimka zaslona programa Explorer dokumenta naredbe Osvježi](./media/documentdb-view-JSON-document-explorer/documentexplorerrefresh.png)

## <a name="bulk-add-documents"></a>Skupno dodavanje dokumenata

Dokument Explorer podržava skupno ingestion jedan ili više postojećih JSON dokumenata, najviše 100 datoteka JSON po operaciji prijenos.  

1. [Pokretanje programa Explorer dokumenta](#launch-document-explorer).

2. Da biste pokrenuli postupak za prijenos, kliknite **Prenesi dokument**.

    ![Snimka zaslona programa Explorer dokumenta skupno ingestion funkcija](./media/documentdb-view-JSON-document-explorer/uploaddocument1.png)

    Otvorit će se plohu **Prenesi dokument** . 

2. Kliknite gumb Pregledaj da biste otvorili prozor programa explorer datoteku, odaberite dokumenti JSON prenijeti, a zatim **otvorite**.

    ![Snimka zaslona dokument Explorer masovne ingestion](./media/documentdb-view-JSON-document-explorer/uploaddocument2.png)

    > [AZURE.NOTE] Dokument Explorer trenutno podržava do 100 JSON dokumenata po operaciji pojedinačne prijenos.

3. Kada ste zadovoljni odabir, kliknite gumb **Prenesi** .  Dokumenti se automatski dodaju u rešetku Explorer dokument i prijenos Rezultati prikazuju se kao napreduje operacija. Uvoz pogreške su potrebnom za pojedinačne datoteke.

    ![Snimka zaslona programa Explorer dokumenta skupno ingestion rezultata](./media/documentdb-view-JSON-document-explorer/uploaddocument3.png)

4. Nakon dovršetka postupka možete odabrati do druge 100 dokumenata da biste prenijeli.

## <a name="work-with-json-documents-outside-the-portal"></a>Rad s dokumentima JSON izvan portala

Explorer dokument na portalu za Azure je samo jedan od načina za rad s dokumentima u DocumentDB. Možete raditi i dokumenata pomoću [REST API -JA](https://msdn.microsoft.com/library/azure/mt489082.aspx) ili [klijenta SDK-ovi](documentdb-sdk-dotnet.md). Na primjer kod, potražite u članku [primjeri .NET SDK dokument](documentdb-dotnet-samples.md#document-examples) i [primjeri Node.js SDK dokumenta](documentdb-nodejs-samples.md#document-examples).

Ako vam je potrebna za uvoz ili migracija datoteka iz drugog izvora (JSON datoteke, MongoDB, SQL Server, CSV datoteke, Azure tablice za pohranu, Amazon DynamoDB ili HBase), poslužite se DocumentDB [alata za migraciju podataka](documentdb-import-data.md) da biste brzo DocumentDB uvesti podatke.

## <a name="troubleshoot"></a>Rješavanje problema

**Simptoma**: dokument Explorer vraća **pronađenih dokumenata**.

**Rješenje**: Provjerite je li koju ste odabrali ispravnu pretplatu, baza podataka i zbirke u kojem su umetati dokumente. Osim toga, provjerite je li se operacijski unutar kvote za propusnost. Ako radite na Maksimalna propusnost razine i dohvaćanje ograničio vrijeme, smanjite korištenje aplikacije raditi u odjeljku Maksimalna propusnost kvote zbirke.

**Objašnjenje**: je portal programa kao što su bilo kojeg drugog upućivanje poziva za prikupljanje i DocumentDB baze podataka. Ako vašim zahtjevima su trenutno se ograničio vrijeme zbog pozive u tijeku s zasebna aplikacija, portala možda također biti ograničio vrijeme, uzrokuje resursa ne pojavi na portalu. Da biste riješili taj problem, adresa uzrok visoke propusnost korištenje, a zatim osvježite portala plohu. Informacije o kako se mjere i korištenje donjem propusnost pronaći ćete u odjeljku [propusnost](documentdb-performance-tips.md#throughput) članka [savjeta performansi](documentdb-performance-tips.md) .

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o gramatike DocumentDB SQL podržani u programu Explorer dokumenta, potražite u članku [SQL upita i njegova SQL sintaksa](documentdb-sql-query.md) ili ispisati [SQL upita cheat lista](documentdb-sql-query-cheat-sheet.md).

[Tečaj](https://azure.microsoft.com/documentation/learning-paths/documentdb/) i koristan je resurs će vas voditi kroz dok učite više o DocumentDB. 
