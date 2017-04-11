<properties
    pageTitle="Postavke i podaci za roaming najčešća pitanja vezana uz | Microsoft Azure"
    description="Nudi odgovore na neka pitanja administratori mogu imati o postavkama i sinkroniziranje podataka aplikacije."
    services="active-directory"
    keywords="Enterprise stanja zajedničke postavke oblaka windows najčešća pitanja na enterprise stanje roaming"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="settings-and-data-roaming-faq"></a>Postavke i podaci za roaming najčešća Pitanja

U ovoj se temi navedeni odgovori na neka pitanja administratori možda o postavkama i sinkroniziranje podataka aplikacije.

## <a name="what-data-roams"></a>Što su podaci roams?
**Postavke sustava Windows**: postavke PC-JA koji su ugrađeni u operacijskog sustava Windows. Općenito govoreći, ovo su postavke koje prilagodite svoj PC i sadrže sljedeće široke kategorije:

- *Temu*koja sadrži značajke kao što su teme i programske trake postavke na radnoj površini.
- *Postavke preglednika Internet Explorer*, uključujući nedavno otvoriti kartice i Favoriti.
- *Postavke preglednika rub*, kao što su Favoriti i popis za čitanje.
- *Lozinke*, uključujući Internet lozinki, profili Wi-Fi i drugim korisnicima.
- *Osobne jezične postavke*, što uključuje postavke za raspored tipkovnice, jezik sustava, datum i vrijeme i više.
- *Olakšani pristup značajkama*, kao što su teme jakog kontrasta, pripovjedača i Povećalo.
- *Postavke drugih sustava Windows*, kao što su postavke naredbeni redak i popis aplikacija.


**Podataka aplikacije**: univerzalni Windows aplikacije možete pisati podataka postavke roaming mapu, a svi podaci mogli unijeti u ovu mapu automatski će se sinkronizirati. Je prema gore za pojedinačne proizvođač dizajnirati aplikacije da biste iskoristili tu mogućnost. Dodatne informacije o razvoju univerzalni Windows aplikacije koja koristi roaming, potražite u članku [appdata prostora za pohranu API-JA](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) i [Windows 8 appdata roaming za razvojne inženjere bloga](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Koji račun koristi postavkama sinkronizacije?
U sustavu Windows 8 i Windows 8.1, postavke sinkronizacije uvijek koristi korisničke račune za Microsoft. Enterprise korisnici imali mogućnost da biste se povezali Microsoftov račun za svoj račun servisa Active Directory domene da biste pristupili postavkama sinkronizacije. U sustavu Windows 10, to povezani Microsoftov račun je funkcija zamijenjena s računa primarni/sekundarni framework.

Primarni račun se definira kao račun koji se koristi za prijavu u Windows. To se može biti Microsoftov račun, računa za Azure Active Directory (Azure AD), račun lokalnog servisa Active Directory ili lokalni račun. Osim primarni račun korisnici sustava Windows 10 možete dodati jedan ili više računa sekundarne oblaka za svoj uređaj. Sekundarni račun obično je Microsoftov račun, račun za Azure AD ili neki drugi račun, kao što su Gmail ili Facebook. Sekundarni poslovne subjekte omogućuje pristup dodatna servisa kao što su jedinstvene prijave i iz Windows trgovine, ali se ne nalaze instaliranog powering postavke sinkronizacije.

U sustavu Windows 10, mogu se koristiti samo primarni račun za uređaj za postavke sinkronizacije (potražite u članku [kako nadograditi iz Microsoftova računa postavke sinkronizacije u sustavu Windows 8 za Azure AD postavke sinkronizacije u sustavu Windows 10?](active-directory-windows-enterprise-state-roaming-faqs.md#How-do-I-upgrade-from-Microsoft-account-settings-sync-in-Windows-8-to-Azure-AD-settings-sync-in Windows-10?)).

Podaci nikad ne miješana između različitih korisničke račune na uređaju. Postoje dva pravila za postavke sinkronizacije:

- Postavke sustava Windows uvijek se premještati s primarni račun.
- Aplikacija podataka će označeni račun koji se koristi za aplikaciju. Sinkronizirat će se samo aplikacije označeni primarni račun. Označavanje za aplikaciju vlasništvo određuje kada aplikacije strani – koja se učitava putem iz Windows trgovine i upravljanje mobilnim uređajima (MDM).

Ako nije moguće prepoznati aplikacije programa vlasnika, premještati će s primarni račun. Ako na uređaju se nadogradili sustav Windows 8 ili Windows 8.1 na Windows 10, sve aplikacije će označili dobivenog putem Microsoftova računa. To je zato što većina korisnika nabaviti aplikacije u trgovini Windows i pojavio ne podržava iz trgovine Windows Azure AD računi prije no što Windows 10. Ako je instaliran aplikaciju putem licencu za izvanmrežno, aplikacija će strukturiranih pomoću primarni račun na uređaju.

>[AZURE.NOTE]  
> Uređaje Windows 10 koje su enterprise vlasništvo i povezani ste Azure AD više ne možete povezati računa za Microsoft račun domene. Mogućnost za Microsoftov račun povezivanja s računom domene i sinkroniziranje podataka svih korisnika na Microsoftov račun (to jest, Microsoftov račun roaming putem povezanog Microsoftov račun i funkcionalnost imenika Active Directory) je uklonjena s uređaja Windows 10 pridruženim povezanog web-mjesto servisa Active Directory ili u okvir za Azure AD okruženje.

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Kako nadograditi iz Microsoftova računa postavke sinkronizacije u sustavu Windows 8 za Azure AD postavke sinkronizacije u sustavu Windows 10?
Ako su se pridružite domene servisa Active Directory pomoću Microsoftova računa povezanog izvodi Windows 8 ili Windows 8.1, postavke će se sinkronizirati putem Microsoftova računa. Nakon nadogradnje na Windows 10, nastavit ćete sinkronizirati korisničkih postavki putem Microsoftova računa kao su domene pridruženo korisnika i domene servisa Active Directory povezati se s Azure AD.

Ako lokalne domene servisa Active Directory povezati s Azure AD, uređaj će pokušati sinkronizirati postavki pomoću s povezanog računa Azure AD. Ako administrator Azure AD omogućite Enterprise stanje Roaming, vaše povezani račun za Azure AD će se prestati sinkronizirati postavke. Ako ste korisnik sustava Windows 10, a zatim se prijavite pomoću identitet Azure AD, će početi sinkronizirati postavke sustava windows čim administratoru omogućuje postavke sinkronizacije putem Azure AD.

Ako se pohranjuju osobne podatke na uređaju tvrtke, imajte na umu da s operacijskim Sustavom Windows i podataka iz aplikacija će početi sa sinkronizacijom za Azure AD. To je utjecaju na sljedeće:

- Osobne postavke računa Microsoft će drift osim postavke na svoj rad ili školske račune za Azure AD. To je zato što je Microsoftov račun i Azure AD postavke sinkronizacije sada koristite odvojene račune.
- Osobne podatke kao što su Wi-Fi lozinki, vjerodajnice web i Favoriti preglednika Internet Explorer koje su prethodno sinkroniziran putem Microsoftova računa povezanog će se sinkronizirati putem Azure AD.


## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Kako učiniti Microsoftov račun i Azure AD Enterprise stanje Roaming interoperabilnost rada?
U studenom 2015 ili novija verzija izdanja sustava Windows 10, Enterprise stanje Roaming podržano je samo za jedan račun odjednom. Ako se prijavite u sustav Windows pomoću tvrtke ili obrazovne ustanove Azure AD, svi podaci će se sinkronizirati putem Azure AD. Ako ste se prijavili u sustav Windows pomoću osobnog Microsoftova računa, svi podaci će se sinkronizirati putem Microsoftova računa. Univerzalni appdata premještati pomoću samo primarni prijavu račun na uređaju pa će premještati samo ako je licenca aplikacije vlasnik primarni račun. Univerzalni appdata za aplikacije posjeduje svih računa za sekundarne će se sinkronizirati.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Postavke sinkronizirati za Azure AD račune iz više klijenata?
Kada više Azure AD računa iz različitih klijenata za Azure AD se na istom uređaju, morate ažurirati registra uređaja možete komunicirati s upravljanjem pravima za Azure (Azure RMS) za svaki klijent Azure AD.  

1. Pronađite GUID za svaki klijent Azure AD. Otvorite portal sustava Azure klasični i odaberite klijent za Azure AD. GUID za klijent se URL u adresnoj traci preglednika. Ako, na primjer: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Nakon što ste GUID, morat ćete dodati ključa registra **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<smjernice ID GUID >**.
Iz ključa **klijentu GUID ID -a** stvorite novu više nizova vrijednost (REG-VIŠESTRUKI-SZ) pod nazivom **AllowedRMSServerUrls**. Za svoje podatke navesti licenciranja raspodjele točke URL-u drugim Azure klijenata koji uređaj pristupa.
3. Licenciranje raspodjele točke URL-ove možete pronaći tako da pokrenete cmdlet **Get-AadrmConfiguration** . Ako vrijednosti za **LicensingIntranetDistributionPointUrl** i **LicensingExtranetDistributionPointUrl** razlikuju, navedite obje se vrijednosti. Ako su vrijednosti jednake, samo jednom odredite vrijednost.


## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Što su roaming mogućnosti postavki za postojeće aplikacije za radnu površinu sustava Windows?
Roaming funkcionira samo za univerzalni Windows aplikacije. Za omogućivanje roaming postojeću radnu površinu programa Windows dostupne su dvije mogućnosti:

- [Radna površina most](http://aka.ms/desktopbridge) pomaže u premjestiti postojeće Windows aplikacijama za stolna računala i univerzalni platformu sustava Windows. Na tom mjestu minimalnog kod promjene će se zatražiti da biste iskoristili roaming za Azure AD aplikacije podataka. Na radnoj površini most sadrži aplikacije s identitet aplikacije koje je potrebno da biste omogućili podataka aplikacije roaming za postojeće aplikacija za stolna računala.
- [Korisničko sučelje virtualizacije (vrijednost-V)](https://technet.microsoft.com/library/dn458947.aspx) olakšava stvaranje predloška prilagođene postavke za postojeće Windows aplikacija za stolna računala i omogućivanje roaming za Win32 aplikacija. Ta mogućnost ne zahtijeva proizvođač da biste promijenili kod aplikacije. Vrijednost-V ograničeno je na lokalni Active Directory roaming za korisnike koji ste kupili paket za optimizaciju Microsoft radnu površinu.

Administratori mogu konfigurirati vrijednost-V tako da se premještati podataka za stolna računala za Windows tako da promijenite roaming postavke s operacijskim Sustavom Windows i univerzalni aplikacije podataka putem [vrijednost V grupe pravila](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), uključujući:

- Premještati pravila grupe postavke za Windows
- Sinkronizacija pravilnik grupe aplikacija za Windows
- Prebacivanje u odjeljku aplikacije u pregledniku Internet Explorer

U budućnosti Microsoft može istražiti probleme načina za upućivanje vrijednost V Duboko integriran u Windows i proširivanje vrijednost-V tako da se premještati postavke putem oblaka Azure AD.


## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Mogu li pohraniti sinkronizirane postavke i podaci za lokalno?
Enterprise stanje Roaming sprema sve sinkronizirane podatke u Azure oblaka. Vrijednost-V nudi lokalnog roaming rješenja.

## <a name="who-owns-the-data-thats-being-roamed"></a>Tko je vlasnik podatke koje je u tijeku prebacivanje?
Korporacije vlastite podatke prebacivanje putem Roaming stanje Enterprise. Podaci se pohranjuju u Azure podatkovnog centra. Sve korisničke podatke šifriran na putu i na ostale u oblak pomoću servisa Azure RMS. Ovo je poboljšanje u usporedbi s Microsoft utemeljen na račun postavke sinkronizacije, što je šifrira samo određenim povjerljive podatke kao što su korisničke vjerodajnice prije ostavlja uređaj.

Microsoft nastoji zaštita korisničkih podataka. Enterprise korisničkih podataka u postavkama automatski šifriran po Azure RMS prije ostavlja uređaj sa sustavom Windows 10 da bi drugi korisnik ne može čitati te podatke. Ako vaša tvrtka ili ustanova plaćenu pretplatu za Azure RMS, možete koristiti druge značajke servisa Azure RMS, kao što su praćenje i opoziv dokumenata, automatski zaštita poruke e-pošte koja sadrži povjerljive podatke, i upravljati vlastite tipke ("Premjesti vlastiti ključ" rješenje, poznata i kao BYOK). Dodatne informacije o značajki i funkcioniranje servisa Azure RMS potražite u članku [što je upravljanje pravima za Azure](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Možete upravljati sinkronizacije za određenu aplikaciju ili postavku?
U sustavu Windows 10, ne postoji MDM ili pravilnik grupe postavka da biste onemogućili roaming za pojedinačne aplikacije. Administratori za vanjske korisnike možete onemogućiti sinkronizaciju appdata za sve aplikacije na uređaju upravljanih, ali postoji bez precizniji kontrole na razini po aplikaciji ili unutar aplikacije.

## <a name="how-can-i-enable-or-disable-roaming"></a>Kako omogućiti ili onemogućiti roaming?
U aplikaciji **Postavke** odaberite **računi** > **Sinkronizacija postavki**. S ove stranice možete vidjeti koji račun koristi se premještati postavke, a možete omogućiti ili onemogućiti pojedinačne grupe postavke tako da se prebacivanje.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Što je Microsoftov preporuke za omogućivanje roaming u sustavu Windows 10?
Microsoft ima nekoliko različite postavke roaming rješenja dostupna, uključujući Roaming korisničke profile, vrijednost V i Enterprise stanje mreže.  Microsoft predano radi na što ulaganja u Enterprise stanja Roaming u budućim verzijama sustava Windows. Ako vaša tvrtka ili ustanova nije spreman ili upoznati s premještanje podataka s oblakom, pa preporučujemo korištenje vrijednost V kao primarni tehnologije za roaming. Ako vaša tvrtka ili ustanova zahtijeva roaming podrška za postojeće aplikacije za radnu površinu sustava Windows, ali je krenuti da biste premjestili u oblak, preporučujemo da koristite Enterprise stanje mreže i vrijednost V. Iako su slične tehnologije vrijednost V i Enterprise stanje Roaming, nisu isključuju. Oni nadopunjuju međusobno kako biste bili sigurni da vašoj tvrtki ili ustanovi omogućuje roaming servise koji su potrebni za korisnike.  

Prilikom korištenja Enterprise stanje Roaming i vrijednost V, primjenjuju se sljedeća pravila:

- Enterprise stanje Roaming je primarni agent za roaming na uređaju. Vrijednost-V koristi se dodatku izjavi "Win32 praznina."
- Vrijednost-V roaming za postavke sustava Windows i moderan podataka UWP aplikacije trebalo bi ga onemogućiti prilikom korištenja grupi vrijednost V pravila. Te su pokriven Roaming stanje Enterprise.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Kako Enterprise stanje Roaming podržava na Infrastruktura virtualne radne površine (VDI-JA)?
Enterprise stanje Roaming podržana je na klijentskom računalu Windows 10 SKU-ove, ali ne i na poslužitelju SKU-ove. Ako klijent VM nalazi se na računalo hypervisor i daljinsko prijave virtualnog računala, podaci će se premještati. Ako više korisnika zajednički koristiti istu OS, a korisnici daljinski prijaviti na poslužitelj za sučelje za radnu površinu, roaming možda neće funkcionirati. Scenarij potonjem temelji na sesiji službeno nije podržana.


## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Što se događa kada moje tvrtke ili ustanove kupi Azure RMS nakon korištenja roaming?
Ako vaša tvrtka ili ustanova već koristi roaming u sustavu Windows 10 s besplatna pretplata ograničeno pomoću servisa Azure RMS kupnju plaćenu pretplatu za Azure RMS neće imati utjecaja na roaming značajku i bez promjene konfiguracije bit će vam potreban je vaš IT administrator.

## <a name="known-issues"></a>Poznati problemi

- Ako pokušate prijavite se na uređaju sa sustavom Windows pomoću pametne kartice ili virtualne pametna kartica, postavke sinkronizacije će prestati s radom. Buduća ažuriranja za Windows 10 može riješiti problem.
- Za Internet Explorer Favoriti sinkronizaciju da biste radili već morat ćete kumulativnim ažuriranjem za srpanj za Windows 10 (Sastavi 10586.494 ili noviji).
- Podatke koji su zaštićeni s zaštiti podataka Windows se neće sinkronizirati do Roaming stanje Enterprise. Osim toga, strojevima koji imaju Windows informacije zaštitu će sučelje sinkronizaciju teme. 
- U određenim uvjetima Enterprise stanje Roaming mogu se neće sinkronizirati podatke ako je konfiguriran Azure višestruke provjere autentičnosti.
    - Ako je uređaj konfiguriran obavezne [Višestruka provjera autentičnosti](multi-factor-authentication.md) na portalu servisa Azure Active Directory, možda neće sinkronizirati postavke prilikom prijave u Windows 10 uređaja pomoću lozinke. Ta vrsta konfiguracije višestruka provjera autentičnosti namijenjen je da biste zaštitili Azure administratorskog računa. Administrator korisnici će moći sinkronizirati prijavom svoje uređaje Windows 10 s njihovim [Microsoft Passport za posao](active-directory-azureadjoin-passport.md) PIN-a ili s povećanim višestruka provjera autentičnosti prilikom pristupa druge Azure servise kao što je Office 365.
    - Sinkronizacija uspijeva Ako administrator konfigurira pravila uvjetnog pristup Active Directory vanjski pristup servisima višestruke provjere autentičnosti i istekne token za pristup na uređaju.  Provjerite je li da prijava i odjava PIN-a za [Microsoft Passport za posao](active-directory-azureadjoin-passport.md) ili dovršiti višestruka provjera autentičnosti prilikom pristupa druge Azure servise kao što je Office 365.

- Ako je računalo pridruženo domene s automatsko Registracija uređaja Azure Active Directory, ga može doći do prekida sinkronizaciju ako je računalo službenoj za prošireni vremenskog razdoblja, a možete završiti provjeru autentičnosti domene. Da biste riješili taj problem, povežite računalo s korporacijskom mrežom tako da možete nastaviti sinkronizaciju.


## <a name="related-topics"></a>Povezane teme
- [Pregled za roaming stanje Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
- [Omogućivanje enterprise stanje roaming servisa Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
- [Pravilnik grupe i postavke MDM za postavke sinkronizacije](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Referenca za Windows 10 roaming postavke](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
