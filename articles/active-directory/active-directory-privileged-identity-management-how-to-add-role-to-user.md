<properties
   pageTitle="Kako dodati ili ukloniti korisnička uloga | Microsoft Azure"
   description="Saznajte kako dodati uloge povlaštene identitetima s aplikacijom Azure Active Directory povlaštene upravljanje identitetom."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD povlaštene upravljanje identitetom: Kako dodati ili ukloniti korisnička uloga

S Azure Active Directory (AD), globalni administrator (ili administrator tvrtke) možete ažurirati korisnike koji su **trajno** dodijeljenih ulogama u Azure AD. To možete učiniti pomoću cmdleta ljuske PowerShell kao što su `Add-MsolRoleMember` i `Remove-MsolRoleMember`. Ili možete pomoću portala za Azure klasični kao što je opisano u [Dodjela administratorskih uloga u Azure Active Directory](active-directory-assign-admin-roles.md).

Aplikacija za Azure AD povlaštene upravljanje identitetom omogućuje povlaštene ulogu administratora da biste trajno dodjele uloga, kao i. Uz to, povlaštene ulogu administratora olakšavaju korisnicima **uvjete** za administratorskih uloga. Administrator uvjete možete aktivirati ulogu kada im je to potrebno, a zatim njihove dozvole istječe nakon što ih gotovo.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Upravljanje ulogama s PIM na portalu za Azure

U tvrtki ili ustanovi možete dodijeliti korisnicima različite administratorske uloge u Azure AD, Office 365 i druge Microsoftove servise i aplikacije.  Dodatne informacije o dostupna uloge možete pronaći na [ulogama u Azure AD PIM](active-directory-privileged-identity-management-roles.md).

Da biste dodali ili uklonili korisnika u ulozi pomoću povlaštene upravljanje identitetom, otvorili PIM nadzornu ploču. Zatim kliknite gumb **korisnika u administratorskih uloga** , ili odaberite određenu ulogu (kao što je globalni Administrator) iz tablice uloge.

> [AZURE.NOTE] Ako još niste omogućili PIM na portalu za Azure još, otvorite da [biste započeli Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) detalje.

Ako želite dati pristup neki drugi korisnik PIM sam uloge PIM potrebne da korisnik ima je podrobnije opisan dodatno [kako omogućiti pristup PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Dodavanje korisnika u grupu uloga

1. [Portal za Azure](https://portal.azure.com/), odaberite pločicu **Azure AD povlaštene upravljanje identitetom** na nadzornoj ploči.
2. Odaberite **Upravljanje ulogama povlaštene**.
3. U tablici **Sažetak uloga** odaberite uloge kojima želite upravljati.
4. U plohu uloga odaberite **Dodaj**.
5. Kliknite **Odabir korisnika** i traženje korisnika na plohu **Odaberite korisnici** .  
6. Odaberite korisnika s popisa rezultata pretraživanja, a zatim kliknite **gotovo**.
4. Kliknite **u redu** da biste potvrdili odabir. Korisnik koji ste odabrali pojavit će se na popisu kao uvjete za ulogu.

> [AZURE.NOTE]
>Novi korisnici u ulozi su samo uvjete za uloga prema zadanim postavkama. Ako želite da bi se trajno ulogu, kliknite korisnika na popisu. Podatke o korisniku će se pojaviti u novi plohu. Odaberite **provjerite perm** na izborniku informacije korisnika.  
>Ako korisnik ne može registrirati za Azure višestruke provjere autentičnosti (MFA) ili je pomoću Microsoftova računa (obično @outlook.com), koje morate donijeti trajna njihove uloge. Administratori uvjete će se zatražiti da biste registrirali za MFA tijekom aktivacije.

Sad kad korisnik ispunjava uvjete za uloge, obavijestite ih da ih možete aktivirati prema uputama u [tome da biste aktivirali ili deaktivirali ulogu](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Uklanjanje korisnika iz uloge

Možete ukloniti korisnike iz dodjele uloga uvjete, ali provjerite je li uvijek najmanje jedan korisnik trajna globalnog administratora.

Slijedite ove korake da biste uklonili određenog korisnika iz uloge:

1. Pomaknite se do uloga na popisu uloge odabirom uloge na nadzornoj ploči Azure AD PIM ili klikom na gumb **korisnika u administratorskih uloga** .
2. Kliknite na korisnike na popisu korisnika.
3. Kliknite **Ukloni**. Poruke zatražit će da biste je potvrdili.
4. Kliknite **da** da biste uklonili ulogu od korisnika.

Ako ne znate koja je korisnicima potrebna vam je njihove dodjele uloga, pa možete [pokrenuti pregled programa access za ulogu](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
