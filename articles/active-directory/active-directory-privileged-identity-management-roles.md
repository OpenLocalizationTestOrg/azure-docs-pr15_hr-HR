<properties
   pageTitle="Uloga u PIM | Microsoft Azure"
   description="Saznajte koje uloge koriste se za povlaštene identitete s nastavkom povlaštene upravljanje identitetom Azure."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Uloga upravljanje identitetom povlaštene Azure AD

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Možete dodijeliti korisnicima u svojoj tvrtki ili ustanovi za različite uloge administratora Azure AD. Ove dodjele uloga odrediti koje zadatke, na primjer dodavanjem ili uklanjanjem korisnika ili promjena postavki servisa korisnici će moći izvedete Azure AD, Office 365 i drugim Microsoft Online Services i povezani aplikacijama.  

Globalni administrator može ažurirati koje korisnicima **trajno** dodijeljenih ulogama u Azure AD pomoću cmdleta ljuske PowerShell kao što su `Add-MsolRoleMember` i `Remove-MsolRoleMember`, ili pomoću portala za klasični kao što je opisano u [Dodjela administratorskih uloga u Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD povlaštene identiteta Management (PIM) upravlja pravila za povlaštene pristupa za korisnike u Azure AD. PIM dodjeljuje korisnicima jednu ili više uloga u Azure AD, a možete dodijeliti nekoga da se trajno u ulozi, ili uvjete za ulogu. Kada korisnik trajno dodijeljena uloga ili aktivira dodjelu uvjete uloge, a zatim ih možete upravljati Azure Active Directory, Office 365 i drugih aplikacija s dozvole dodijeljene njihove uloge.

Nema razlike u programu access dodijeljen osoba koja ima stalni nasuprot dodjelu uloga uvjete. Jedina razlika je da neke osobe ne moraju da access cijelo vrijeme. Vrše uvjete za ulogu i možete uključiti i isključiti bilo koje su im potrebne za.

## <a name="roles-managed-in-pim"></a>Uloge upravljanih u PIM

Povlaštene upravljanje identitetom omogućuje korisnicima dodijeliti uobičajenih administratorske uloge, uključujući:


- **Globalni administrator** (poznat i kao administrator tvrtke) ima pristup svim značajkama administratora. Imate više od jedne globalni administrator u tvrtki ili ustanovi. Osoba koja registrira kupiti Office 365 automatski postaje globalni administrator.
- **Povlaštene ulogu administratora** upravlja Azure AD PIM i prilagođava dodjele uloga za druge korisnike.  
- **Administrator za naplatu** kupuje, upravlja pretplatama, upravlja karticama za podršku i nadzire funkcioniranje servisa.
- **Administrator lozinki** vraća lozinki, upravlja zahtjevima za uslugu i nadzire funkcioniranje servisa. Administratori lozinku ograničeni su na ponovno postavljanje lozinke za korisnike.
- **Administrator servisa** upravlja zahtjevima za uslugu i monitora stanje servisa.

  > [AZURE.NOTE] Ako koristite Office 365, zatim prije dodjele administratorska uloga servisa korisniku, najprije korisnika dodijeliti administratorske dozvole servisa, kao što je Exchange Online.

- **Administrator za upravljanje korisnicima** vraća lozinki, nadzire funkcioniranje servisa i upravlja korisničke račune, grupe korisnika i zahtjeve za uslugu. Administrator za upravljanje korisnicima ne možete izbrisati globalni administrator, stvorite drugi administratorskih uloga ili ponovno postavljanje lozinki za naplatu, globalne administratore i servisne administratore.
- **Administrator sustava Exchange** ima Administrativni pristup sustavu Exchange Online putem centra za administratore sustava Exchange (EAC) i može obaviti gotovo svaki zadatak koji se u sustavu Exchange Online.
- **Administrator sustava SharePoint** ima Administrativni pristup sustavu SharePoint Online putem centra za administratore sustava SharePoint Online i može obaviti gotovo svaki zadatak u sustavu SharePoint Online.
- **Skype za tvrtke administrator** ima Administrativni pristup Skype za tvrtke putem servisa Skype za tvrtke centar za administratore i može obaviti gotovo svaki zadatak u programu Skype za tvrtke Online.

Pročitati u ovim člancima više pojedinosti o [dodjeljivati administratorske uloge u Azure AD](active-directory-assign-admin-roles.md) i [Dodjela administratorskih uloga u sustavu Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Iz PIM, možete ga [Dodjeljivanje te uloge korisniku](active-directory-privileged-identity-management-how-to-add-role-to-user.md) tako da korisnik može [aktivirali ulogu po potrebi](active-directory-privileged-identity-management-how-to-activate-role.md).

Ako želite omogućiti neki drugi korisnik pristup upravljanju PIM sam uloge PIM potrebne da korisnik ima je podrobnije opisan dodatno [kako omogućiti pristup PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Uloge ne upravlja u PIM

Uloga u sustavu Exchange Online ili SharePoint Online, osim onih spomenutih, nisu prikazani su u Azure AD i tako da nisu vidljive u PIM. Dodatne informacije o promjeni dodjele preciznije uloga te usluga sustava Office 365 potražite u članku [dozvole u sustavu Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure pretplate i grupa resursa i nisu prikazani su u Azure AD. Da biste upravljali Azure pretplate, potražite u članku [Dodavanje ili promjenu Azure administratorske uloge](../billing-add-change-azure-subscription-administrator.md) i dodatne informacije o Azure RBAC potražite u članku [Upravljanje pristupom Azure Role-Based](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Uloge korisnika i prva prijava
Za neke Microsoftove servise i aplikacije korisnika dodijelite uloge možda neće biti dovoljni da biste omogućili taj korisnik biti administrator.

Pristup portalu Azure klasični potreban je korisnik biti administrator servisa ili zajednički administrator Azure pretplate, čak i ako se korisnik ne morate upravljati Azure pretplate.  Na primjer, da biste upravljali postavkama konfiguracije za Azure AD na portalu klasični, korisnik mora biti globalni administrator u Azure AD i administrator pretplatu zajednički Azure pretplate na.  Da biste saznali kako dodati korisnike Azure pretplate, pogledajte [upute za dodavanje ili promjenu Azure administratorskih uloga](../billing-add-change-azure-subscription-administrator.md).

Pristup Microsoft Online Services može zahtijevati korisniku dodijeliti licence mogli otvorite portal servisa i obavljanje zadataka administracije.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Dodjela licence korisniku u Azure AD

1. Prijavite se u [Azure klasični portalu] (http://manage.windowsazure.com) s računom globalni administrator ili na zajednički administratorski račun.
2. Odabir **Svih stavki** iz glavnog izbornika.
3. Odaberite imenik želite raditi, a koja ima licenci koje su povezane s njom.
4. Odaberite **licence**. Prikazat će se popis dostupnih licenci.
5. Odaberite plan licenca koja sadrži licence koje želite distribuirati.
6. Odaberite **dodijeliti korisnicima**.
7. Odaberite korisnika kojeg želite dodijeliti licencu.
8. Kliknite gumb **dodijeliti** .  Korisnik može odmah prijavite Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
