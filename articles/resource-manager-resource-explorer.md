<properties
   pageTitle="Azure resursa Explorer | Microsoft Azure"
   description="U članku se opisuje Azure resursa Explorer i kako ga može koristiti za prikaz i ažuriranje implementacijama putem upravitelja za Azure resursa"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Korištenje Eksplorera za Azure resursa za prikaz i mijenjanje resursi
[Azure resursa Explorer](https://resources.azure.com) je sjajan alat za pogled na resurse koji ste već stvorili u svoju pretplatu. Pomoću ovog alata mogu razumjeti kako resurse strukturirane su pa provjerite svojstva dodijeljene svakog resursa. Informirajte se o na operacije REST API-JA i cmdleta ljuske PowerShell koji su dostupni za vrstu resursa, a možete izdati naredbi putem sučelja. Resurs Explorer može biti osobito korisni prilikom stvaranja predložaka Voditelj resursa jer je omogućuje vam da biste pogledali svojstva za postojeće resurse.

Izvor za alat za Explorer resursa dostupan je na [github](https://github.com/projectkudu/ARMExplorer), koji omogućuje reference koristan ako trebate implementirati slično ponašanje u vlastiti aplikacijama.

## <a name="view-resources"></a>Prikaz resursa
Dođite do [https://resources.azure.com](https://resources.azure.com) i prijavite se pomoću istih vjerodajnica koje želite koristiti za [Azure Portal](https://portal.azure.com).

Nakon učitavanja, treeview na lijevoj strani omogućuje vam da kroz razine naniže u pretplate i grupa resursa:

![TreeView](./media/resource-manager-resource-explorer/are-01-treeview.png)

Kao što je dubinski grupu resursa prikazat će se davatelja usluga za koje postoje resursa u toj grupi:

![Davatelji usluga](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Iz njega možete pokrenuti Dubinska analiza instance resursa. U nastavku snimku zaslona možete vidjeti na `sltest` instancu sustava SQL Server u na treeview. Na desnoj strani vidjet ćete informacije o zahtjeve REST API-JA možete koristiti s resursa. Tako da odete čvor resursa, resursa Explorer automatski stvara GET zahtjev za dohvaćanje informacija o resursu. U području veliki tekst ispod URL-a, vidjet ćete odgovor na API-JA. 

Kao što ste se upoznali s predlošcima resursima sadržaj tijela pokreće potražite poznatih! Odjeljak **Svojstva** odgovor odgovara vrijednosti možete unijeti u odjeljku **Svojstva** predloška.

![SQL server](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Resurs Explorer omogućuje zadržati naniže da biste istražili podređeni resursa, ako bazu podataka sustava SQL Server, postoje podređeni resursi za stvari kao što su baze podataka i pravila vatrozida.

Istraživanje baze podataka prikazuju nam svojstva za tu bazu podataka. U nastavku snimka Vidimo koji baza podataka `edition` je `Standard` i `serviceLevelObjective` (ili sloju baze podataka) je `S1`.

![SQL baze podataka](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Promjena resursi

Kada se krećete za neki resurs, možete odabrati gumb Uređivanje da biste JSON sadržaj koji se može uređivati. Explorer resursa možete koristiti da biste uredili u JSON i pošaljite zahtjev za STAVI da biste promijenili resurs. Na primjer, na donjoj slici prikazano sloju baze podataka koji se mijenjaju se u `S0`:

![baze podataka – STAVI zahtjev](./media/resource-manager-resource-explorer/are-05-database-put.png)

Odabirom **STAVITI** pošaljete zahtjev. 

Nakon slanja zahtjeva resursa Explorer ponovno šalje zahtjev GET da biste osvježili stanje. U ovom slučaju Vidimo koji na `requestedServiceObjectiveId` je ažurirana i razlikuje se od na `currentServiceObjectiveId` koji označava koja je u tijeku skaliranja operacija. Možete kliknuti gumb DOHVATI da biste ručno osvježili stanje.

![baze podataka – GET request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Izvođenje akcije resursa

Karticu **Akcije** omogućuje vam da biste vidjeli i izvođenje operacija dodatne OSTALE. Na primjer, nakon što ste odabrali resursa web-mjesta, karticu akcije predstavlja dugačak popis dostupnih operacije, neke od kojih su prikazano u nastavku.

![web - zahtjev za objavu](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Pozivanje API PowerShell
Na kartici PowerShell u programu Explorer resursa prikazuje Cmdlete da biste omogućuje interakciju s resurs koji trenutno Istraživanje. Ovisno o vrsti resursa koje ste odabrali, prikazane skriptu PowerShell može biti u rasponu od jednostavnih cmdleta (kao što su `Get-AzureRmResource` i `Set-AzureRmResource`) za složenije cmdleta (kao što su zamjena slobodnih na web-mjestu). 

![PowerShell](./media/resource-manager-resource-explorer/are-07-powershell.png)

Dodatne informacije o alatu Azure PowerShell cmdleta potražite u članku [Pomoću ljuske Azure s Azure Voditelj resursa](powershell-azure-resource-manager.md)

## <a name="summary"></a>Sažetak
Kada radite s Voditelj resursa, Explorer resursa može biti iznimno koristan alat. Je odličan način da biste pronašli načini pomoću komponente PowerShell upita, a zatim unesite promjene. Ako radite s REST API-JA je odličan način za početak rada i brzo testiranje API poziva prije nego što počnete pisanja koda. I Ako pišete predložaka može biti korisne za razumijevanje hijerarhiji resursa i pronašli mjesto koje želite smjestiti konfiguracija – promjene na portalu i zatim pronađite odgovarajuće stavke u programu Explorer resursa!

Da biste saznali više, pogledajte [kanala 9 videozapisa s Hanselman luka i Nevena Ebbo](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo)


