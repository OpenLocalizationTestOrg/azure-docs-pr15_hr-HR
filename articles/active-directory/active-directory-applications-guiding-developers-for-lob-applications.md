<properties
    pageTitle="Azure AD i aplikacije: navođenjem razvojnim inženjerima | Microsoft Azure"
    description="Sastavljene za IT profesionalce, ovaj članak sadrži upute za Azure aplikacije Integracija sa servisom Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD i aplikacije: razvoj redak poslovnih aplikacija

Ovaj vodič sadrži pregled razvoj aplikacija redak tvrtke (LoB) za Azure Active Directory (AD). Ciljnu publiku je globalni administratori Active Directory/Office 365.

## <a name="overview"></a>Pregled

Stvaranje aplikacija integriran s Azure AD omogućuje korisnicima u vaše tvrtke ili ustanove jedinstvenu prijavu sa sustavom Office 365. Imate aplikaciju u Azure AD daje pravila provjere autentičnosti za aplikaciju. Dodatne informacije o uvjetno pristup i kako zaštititi aplikacije s višestruke provjere autentičnosti (MFA) potražite u članku [Konfiguriranje pravila za pristup](active-directory-conditional-access-azuread-connected-apps.md).

Registracija aplikacije da biste koristili Azure Active Directory. Registracija aplikacije znači da vaše razvojnim inženjerima možete koristiti Azure AD za provjeru autentičnosti korisnika i traženje pristupa resursima korisnika, kao što su e-pošte, kalendara i dokumenata.

Bilo koji član direktorija (ne Gosti) možete registrirati aplikaciju, u suprotnom naziva *Stvaranje objekt aplikacija*.

Registracija aplikacije omogućuje svakom korisniku da učinite sljedeće:

- Dobivanje identitet za svoje aplikacije koji prepoznaje Azure AD
- Dobiti jednu ili više tajne/tipke koje aplikaciju možete koristiti radi provjere autentičnosti AD
- Dodavanje aplikacije na portalu za Azure pomoću prilagođenog naziva, logotipa, itd.
- Primjena značajki autorizacije Azure AD na aplikaciju, uključujući:
  - Kontrola pristupa na temelju uloga (RBAC)
  - Azure Active Directory kao oAuth provjeru autentičnosti poslužitelja (secure API-JA koji prikazuje aplikacija)

- Deklariranje potrebne dozvole potrebne za aplikaciju funkciji kako treba, uključujući:-dozvole za aplikaciju (samo globalni Administratori). Na primjer: članstvo u ulozi u drugom Azure AD aplikacije ili uloga članstvo u odnosu Azure resursa, grupa resursa, ili pretplate - delegirana dozvole (bilo kojeg korisnika). Na primjer: Azure AD prijave, i čitanje profila


> [AZURE.NOTE]Prema zadanim postavkama, bilo koji član možete registrirati aplikaciju. Da biste saznali kako ograničiti dozvole za registriranje aplikacije određenim članovima, potražite u članku [kako se dodaju aplikacije za Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Evo što vam globalni administrator je potrebno učiniti da biste razvojnim inženjerima svoje aplikacije provjerite jeste li spremni za proizvodnju:

- Konfiguriranje pravila programa access (pristup pravila/MFA)
- Konfiguriranje aplikacija zahtijevaju Dodjela korisnika i dodijeliti korisnicima
- Izostavlja zadani pristanak doživljaja rada

## <a name="configure-access-rules"></a>Konfiguriranje pravila za pristup

Konfiguriranje pravila-aplikacijama programa access za SaaS aplikacija. Ako, na primjer, zahtijevaju MFA ili samo dopustiti pristup korisnicima pouzdano mrežama. Detalje za tu su dostupne u dokumentu [Konfiguriranje pravila za pristup](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Konfiguriranje aplikacija zahtijevaju Dodjela korisnika i dodijeliti korisnicima

Prema zadanim postavkama, korisnici mogu pristupati aplikacije bez dodijeljene. Međutim, ako aplikacija izlaže uloge ili ako želite da se aplikacija na ploči pristup korisničke, trebali biste zahtijevaju Dodjela korisnika.

[Zahtijevanje Dodjela korisnika](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Ako ste pretplatnik programa Azure AD Premium ili Enterprise mobilnost paket (EMS), preporučujemo korištenje grupa. Dodjeljivanje grupe aplikacija omogućuje delegiranje stalnog pristupa upravljanja vlasnik grupe. Možete stvoriti grupu ili zamolite odgovorni zabavu u tvrtki ili ustanovi da biste stvorili grupu pomoću svoje funkcijom upravljanje grupe.

[Dodjela korisnika u aplikaciju](active-directory-applications-guiding-developers-assigning-users.md)  
[Dodjela grupe u aplikaciju](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Izostavlja dozvole korisnika

Prema zadanim postavkama svaki korisnik prolazi kroz pristanak sučelje za prijavu. Sučelje za pristanak, s pitanjem korisnicima Dodjela dozvola za aplikaciju, može biti disconcerting za korisnike koji nisu poznati takvih odluka.

Za aplikacije koji smatrate pouzdanim, može pojednostavniti korisnički doživljaj po pristali na to da aplikacije ime tvrtke ili ustanove.

Dodatne informacije o dozvole korisnika i sučelje za pristanak u Azure potražite u članku [Integraciji aplikacije pomoću servisa Azure Active Directory](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Povezani članci

- [Omogućivanje daljinskog pristupa sigurne lokalnog aplikacijama s Proxy aplikacije za Azure AD](active-directory-application-proxy-get-started.md)
- [Azure uvjetno pretpregled programa Access za SaaS aplikacije](active-directory-conditional-access-azuread-connected-apps.md)
- [Upravljanje pristupom aplikacije s Azure AD](active-directory-managing-access-to-apps.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
