<properties
    pageTitle="Upravljanje prilagođenih naziva domena u pretpregledu Azure Active Directory | Microsoft Azure"
    description="Upravljanje koncepata i how-tos za upravljanje naziv domene u servisu Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>Upravljanje prilagođenih naziva domena u pretpregledu Azure Active Directory

Naziv domene je važan dio identifikator za mnoge direktorija resurse: je dio korisničko ime ili e-pošte adrese za korisnika, dio adrese za grupu, i može biti dio aplikacije ID URI za aplikaciju. Resursa u pretpregledu Azure Active Directory (Azure AD) možete uključiti naziv domene koji je već potvrđena da biste se posjeduje direktorij koji sadrži resurs. [Novosti u pretpregledu?](active-directory-preview-explainer.md) Samo globalni administrator mogu obavljati zadatke upravljanja domene u Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Postavite naziv primarne domene za Azure AD direktorija

Prilikom stvaranja direktorija početni naziv domene, primjerice "contoso.onmicrosoft.com," je naziv primarne domene. Primarna domena je zadani naziv domene za nove korisnike, prilikom stvaranja novog korisnika. To pojednostavljuje postupak administrator da biste stvorili novi korisnici na portalu. Da biste promijenili naziv primarne domene:

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **više usluga**, unesite **Azure Active Directory** u tekstni okvir, a zatim pritisnite **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na plohu ***Naziv direktorija*** odaberite **naziva domena**.

4. Na plohu ** *Naziv direktorija* - nazivi domena** odaberite naziv domene koju želite provjerite naziv primarne domene.

5.  Na plohu ***naziv domene*** (to jest, na plohu koji će se otvoriti koja ima novi naziv domene u naslovu), odaberite naredbu **provjerite primarni** . Provjerite je li vam na temelju odabira kada se to od vas zatraži.

    ![Provjerite naziv domene primarni](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Možete promijeniti naziv primarne domene za direktorija se sve provjerene prilagođene domene nije naveden kao pridruženi. Promjena primarnu domenu za direktorija neće se promijeniti imena korisnika za sve postojeće korisnike.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Dodavanje prilagođenih naziva domena Azure AD

Svaki Azure AD direktorija možete dodati do 900 prilagođenih naziva domena. Postupak da biste [dodali na dodatne prilagođeni naziv domene](active-directory-domains-add-azure-portal.md) je isti za prvi naziv prilagođene domene.

## <a name="add-subdomains-of-a-custom-domain"></a>Dodavanje poddomene prilagođene domene

Ako želite da biste dodali naziv domene treće razine kao što su "europe.contoso.com" direktorija, morate dodati i provjerite je li domena druge razine, kao što je contoso.com. Poddomena provjerit će se automatski po Azure AD. Da biste vidjeli koji nisu provjereni poddomena koju ste upravo dodali, osvježite stranicu u pregledniku koja sadrži popis domena.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Što učiniti ako promijenite registrara DNS-a za prilagođeni naziv domene

Ako promijenite registrara DNS-a za prilagođeni naziv domene, možete nastaviti koristiti prilagođeni naziv domene s Azure AD sam bez prekida i bez dodatne konfiguracijske zadatke. Ako koristite prilagođeni naziv domene u sustavu Office 365, Intune ili druge servise koje ovise o prilagođenih naziva domena u Azure AD potražite u dokumentaciji za te servise.

## <a name="delete-a-custom-domain-name"></a>Brisanje prilagođenog naziva domene

Ako tvrtka ili ustanova ne koristi taj naziv domene ili ako je potrebno koristiti taj naziv domene s drugom Azure AD možete izbrisati prilagođenog naziva domene s Azure AD.

Da biste izbrisali prilagođeni naziv domene, morate provjeriti resursa u direktoriju oslanjate naziv domene. Naziv domene nije moguće izbrisati iz imenika ako:

-   Bilo koji korisnik ima korisničko ime, adresu e-pošte ili proxy adresu koja sadrži naziv domene.

-   Bilo koji ima adresu e-pošte ili proxy adresu koja sadrži naziv domene.

-   Bilo koju aplikaciju u Azure AD sadrži aplikaciju ID URI koji sadrži naziv domene.

Morate promijeniti ili izbrisati takve resursa u direktoriju Azure AD prije brisanja naziv prilagođene domene.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Korištenje ljuske PowerShell ili grafikonu API-JA za upravljanje nazivima domena

Većinu zadataka upravljanja za nazive domena u Azure Active Directory možete izvršiti i koristite Microsoft PowerShell ili programski pomoću Azure AD grafikonu API-JA (u javnom pretpregled).

-   [Upravljanje nazivima domena u Azure AD pomoću komponente PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Upravljanje nazivima domena u Azure AD pomoću API grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Daljnji koraci

-   [Dodavanje prilagođenih naziva domena](active-directory-domains-add-azure-portal.md)
