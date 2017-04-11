<properties
pageTitle="Dodavanje jezično specifične tvrtke brendiranja na stranici za prijavu u pretpregledu Azure Active Directory | Microsoft Azure"
description="Saznajte kako dodati tvrtke za određeni jezik brendiranje slike i tekst programa Azure stranicu za prijavu"
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

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Dodavanje jezično specifične tvrtke brendiranja na stranici za prijavu u pretpregledu Azure Active Directory

Da biste izbjegli zbrku, mnoge tvrtke želite primijeniti dosljedan izgled i dojam preko svih web-mjesta i servisa mogu upravljati. Pretpregled Azure Active Directory sadrži tu mogućnost omogućujući vam da biste prilagodili izgled stranice prijava s logotip tvrtke i korisničke sheme boja. [Novosti u pretpregledu?](active-directory-preview-explainer.md) Stranica za prijavu u je stranica koja se prikazuje kada se prijavite u Office 365 ili drugih web-aplikacija koje koriste Azure AD za davatelja identiteta. Raditi s ove stranice da biste unijeli vjerodajnice.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Prilagodbe stranice za prijavu na drugom jeziku

Elementi jezično specifične možete dodati na prilagođenu stranicu za prijavu samo ako ste već stvorili prilagođenu prijavu stranicu kao što je opisano u [Dodavanje tvrtke brendiranja na stranici za prijavu](active-directory-branding-custom-signon-azure-portal.md). Možete konfigurirati jednu stranicu za prijavu u po directory pomoću zadane skupa prilagodljivih elemenata. Nakon konfiguriranja zadani skup elemenata stranice, možete konfigurirati dodatne verzije za različite regionalne sheme. Možete kombinirati i odgovaraju različitih elemenata. Na primjer, možete:

- Stvaranje zadana **Slika stranice za prijavu** koja odgovara sve kulturama, a zatim Stvori određenih verzija za engleski jezik i francuski. Kada svi preglednici postavljena na jedan od tih dviju jezika, jezično specifične slike pojavljuje se dok pojavit će se zadana slika za sve jezike.

- Konfiguriranje različitih logotipa za tvrtku ili ustanovu (na primjer, japanski ili hebrejski verzije).

Preporučujemo da ostavite broj jezika varijacije niskog održavanje i performanse razloga.

**Da biste dodali tvrtke brendiranje direktorija:**

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **nove servise**, u tekstni okvir unesite **korisnici i grupe** , a zatim **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. Na plohu **korisnici i grupe** odaberite **brendiranje tvrtke**.

4. Na na **korisnicima i grupama – tvrtke brendiranje** plohu, odaberite naredbu **Dodaj jezik** .

    ![Dodavanje jezično specifične brendiranje elemenata](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Izmjena elemenata koji želite prilagoditi. Svi elementi nisu obavezni.

6. Kliknite **Spremi**.

To može potrajati prema gore za jedan sat za sve promjene na stranicu za prijavu u brendiranje da se pojavi.

## <a name="next-steps"></a>Daljnji koraci

[Dodavanje tvrtke brendiranja na stranici za prijavu](active-directory-branding-custom-signon-azure-portal.md)
