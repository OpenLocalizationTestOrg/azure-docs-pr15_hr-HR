<properties
    pageTitle="Aplikacije koje koriste pravila za uvjetno pristup u servisu Azure Active Directory | Microsoft Azure"
    description="S kontrolom uvjetnog pristup Azure Active Directory provjerava za određene uvjete kada je potvrđuje korisnika i dopustili pristup aplikacije."
    services="active-directory"
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
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Aplikacije koje koriste pravila za uvjetno pristup u servisu Azure Active Directory

Pravila za uvjetno pristup podržava Azure Active Directory (Azure AD) – povezani aplikacije, unaprijed integriran pridruženim softver kao aplikacijama servisa (SaaS) i aplikacije koje koriste lozinku jedinstvenu prijavu (SSO), redak poslovnih aplikacija i aplikacije koje koriste Proxy aplikacije za Azure AD. Detaljni popis aplikacija za koje možete koristiti uvjetno access potražite u članku [Servisi omogućeni pomoću uvjetnog programa access](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Uvjetno programa access radi oba s prijenosnih i stolnih aplikacija koje koriste modernim provjeru autentičnosti. U ovom se članku ćemo objasniti kako uvjetno radi pristupa u aplikacijama za mobilne uređaje i stolna računala.

Stranica za prijavu Azure AD možete koristiti u aplikacijama koje koriste modernim provjeru autentičnosti. Sa stranice prijavu, korisnik je tražiti višestruke provjere autentičnosti. Poruka se prikazuje korisnički pristup blokiran. Moderna provjere autentičnosti za uređaj za provjeru s Azure AD potreban je tako da se vrednuju se pravila za pristup uvjetno utemeljen na uređaj.

Nije važno je znati koje aplikacije mogu koristiti pravila za uvjetno pristup i koraci koje morate poduzeti da biste druge točke unosa aplikacije za sigurnu.

## <a name="applications-that-use-modern-authentication"></a>Aplikacije koje koriste Moderna provjere autentičnosti

> [AZURE.NOTE] Ako imate pristup uvjetnog pravila u Azure AD koja ima u jednaku vrijednost u sustavu Office 365, konfiguriranje i pravila uvjetnog programa access. To odnosi, na primjer, pravila uvjetnog pristup za Exchange Online ili SharePoint Online.

Uvjetno access podržavaju sljedeće aplikacije za Office 365 i drugim aplikacijama za Azure AD povezani servisa:

| Servis za cilj  | Platforme  | Aplikacija                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365 Exchange Online | Windows 10|Aplikacija kalendar/pošte/osobe, Outlook 2016, Outlook 2013 (s Moderna provjera autentičnosti), a zatim Skype za tvrtke (s Moderna provjera autentičnosti)|
|Office 365 Exchange Online| Windows 8.1 i Windows 7 |Outlook 2016, Outlook 2013 (s modernim provjera autentičnosti), Skype za tvrtke (s modernim provjera autentičnosti)|
|Office 365 Exchange Online|iOS, Android|  Mobilne aplikacije za Outlook|
|Office 365 Exchange Online|Mac OS X| Outlook 2016 za višestruke provjere autentičnosti i mjesto samo; Podrška za pravila utemeljenih na uređaju planirano u budućnosti Skype za poslovnu podršku planirano u budućnosti|
|Office 365 SharePoint Online|Windows 10| Aplikacije sustava Office 2016, univerzalni aplikacije sustava Office, Office 2013 (s modernim provjera autentičnosti), OneDrive za podršku tvrtke aplikacije (sljedeći klijent za sinkronizaciju generiranje ili NGSC) planirano u budućnosti grupama sustava Office podršku planirano u budućnosti podrška za aplikacije SharePoint planirano u budućnosti|
|Office 365 SharePoint Online|Windows 8.1 i Windows 7|Aplikacije sustava Office 2016, Office 2013 (s modernim provjera autentičnosti), aplikacije OneDrive za tvrtke (programa Groove klijent za sinkronizaciju)|
|Office 365 SharePoint Online|iOS, Android|  Mobilne aplikacije sustava Office |
|Office 365 SharePoint Online|Mac OS X| Aplikacije sustava Office 2016 za višestruke provjere autentičnosti i mjesto samo; Podrška za pravila utemeljenih na uređaju planirano u budućnosti|
|Yammer za Office 365|Windows 10, iOS i Android | Aplikacija Yammer za Office|
|Dynamics CRM|Windows 10, Windows 8.1, Windows 7, iOS i Android | Aplikacija za Dynamics CRM|
|Servisa PowerBI|Windows 10, Windows 8.1, Windows 7, iOS i Android | PowerBI aplikacija|
|Udaljena aplikacije servisa za Azure|Windows 10, Windows 8.1, Windows 7, iOS, Android i Mac OS X |Aplikacija za Azure Remote|
|Sve aplikacije servisa za Moje aplikacije|Android i iOS|Sve aplikacije servisa za Moje aplikacije |


## <a name="applications-that-do-not-use-modern-authentication"></a>Aplikacije koje koriste Moderna provjere autentičnosti

Trenutno, morate koristiti druge metode da biste blokirali pristup aplikacijama koje koriste Moderna provjeru autentičnosti sustava. Pravila za pristup aplikacije za koje ne koristite Moderna provjeru autentičnosti nije postavio uvjetno programa access. To se prvenstveno je važna za pristup sustavu Exchange i SharePoint. Većini starijih verzija aplikacije koristiti starije protokole za kontrolu pristupa.

### <a name="control-access-in-office-365-sharepoint-online"></a>Upravljanje pristupom u sustavu Office 365 SharePoint Online
Stari protokola za pristup sustavu SharePoint možete onemogućiti pomoću cmdleta skup SPOTenant. Pomoću tog cmdleta da biste spriječili klijenti sustava Office koje koriste provjeru autentičnosti – Moderni protokole pristupa resursima sustava SharePoint Online.

**Primjer naredbe**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Upravljanje pristupom u sustavu Office 365 Exchange Online

Exchange nudi dvije glavne kategorije protokola. Pregledajte sljedeće mogućnosti, a zatim odaberite pravila koja najbolje odgovara vašoj tvrtki ili ustanovi.

-   **Exchange ActiveSync**. Prema zadanim postavkama pravilnike za pristup uvjetnog višestruka provjera autentičnosti i mjesta ne provode Exchange ActiveSync. Morate štititi pristup tih servisa konfiguriranjem pravila za Exchange ActiveSync izravno ili tako da blokira Exchange ActiveSync pomoću pravila Active Directory Federation Services (AD FS).
-   **Naslijeđeno protokole**. Možete blokirati naslijeđene protokola AD fs. U ovom blokira pristup starije klijenti sustava Office, kao što je Office 2013 bez Moderna omogućena provjera autentičnosti i starije verzije sustava Office.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Da biste blokirali naslijeđene protokol koristite AD FS

Sljedeći primjer pravila možete koristiti da biste blokirali pristup naslijeđene protokol na razini AD FS. Odaberite dva uobičajena konfiguracije.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Mogućnost 1: Dopusti Exchange ActiveSync i Dopusti naslijeđene aplikacije, ali samo na intranetu

Primjenom sljedeća tri pravila za AD FS potrebe za oslanjanjem strana pouzdanost za Microsoft Office 365 identiteta platformu promet za Exchange ActiveSync i preglednik i promet Moderna provjere autentičnosti imaju pristup. Stari aplikacije blokirana je u ekstranet.

##### <a name="rule-1"></a>Pravilo 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Pravilo 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Pravilo 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Mogućnost 2: Dopusti Exchange ActiveSync i blokiraj naslijeđene aplikacije

Primjenom sljedeća tri pravila za AD FS potrebe za oslanjanjem strana pouzdanost za Microsoft Office 365 identiteta platformu promet za Exchange ActiveSync i preglednik i promet Moderna provjere autentičnosti imaju pristup. Stari aplikacije su blokirane s bilo kojeg mjesta.

##### <a name="rule-1"></a>Pravilo 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Pravilo 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Pravilo 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
