
<properties
    pageTitle="Sigurne aplikacija i resursa u Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako zaključati aplikacija i resursa u Azure RemoteApp"
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



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Sigurne aplikacija i resursa u Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp omogućuje korisnicima programa access središnje upravljane aplikacije Windows koja vam upravljanja radnjama koje korisnici mogu i ne možete.  To je posebno korisno kada korisnik povezuje s neupravljani uređaja (kao što je njihove osobnim uređaja Macbook), a želite kontrola korisničkog pristupa ili iskustvo.

Na primjer, ako koristite Active Directory za provjeru autentičnosti korisnika i želite da biste korisnicima onemogućili kopiranje podataka iz aplikacije, udaljene radne površine pravilnika grupe možete konfigurirati da biste korisnicima onemogućite kopiranje podataka.

Drugi primjer je ako želite blokirati pristup Internetu za određenu aplikaciju u sklopu zbirke. Možete stvoriti pravilo vatrozid blokira pristup prilikom stvaranja slika zbirke.

## <a name="implementation-options"></a>Mogućnosti implementacije

  Evo ključa implementaciju mogućnosti koje se mogu koristiti pojedinačno ili u tandemu prema potrebi:

1.  Ako je vaše zbirke RemoteApp domene pridruženo možete nametnuti svih [Pravila grupe](https://technet.microsoft.com/library/cc725828.aspx) (osim u stanje neaktivnosti i prekid veze vremenskog ograničenja pravila što je opisano [u nastavku](../azure-subscription-service-limits.md)).
2.  Kao zamjena za pravila grupe (Ako vaše zbirke nije domene pridruženo ili nemate ovlasti za desno u AD), možete konfigurirati [Lokalne pravila](https://technet.microsoft.com/library/cc775702.aspx) u predložak sliku.  Imajte na umu toj grupi pravila adut Lokalna pravila kada nema sukoba.
3.  Neke postavke OS/aplikacija se ne može konfigurirati putem pravilnika, ali može biti putem ključa registra pomoću [alata za RegEdit](./remoteapp-hybridtrouble.md) prilikom konfiguriranja predloška sliku.
4.  Koristite [Vatrozid za Windows](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) da biste kontrolirali pristup mreži i na računalu u kojem se pokreće aplikaciju. Samo se pobrinite ne blokira URL-ovi i priključke definirani ovdje.
5.  [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) možete koristiti za kontrolu koja se može pokrenuti aplikacija i datoteka korisnika. Ako, na primjer, savvy korisnicima možete otkriti pokretanje aplikacija se nije objavljivanja, ali koje su dostupne u slike koje se koriste za stvaranje zbirke – to možete blokirati AppLocker.

## <a name="detailed-information"></a>Detaljne informacije

- Sljedeća pravila ZAPISI vjerojatno najkorisniji:
    - [Preusmjeravanje resursa i uređaja](https://technet.microsoft.com/library/ee791794.aspx)
    - [Preusmjeravanje pisača](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profili](https://technet.microsoft.com/library/ee791865.aspx).
- Imajte na umu da konfiguriranje preusmjeravanja PowerShell RemoteApp modul (kao što je vide [ovdje](./remoteapp-redirection.md)) ovisi na klijentskom računalu da biste nametnuli pravila, pa ako vam je sigurnost primarni cilj ćete htjeti primjenu pravila putem lokalnog pravila za predložak slike ili putem pravilnika grupe.
- [Windows Server 2012 R2 pravila](https://technet.microsoft.com/library/hh831791.aspx).
- [Pravila za Office 2013](https://technet.microsoft.com/library/cc178969.aspx) (uključujući [Prilagodba alatne trake za Office](https://technet.microsoft.com/library/cc179143.aspx)).
