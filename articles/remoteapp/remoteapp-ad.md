
<properties 
    pageTitle="Azure AD + preduvjeti za Active Directory za Azure RemoteApp | Microsoft Azure" 
    description="Saznajte kako postaviti servisa Active Directory za rad s Azure RemoteApp." 
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



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + preduvjeti za Active Directory za Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.


Zbirka hibridnog Azure RemoteApp ili oblaka zbirke koju želite združivanje pomoću AD Connect potrebno je učinite sljedeće.

### <a name="connect-azure-ad-and-active-directory"></a>Povezivanje Azure AD i servisa Active Directory

Ako se želite povezati, a vaš klijent Azure AD lokalnim okruženjima za Active Directory, koristite AD Connect. Trebat će vam samo [4 klikova](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) za povezivanje dva direktorija.

Imajte na umu - sinkronizacije direktorija nužan za hibridno zbirke.

### <a name="make-sure-your-domaincom-match"></a>Provjerite je li vaša "@domain.com" podudaraju
Prije nego što počnete, provjerite odgovara li UPN-a za vaše lokalne skup stabala sufiks domene Azure AD li. 

Nakon postavljanja domene UPN nastavka u Azure AD, svi korisnici zapisivanje u Azure RemoteApp će prijavite se kao “user@ <the suffix you set up>". Provjerite mogu li korisnici i prijaviti pomoću isti user@suffix u lokalne domene. U određenim slučajevima možete postaviti naziv jednu domenu u Azure AD prilikom određivanja nastavka drugu domenu za na korisnika na-prem. U ovom slučaju, korisnici neće biti moguće povezati s bilo koje domene pridruženo računala ili resursa pomoću servisa Azure RemoteApp.

Na primjer, ako ste postavili UPN nastavka domene u AAD kao contoso.com, ali nekim korisnicima na lokalni/AD konfigurirani tako da biste se prijavili u @contoso.uk, , a zatim ti korisnici će moći pravilno prijaviti ARA zbirke. Korisnici UPN u AAD i AD moraju biti iste za prijavu u moguće"

### <a name="create-objects-for-azure-remoteapp"></a>Stvaranje objekata za Azure RemoteApp
Morate stvoriti na sljedeće lokalnog objektima servisa Active Directory:

- Račun servisa za pristup domeni resursi za programe RemoteApp spajanjem RDSH krajnje točke za lokalne domene.
- Na tvrtke ili ustanove jedinice (OU) koja će sadržavati objekte strojno RemoteApp. Korištenje organizacijsku jedinicu preporučuje (se, ali nije obavezno) izdvojiti računi i pravila koje ćete koristiti s RemoteApp.

Morate od tih objekata kada stvarate vaše zbirke RemoteApp stoga svakako najprije učinite sljedeće korake.