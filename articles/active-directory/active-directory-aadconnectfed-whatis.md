<properties
    pageTitle="Azure AD Connect i vanjski pristup | Microsoft Azure"
    description="Ova stranica je središnje mjesto za sve dokumentaciju o AD FS operacije pomoću Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect i vanjski pristup

Azure AD Connect omogućuje konfiguriranje pridruživanja na lokalni AD FS i Azure AD. Pomoću vanjski pristup za prijavu, možete omogućiti korisnicima da biste se prijavili Azure AD temelji servisa s lokalnim lozinke i, na mreži tvrtke, bez potrebe za unosom lozinke ponovno. Mogućnosti vanjski pristup za AD FS omogućuje implementacije novog ili odredite postojeće AD FS u farmi sustava Windows Server 2012 R2.

U ovoj se temi je početna stranica za podatke na vanjski pristup odnose radovi za Azure AD Connect i popisi veze na sve druge teme koji se odnose na njega. Veze na Azure AD Connect potražite u članku integriranje vaših lokalnih identiteta sa servisu Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect – teme vanjski pristup

| Tema | Što obuhvaća i kada za čitanje |
|:------|:-----------|
| **Azure AD Connect prijavu mogućnosti korisnika** ||
| [Objašnjenje mogućnosti za prijavu korisnika](active-directory-aadconnect-user-signin.md) | Opis različitih korisnika prijavu mogućnosti i utjecaj Azure prijavu doživljaja rada |
| **AD FS instalacije pomoću Azure AD Connect**||
| [Prije requisites](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Prije requisites za uspješnu instalaciju AD FS putem Azure AD Connect|
| [Konfiguriranje servisa AD FS farme](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Kako instalirati novu AD FS farmu pomoću Azure AD Connect |
| **Izmjena konfiguracije AD FS** | |
| [Popravak pouzdanost](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Kako popraviti trenutnu pouzdanosti između lokalne AD FS i Office 365 / Azure |
| [Dodavanje novi poslužitelj za ADFS](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Proširivanje AD FS farmu s dodatnim AD FS poslužitelja objavu početne instalacije |
| [Dodavanje novog poslužitelja za AD FS WAP](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Proširivanje AD FS farmu s dodatne WAP poslužitelja objavu početne instalacije |
| [Dodavanje novog pridruženoj domeni](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Dodavanje domene da biste biti povezani Azure AD |
|**Instalacija zadataka**||
| [Dodajte prilagođeni logotip i slika](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Izmjena sučelje za prijavu navođenjem prilagođeni logotip koji će biti prikazani na stranicu za prijavu u AD FS |
| [Dodavanje opisa za prijavu](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Promjena opisa prijavu na stranicu za prijavu u AD FS | 
| [Izmjena AD FS zahtjeva pravila](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Izmjena / dodavanje zahtjeva pravila u AD FS odgovara konfiguraciju sinkronizacije Azure AD Connect |


## <a name="additional-resources"></a>Dodatni resursi

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
* [AD FS implementacije u Azure](active-directory-aadconnect-azure-adfs.md)
* [Visoke dostupnosti unakrsno geografske AD FS implementacije u Azure s Azure promet Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


