<properties
    pageTitle="Promjena potpisa raspršivanje algoritam za Office 365 odgovaranje strana pouzdanost | Microsoft Azure"
    description="Ova stranica sadrži upute za promjenu SHA algoritam za pouzdanost vanjski pristup sustavu Office 365"
    keywords="Sha1, SHA256, O365, vanjski pristup, aadconnect, adfs, ad fs, sha promjena pouzdanost vanjski pristup potrebe za oslanjanjem pouzdanost proizvođača"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Promjena potpisa raspršivanje algoritam za Office 365 odgovaranja pouzdanost za zabavu

## <a name="overview"></a>Pregled

Azure Active Directory Federation Services (AD FS) potpisuje njegov tokeni Microsoft Azure Active Directory da biste bili sigurni da se ne može biti neovlašteno. Potpis mogu se temeljiti na SHA1 ili SHA256. Azure Active Directory sada podržava tokeni prijavljeni pomoću algoritam SHA256 pa preporučujemo da postavke algoritam za potpisivanje tokena za SHA256 najvišu razinu sigurnosti. U ovom se članku opisuje korake potrebne da biste postavili algoritam za potpisivanje tokena na sigurnija SHA256 razine.

## <a name="change-the-token-signing-algorithm"></a>Promjena algoritam za potpisivanje tokena

Nakon postavljanja algoritam potpisa s jednim od dva procesa ispod AD FS potpisuje tokena za Office 365 potrebe za oslanjanjem pouzdanosti strana SHA256. Ne morate promjene dodatna konfiguracija, a ta promjena ima bez utjecaja na mogućnost za pristup sustavu Office 365 ili drugim aplikacijama za Azure AD.

### <a name="ad-fs-management-console"></a>Konzole za upravljanje AD FS

1. Otvorite konzolu za upravljanje AD FS na primarni poslužitelj za AD FS.
2. Proširite čvor AD FS i kliknite **Potrebe za oslanjanjem strana smatra pouzdanima**.
3. Desnom tipkom miša kliknite sustava Office 365 i Azure relying strana pouzdanost, a zatim odaberite **Svojstva**.
4. Odaberite karticu **Dodatno** , a zatim odaberite algoritam sigurne raspršivanje SHA256.
5. Kliknite **u redu**.

![Algoritam SHA256 potpisa – BLOG](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>Cmdleti za AD FS PowerShell

1. Bilo koji poslužitelja za AD FS, otvorite PowerShell u odjeljku administratorske ovlasti.
2. Postavite algoritam sigurne raspršivanje pomoću cmdleta **Skup AdfsRelyingPartyTrust** .

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Čitati i

* [Popravak sustava Office 365 pouzdanosti Azure AD Connect](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
