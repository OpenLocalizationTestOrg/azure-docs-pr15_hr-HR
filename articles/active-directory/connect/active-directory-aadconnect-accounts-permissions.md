<properties
   pageTitle="Azure AD Connect: Računa i dozvola | Microsoft Azure"
   description="U ovoj se temi opisuju računa koristi i stvara i potrebne dozvole."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Računa i dozvola
Čarobnjak za instalaciju Azure AD Connect nudi dvije različite putovi:

- U odjeljku postavke Express čarobnjaka potrebne ovlasti više tako da je jednostavno, postavljanje konfiguraciju ne zahtijeva Stvaranje korisnika ili zasebno konfigurirajte dozvole.

- U odjeljku postavke prilagođene Čarobnjak nudi dodatne mogućnosti i mogućnosti, ali postoje situacije u kojima morate da biste bili sigurni da imate odgovarajuće dozvole sami.

## <a name="related-documentation"></a>Srodni dokumentacija
Ako ne uspijete pročitajte dokumentaciju Integracija [vaših lokalnih identiteta sa servisu Azure Active Directory](../active-directory-aadconnect.md), u sljedećoj su tablici navedene veze na povezane teme.

Tema |  
--------- | ---------
Instalacija pomoću postavki Express | [Express instalacije Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Instalacija koristi prilagođene postavke | [Prilagođene instalacije Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Nadogradnja s DirSync | [Nadogradnja s alat za sinkronizaciju Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Express postavke instalacije
U odjeljku postavke Express Čarobnjak za instalaciju traži AD DS Enterprise administratorske vjerodajnice kako lokalni Active Directory može konfigurirati potrebne dozvole za Azure AD Connect. Ako nadograđujete s DirSync, vjerodajnice AD DS Enterprise administratori koriste se za ponovno postavljanje lozinke za račun koji se koristi DirSync. Trebat ćete i vjerodajnice za Azure AD globalni Administrator.

Stranica čarobnjaka  | Vjerodajnice prikupljeni | Potrebne dozvole| Koristi se za
------------- | ------------- |------------- |-------------
N/D|Korisnik koji pokreće čarobnjak za instalaciju| Administrator lokalnog poslužitelja| <li>Stvara se lokalni račun koji se koristi kao [račun servisa modula za sinkroniziranje](#azure-ad-connect-sync-service-account).
Povezivanje s Azure AD| Vjerodajnice directory Azure AD | Uloga globalnog administratora u Azure AD | <li>Omogućivanje sinkronizacije u direktoriju Azure AD.</li>  <li>Stvaranje [računa za Azure AD](#azure-ad-service-account) koja se koristi za sinkronizaciju na početak operacije u Azure AD.</li>
Povezivanje s AD DS | Lokalne vjerodajnice za Active Directory | Član grupe Administratori tvrtke (EA) u servisu Active Directory| <li>Stvara [računa](#active-directory-account) u servisu Active Directory i daje dozvola na njemu. Stvorili račun koristi se za čitanje i pisanje informacije directory tijekom sinkronizacije.</li>

### <a name="enterprise-admin-credentials"></a>Enterprise administratorske vjerodajnice
Vjerodajnica koriste se samo tijekom instalacije i koriste nakon dovršetka instalacije. Nije Enterprise administrator, a ne administrator domene, da biste provjerili dozvole u servisu Active Directory moguće je postaviti u sve domene.

### <a name="global-admin-credentials"></a>Globalni administratorske vjerodajnice
Vjerodajnica koriste se samo tijekom instalacije i ne koriste se nakon dovršetka instalacije. Koristi se za stvaranje [Azure AD računa](#azure-ad-service-account) koji se koristi za sinkronizaciju promjene Azure AD. Račun omogućuje sinkronizaciju i kao značajka u Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Dozvole za stvoreni AD DS računa za eksplicitnih postavke
[Račun](#active-directory-account) za čitanje i pisanje u AD DS stvara imati sljedeće dozvole prilikom stvaranja eksplicitnih postavke:

Dozvole | Koristi se za
---- | ----
<li>Replicirati promjene direktorija</li><li>Sve se mijenja repliciranje direktorija | Sinkroniziranje lozinkom
Čitanje/pisanje sva svojstva korisnika | Uvoz i Exchange hibridnog
Čitanje/pisanje sve iNetOrgPerson svojstva | Uvoz i Exchange hibridnog
Grupe za čitanje/pisanje sva svojstva | Uvoz i Exchange hibridnog
Čitanje/pisanje sva svojstva kontakta | Uvoz i Exchange hibridnog
Ponovno postavljanje lozinke | Priprema za omogućivanje upisima lozinke

## <a name="custom-settings-installation"></a>Prilagođene postavke instalacije
Kada pomoću prilagođenih postavki računa koji se koristi za povezivanje sa servisom Active Directory moraju se stvoriti prije instalacije. Dozvole mora odobriti ovaj račun pronaći ćete u [Stvaranje AD DS računa](#create-the-ad-ds-account).

Stranica čarobnjaka  | Vjerodajnice prikupljeni | Potrebne dozvole| Koristi se za
------------- | ------------- |------------- |-------------
N/D | Korisnik koji pokreće čarobnjak za instalaciju|<li>Administrator lokalnog poslužitelja</li><li>Ako koristite cijelog sustava SQL Server, korisnik mora imati sistemski Administrator (SA) u SQL</li>| Prema zadanim postavkama stvara lokalni račun koji se koristi kao [račun servisa modula za sinkroniziranje](#azure-ad-connect-sync-service-account). Račun samo kada se stvara administrator odredite određeni račun.
Instaliranje servisa sinkronizacije, mogućnost za račun servisa | AD ili lokalne korisničke vjerodajnice za račun | Korisniku, su dozvole dodijeljene pomoću čarobnjaka za instalaciju | Ako administrator određuje račun, taj račun se koristi kao račun servisa za sinkronizaciju servisa.
Povezivanje s Azure AD | Vjerodajnice directory Azure AD| Uloga globalnog administratora u Azure AD| <li>Omogućivanje sinkronizacije u direktoriju Azure AD.</li>  <li>Stvaranje [računa za Azure AD](#azure-ad-service-account) koja se koristi za sinkronizaciju na početak operacije u Azure AD.</li>
Povežite svoje direktorija | Lokalni Active Directory vjerodajnice za svaki skup stabala povezan s Azure AD | Dozvole ovise na koje značajke omogućiti i nalazi se u [račun za stvaranje u AD DS](#create-the-ad-ds-account) |Taj račun se koristi za čitanje i pisanje informacije directory tijekom sinkronizacije.
AD FS poslužitelja | Za svaki poslužitelj na popisu čarobnjak prikuplja vjerodajnice kada su vjerodajnice za prijavu korisnika čarobnjaka dovoljno za povezivanje | Administrator domene | Instalacija i konfiguracija uloga poslužitelja za AD FS.
Web-aplikacije proxy poslužiteljima |Za svaki poslužitelj na popisu čarobnjak prikuplja vjerodajnice kada su vjerodajnice za prijavu korisnika čarobnjaka dovoljno za povezivanje | Lokalni administrator na ciljnom računalu | Instalacija i konfiguracija uloga poslužitelja WAP.
Vjerodajnice za pouzdanost proxy poslužitelj |Servis za vanjski pristup vjerodajnice (vjerodajnice proxy koristi da biste registrirali za potvrdom pouzdanost u FS pouzdanima |Račun domene koja je lokalni administrator poslužitelja za AD FS | Početna registraciju FS WAP pouzdanosti certifikata.
AD FS račun servisa stranice, "Koristi domenu korisnički račun odabir" | AD korisničke vjerodajnice za račun | Korisnik domene | AD korisničkog računa čije vjerodajnice navedene koristi se kao račun za prijavu servisa AD FS.

### <a name="create-the-ad-ds-account"></a>Stvaranje računa za AD DS
Kada instalirate Azure AD Connect, račun koji navedete na stranici **Povezivanje vašeg direktorija** mora postojati u servisu Active Directory i imaju obavezne pravo. Čarobnjak za instalaciju provjerite dozvole i eventualne probleme s samo pronađene tijekom sinkronizacije.

Ako su vam potrebne dozvole ovisi o dodatne značajke koje omogućuju. Ako imate više domena, morate dobiti dozvole za sve domene u u skup stabala. Ako ne omogućite primjene tih značajki, potrebne su dozvole za **Korisnik domene** zadani.

Značajka | Dozvole
------ | ------
Sinkroniziranje lozinkom | <li>Replicirati promjene direktorija</li>  <li>Sve se mijenja repliciranje direktorija
Hibridna implementacija sustava Exchange | Dozvole za pisanje atribute navedenih u [upisima hibridnog sustava Exchange](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) za korisnike, grupe i Kontakti.
Lozinka upisima | Dozvole za pisanje atribute navedenih u [Početak rada s upravljanjem lozinke](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) za korisnike.
Uređaj upisima | Dozvole odobren s skriptu PowerShell kao što je opisano u [upisima uređaja](../active-directory-aadconnect-feature-device-writeback.md).
Grupa upisima | Čitanje, stvaranje, ažuriranje i brisanje Grupiraj objekte parametra OU gdje mora biti smješten distribucija grupe.

## <a name="upgrade"></a>Nadogradnja
Kada nadogradite s jedne verzije Azure AD Connect novo izdanje, potrebne su vam sljedeće dozvole:

Glavni | Potrebne dozvole | Koristi se za
---- | ---- | ----
Korisnik koji pokreće čarobnjak za instalaciju | Administrator lokalnog poslužitelja | Ažurirajte binarne datoteke.
Korisnik koji pokreće čarobnjak za instalaciju | Član ADSyncAdmins | Promjene pravila za sinkronizaciju i druge konfiguracije.
Korisnik koji pokreće čarobnjak za instalaciju | Ako koristite cijelog sustava SQL server: vlasnika baze podataka (ili sličan) sinkronizaciju modul baze podataka | Unesite promjene razine baze podataka, kao što su ažuriranje tablice s nove stupce.

## <a name="more-about-the-created-accounts"></a>Dodatne informacije o stvoreni računi

### <a name="active-directory-account"></a>Active Directory račun
Ako koristite eksplicitnih postavke, račun se stvara u servisu Active Directory koja se koristi za sinkronizaciju. Stvorili račun nalazi se u korijensku domenu skupa stabala u spremniku korisnika, a sadrži s **MSOL_**mjestu njegov naziv. Račun se stvara pomoću dugo složene lozinke koja istječe. Ako imate pravila za lozinke u vašoj domeni, provjerite je li Dugi i složene lozinke želite imati dopuštenje za taj račun.

![AD računa](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure AD Connect računa servisa sinkronizacije
Servis za lokalni račun se stvara pomoću čarobnjaka za instalaciju (osim ako ne navedete račun koji želite koristiti u prilagođene postavke). Račun je mjestu **AAD_** i koristiti za servis stvarni sinkronizaciju da biste pokrenuli kao. Ako instalirate Azure AD Connect na kontroleru domene, račun se stvara u domeni. Ako koristite udaljeni poslužitelj sa sustavom SQL server ili ako koristite proxy poslužitelj zahtijeva provjeru autentičnosti, račun servisa **AAD_** moraju nalaziti u domeni.

![Račun servisa za sinkronizaciju](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Račun se stvara pomoću dugo složene lozinke koja istječe.

Taj račun se koristi za pohranu lozinki za drugih računa na siguran način. Takve lozinke računa spremaju se na šifrirane u bazi podataka. Privatni ključevi za ključeve za šifriranje zaštićene šifriranjem cryptographic services tajna ključ pomoću Windows podataka zaštitu API-JA (sučelja DPAPI). Poništavanje lozinke na račun servisa nije jer Windows zatim uništiti ključeva za šifriranje za sigurnosnih vam razloga.

Ako koristite cijelog sustava SQL Server, račun servisa je vlasnika baze podataka stvorene baze podataka za sinkronizaciju modula. Servis neće funkcionirati kako je predviđeno s druge dozvole. Prijava na SQL također se stvara.

Račun također se dodijeliti dozvole za datoteke, ključevima registra i druge objekte vezane uz sinkronizaciju modula.

### <a name="azure-ad-service-account"></a>Račun servisa Azure AD
Račun u Azure AD se stvara za korištenje sinkronizacije servisa. Ovaj račun možete otkrije njegov naziv prikaza.

![AD računa](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Naziv poslužitelja koji se koristi s računom se možete otkrili u drugom dijelu korisničko ime. Na slici, naziv je poslužitelja FABRIKAMCON. Ako imate pripremna poslužitelje, svaki poslužitelj ima vlastiti račun. Nema ograničenja od 10 računa servisa za sinkronizaciju u Azure AD.

Račun servisa se stvara pomoću dugo složene lozinke koja istječe. To je dodijeljena posebno uloga **Račune za sinkronizaciju direktorija** koji ima dozvole za izvođenje zadaci sinkronizacije direktorija. Ta uloga posebne ugrađene ne može biti odobren izvan Čarobnjak za Azure AD Connect i Azure portal prikazuje taj račun s ulogom **korisnika**.

![AD računa uloga](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](../active-directory-aadconnect.md).
