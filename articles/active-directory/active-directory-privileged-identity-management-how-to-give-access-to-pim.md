<properties
   pageTitle="Kako omogućiti pristup PIM | Microsoft Azure"
   description="Saznajte kako dodati uloge korisnika s nastavkom Azure Active Directory povlaštene upravljanje identitetom tako da ih možete upravljati PIM."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Kako omogućiti pristup da biste upravljali Azure AD povlaštene upravljanje identitetom

Globalni administrator koji omogućuje Azure AD povlaštene upravljanje identitetom (PIM) za organizaciju automatski se dodjele uloga i pristup PIM. Nitko dobiti pristup za pisanje po zadanom kroz, uključujući drugih globalnog administratora. Druge globalni adminstrators, administratora za sigurnost i sigurnost čitača imaju pristup samo za čitanje Azure AD PIM. Da biste omogućili pristup PIM prvog korisnika možete dodijeliti drugim korisnicima ulogu **povlaštene ulogu administratora** . Ta se Dodjela moraju izvršiti iz unutar PIM sam te putem komponente PowerShell ili druge portalima se ne može promijeniti.

> [AZURE.NOTE] Upravljanje Azure AD PIM zahtijeva Azure MFA. Budući da se Microsoftovi računi ne može registrirati za Azure MFA, korisnika koji se pomoću Microsoftova računa ne može pristupiti Azure AD PIM.

Provjerite je li postoje uvijek barem dva korisnika u administratorsku ulogu za povlaštene uloga u slučaju da je zaključan jednog korisnika ili izbrišete svoj račun.

## <a name="give-another-user-access-to-manage-pim"></a>Dodijelite drugi korisničkog pristupa za upravljanje PIM

1. Prijava na [portal za Azure](https://portal.azure.com/) , a zatim odaberite aplikaciju **Azure AD povlaštene upravljanje identitetom** na nadzornoj ploči.
2. Odaberite **Upravljanje ulogama povlaštene** > **povlaštene ulogu administratora** > **Dodaj**.

    ![Dodavanje povlaštene ulogu administratora - snimka][1]

4. Na upravljanih korisnici plohu Dodaj korak 1 već je dovršena. Odaberite korak 2, **Odaberite korisnike** i traženje korisnika koje želite dodati.

    ![Odabir korisnika – snimka][2]

6. Odaberite korisnika iz rezultata pretraživanja, a zatim kliknite **gotovo**.
7. Kliknite **u redu** da biste potvrdili odabir. Korisnik koji ste odabrali pojavit će se na popisu povlaštene ulogu administratora.

    - Kad god se nekome dodijelite novu ulogu, automatski postavljaju kao uvjete za aktiviranje ulogu. Ako želite da biste trajno u ulogu, kliknite korisnika na popisu. Odaberite **provjerite perm** na izborniku informacije korisnika.

8. Korisnik Pošalji vezu za [Početak rada s Azure AD povlaštene upravljanje identitetom](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Uklanjanje drugi korisnička prava pristupa za upravljanje PIM

Prije uklonili nekoga iz ulogu povlaštene ulogu administratora, uvijek provjerite postoje će i dalje biti dva korisnika koji je dodijeljen.

1. Na nadzornoj ploči PIM kliknite ulogu **povlaštene ulogu administratora**.  Prikazat će se popis korisnika trenutno u uloge.
2. Kliknite na korisnike na popisu korisnika.
3. Kliknite **Ukloni**.  Predstavljanja potvrdnu poruku.
4. Kliknite **da** da biste korisnika uklonili iz uloge.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
