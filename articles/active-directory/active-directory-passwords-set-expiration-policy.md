<properties
    pageTitle="Postavljanje pravila isteka lozinke u Azure Active Directory | Microsoft Azure"
    description="Saznajte kako provjeriti pravilnika za istek i promjena Istek lozinke korisnika singly ili grupno za Azure Active directory lozinke"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Postavljanje pravila isteka lozinke u servisu Azure Active Directory

> [AZURE.IMPORTANT] **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).

Kao globalni administrator s Microsoftovim servisom u oblaku, možete koristiti u programu Microsoft Azure Active Directory Module za Windows PowerShell za postavljanje korisničke lozinke ne istječu. Cmdleta ljuske Windows PowerShell možete koristiti i da biste uklonili se nikad ne-ističe konfiguraciju ili da biste vidjeli koji je korisnik lozinke postavljeni ne istječe. Ovaj članak sadrži pomoć za servise u oblaku, kao što je Microsoft Intune i Office 365, koji su potrebni za identitet i imenik servisa Microsoft Azure Active Directory.

  > [AZURE.NOTE] Samo lozinke za korisničke račune koji se sinkroniziraju putem sinkronizacije direktorija možete nije konfiguriran za isteći. Dodatne informacije o sinkronizacije direktorija potražite u članku popis tema u [Vodič za sinkronizaciju imenika](https://msdn.microsoft.com/library/azure/hh967642.aspx).

Da biste koristili cmdleta ljuske Windows PowerShell, najprije morate instalirati ih.

## <a name="what-do-you-want-to-do"></a>Što želite učiniti?

- [Kako provjeriti pravilnika za istek za lozinke](#how-to-check-expiration-policy-for-a-password)

- [Postavljanje lozinke koja istječe](#set-a-password-to-expire)

- [Postavljanje lozinke tako da se ona istječe](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Kako provjeriti pravilnika za istek za lozinke

1.  Povežite se s Windows PowerShell pomoću vjerodajnica poduzeće administratora.

2.  Učinite nešto od sljedećeg:

    - Da biste vidjeli je li jednom korisniku lozinka postavljena tako da nikada ne istječe, pokrenite sljedeći cmdlet pomoću korisnikovo Glavno ime (UPN) (na primjer, aprilr@contoso.onmicrosoft.com) ili korisnički ID korisnika koji želite provjeriti:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Da biste vidjeli postavku "Lozinka nikad ne ističe" za sve korisnike, pokrenite sljedeći cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Postavljanje lozinke koja istječe

1.  Povežite se s Windows PowerShell pomoću vjerodajnica poduzeće administratora.

2.  Učinite nešto od sljedećeg:

    - Da biste postavili lozinku za jednog korisnika koja će isteći lozinku, pokrenite sljedeći cmdlet pomoću korisnikovo Glavno ime (UPN) ili korisnički ID korisnika:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Da biste postavili lozinke svi korisnici u tvrtki ili ustanovi da bi oni će isteći, koristite sljedeći cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Postavljanje lozinke koja nikada ne istječe

1. Povežite se s Windows PowerShell pomoću vjerodajnica poduzeće administratora.

2.  Učinite nešto od sljedećeg:

    - Da biste postavili lozinku za jednog korisnika koja nikada ne istječe, pokrenite sljedeći cmdlet pomoću korisnikovo Glavno ime (UPN) ili korisnički ID korisnika:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Da biste postavili lozinke svih korisnika u tvrtki ili ustanovi da nikada ne istječe, pokrenite sljedeći cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Daljnji koraci

* **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).
