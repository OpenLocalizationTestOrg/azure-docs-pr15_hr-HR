<properties
    pageTitle="Prijavite se ins nakon većeg broja neuspješnih pokušaja"
    description="Izvješće koje navodi korisnika koji su prijavljeni uspješno nakon uzastopnih više nije uspjela znak u pokušaji."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>Znak dodataka nakon većeg broja neuspješnih pokušaja
Izvješće označava korisnike koji su prijavljeni uspješno nakon uzastopnih više nije uspjela znak u pokušaja. Mogući uzroci uključuju:

- Korisnik imao zaboravili lozinku</li><li>Korisnik se nalazi žrtva uspješno lozinke guessing brute prisilno napada

Rezultati iz izvješća prikazat će broj uzastopnih nije uspjelo prijavu pokušao isporučiti prije prijave uspjelo i vremenske oznake povezan s prve uspješno prijavu.

**Postavke izvješća**: možete konfigurirati najmanji broj uzastopnih nije uspjelo znak u pokušaja mora biti prije nego što je moguće prikazati u izvješću. Kada unesete promjene ove postavke je imati na umu da te promjene će se primijeniti na sve postojeći potpis nije uspjelo dodaci koji trenutno prikazuju se u postojećem izvješću. Međutim, oni će se primijeniti za sve buduće znak dodataka. Promjene na ovom izvješću se može unijeti samo licencirani administratori.


![Prijavite se ins nakon većeg broja neuspješnih pokušaja](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)
