<properties
    pageTitle="Dodavanje novog računa klijenta za Azure stogu servisa Azure Active Directory | Microsoft Azure"
    description="Nakon što implementirate Microsoft Azure stogu PNA, morat ćete stvoriti barem jednog klijenta korisnički račun tako da možete istražiti portal za klijenta."
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

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Dodavanje novog računa klijenta za Azure stogu servisa Azure Active Directory

Nakon [Implementacija PNA snop Azure](azure-stack-run-powershell-script.md), morat ćete klijentu korisnički račun da biste mogli Upoznavanje portala za klijenta i testirati ponude i tarife. Klijentskog računa možete stvoriti pomoću [portala za Azure](#create-an-azure-stack-tenant-account-using-the-azure-portal) ili [pomoću komponente PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Stvorite račun klijentu stogu Azure pomoću portala za Azure

Morate imati Azure pretplatu za korištenje portala za Azure.

1. Prijavite se u sustav [Azure](http://manage.windowsazure.com).

2.  Microsoft Azure lijevoj navigacijskoj traci kliknite **Servisa Active Directory**.

3.  Na popisu direktorija kliknite direktorija koji želite koristiti za Azure stogu ili stvorite novi.

4.  Na ovoj stranici direktorija kliknite **korisnicima**.

5.  Kliknite **Dodaj korisnika**.

6.  U čarobnjaku za **Dodavanje korisnika** , na popisu **Vrsta korisnika** odaberite **novog korisnika u tvrtki ili ustanovi**.

7.  U okvir **korisničko ime** upišite naziv za korisnika.

8.  U na **@** okvir, odaberite odgovarajuću stavku.

9.  Kliknite strelicu za sljedeće.

10.  U **korisničkom profilu** stranici čarobnjaka upišite **ime**, **Prezime**i **zaslonsko ime**.

11. Na popisu **uloga** odaberite **korisnika**.

12. Kliknite strelicu za sljedeće.

13. Na stranici **Početak privremenu lozinku** , kliknite **Stvori**.

14. Kopirajte **novu lozinku**.

15. Prijavite se Microsoft Azure pomoću novog računa. Promijenite lozinku kad se to od vas zatraži.

16. Prijavite se na `https://portal.azurestack.local` pomoću novog računa da biste vidjeli portal za klijenta.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Stvorite račun klijentu stogu Azure pomoću komponente PowerShell

Ako nemate pretplatu na Azure, nećete moći koristiti portal za Azure da biste dodali korisničkom računu klijenta. U ovom slučaju na Azure Active Directory Module za Windows PowerShell možete koristiti umjesto toga.

> [AZURE.NOTE] Ako koristite Microsoft Account (Live ID) za implementaciju Azure stogu PNA AAD PowerShell ne možete koristiti za stvaranje klijentskog računa. 

1.  Instalirajte [Microsoft Online Services Pomoćnik za prijavu za IT profesionalce RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Instalacija [Azure Active Directory Module za Windows PowerShell (64-bitna verzija)](http://go.microsoft.com/fwlink/p/?linkid=236297) , a zatim ga otvorite.

3.  Pokrenite sljedeće Cmdlete:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Prijavite se na Microsoft Azure pomoću novog računa. Promijenite lozinku kad se to od vas zatraži.

5.  Prijavite se u `https://portal.azurestack.local` pomoću novog računa da biste vidjeli portal za klijenta.



