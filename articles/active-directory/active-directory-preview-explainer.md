<properties
    pageTitle="Azure Active Directory pretpregled explainer | Microsoft Azure"
    description="Tema koja objašnjava razlike između Azure Active Directory na portalu klasični i Azure Active Directory Pretpregled na portalu za Azure."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Pretpregled sučelje za upravljanje Azure Active Directory na portalu za Azure

Sučelje za upravljanje Azure Active Directory (Azure AD) nalazi se u pretpregledu na portalu za Azure. Možete ga isprobati smanjivati prijavom na [portalu za Azure](https://portal.azure.com) kao globalni administrator imenik. Zatim na popisu servisa odaberite Azure Active Directory ako je vidljiva ili odaberite **dodatnih servisa** da biste pogledali popis svih servisa. Ne morate Azure pretplatu za korištenje na Azure AD upravljanje doći na portalu za Azure.


## <a name="capabilities-of-the-preview-experience"></a>Mogućnosti sučelje za pretpregled

Sučelje za pretpregled omogućuje vam da biste upravljali mnoge direktorija resurse kao što su korisnike, grupe i aplikacije, kao i postavke direktorija, na portalu za Azure. Ne možemo su unaprjeđenju ovo da biste obuhvatili sve mogućnosti koje postoje na Azure AD upravljanje iskustvo [Azure klasični portal](https://manage.windowsazure.com). Tek onda, postoje neki upravljanje direktorija Dovršavanje zadataka koje morate i dalje na portalu klasični.

## <a name="manage-the-same-azure-ad-tenants"></a>Upravljanje isti klijenata za Azure AD

Sučelje za pretpregled čita i zapisuje u istom klijentu Azure Active Directory kao klasični portal i centru za administratore sustava Office 365. Promjene u bilo kojem od ovih portalima odražavaju se u svim ostalima.

## <a name="use-the-same-authorization-logic"></a>Koristite isti autorizacije logike

Sučelje za pretpregled koristi iste logike autorizacije kao postojeće klijenti servisa Active Directory. Korisnici imaju dozvolu za mijenjanje direktorija resursa na temelju njihove uloge direktorija, kao što je globalni administrator, administrator korisnicima, administratora za lozinke. Imate ulogu Azure resursa ili pretplatu na Azure Autorizirajte korisnik Upravljanje resursima direktorija. Dodatne informacije o Azure AD Upravljanje ulogama potražite u članku [Dodjela administratorskih uloga u Azure Active Directory](active-directory-assign-admin-roles.md). 

Sučelje za pretpregled optimiziran je za globalnog administratora. Ako koristite sučelje za pretpregled dok prijavljeni kao korisnik koji nije globalni administrator, možda će vam se su smanjene iskustvo. Na primjer, možda ćete moći odaberite gumb koji omogućuje vam da počnete zadatak koji se ne možete završiti u direktoriju. Ne možemo su uskoro poboljšanju ovog problema.
 
## <a name="tell-us-what-you-think"></a>Recite nam svoje mišljenje

Pošaljite povratne informacije o sučelje za pretpregled u odjeljak administrator portala [Azure AD forum za povratne informacije](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc).
