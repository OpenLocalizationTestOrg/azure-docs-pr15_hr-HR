<properties
   pageTitle="Azure AD Connect: Povijest verzija izdanje | Microsoft Azure"
   description="Ova tema sadrži popis svih izdanja Azure AD Connect i Azure AD sinkronizacije"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Povijest verzija izdanja

Tim za Azure Active Directory redovito ažurira Azure AD Connect nove značajke i funkcije. Svi dodaci primjenjuju se na sve publike.

Ovaj članak namijenjen je omogućuju vam praćenje verzija objavljene, a da biste shvatili morate li ažurirati na najnoviju verziju ili ne.

Ovo je popis srodnih tema:

Tema |  
--------- | --------- |
Koraci za nadogradnju s Azure AD Connect | Različite načine za [nadogradnju s prethodne verzije za najnoviji](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect izdanje.
Potrebne dozvole | Potrebne dozvole da biste primijenili ažuriranja, potražite u članku [računa i dozvola](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Preuzimanje| [Povezivanje preuzimanje Azure AD](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Objavio: kolovoz 2016

**FIXED problemi:**

- Promjene da biste sinkronizirali interval ne odvija do završetku idućeg kruga sinkronizaciju.
- Čarobnjak za Azure AD Connect prihvatiti Azure AD račun čije korisničko ime započinje podvlakom (\_).
- Čarobnjak za Azure AD Connect neće uspjeti za provjeru autentičnosti Azure AD računa ako lozinku računa sadrži previše posebne znakove. Poruka o pogrešci "nije moguće provjeriti vjerodajnice. Sadrži pojavila se neočekivana pogreška." Vraća.
- Deinstalacija pripremna poslužitelja onemogućuje sinkronizaciju lozinke u klijentu za Azure AD i uzrokuje sinkronizaciju lozinke neće funkcionirati uz aktivnog poslužitelja.
- Sinkronizaciju lozinke ne uspijeva u neuobičajeno slučajevima kada postoji bez lozinke raspršivanje pohranjena na korisnike.
- Kada poslužitelj za Azure AD Connect je omogućen za pripremna način, lozinka upisima nije privremeno onemogućiti.
- Čarobnjak za Azure AD Connect Prikaži sinkronizacije stvarni lozinku i konfiguracija upisima lozinku kada je poslužitelj u pripremna način. Uvijek prikazuje ih kao onemogućena.
- Promjene konfiguracije sinkronizaciju lozinke i lozinka upisima su dosljedan pomoću čarobnjaka za Azure AD Connect kada poslužitelj je u pripremna načinu rada.

**Poboljšanja:**

- Cmdlet ažurirane Start ADSyncSyncCycle određivanje je li može uspješno pokrenuti novi ciklusa sinkronizaciju ili ne.
- Cmdlet Zaustavi ADSyncSyncCycle dodane da biste prekinuli sinkronizaciju ciklusa i operacije koje su trenutno u tijeku.
- Ažurirani cmdlet Zaustavi ADSyncScheduler da biste prekinuli sinkronizaciju ciklusa i operacije koje su trenutno u tijeku.
- Prilikom konfiguriranja [Proširenja direktorija](active-directory-aadconnectsync-feature-directory-extensions.md) u čarobnjaku za Azure AD Connect, AD atribut vrste "Teleteksta niza" sada moguće odabrati.

## <a name="111890"></a>1.1.189.0
Objavio: 2016 lipnja

**FIXED problema i poboljšanja proizvoda:**

- Azure AD Connect sada biti instalirana na poslužitelju usklađen FIPS.
    - Sinkronizaciju lozinke, potražite u članku [sinkronizaciju lozinke i FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Fiksni problem gdje NetBIOS naziv nije moguće razriješiti FQDN u poveznik za Active Directory.

## <a name="111800"></a>1.1.180.0
Objavio: 2016 svibanj

**Nove značajke:**

- Upozorava i pomaže vam potvrđivanja domene ako ne vidite prije pokretanja Azure AD Connect.
- Podrška za [Microsoft Cloud Njemačka](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
- Podrška za najnovije infrastrukture u [oblaku državne Microsoft Azure](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) s novim zahtjevima URL-a.

**FIXED problema i poboljšanja proizvoda:**

- Dodaje filtriranja u sinkronizaciju pravilo uređivač da biste lakše pronašli pravila za sinkronizaciju.
- Bolje performanse prilikom brisanja poveznik razmak.
- Fiksni problema kada je na isti objekt izbrisane i dodali u istom pokrenuti (zovu Izbriši/Dodaj).
- Onemogućeno pravilo za sinkroniziranje neće više objekata ponovno Omogućivanje uključeni i osvježite atributi nadogradnje ili imeničkog shemu.

## <a name="111300"></a>1.1.130.0
Objavio: 2016 u travnju

**Nove značajke:**

- Podrška za više vrijednosti atribute [Proširenja direktorija](active-directory-aadconnectsync-feature-directory-extensions.md).
- Podrška za više konfiguracije varijacija za [Automatska Nadogradnja](active-directory-aadconnect-feature-automatic-upgrade.md) smatraju uvjete za nadogradnju.
- Dodati neke cmdleti za [Prilagođeni raspored](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Objavio: 2016 ožujak

**FIXED problemi:**

- Unijeli li Express instalacija nije moguće koristiti na Windows Server 2008 (pre R2) od sinkronizaciju lozinke nije podržana na ovom operacijski sustav.
- Nadogradnja s DirSync konfiguraciji prilagođenog filtra ne funkcioniraju prema očekivanjima.
- Prilikom nadogradnje na noviju verziju i nema promjena konfiguracije, potpune uvoz/sinkronizacije treba zakazivati.

## <a name="111100"></a>1.1.110.0
Objavio: veljača 2016

**FIXED problemi:**

- Nadogradnja iz prethodnih izdanja neće funkcionirati ako instalacija ne nalazi u zadanu mapu **C:\Program Files** .
- Ako instalirate i poništavanje **pokrenuti sinkronizaciju postupak...** na kraju Čarobnjak za instalaciju ponovno pokrenuti čarobnjak za instalaciju neće moći na raspored.
- Alat za zakazivanje neće funkcionirati prema očekivanjima na poslužiteljima na kojima oblik datuma/vremena nije sad-hr. On će blokira `Get-ADSyncScheduler` da biste se vratili pravilnim vremenima.
- Ako ste instalirali starije verzije Azure AD Connect uz ADFS kao mogućnost za prijavu i nadogradnja, ne možete ponovno pokrenite čarobnjak za instalaciju.

## <a name="111050"></a>1.1.105.0
Objavio: veljača 2016

**Nove značajke:**

- Značajka [automatskog nadogradnje](active-directory-aadconnect-feature-automatic-upgrade.md) za kupce postavke Express.
- Podrška za globalni administrator pomoću MFA i PIM u čarobnjaku za instalaciju.
    - Morate dopustiti proxy poslužitelja i dopušta promet za https://secure.aadcdn.microsoftonline-p.com ako koristite MFA.
    - Morate dodati https://secure.aadcdn.microsoftonline-p.com na popis pouzdanih web-mjesta za MFA ispravno raditi.
- Dopusti promjenu korisnikove prijave način nakon početne instalacije.
- Dopusti [domene i OU filtriranja](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) u čarobnjaku za instalaciju. To dopušta povezivanje s šuma koje su dostupne neke domene.
- [Raspored](active-directory-aadconnectsync-feature-scheduler.md) je ugrađeni modula za sinkroniziranje.

**Značajke ulogu iz pretpregleda GA:**

- [Upisima uređaj](active-directory-aadconnect-feature-device-writeback.md).
- [Proširenja direktorija](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nove značajke pretpregleda:**

- Novi zadani sinkronizirati ciklusa interval je 30 minuta. Koristi se 3 sata za sve prethodnih izdanja. Dodaje podršku da biste promijenili [raspored](active-directory-aadconnectsync-feature-scheduler.md) ponašanje.

**FIXED problemi:**

- Na stranici Dodavanje DNS domene potvrda uvijek nije prepoznala domena.
- Upute za domenu administratorske vjerodajnice prilikom konfiguriranja ADFS.
- Na lokalni računi AD ne prepoznaje Čarobnjak za instalaciju ako koja se nalazi u domeni s različitim stabla DNS od korijensku domenu.

## <a name="1091310"></a>1.0.9131.0
Objavio: prosinac 2015.

**FIXED problemi:**

- Sinkroniziranje lozinkom možda neće funkcionirati kada promijenite lozinke u AD DS, ali radi kada postavite lozinku.
- Kada imate proxy poslužitelj, provjera autentičnosti za Azure AD možda neće uspjeti tijekom instalacije ili poništavanje nadogradnje na stranici Konfiguracija.
- Ažuriranje iz starijeg izdanja programa Azure AD Connect potpuno SQL Server neće uspjeti ako niste Pacifička u SQL.
- Ažuriranje iz starijeg izdanja programa Azure AD Connect u alat za analizu daljinske SQL Server prikazivat će se pogreška "Nije moguće pristupiti bazom podataka sustava ADSync SQL".

## <a name="1091250"></a>1.0.9125.0
Objavio: studenom 2015.

**Nove značajke:**

- Možete konfigurirajte ADFS za Azure AD pouzdanost.
- Možete osvježiti shema servisa Active Directory i Obnovi pravila za sinkronizaciju.
- Možete onemogućiti sinkronizaciju pravila.
- Možete definirati "AuthoritativeNull" kao novi slova u pravilo za sinkronizaciju.

**Nove značajke pretpregleda:**

- [Azure AD povezivanje stanja sinkronizacije](active-directory-aadconnect-health-sync.md).
- Podrška za sinkronizaciju lozinke [Azure servisa Active Directory Domain Services](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) .

**Novi podržani scenarij:**

- Podržava više lokalni Exchange tvrtke ili ustanove. Dodatne informacije potražite u članku [Hibridne implementacije s više šuma servisa Active Directory](https://technet.microsoft.com/library/jj873754.aspx) .

**FIXED problemi:**

- Problema sa sinkronizacijom lozinku:
    - Objekt premjestiti iz dosega u opseg neće imati lozinku sinkronizirati. U ovom incudes OU i atributa filtriranja.
    - Odabir nove OU će se uvrstiti u sinkronizaciji zahtijevaju sinkronizaciju cijelog lozinke.
    - Kada je omogućen onemogućene korisnik lozinka se neće sinkronizirati.
    - Pokušaj red lozinka je beskonačno i prethodne ograničenje od 5000 objekte s biti povučena je uklonjena.
    - [Otklanjanje poteškoća s Improved](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Ne može se povezati sa servisom Active Directory skupa stabala funkcionalni razinom Windows Server 2016.
- Ne mogu mijenjati grupu koja se koristila za filtriranje grupe nakon početne instalacije.
- Će više stvaranje novog korisničkog profila na poslužitelj za Azure AD Connect za sve korisnike s upisima lozinke omogućen način Promjena lozinke.
- Neće moći koristiti Dugi cijeli broj vrijednosti u sinkronizaciji pravilima dosega.
- Potvrdni okvir "uređaj upisima" onemogućen je li kontrolera nedostupno domena.

## <a name="1086670"></a>1.0.8667.0
Objavio: kolovoz 2015.

**Nove značajke:**

- Čarobnjak za instalaciju Azure AD Connect sada lokalizirani za sve jezike u sustavu Windows Server.
- Podrška za račun otključati prilikom korištenja upravljanja Azure AD lozinke.

**FIXED problemi:**

- Čarobnjak za instaliranje servisa Azure AD Connect zatvara se ako neki drugi korisnik ne riješi instalacije umjesto osobe koja je počeli instalaciju.
- Ako prethodni Deinstaliraj ne uspije Azure AD Connect čistu deinstalirati sinkronizaciju Azure AD Connect nije moguće ponovno instalirati.
- Nije moguće instalirati Azure AD Connect pomoću Express Instaliraj ako se korisnik ne korijensku domenu u skup stabala ili ako se koristi u ne-engleskoj verziji servisa Active Directory.
- Ako se ne može riješiti FQDN korisnički račun servisa Active Directory, prikazuje se netočni poruka o pogrešci "Nije uspjela za primjenu u shemi".
- Ako je račun koji se koristi Active Directory Connector promijenili izvan čarobnjaka, čarobnjak neće uspjeti na sljedeći se pokreće.
- Azure AD Connect ponekad ne uspijeva da biste instalirali na kontroler domene.
- Ne možete omogućiti i onemogućiti "Pripremna načinu" ako proširenje atribute dodane.
- Lozinka upisima ne uspijeva u nekim konfiguraciji zbog neispravni lozinku na poveznik za Active Directory.
- Nije moguće nadograditi DirSync ako dn koristi u atribut filtriranja.
- Viškom procesora koristi prilikom korištenja ponovno postavljanje lozinke.

**Uklonjene pretpregled značajke:**

- Značajke pretpregleda privremeno uklonjena [upisima korisnika](active-directory-aadconnect-feature-preview.md#user-writeback) na temelju povratnih informacija iz naših korisnika pretpregleda. Dodat će se ponovno kasnije prilikom smo ste riješili navedeni povratne informacije.

## <a name="1086410"></a>1.0.8641.0
Objavio: 2015 lipnja

**Početna izdanju programa Azure AD Connect.**

Promijenjeni naziv sa sinkronizacijom Azure AD za Azure AD Connect.

**Nove značajke:**

- [Ekspresne postavke](./connect/active-directory-aadconnect-get-started-express.md) instalacije
- Možete [konfigurirati ADFS](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Možete [nadogradili DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Sprječava nenamjerno brisanje](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- Uvodi [pripremna načinu rada](active-directory-aadconnectsync-operations.md#staging-mode)

**Nove značajke pretpregleda:**

- [Korisnik upisima](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Grupa upisima](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Uređaj upisima](active-directory-aadconnect-feature-device-writeback.md)
- [Proširenja direktorija](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Objavio: 2015 svibanj

**Novi zahtjev:**

- Azure AD Sinkroniziraj sada zahtijeva .net framework verzije 4.5.1 instalirati.

**FIXED problemi:**

- Upisima lozinke iz Azure AD se ne uspijeva uz poruku o pogrešci servicebus povezivanje.

## <a name="104910413"></a>1.0.491.0413
Objavio: 2015 Travanj

**FIXED problema i poboljšanja proizvoda:**

- Poveznik za Active Directory procesa briše pravilno ako je omogućeno koš za smeće, a ima više domena na skup stabala.
- Performanse operacije uvoza poboljšan za Azure Active Directory poveznik.
- Kada grupu premašila je ograničenje članstvo (po zadanom ograničenje je postavljeno na 50k objekte), grupi izbrisana je u Azure Active Directory. Novo ponašanje je da će ostati u grupu, pogreška se Expression.Error i nema novih promjena za članstvo će se izvesti.
- Novi objekt nije dostupna ako postupnu Izbriši s istom DN već nalazi u prostoru poveznik.
- Neki objekti su označene za sinkronizira tijekom delta sinkronizacije Iako ne mijenja kopirana bez postavljanja na objekt.
- Prisilno sinkronizaciju lozinke uklonit ćete Preferirani Kontroler popis.
- CSExportAnalyzer ima poteškoća s nekim državama objekte.

**Nove značajke:**

- Spoj sada se možete povezati na "ANY" Vrsta objekta u na MV.

## <a name="104850222"></a>1.0.485.0222
Objavio: veljača 2015.

**Poboljšanja:**

- Poboljšani uvoz performanse.

**FIXED problemi:**

- Sinkroniziranje lozinkom poštuje atribut cloudFiltered koristi atribut filtriranja. Filtrirani objekte više neće biti u opsegu za sinkronizaciju lozinke.
- U rijetko slučajevima gdje topologije imao vrlo mnogo kontrolera domena sinkronizaciju lozinke ne funkcionira.
- "Prestao-poslužitelja" prilikom uvoza iz poveznik za Azure AD nakon upravljanje uređajima omogućena u Azure AD/Intune.
- Uključivanje vanjskih sigurnosni upravitelji (FSPs) iz više domena u isti skup stabala uzrokuje pogrešku dvosmislenog spoja.

## <a name="104751202"></a>1.0.475.1202
Objavio: prosinac 2014.

**Nove značajke:**

- Podržana je sada da biste učinili sinkronizaciju lozinke za filtriranje atributa koji se temelje. Dodatne informacije potražite u članku [sinkronizaciju lozinke za filtriranje](active-directory-aadconnectsync-configure-filtering.md).
- Atribut msDS-ExternalDirectoryObjectID zapisuje natrag na AD. Dodati podrške za oba, pristup Internetu putem OAuth2 aplikacije za Office 365 i lokalnih poštanskih sandučića u Hibridnoj implementaciji sustava Exchange.

**FIXED probleme s nadogradnjom:**

- Novija verzija Pomoćnik za prijavu u dostupna na poslužitelju.
- Prilagođene instalacije put korišten da biste instalirali Azure AD sinkronizaciju.
- Programa kriterij koji nisu valjani prilagođene spoj blokira nadogradnju.

**Druge promjene:**

- Fiksni predložaka za Office Pro Plus.
- FIXED problema s instalacijom uzrokovanih korisničkih imena koje počinju crticu.
- Fiksni gubitka postavku sourceAnchor prilikom pokretanja čarobnjaka za instalaciju drugi put.
- FIXED ETW praćenje za sinkronizaciju lozinke

## <a name="104701023"></a>1.0.470.1023
Objavio: listopad 2014.

**Nove značajke:**

- Sinkronizaciju lozinke iz više lokalnog servisa Active Directory za Azure AD.
- Lokalizirane instalacija korisničkog Sučelja za sve jezike u sustavu Windows Server.

**Nadogradnja s GA AADSync 1.0**

Ako već imate Azure AD sinkronizaciju instalirana, postoji jedan korak dodatne ćete morati poduzeti u slučaju promijenjene pravila sinkronizacije – okvir. Nakon nadogradnje na na 1.0.470.1023 pustite, sinkronizacije postoje pravila koju ste izmijenili. Za svako izmijenjene pravilo sinkronizaciju učinite sljedeće:

- Pronađite pravilo za sinkronizaciju ste izmijenili i uzeti u obzir promjene.
- Brisanje pravila za sinkronizaciju.
- Pronađite novo pravilo sinkronizaciju stvorio Azure AD sinkronizaciju i ponovno primijenili promjene.

**Dozvole za račun servisa Active Directory**

AD računa mora biti odobren dodatne dozvole da biste mogli čitati raspršivanja lozinke iz servisa Active Directory. Da biste dodijelili dozvole su pod nazivom "Replikaciju direktorija promjene" i "Replikaciju direktorija sve promjene". Oba dozvole potrebne za može čitati raspršivanja lozinke

## <a name="104190911"></a>1.0.419.0911
Objavio: rujan 2014.

**Početna izdanje Azure AD sinkronizacije.**

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
