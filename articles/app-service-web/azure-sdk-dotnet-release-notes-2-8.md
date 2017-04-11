
<properties 
   pageTitle="Azure SDK za .NET 2,8 napomene" 
   description="Azure SDK za .NET 2,8 napomene" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK za .NET 2,8, 2.8.1 i 2.8.2

##<a name="overview"></a>Pregled
 
Ovaj članak sadrži napomene (koja obuhvaća poznatih problema i promjene prijelom) za Azure SDK za .NET 2,8 2.8.1 i 2.8.2 izdanja. 

Potpuni popis novih značajki i ažuriranjima u ovom izdanju potražite u članku objava [Azure 2,8 SDK za Visual Studio 2013 i Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) . 

##  <a name="azure-sdk-for-net-28"></a>Azure SDK za .NET 2,8

### <a name="download-azure-sdk-for-net-28"></a>Preuzmite Azure SDK za .NET 2,8

[Azure SDK za .NET 2,8 za Visual Studio 2015.](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK za .NET 2,8 za Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>Podrška za .NET 4.5.2 

####<a name="known-issues"></a>Poznati problemi

Azure 2,8 SDK .NET omogućuje stvaranje .NET 4.5.2 paketa u Oblaku. No .NET 4.5.2 framework nije instaliran na zadanom pustite goste OS slike dok siječanj 2016 goste OS. Prije tog, 4.5.2 .NET framework bit će dostupan putem u zasebnom goste verzija OS-a izdanja – studenom 2015 02. Posjetite stranicu [Azure goste OS izdanja i SDK kompatibilnosti matrice](../cloud-services/cloud-services-guestos-update-matrix.md) za praćenje slike će biti objavljeni.  Kada je objavio 2015 02 slika studenom možete koristiti tu sliku ažuriranjem servis u Oblaku konfiguracijskoj datoteci datoteke (.cscfg). U Konfiguracija servisa datoteke postavite atribut osVersion elementa ServiceConfiguration niz "WA-GOSTE-OS-4.26_201511-02". Ako odaberete da biste se uključili u da biste koristili sliku, a zatim više nećete imati Automatska ažuriranja za goste OS. Da biste dobili automatskog ažuriranja u osVersion mora biti postavljeno na "*" i .NET 4.5.2 samo bit će dostupan putem automatskog ažuriranja u siječnju 2016.

###<a name="azure-data-factory"></a>Tvorničke Azure podataka

####<a name="known-issues"></a>Poznati problemi 

Azure power Jezgrena skripta možda tijekom na **Predlošku tvorničke podataka** stvaranjem projekta koje obuhvaćaju ogledne podatke, neće uspjeti ako je azure power shell verzija na računalu instaliran nakon 0.9.8.

Da biste uspješno stvorili ovu vrstu projekta, morate instalirati [azure power shell verziju 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).


### <a name="azure-resource-manager-tools"></a>Alati za Azure Voditelj resursa 

####<a name="breaking-changes"></a>Najnovije promjene

U ovom izdanju za rad s Azure PowerShell cmdleti nove verzije 1.0 ažurirao skriptu PowerShell nudi projekta Azure grupa resursa.  Ovaj nove skripte neće raditi s Visual Studio kada koristite verziju SDK prije 2,8.  

Skripti projekata stvorenih u starijim verzijama SDK neće funkcionirati iz programa Visual Studio prilikom korištenja 2,8 SDK.  Sve skripte nastavit će funkcionirati izvan Visual Studio odgovarajuće verzijom cmdleta Azure PowerShell.  

2,8 SDK zahtijeva verziju 1.0 cmdleta Azure PowerShell.  Sve druge verzije programa SDK zahtijevaju verziju 0.9.8 cmdleta Azure PowerShell.  Dodatne informacije potražite u članku [ovom](http://go.microsoft.com/fwlink/?LinkID=623011) blogu.

###<a name="web-tools-extensions"></a>Extensions alata za web

####<a name="known-issues"></a>Poznati problemi

Sljedeće poznate probleme s će se spomenuti u sljedeće izdanje.

- Aplikacije servisa za povezane oblaka i poslužitelja Explorer gesta za koje nisu radnog okruženja (kao što je Azure Kine i kupcima stogu Azure) ne funkcionira. U slučaju korisnika iz tih impacted područja preuzimanje profila za objavljivanje na portalu Azure omogućit će mogućnost objavljivanje. Buduće izdanje će popravite geste kao što su "Prilaganje ispravljanje pogrešaka" i "Prikaz strujanje zapisnika" Kina Azure i stoga klijentima. 
- Korisnici mogu potražite u članku pogreške tijekom aplikacije servisa za stvaranje kada instancu uvida aplikacije koje su oni implementacije je u regiji osim Istočni SAD-a. U tim slučajevima, stvaranje aplikacije servisa na portalu i preuzimanje profila Objavi omogućit će scenariji za objavljivanje. 

###<a name="azure-hdinsight-tools"></a>Alati za Azure HDInsight

####<a name="new-updates"></a>Novih ažuriranja

- Možete izvoditi upit grozd klasteru putem HiveServer2 s gotovo nijedna indirektni i vidjeti posao zapisnike u stvarnom vremenu.
- Pomoću nove vrste Hive izvođenja prikaz zadatka možete istražujte u dublju posla, dodatne detalje potražite i pronađite potencijalne probleme.

Informacije potražite u članku [Azure 2,8 SDK za Visual Studio 2013 i Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK za .NET 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Poznati problemi za Visual Studio 2013 i Visual Studio 2015.
 
1. Potaknute WebJob objavljuje na slobodnih će Prikaži i pogreške i nećete skup rasporeda, ali će automatske na WebJob za Azure. Korisnici koji su u kojoj su potrebne za posao zakazano možete koristiti Portal za Azure da biste postavili raspored na WebJob. 
2. Python korisnici mogu se pojaviti problemi za ispravljanje pogrešaka. Izvan popravak vodoravnim tima službe za to, ali ako su korisnici, provjerite obavijestili Microsoft informacije na forumima ili na blogu objave ili oslobađanje odjeljak Komentari bilješke. 
3. Korisnici u određenim regijama (kao što su Južna Indija) će se dogoditi aplikacije servisa za pogreške u dodjeljivanju. To je u skladu s portala sustava, a taj se problem pojavljuje korisnike možete koristiti portal za Azure da biste zatražili pristup da biste objavili ove zemlj područja. Kada su zatražiti pristup te područja pomoću na Azure portala dodjeljivanja surađivati. 

##<a name="azure-sdk-for-net-282"></a>Azure SDK za .NET 2.8.2

Nakon instalacije na 2.8.2 Alati, korisnici mogu se pojaviti na sljedeći problem.         

- Ako koristite Windows 10, a niste instalirali Internet Explorer, može se pogreška "Internet Explorer nije moguće pronaći".
Da biste riješili taj problem, instalirajte Internet Explorer pomoću dijaloškog okvira Dodaj/ukloni komponente sustava Windows.

Ako primijetite taj problem, pomoću značajke Pošalji u pohvala da biste se prijavili.

Dodatne informacije potražite u članku [ovaj](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) objaviti.
##<a name="other-updates"></a>Ostala ažuriranja

Ostala ažuriranja potražite u članku [Azure SDK 2,8 objava objaviti](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Vidi također

[Azure objavu SDK 2,8 objava](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Podrška i njegovo povlačenje iz upotrebe informacije za Azure SDK za .NET i API-](https://msdn.microsoft.com/library/azure/dn479282.aspx)

