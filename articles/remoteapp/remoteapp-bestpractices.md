<properties
    pageTitle="Azure RemoteApp najbolje prakse | Microsoft Azure"
    description="Najbolje prakse za konfiguriranje i korištenje Azure RemoteApp."
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

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Najbolje prakse za konfiguriranje i korištenje Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Sljedeće informacije omogućuju konfiguriranje i korištenje Azure RemoteApp nude o produktivnijem.

## <a name="connectivity"></a>Povezivanje


- Uvijek koristite najnoviju verziju klijenta. Korištenje starijih klijenti može dovesti do problema s povezivanjem i druge su smanjene sučelja. Omogućivanje automatskog aplikacije ažuriranja za svoj uređaj će je li najnovija klijent uvijek instaliran.
- Uvijek koristi stabilan i pouzdane internetsku vezu dostupne.  
- Korištenje podržana samo proxy veze za povezivanje optimalnih performansi.  SOCKS proxy nije podržana.

## <a name="applications"></a>Aplikacija


- Spremite i zatvorite RemoteApp aplikacije kada ste gotovi s aplikacijom. Ne zatvaranja aplikacije može rezultirati gubitka podataka.
- Provjerite valjanost prilagođene aplikacije prije njihova korištenja Azure RemoteApp. To obuhvaća osiguravanje i rad na više sesiju platformi te ne trošiti nepotrebne resurse kao što su memorije i procesora koji se mogu starve drugi korisnik u istoj zbirci. Informacije, preuzmite i pregledajte [Aplikacije kompatibilnosti najbolje prakse za udaljene radne površine](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfiguriranje i upravljanje njima


- Ažuriranje predloška slike, prilikom instalacije softverska ažuriranja i druge promjene ključnih prema potrebi. Time se osigurava da kao Azure RemoteApp automatski-ljestvice da bi odgovarao vaš kapaciteta, svaku instancu je patched.  
- Provjerite je li implementaciju sustava Active Directory Federation Services (AD FS) sigurne i pouzdan. U suprotnom authentications klijent možda neće uspjeti, korisnicima onemogućuje pristup Azure RemoteApp.
- Konfigurirajte slika predložaka instalirane aplikacije, uloge ili značajke tako da su bez praćenja stanja. Ne mogu oslanjate na sve instance virtualnim strojevima RemoteApp servisu koji se u stalni stanju.
    - Pohranite sve korisničke podatke u korisničke profile ili drugim vanjskim sa servisom mjesta za pohranu, kao što su lokalne datoteke zajedničko korištenje ili OneDrive.
    - Spremište zajedničke podatke u mjesta za pohranu izvan servisa, kao što su lokalne datoteke zajedničko korištenje ili OneDrive.
    - Konfigurirati postavke sistemskih slike u predložak, a ne na pojedinačne virtualnim strojevima na servis.
    - Onemogućivanje automatskog softverska ažuriranja za objavljene aplikacije – umjesto ručno ih primijeniti na sliku predloška i testiranje prije implementacije iz predloška.
