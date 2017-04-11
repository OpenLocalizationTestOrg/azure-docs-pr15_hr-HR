<properties
    pageTitle="Upravljanje dozvolama resursima po korisniku u stogu Azure (administrator servisa i klijenta) | Microsoft Azure"
    description="Kao administrator servisa ili klijent, Saznajte kako upravljati dozvolama resursima po korisniku."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Upravljanje korisničkim dozvolama

Korisnik u stogu Azure može biti čitač, vlasnik ili suradnika za svaku instancu pretplatu, grupa resursa ili usluge. Na primjer, korisnika možda imate dozvole za čitanje pretplate 1, ali imate dozvole vlasnika 7 virtualnog računala.

-   Čitač: Korisnik možete pogledati sve, ali ne možete mijenjati.

-   Suradnik: Korisnik možete upravljati sve osim pristup resursima.

-   Vlasnik: Korisnik možete upravljati sve, uključujući pristup resursima.


## <a name="set-access-permissions-for-a-user"></a>Postavljanje dozvola pristupa za korisnika

1.  Prijavite se pomoću računa koji ima dozvole vlasnika resursu želite upravljati.

2.  U plohu resursa, kliknite ikonu **programa Access** ![](media/azure-stack-manage-permissions/image1.png).

3.  U plohu **korisnika** kliknite **uloge**.

4.  U plohu **uloge** kliknite **Dodaj** da biste dodali dozvole za korisnika.

## <a name="next-steps"></a>Daljnji koraci

[Dodavanje klijent za Azure stogu](azure-stack-add-new-user-aad.md)
