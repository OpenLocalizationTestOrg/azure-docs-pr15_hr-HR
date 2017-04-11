<properties
    pageTitle="Azure AD Connect u Microsoft Cloud Njemačka"
    description="Azure AD Connect će vaše lokalne direktorija integrirati s Azure Active Directory. Time koje možete unijeti uobičajenih identiteta za Office 365, Azure i SaaS integriran s Azure AD aplikacije."
    keywords="Uvod za Azure AD Connect Azure AD Connect pregled, što je Azure AD Connect instaliranje servisa active directory, Njemačka, skup stabala crno"
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
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect u Microsoft Cloud Njemačka – javno pretpregled

## <a name="introduction"></a>Uvod
Azure AD Connect nudi sinkronizaciju između lokalnog imeničkog servisa Active Directory i Azure Active Directory.
Trenutno mnoge scenariji u [Microsoft Cloud Njemačka](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) mora poduzeti operator. Kada koristite Microsoft Cloud Njemačka, koje morate imati na umu sljedeće:


- Sljedeći URL-ovi moraju biti otvoreni na proxy poslužitelju za uspješno sinkroniziranja:
    - *. microsoftonline.de
    - *. windows.net
    - + Popisi opozvanih certifikata

- Kada se prijavite direktoriju Azure AD, morate koristiti račun na domeni onmicrosoft.de.
- Dostupne su sljedeće značajke:
    - Azure AD Connect stanja
    - Automatska ažuriranja
    - Lozinka upisima

## <a name="download"></a>Preuzimanje
Azure AD Connect možete preuzeti iz plohu Azure AD Connect unutar portala.  Koristite upute u nastavku da biste pronašli plohu Azure AD Connect.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect plohu

Kada ste prijavljeni na portal sustava Azure, učinite sljedeće:

1. Idite na pregled
2.  Odaberite Azure Active Directory
3.  Odaberite Azure AD Connect

Trebali biste vidjeti sljedeće:

![Azure AD Connect plohu](media\active-directory-aadconnect-germany\germany1.png)

 
U sljedećoj su tablici opisane su značajke prikazano na plohu.


Naslov|Opis|
----- | ----- |
STATUS SINKRONIZACIJE|Pogledajmo znate li sinkronizacije omogućiti ili onemogućiti.|
ZADNJE SINKRONIZACIJE|Zadnji put sinkronizaciju uspješno dovršena.|
PRIDRUŽENIM DOMENAMA|Prikazuje broj pridruženim domenama trenutno postavljenima.|


## <a name="installation"></a>Instalacija
Da biste instalirali Azure AD Connect, možete koristiti u dokumentaciji [ovdje](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Dodatne značajke i dodatne informacije
Dodatne informacije i smjernice o prilagođenim postavkama ili naprednih konfiguracija, započnite [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).  Ova stranica sadrži informacije i veze na dodatne informacije.
