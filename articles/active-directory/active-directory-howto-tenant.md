<properties
    pageTitle="Kako nabaviti klijent za Azure AD | Microsoft Azure"
    description="Kako nabaviti klijent za Azure Active Directory za registriranje i stvaranje aplikacija."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Kako nabaviti klijent za Azure Active Directory

U Azure Active Directory (Azure AD), [klijent](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) je predstavnika tvrtke ili ustanove.  To je Namjenska instanca servisa Azure AD tvrtkom ili ustanovom prima i vlasništvu kad je registrira za s Microsoftovim servisom u oblaku kao što su Azure, Microsoft Intune ili Office 365.  Svaki klijent Azure AD je istaknutije i neovisno o drugim klijenata za Azure AD.  

Klijent nalaze se korisnici u tvrtke i podatke o njima - njihove lozinki, podataka korisničkog profila, dozvolama i tako dalje.  Također sadrži grupe, aplikacije i ostale informacije vezani uz u tvrtki ili ustanovi i sigurnost.

Da biste Azure AD korisnicima da biste se prijavili aplikaciju, morate registrirati vaše aplikacije u klijentu vlastitu.  Objavljivanje aplikacija u klijentu za Azure AD je **apsolutno besplatno**.  Zapravo, većina razvojni inženjeri će stvoriti nekoliko klijenata i aplikacije za početak, razvoj pripremna i testiranja.  Tvrtke i ustanove koji prijavite za korištenje aplikacije po želji možete odabrati da kupite licence ako se želite iskoristiti direktorija napredne značajke.

Dakle, kako vam se o tome kako izvući klijent za Azure AD?  Postupak može biti mnogo različitih ako ste:

- [Imate postojeću pretplatu na Office 365](#use-an-existing-office-365-subscription)
- [Imate postojeću pretplatu Azure povezan s Microsoftovim Account](#use-an-msa-azure-subscription)
- [Imate postojeću pretplatu Azure pridružene račun tvrtke ili ustanove](#use-an-organizational-azure-subscription)
- [Ništa od navedenog imati i želite početi ispočetka](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Koristite postojeću pretplatu na Office 365
Ako imate postojeću pretplatu na Office 365, već imate klijent za Azure AD! Možete prijaviti na [portal za Azure](https://portal.azure.com) s računa za O365 i početak korištenja Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Koristite pretplatu na MSA Azure
Ako ste prethodno se prijavili za pretplatu na Azure pomoću Microsoftova Account pojedinačne, već imate klijentom!  Kad se prijaviti na [Portal za Azure](https://portal.azure.com), koji automatski zapisat će se u klijent sustava zadani. Ste besplatne za korištenje tog klijenta u odjeljku Prilagodi -, ali želite stvoriti račun tvrtke ili ustanove administratora.

Da biste to učinili, slijedite ove korake.  Osim toga, možda želite stvoriti novi klijent a administrator u klijentu slične koraka.

1.  Prijavite se na [Portal za Azure](https://portal.azure.com) s računom za pojedinačne
2.  Dođite do odjeljka "Azure Active Directory" portal (pronaći na lijevoj navigacijskoj traci u odjeljku **Više servisi**)
3.  Koje treba automatski biti prijavljeni u "Zadani imenik", ako ne možete se prebacivati direktorija klikom na naziv računa u gornjem desnom kutu.
4.  Iz odjeljka **Brzi zadataka** odaberite **Dodaj korisnika**.
5.  U obrascu za dodavanje korisnika navedite sljedeće detalje:

    - Naziv: (odaberite odgovarajuću vrijednost)
    - Korisničko ime: (Odaberite korisničko ime za ovu administratora)
    - Profil: (unijeli odgovarajuće vrijednosti za ime, posljednje ime, naziv radnog mjesta i odjela)
    - Uloga: Globalni Administrator

6.  Kada dovršite obrazac za dodavanje korisnika i primanje privremenu lozinku za novi korisnik administratora, obavezno zabilježite lozinku kao što je ćete morati prijaviti s novom korisniku da bi se promjena lozinke. Lozinke možete poslati i izravno u korisniku, zamjenski e-poštom.
7.  Kliknite na **Stvori** da biste stvorili novog korisnika.
8.  Da biste promijenili privremenu lozinku, prijavite [https://login.microsoftonline.com](https://login.microsoftonline.com) s novog korisničkog računa i promjena lozinke primitku.


## <a name="use-an-organizational-azure-subscription"></a>Korištenje Azure pretplate za tvrtke ili ustanove
Ako ste prethodno se prijavili za pretplatu na Azure pomoću računa tvrtke ili ustanove, već imate klijentom!  [Portal za Azure](https://portal.azure.com)biste trebali pronaći klijentom kada dođete do "Više servisa" i "Azure Active Directory."  Vi ste slobodni za korištenje tog klijenta svojim potrebama. 


## <a name="start-from-scratch"></a>Stvaranje ispočetka
Ako je sve od navedenog besmislenim vama, ne brinite.  Jednostavno posjetite [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) da biste se registrirali za Azure novi tvrtki ili ustanovi.  Nakon dovršetka postupka, imat ćete vlastite vrlo Azure AD klijenta s nazivom domene koju ste odabrali prilikom prijave prema gore.  [Portal za Azure](https://portal.azure.com)vaš klijent možete pronaći tako da odete na lijevoj strani navigaciju "Azure Active Directory"

U sklopu postupka prijave za Azure, vas će se tražiti da navede kreditne kartice.  Možete nastaviti pouzdano - vam neće biti naplaćen za objavljivanje aplikacije u Azure AD ili pri stvaranju nove drugih korisnika.
