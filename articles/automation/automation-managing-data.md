<properties 
   pageTitle="Upravljanje podacima Azure Automatizacija | Microsoft Azure"
   description="Ovaj članak sadrži više tema za upravljanje okruženje za automatizaciju Azure.  Trenutno sadrži zadržavanje podataka i sigurnosno kopiranje Azure Automatizacija Izrada oporavak u automatizaciji Azure."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Upravljanje podacima Automatizacija Azure

Ovaj članak sadrži više tema za upravljanje okruženje za automatizaciju Azure.

## <a name="data-retention"></a>Zadržavanje podataka

Kada izbrišete resursa u automatizaciji Azure, zadržava se za 90 dana nadzora u svrhu prije trajno uklonjene.  Nije moguće potražite i korištenje resursa za to vrijeme.  Ovo pravilo odnosi i na resurse koji pripadaju Automatizacija računa koji je izbrisan.

Azure Automatizacija automatski briše i trajno uklanja poslove starije od 90 dana.

U sljedećoj su tablici navedene pravila zadržavanja za različite resurse.

|Podataka|Pravila|
|:---|:---|
|Poslovni subjekti|Trajno se uklanjaju 90 dana nakon brisanja račun i korisnik.|
|Resursi|Trajno se uklanjaju 90 dana nakon brisanja imovine korisnik ili 90 dana nakon računa koja sadrži imovine izbriše korisnika.|
|Moduli|Trajno se uklanjaju 90 dana nakon brisanja modul korisnik ili 90 dana nakon računa koja sadrži modul izbriše korisnika.|
|Runbooks|Trajno se uklanjaju 90 dana nakon brisanja resursa korisnik ili 90 dana nakon računa koja sadrži resurs je izbrisao korisnik.|
|Zadaci|Izbrisane i trajno uklonjene 90 dana nakon zadnje onemogućiti izmjenu. To se može biti kada posao dovrši, je zaustavljen ili je obustavljena.|
|Datoteka konfiguracije/MOF čvor| Stari konfiguracije čvor je trajno uklanjate 90 dana nakon generira se nova konfiguracija čvor.|
|DSC čvorove| Trajno se uklanjaju 90 dana nakon čvor nije registriran s računa za automatizaciju pomoću portala za Azure ili cmdlet [Neregistriranja AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) u ljusci Windows PowerShell. Čvorovi trajno uklanjaju 90 dana nakon računa koja sadrži čvor izbriše korisnika. |
|Čvor izvješća| Trajno se uklanjaju 90 dana nakon novo izvješće generira za taj čvor|

Pravila zadržavanja odnosi se na sve korisnike, a trenutno nije moguće prilagoditi.

## <a name="backing-up-azure-automation"></a>Sigurnosno kopiranje Automatizacija Azure

Kada izbrišete računa za automatizaciju u Microsoft Azure, brišu se svi objekti na račun uključujući runbooks, module, konfiguracije, postavke, zadatke i resursi. Objekte nije moguće oporaviti nakon brisanja računa.  Da biste sigurnosno kopirajte sadržaj računa automatizacije prije brisanja, poslužite se sljedećim informacijama. 

### <a name="runbooks"></a>Runbooks

Datoteka skripte pomoću portala za upravljanje Azure ili cmdlet [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) u ljusci Windows PowerShell možete izvesti na runbooks.  Te datoteke skripte uvoze se u neki drugi račun za automatizaciju kako je opisano u [Stvaranje ili uvoz u Runbook](https://msdn.microsoft.com/library/dn643637.aspx).


### <a name="integration-modules"></a>Integracija moduli

Nije moguće izvesti module za integraciju s Azure automatizaciju.  Morate biti sigurni da su dostupne izvan račun za automatizaciju.

### <a name="assets"></a>Resursi

[Resursi](https://msdn.microsoft.com/library/dn939988.aspx) nije moguće izvesti iz Azure automatizaciju.  Pomoću portala za upravljanje Azure, morate zabilježite detalje o varijable, vjerodajnice, certifikata, veze i rasporede.  Zatim morate ručno stvoriti neku imovinu koje koriste runbooks koje uvozite u drugom automatizaciju.

Možete koristiti [Azure cmdleti za](https://msdn.microsoft.com/library/dn690262.aspx) dohvaćanje pojedinosti šifrirane resursi, a zatim spremite ih za buduću upotrebu ili stvaranje ekvivalentan imovine u neki drugi račun za automatizaciju.

Nije moguće dohvatiti vrijednost za šifriranu varijable ili polje za lozinku vjerodajnice pomoću cmdleta.  Ako ne znate te vrijednosti, možete ih dohvatiti iz runbook pomoću [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) i [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) aktivnosti.

Certifikati nije moguće izvesti iz Azure automatizaciju.  Morate provjerite jesu li sve potvrde dostupna izvan Azure.

### <a name="dsc-configurations"></a>DSC konfiguracija

Datoteka skripte pomoću portala za upravljanje Azure ili cmdlet [Izvoz AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) u ljusci Windows PowerShell možete izvesti na konfiguracije. Ove konfiguracije možete uvesti i koristiti u neki drugi račun za automatizaciju.


##<a name="geo-replication-in-azure-automation"></a>Zemlj replikacijom u Azure automatizacije

Zemlj. – replikacijom, standardna u računi Azure Automatizacija sigurnosno podataka računa za različite zemljopisnom području zalihosti. Možete odabrati primarni regija prilikom postavljanja računa, a zatim sekundarne regija joj je dodijeljena automatski. Sekundarni podatke kopirane s područjem primarni neprestano ažuriraju u slučaju gubitka podataka.  

Sljedeća tablica prikazuje uparivanja dostupna primarnih i sekundarnih regija.

|Primarni            |Sekundarni
| ---------------   |----------------
|Južna središnje SAD-a   |Sjeverna središnje SAD-a
|Istok SAD 2          |Središnje SAD-a
|Europa Zapad        |Sjeverna Europa
|Južna istočnoazijski    |Istočnoazijski
|Istok Japan         |Japan Zapad

U događaj vjerojatno nestaje primarni područje podataka, Microsoft će pokušati oporaviti. Ako se primarni podataka nije moguće oporaviti, pa se provodi zemlj prebacivanje i problematične kupci ćete biti obaviješteni o tome putem pretplate.

