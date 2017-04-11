<properties
   pageTitle="Azure AD Connect: Nadogradnja s prethodne verzije | Microsoft Azure"
   description="U članku se objašnjava različite načine za nadogradnju na najnovije izdanje Azure Active Directory povezivanje, uključujući nadogradnje i swing migracije."
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
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Nadogradnja iz prethodne verzije najnoviji
U ovoj se temi opisuju različite metode koje možete koristiti da biste nadogradili instalacijom Azure AD Connect na najnovije izdanje. Preporučujemo da ostavite sami trenutnim izdanja Azure AD Connect. Koraci opisani u [swing migracije](#swing-migration) se koriste se pri upućivanju znatno konfiguracije promjena.

Ako želite da biste nadogradili DirSync, potražite u članku [Nadogradnja iz alata za sinkronizaciju Azure AD (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) umjesto toga.

Postoji nekoliko različitih strategije nadogradnje Azure AD Connect.

Način | Opis
--- | ---
[Automatska Nadogradnja](active-directory-aadconnect-feature-automatic-upgrade.md) | U slučaju korisnika s instalacijom sustava express to je najjednostavniji način.
[Nadogradnja na mjestu](#in-place-upgrade) | Ako imate pojedinačni poslužitelj, nadogradite na instalacije na mjestu na istom poslužitelju.
[Swing migracije](#swing-migration) | Dva poslužitelja možete Priprema nešto poslužitelje s novim izdanjima ili konfiguracija i promjena aktivnog poslužitelja kada ste spremni.

Potrebne dozvole, potražite u članku [dozvole potrebne za nadogradnju](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade).

## <a name="in-place-upgrade"></a>Nadogradnja na mjestu
Nadogradnju na mjestu funkcionira za prijelaz s programa Azure AD sinkronizaciju ili Azure AD Connect. Ga neće funkcionirati za DirSync ili rješenje s FIM + Azure AD poveznik.

Ta je metoda Preferirani kada se jednom poslužitelju i manje od otprilike 100 000 objekte. Ako su promjene pravila – okvir Sinkroniziraj potpunog uvoza i potpune sinkronizacije pojavljuje se nakon nadogradnje. Time se osigurava da se nova konfiguracija primjenjuje na sve postojeće objekte u sustavu. To može potrajati od nekoliko sati ovisno o broju objekata u opsegu modula za sinkroniziranje. Sinkronizacija raspored normalni delta po zadanom svakih 30 minuta obustavljena je, no i dalje sinkronizaciju lozinke. Preporučujemo da biste učinili nadogradnju na mjestu tijekom vikenda. Ako nema promjena konfiguracije – okvir s novim izdanjima Azure AD Connect, zatim normalno delta uvoz/sinkronizaciju počet će umjesto toga.  
![Nadogradnja na mjestu](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Ako ste unijeli promjene u pravila – okvir sinkronizacije, te postavit će se vratiti zadane konfiguracije pri nadogradnji. Da biste provjerili je li konfiguraciju čuva se između nadogradnje, provjerite je li promjena kao što je opisano u [najbolje prakse za promjenu Zadana konfiguracija](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

## <a name="swing-migration"></a>Swing migracije
Ako imate složene implementacije ili vrlo više objekata, možda će nepraktično za nadogradnju na mjestu u sustavu uživo. To se može za neke korisnike koji potrajati više dana, a za to vrijeme bez promjena delta obradit će se. Ta metoda koristi se kada namjeravate znatno promjene konfiguracije i želite ih isprobate prije s oblakom te pomiču se.

Preporučeni način za scenarija jest korištenje swing migracije. Morate (najmanje) dva poslužitelja, jedan aktivno i jedan pripremna poslužitelj. Odgovoran za učitavanje aktivni radni je active server (puna crta plava slici u nastavku). Pripremna server (isprekidane crte Ljubičasta slici u nastavku) se pripremite novo izdanje ili konfiguracija, a kada je potpuno jeste li spremni ovaj poslužitelj je aktivna. Prethodne aktivnog poslužitelja, sada sa starije verzije ili konfiguracije instaliran, ne izvrši pripremna poslužitelja i nadograditi.

Dva poslužitelja možete koristiti različite verzije. Ako, na primjer, active server namjeravate isključiti možete koristiti Azure AD sinkronizaciju i novi pripremna poslužitelj možete koristiti Azure AD Connect. Ako koristite swing migracije za razvoj Nova konfiguracija je dobro imati iste verzije na dva poslužitelja.  
![Pripremna poslužitelja](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Napomena: Ga je je Primijetit ćete da se neki klijenti radije da bi se tri ili četiri poslužitelji za taj scenarij. Kad je ažuriran pripremna poslužitelj, nemate poslužitelj za sigurnosne kopije u slučaju [Izrada oporavak](active-directory-aadconnectsync-operations.md#disaster-recovery). S tri ili četiri poslužiteljima jedan skup primarni/čekanja poslužitelje s novom verzijom moguće je pripremiti, jamči da su uvijek pripremna poslužitelja spremna za preuzimanje uloge.

Ove korake funkcionira i premjestiti iz Azure AD sinkronizaciju ili rješenje s FIM + Azure AD poveznik. Ti koraci ne funkcioniraju za DirSync, ali iste swing migracije (naziva se i paralelno implementacije) metode koracima za DirSync pronaći ćete u [Nadograditi Azure Active Directory (DirSync) sinkronizirati](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Swing koraka migracije

1. Ako koristite Azure AD Connect na poslužiteljima i samo Svrha promjene konfiguracije, provjerite je li aktivnog poslužitelja i pripremna poslužitelja i koristite istu verziju. Koji olakšava da biste usporedili razlike kasnije. Ako nadograđujete s Azure AD sinkronizaciju, sljedećim poslužiteljima koriste različite verzije. Ako nadograđujete iz starije verzije Azure AD Connect, preporučuje se u da biste započeli s dva poslužitelja koristeći istu verziju, ali nije obavezno.
2. Ako ste unijeli neke prilagođene konfiguracije, a poslužitelj pripremna ste, slijedite korake u odjeljku [pomicanje prilagođene konfiguracije iz Aktivno pripremna poslužitelj](#move-custom-configuration-from-active-to-staging-server).
3. Ako nadograđujete s instaliranom prethodnom verzijom Azure AD Connect, nadogradite pripremna poslužitelja na najnoviju verziju. Ako premještate iz Azure AD sinkronizaciju, instalirajte Azure AD Connect na poslužitelj za pripremna.
4. Obavijestite modula za sinkroniziranje izvođenje potpunog uvoza i potpune sinkronizacije na poslužitelj za pripremna.
5. Provjera je nova konfiguracija jeste li uzrokovati neočekivane promjene pomoću koraka u odjeljku **Potvrda** potvrda [konfiguraciju poslužitelja](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Ako nešto nije kao očekivanog, ispravne, izvođenja uvoz i sinkronizacija pa provjerite je li sve dok ne izgleda dobro podataka. Povezane teme pronaći ćete korake u nastavku.
6. Parametar pripremna poslužitelja aktivnog poslužitelja. To je u posljednjem koraku **Prebacivanje aktivnog poslužitelja** potvrda [konfiguraciju poslužitelja](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Ako nadograđujete Azure AD Connect, nadogradite poslužitelja u pripremna načinu rada na najnovije izdanje. Da biste dobili podatke i konfiguracije nadograditi poduzmite iste korake kao prije. Ako ste nadogradili Azure AD sinkronizaciju, sada možete isključiti i isključiti poslužitelj staru.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Premještanje prilagođena konfiguracija iz Aktivno pripremna poslužitelj
Ako ste unijeli promjene konfiguracije u active server, morate provjerite je li iste promjene će se primijeniti pripremni poslužitelj.

Pravila za prilagođene sinkronizaciju ste stvorili, možete premjestiti sa servisom PowerShell. Druge promjene potrebno primijeniti na isti način kao u oba sustava, a nije moguće migrirati.

Stvari koje morate provjeriti je konfiguriran na isti način kao na poslužiteljima:

- Veza na istom šuma.
- Željene domene i OU filtriranja.
- Zajedničke neobavezno značajke, kao što su sinkronizaciju lozinke i upisima lozinku.

**Premještanje pravila za sinkronizaciju**  
Da biste premjestili pravila za prilagođene sinkronizacije, učinite sljedeće:

1. Otvorite **Uređivač pravila za sinkronizaciju** na aktivnog poslužitelja.
2. Odaberite prilagođenog pravila. Kliknite **Izvezi**. Tako ćete prikazati u prozoru Blok za pisanje. Spremanje privremene datoteke s nastavkom PS1. Time se omogućuje skriptu PowerShell. Kopirajte datoteku ps1 pripremni poslužitelj.  
![Izvoz pravila za sinkronizaciju](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. GUID poveznik razlikuje se na poslužitelju pripremna i mora se promijeniti. Da biste dobili GUID pokrenuti **Sinkronizaciju pravila Editor**, odaberite jednu od-okvir pravila koja predstavlja isti povezanog sustav i kliknite **Izvezi**. Zamijenite GUID u datoteci PS1 GUID pripremna poslužitelja.
4. U promptu ljuske PowerShell pokrenite datoteku PS1. To će stvoriti pravila za prilagođene sinkronizacije na pripremna poslužitelju.
5. Ako imate više pravila za prilagođene, ponovite za sva prilagođena pravila.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
