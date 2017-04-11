<properties
    pageTitle="Azure Active Directory B2C: Atributi Prilagođeno | Microsoft Azure"
    description="Kako koristiti prilagođene atribute u Azure Active Directory B2C za prikupljanje informacija o vašem koje korisnici"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Korištenje prilagođene atribute za prikupljanje informacija o vašem koje korisnici

Azure Active Directory (Azure AD) B2C direktorija u sklopu ugrađeni skup podataka (atribute): ime, prezime, Grad, poštanski broj i druge atribute. Međutim, svaka aplikacija za korisničke dostupnog ima specifičnim potrebama na što atributi prikupljanje iz koje korisnici. S Azure AD B2C, možete proširiti skup atribute pohranjene na svakom potrošačkog računa. Možete stvoriti prilagođene atribute [Azure portal](https://portal.azure.com/) i koristiti u registracije pravila, kao što je prikazano u nastavku. Čitanje i pisanje te atributa pomoću [API Azure AD grafikonu](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Prilagođene atribute pomoću [proširenja shema za Azure AD grafikonu API direktorija](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## <a name="create-a-custom-attribute"></a>Stvaranje prilagođenog atributa

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **korisničke atribute**.
3. Kliknite **Dodaj +** pri vrhu na plohu.
4. Navedite **naziv** za prilagođeni atribut (na primjer, "ShoeSize") i po želji **Opis**. Kliknite **Stvori**.

    > [AZURE.NOTE]
    Samo "Niz" **Vrsta podataka** trenutno nije dostupno.

Prilagođeni atribut sada je dostupan na popisu atributa **korisnika**i za korištenje u pravilnicima za prijavu.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Korištenje prilagođenih atributa u Pravilnik za prijavu

1. [Slijedite ove korake da biste došli do plohu značajke B2C portala za Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknite **pravila za prijavu**.
3. Kliknite prijavi pravilnika (na primjer, "B2C_1_SiUp") da biste ga otvorili. Kliknite **Uređivanje** pri vrhu na plohu.
4. Kliknite **Prijavi atributi** i odaberite prilagođeni atribut (na primjer, "ShoeSize"). Kliknite **u redu**.
5. Kliknite **zahtjevima aplikacije** i odaberite prilagođeni atribut. Kliknite **u redu**.
6. Kliknite **Spremi** pri vrhu na plohu.

Koristite značajku "Izvedi sada" na pravila da biste provjerili korisnička sučelja. Trebali biste sada potražite u članku "ShoeSize" na popisu atributa koji se prikupljaju tijekom korisničke prijave i vidjeti u token šalje natrag u aplikaciji.

## <a name="notes"></a>Bilješke

- Uz registracije pravila prilagođene atribute može se koristiti i u registracije ili prijave pravila i profila uređivanje pravilnika.
- Postoji poznato ograničenje prilagođene atribute. Samo se stvara se koristi u bilo koje pravilo, a ne kada ga dodate na popis **korisničke atribute**prvi put.
