<properties
    pageTitle="Azure AD Connect: Sinkronizacija instanci servisa | Microsoft Azure"
    description="Ova stranica dokumenti posebne napomene za Azure AD instance."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Posebne napomene za instanci
Azure AD Connect najčešće koriste s world wide pojavljivanja Azure AD i Office 365. No postoje i druge instance i te različito tretiraju URL-ovi i druge posebne napomene.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Njemačka
[Njemačka Microsoft Cloud](http://www.microsoft.de/cloud-deutschland) je sovereign oblak putem njemački podataka povjerenički.

U ovom oblaka trenutno u pretpregledu. Mnoge scenariji obično možete učiniti sami, kao što su potvrda domene, morate poduzeti operator. Dodatne informacije o tome da biste sudjelovali u pretpregledu zatražite od lokalnog Microsoftova predstavnika.

URL adresa da biste otvorili u proxy poslužitelj |
--- |
\*. microsoftonline.de |
\*. windows.net |
+ Popisi opozvanih certifikata |

Kada se prijavite direktoriju Azure AD morate koristiti račun na domeni onmicrosoft.de.

Trenutno ne postoji u Microsoft Cloud Njemačka značajke:

- Povežite zdravlje Azure AD nije dostupna.
- Automatska ažuriranja nije dostupna.
- Lozinka upisima nije dostupna.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure državne oblaka
[Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) je oblaka za vlada SAD-a.

U ovom oblaka sadrži podržava prethodnih izdanja DirSync. Iz međuverzije 1.1.180 Azure AD Connect uz podržano je generacije oblak. U ovom generacije koristi samo za SAD-a na temelju točke i imaju različite popis URL adresa da biste otvorili u proxy poslužitelj.

URL adresa da biste otvorili u proxy poslužitelj |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Popisi opozvanih certifikata |

Azure AD Connect nećete moći automatski prepoznati direktorija Azure AD nalazi u oblaku državne. Umjesto toga morate poduzeti sljedeće kada instalirate Azure AD Connect.

1. Pokrenite instalaciju Azure AD Connect.
2. Čim se prikaže na prvu stranicu na mjesto koje morate prihvatiti EULA, nastavite, ali ostavite pokrenuti čarobnjak za instalaciju.
3. Pokrenite regedit i promjena ključa registra `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` vrijednosti `2`.
4. Prijeđite natrag u čarobnjaku za instalaciju Azure AD Connect Prihvaćam EULA i nastaviti. Tijekom instalacije, provjerite jeste li koristiti **prilagođene konfiguracije** put instalacije (i ne izražavanje instalacije). Nastavite s instalacijom kao i obično.

Trenutno ne postoji u oblaku Microsoft Azure državne značajke:

- Povežite zdravlje Azure AD nije dostupna.
- Automatska ažuriranja nije dostupna.
- Lozinka upisima nije dostupna.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
