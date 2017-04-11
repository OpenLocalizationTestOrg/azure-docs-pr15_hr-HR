<properties
   pageTitle="SQL Azure s Azure RemoteApp | Microsoft Azure"
   description="Saznajte kako pomoću SQL Azure pomoću Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure s Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Često kada korisnici za hostiranje svoje aplikacije sustava Windows u oblak pomoću servisa Azure RemoteApp žele i migrirati svoje podatke kao što je SQL Server oblaka za implementacije sustava cijelu oblaka. Time se omogućuje za cijelu oblaka hostira rješenje koje se može pristupiti bilo kojem trenutku bilo kojeg uređaja s bilo kojeg mjesta uz Azure RemoteApp. U nastavku su veze i reference uz upute koje će vam pomoći u ovaj postupak.  

## <a name="migrate-your-sql-data"></a>Migriranje podataka SQL

Pokretanje migracije [baze podataka SQL Server s bazom podataka SQL Azure](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfiguriranje Azure RemoteApp
Glavno računalo aplikacija Windows Azure RemoteApp. U nastavku je vrlo visoka razina korak po korak:

1.     Stvaranje [predloška Azure RemoteApp VM](remoteapp-imageoptions.md). 
2.     Kliknite pločicu potrebna aplikacija na VM.
3.     Konfiguriranje aplikacije da bi se povezuje SQL DB i provjerili funkcionira li.
4.     Sysprep i isključivanja s VM. Snimite to kao slika za Azure. **Bilješke:** Morat ćete provjerite je li aplikacija moći zadržati informacije povezivanje DB kroz postupak sysprep. Ako aplikacija nije moguće zadržati podatke za povezivanje baze podataka, možda ćete morati sudjelovati dobavljača aplikacije da biste provjerili kako možete odrediti niz za povezivanje.
5.     Uvezite prilagođenu sliku u biblioteci Azure RemoteApp odabirom odgovarajuće geografski koji se nalazi za implementaciju sustava SQL Azure. 
6.     Implementacija RemoteApp zbirke u istom podatkovnog centra kao implementaciju sustava SQL Azure pomoću predloška za gornje i objavljivanje aplikacija. Implementacija Azure RemoteApp u centru za iste podatke kao SQL Azure implementacije olakšava provjerite je li najbrže brzina veze i smanjivanje Latencija. 

## <a name="app-and-sql-configuration-considerations"></a>Aplikacije i SQL konfiguracije pitanja vezana uz:
Postoji nekoliko točaka u obzir prilikom korištenja Azure SQL s RemoteApp:

Saznajte [kako konfigurirati Vatrozid za bazu podataka sustava Azure SQL](../sql-database/sql-database-firewall-configure.md). U isječak iz stanja članak "prethodno, sve programa access s poslužiteljem baze podataka SQL Azure blokirao je vatrozida. Da biste počeli koristiti vaš poslužitelj baze podataka SQL Azure, idite na klasični Portal i navedite jednu ili više pravila vatrozida razini poslužitelja koji omogućuju pristup s poslužiteljem baze podataka SQL Azure. Pomoću pravila vatrozida da biste odredili koji rasponi IP adresa s Interneta dopušteno i hoće li Azure aplikacije možete pokušati povezati s poslužiteljem baze podataka SQL Azure."

Osim toga, kada računalo pokuša povezati s poslužiteljem baze podataka s Interneta, vatrozida provjerava izvorišne IP adresu zahtjev na cijelom skupu razini poslužitelja i (po potrebi) baza podataka razinom pravila vatrozida. "Ako je IP adresa zahtjeva unutar nekog raspona koji je naveden u pravila vatrozida razini poslužitelja, veza moguć je s poslužiteljem baze podataka SQL Azure." Dakle, ne možemo olakšavaju korištenje IP rasponi, a ne samo pojedinačne izvorni IP adrese.

Slijedite upute korak po korak [Kako: konfigurirati postavke vatrozida u SQL baze podataka pomoću portala za Azure](../sql-database/sql-database-configure-firewall-settings.md) da biste odredili IP raspona. Prilikom konfiguriranja pravila vatrozida za SQL, navedite raspon IP podmreže koji je naveden za zbirku Azure RemoteApp. To dopustite ARA poslužitelji za povezivanje s SQL DB čak i ako oni će imati dinamički-dodijeliti IP adrese.

## <a name="troubleshooting"></a>Otklanjanje poteškoća
Ako doživljaj prilikom korištenja klijentska aplikacija hostira Azure RemoteApp koja se povezuje s SQL baze podataka gdje se nalaze na Azure ili na lokalnim sporo može postojati nekoliko razloga zašto.  

- Latenciju mreže s uređaja Azure je visoka. Premještanje najbolje i najbrže mrežna veza možete radi ostvarivanja najboljih performansi. Koristite [azurespeed.com](http://azurespeed.com/) kao Općenito alat za provjeru vašeg uređaja Latencija Azure podatkovnog centra.  
- Aplikacija za klijent smješten u Azure RemoteApp je u odjeljku opterećenjem. Da odaberete drugu tarifu za naplatu kao što su Premium naplata će poboljšati performanse. Drugi trik je praćenje resursi koji se koristi aplikacija: aktivna sesiji izvođenje redoslijed tipki ctrl-alt-end koji će pokrenuti SAS zaslona, odaberite upravitelj zadataka i pridržavajte Upotreba resursa za aplikaciju.
- SQL server je u odjeljku opterećenjem ili nisu optimizirane. Slijedite upute za SQL za otklanjanje poteškoća. 

