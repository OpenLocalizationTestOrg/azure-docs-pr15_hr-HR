
<properties
    pageTitle="Preduvjeti za aplikaciju za Azure RemoteApp | Microsoft Azure"
    description="Dodatne informacije o preduvjetima za aplikacije koje želite koristiti u Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>Preduvjeti za aplikaciju

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp podržava strujanje 32-bitne ili 64-bitni utemeljen na sustavu Windows aplikacije iz sustava Windows Server 2012 R2 slike. Većina postojeće 32-bitne ili 64-bitni utemeljen na sustavu Windows aplikacije pokrenuti "kakav jest" Azure RemoteApp (udaljene radne površine ili prethodno poznati kao Terminal Services) okruženje. No postoji razlika između operacijski sustav, a dobro radi - neke aplikacije pravilno funkcionirati i izvesti, dok drugi ne. Sljedeće informacije sadrži smjernice za razvoj aplikacija u okruženju udaljene radne površine i testiranje da biste bili sigurni kompatibilnosti.

Savjet: Radimo na stvaranje primjera rad aplikacije za vas. Vidjet ćete nove teme kojima se govori koristeći Microsoft Access, QuickBooks i aplikacije V RemoteApp.

## <a name="requirements"></a>Preduvjeti
Preduvjeta tri ako iza, pomoći pokretanje i u RemoteApp aplikacije:

1.  Aplikacije koje zadovoljavaju sve [ustanova preduvjeti za Windows aplikacija za stolna računala](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) i drži [udaljene radne površine](https://msdn.microsoft.com/library/aa383490.aspx) programiranje smjernice imat će potpuni kompatibilnosti s RemoteApp.
2.  Aplikacija nikad pohranite podataka lokalno na sliku ili RemoteApp instance koje mogu se izgubiti.  Kada stvorite zbirku RemoteApp, pojavljivanja su klonirana se bez praćenja stanja i mora sadržavati samo aplikacijama. Pohrana podataka na vanjskom izvoru ili unutar profila korisnika.
3.  Prilagođeni slike nikad ne smiju sadržavati podatke koji mogu se izgubiti.  

## <a name="testing-your-apps"></a>Testiranje aplikacija
Pomoću ovih koraka za testiranje aplikacije:

1.  Instalirajte Windows Server 2012 R2 i aplikacija
2.  Omogućivanje udaljene radne površine
3.  Stvorite dva korisničke račune, UserA i UserB, dodavanje i korisničkih računa u sigurnosne grupe udaljene radne površine.
4.  Provjera kompatibilnosti višestruko uspostavljanjem dva istodobno ZAPISI sesije s PC-jem prilikom pokretanja aplikacije.
5.  Provjerite valjanost ponašanje aplikacije

## <a name="application-development-guidelines"></a>Smjernice za razvoj aplikacija
Pridržavajte se sljedećih smjernica za razvoj aplikacija za RemoteApp.

### <a name="multiple-users"></a>Više korisnika

- Instaliranje [aplikacije za jednog korisnika ](https://msdn.microsoft.com/library/aa380661.aspx)može stvarati probleme u okruženju višekorisnička.
- Aplikacija trebali biste [pohranili podatke specifične za korisnika](https://msdn.microsoft.com/library/aa383452.aspx) mjesta specifične za korisnika, odvojeno od globalni informacije koje se odnose na sve korisnike.
- RemoteApp koristi više [prostora naziva za otklanjanje objekte](https://msdn.microsoft.com/library/aa382954.aspx); globalni prostor naziva koristi prvenstveno services u aplikacijama za klijent poslužitelj.
- Nije sigurno pretpostavlja da naziv računala ili [IP adresa](https://msdn.microsoft.com/library/aa382942.aspx) dodijeljena računalu su povezani s jednom korisniku jer više korisnika može biti prijavljeni istodobno udaljene radne površine glavnog (RD glavnog) poslužitelja.

### <a name="performance"></a>Performanse
- Onemogućivanje [grafičkim efektima](https://msdn.microsoft.com/library/aa380822.aspx) prije nego što dodate aplikaciju RemoteApp.
- Da biste maksimizirali dostupnost procesora za sve korisnike, onemogućite [pozadinske zadatke](https://msdn.microsoft.com/library/aa380665.aspx) ili stvorite učinkovitog pozadinske zadatke koji nisu resursa ćete morati usko.
- Trebali biste ugađanje i saldo aplikacije [niti korištenje](https://msdn.microsoft.com/library/aa383520.aspx) višekorisnička, višeprocesorskim okruženju.
- Da biste optimizirali performanse, dobro je za aplikacije da biste [otkrili](https://msdn.microsoft.com/library/aa380798.aspx) hoće li se pokreće u sesiji klijenta.
