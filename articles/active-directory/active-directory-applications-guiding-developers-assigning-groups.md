<properties
    pageTitle="Azure AD i aplikacije: Dodjela grupe u aplikaciju | Microsoft Azure"
    description="Kako implementirati e-pošta za Azure aplikacije."
    services="active-directory"
    documentationCenter=""
    authors="IHenkel"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/03/2015"
    ms.author="inhenk"/>

# <a name="azure-ad-and-applications-assigning-groups-to-an-application"></a>Azure AD i aplikacije: Dodjela grupe u aplikaciju
Prije no što možete dodijeliti korisnicima i grupama u aplikaciju, morate zahtijevati dodjele korisnika. Da biste saznali kako zahtijevaju Dodjela korisnika potražite u članku [Dodjela korisnika za koje je obavezna](active-directory-applications-guiding-developers-requiring-user-assignment.md) .

U ovom se članku pretpostavlja da ste već stvorili grupe u servisu active directory koji koristite za ovu aplikaciju.

## <a name="assigning-groups-to-an-application"></a>Dodjela grupe u aplikaciju
1. Prijavite se na portal Azure pomoću administratorskog računa.
2. Kliknite stavku **Svih stavki** u glavni izbornik.
3. Odaberite imenik koristite za aplikaciju.
4. Kliknite karticu **APLIKACIJE** .
5. Odaberite aplikaciju na popisu aplikacija koje su povezane s tom direktoriju.
6. Kliknite karticu **KORISNICI i GRUPE** .
7. Filtriranje popisa grupa u servisu active directory pomoću na padajućem popisu **grupe** .
8. Odaberite grupu.
9. Kliknite **DODIJELI**.
10. Kliknite **da** kada se to od vas zatraži.

## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
