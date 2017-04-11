<Properties
    pageTitle="Azure Active Directory uvjetno pristup | Microsoft Azure"  
    description="Da biste potražili određene uvjete prilikom provjere autentičnosti za pristup aplikacijama pomoću kontrola uvjetno pristupa u Azure Active Directory."  
    services="active-directory"
    keywords="Uvjetno pristup aplikacijama, uvjetno pristupa pomoću Azure AD, a zatim sigurnog pristupa resursima tvrtke, pravila uvjetnog programa access"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Uvjetno pristupa u Azure Active Directory   

Mogućnosti upravljanja u programu access na uvjetno Azure Active Directory (Azure AD) nude jednostavnih načina da biste lakše zaštita resursa u oblak i lokalne. Pravila uvjetnog access kao višestruka provjera autentičnosti možete zaštititi od rizika od krađe i phished vjerodajnice. Druge pravila uvjetnog pristup može zaštititi podatke u vašoj tvrtki ili ustanovi sigurni. Ako, na primjer, osim zahtijeva vjerodajnice, možda pravila te samo uređaje koji se upisani u sustavu za upravljanje mobilnim uređajima kao što je Microsoft Intune mogu pristupiti osjetljive usluge tvrtke ili ustanove.


## <a name="prerequisites"></a>Preduvjeti

Azure AD uvjetno pristup je značajka servisa [Azure Active Directory Premium](http://www.microsoft.com/identity). Svaki korisnik tko može pristupiti programa access uvjetnog pravila primjenjuju mora imati licencu za Azure AD Premium. Dodatne informacije o preduvjetima za licence u [izvješću Nelicenciranih korisnika](https://aka.ms/utc5ix).


## <a name="how-is-conditional-access-control-enforced"></a>Kako se provodi kontrola uvjetno pristupa?  

S kontrolom uvjetno pristup mjestu, Azure AD provjerava za određenim određene kriterije za korisnika da pristupi aplikacije. Kada se zadovolje uvjeti za access, korisnik provjere autentičnosti i možete pristupiti s računala.  

![Pregled uvjetno programa access](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Uvjeta

Ovo su uvjeta koje možete uvrstiti u pravilo uvjetnog pristupa:

- **Članstvo u grupi**. Kontrola korisničkog pristupa na temelju članstvo u grupi.

- **Mjesto**. Pomoću mjesto korisniku okidača višestruke provjere autentičnosti, a pomoću kontrola bloka kada korisnik nije pouzdano mreži.

- **Platforme uređaja**. Pomoću platforme uređaja, kao što su iOS, Android, Windows Mobile ili Windows, kao uvjeta za primjenu pravila.

- **Uređaj s omogućenim**. Savezna država uređaj, hoće li omogućiti ili onemogućiti, se provjerava tijekom izvođenja pravila uređaja. Ako onemogućite izgubljene ili ukradeno uređaj u imeniku, više ne možete ispunjavaju zahtjeve pravila.

- **Odgovornost za prijavu i korisničko**. Možete koristiti [zaštitu Azure AD identiteta](active-directory-identityprotection.md) za pravila uvjetnog pristup rizika. Pravila uvjetnog pristup rizika pomoći dati vaše tvrtke ili ustanove advance zaštite ovisno o događajima rizik i neobično aktivnosti za prijavu.


## <a name="controls"></a>Kontrole

Ovo su kontrole koje možete koristiti da biste nametnuli pravilo uvjetnog pristupa:

- **Višestruka provjera autentičnosti**. Možete zatražiti Jaka provjera autentičnosti putem višestruke provjere autentičnosti. Višestruka provjera autentičnosti možete koristiti s Azure višestruke provjere autentičnosti ili pomoću višestruka provjera autentičnosti davateljem lokalnog u kombinaciji s Active Directory Federation Services (AD FS). Višestruka provjera autentičnosti pomoću štiti resursa od pristupa korisnik koji ste stekli pristup vjerodajnice valjanog korisnika.

- **Blokiranje**. Možete primijeniti uvjeta kao što su korisničke lokacije da biste blokirali korisničkog pristupa. Ako, na primjer, možete blokirati pristup kada korisnik nije pouzdano mreži.

- **Uređaji**. Pravila uvjetnog access možete postaviti na razini uređaja. Možete postaviti pravilo tako da samo računala koja su domene pridruženo ili mobilnih uređaja koji su se prijavili aplikacija za upravljanje mobilnim uređajima, mogu pristupiti resursima za vaše tvrtke ili ustanove. Na primjer, koristite Intune za provjeru usklađenosti uređaja, a zatim ga o Azure AD za provođenje kada se korisnik pokuša pristupiti aplikacije sustava. Detaljne upute o tome kako koristiti Intune da biste zaštitili aplikacije i podataka potražite u članku [Zaštita aplikacije i podataka pomoću programa Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Također možete koristiti Intune da biste nametnuli zaštita podataka za uređaje izgubljene ili ukradeno. Dodatne informacije potražite u članku [Zaštita podataka pomoću cijelog ili selektivno brisanje koristeći Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Aplikacija

Možete nametnuti pravila uvjetnog pristup na razini aplikacije. Postavljanje razine pristupa za aplikacije i servise u oblaku ili na lokalnim poslužiteljima. Pravilo primjenjuje se izravno na web-mjesto ili servis. Pravila se primjenjivati za pristup web-pregledniku i aplikacije koje pristup servisu.


## <a name="device-based-conditional-access"></a>Pristup uvjetno utemeljen na uređaju

Aplikacije mogu ograničiti pristup putem uređaja koji je registriran u Azure AD i koje ispunjavaju određene uvjete. Pristup uvjetno utemeljen na uređaju štiti resursi za tvrtke ili ustanove s korisnicima koji se pokušaju pristupiti resursi iz:

- Nepoznatu ili neupravljani uređaji.
- Uređaje koji ne zadovoljava sigurnosne pravilnike vašoj tvrtki ili ustanovi postaviti.

Možete postaviti pravila na temelju ove uvjete:

- **Domene pridruženo uređaja**. Postavljanje pravila da biste ograničili pristup uređajima koji su pridruženi programa lokalne domene servisa Active Directory i koje i povezanih s programom Azure AD. To pravilo primjenjuje se na Windows stolna računala, kao i prijenosna računala i tableta enterprise.
Dodatne informacije o postavljanju automatskih registracije domene pridruženo uređaja s Azure AD potražite u članku [Postavljanje automatskog Registracija sustava Windows domene pridruženo uređaja s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

- **Uređaji**. Postavljanje pravila da biste ograničili pristup uređajima koji su označeni kao **sukladan** u imeniku sustava upravljanja. Ovo pravilo osigurava samo uređaje koji zadovoljavaju sigurnosna pravila kao što su provoditi šifriranje datoteke na uređaju dopušteno programa access. Ovo pravilo možete koristiti za ograničavanje pristupa sljedeće uređaje:

    - **Domene pridruženo uređaje sa sustavom Windows**. Upravlja tako da sustav centar Upravitelj konfiguracije (u trenutnom grani) implementiran u hibridnoj konfiguraciji.
    - **Rad u sustavu Windows 10 Mobile ili osobnih uređaja**. Upravlja Intune ili sustava za upravljanje trećih strana mobilne uređaje podržane.
    - **iOS i uređaje sa sustavom Android**. Upravlja Intune.


Korisnici koji pristupiti aplikacije koje nisu zaštićeni utemeljen na uređaju, ustanova za izdavanje certifikata pravila morate pristupiti aplikacija s uređaja koji zadovoljava preduvjete za ovo pravilo. Pristup je zabranjen ako pokušaj na uređaju koji ne zadovoljava preduvjete pravila.

Informacije o konfiguriranju na uređaj, ustanova za izdavanje certifikata pravila utemeljenih na Azure AD potražite u članku [Postavljanje pravilnika za pristup uvjetno utemeljen na uređaj za Azure Active Directory povezani aplikacije](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Resursi

Potražite sljedeće kategorije resursa i članci da biste saznali više o postavljanju uvjetno pristup za tvrtku ili ustanovu.


### <a name="multi-factor-authentication-and-location-policies"></a>Višestruke provjere autentičnosti i mjesto pravila

- [Uvod u uvjetno pristup Azure AD povezani aplikacijama ovisno o grupi, mjesto i pravila višestruka provjera autentičnosti](active-directory-conditional-access-azuread-connected-apps.md)
- [Aplikacijama koje su podržane](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Pristup uvjetno utemeljen na uređaju

- [Postavljanje pravilnika za pristup uvjetno utemeljen na uređaj za kontrolu pristupa Azure Active Directory povezani aplikacijama](active-directory-conditional-access-policy-connected-applications.md)
- [Postavljanje automatskog Registracija sustava Windows domene pridruženo uređajima pomoću servisa Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Otklanjanje poteškoća u vezi Azure Active Directory programa access](active-directory-conditional-access-device-remediation.md)
- [Zaštita podataka na uređajima izgubljene ili ukradeno pomoću Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Zaštita resurse ovisno o tome odgovornost za prijavu

-   [Zaštita identiteta za Azure AD](active-directory-identityprotection.md)

### <a name="next-steps"></a>Daljnji koraci

- [Uvjetno pristup najčešća pitanja](active-directory-conditional-faqs.md)
- [Tehnička referenca](active-directory-conditional-access-technical-reference.md)
