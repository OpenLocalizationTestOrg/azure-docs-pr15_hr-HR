<properties
pageTitle="Prilagodba stranice za prijavu u pretpregledu Azure Active Directory | Microsoft Azure"
description="Saznajte kako dodati tvrtke brendiranje Azure stranicu za prijavu"
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
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Dodavanje tvrtke brendiranja na stranici za prijavu u pretpregledu Azure Active Directory

Da biste izbjegli zbrku, mnoge tvrtke želite primijeniti dosljedan izgled i dojam preko svih web-mjesta i servisa mogu upravljati. Pretpregled Azure Active Directory sadrži tu mogućnost omogućujući vam da biste prilagodili izgled stranice prijava s logotip tvrtke i korisničke sheme boja. [Novosti u pretpregledu?](active-directory-preview-explainer.md) Stranica za prijavu u je stranica koja se prikazuje kada se prijavite u Office 365 ili drugih web-aplikacija koje koriste Azure AD za davatelja identiteta. Raditi s ove stranice da biste unijeli vjerodajnice.

Ako želite da bi se prikazala proizvođača tvrtke, boja i druge prilagodljive elemenata na toj stranici potražite u članku sljedeće slike da biste razlika između dvaju sučelja.

Sljedeći snimka zaslona prikazuje i primjer stranici sustava Office 365 prijave na na stolnom računalu **prije** Prilagodba:

![Office 365 stranica za prijavu u prije prilagodbe](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Sljedeći snimka zaslona prikazuje i primjer stranici sustava Office 365 prijave na na stolnom računalu **Kada** Prilagodba:

![Office 365 stranica za prijavu u nakon prilagodbe](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>Prilagodba stranica za prijavu u

Obično ako trebate utemeljenima na pregledniku pristup oblaka aplikacije i servise koje je vaša tvrtka ili ustanova pretplaćena, koristite stranicu za prijavu u.

Ako ste primijenili promjene na stranicu za prijavu, može proći do sat vremena da se prikazuju promjene.

Brandiranog stranice prijave prikazuje se samo kada posjećujete servisa s URL-om specifične za klijent kao što su https://outlook.com/**contoso**.com ili https://mail. **Contoso**. com.

Kada posjećujete servis određenih URL-ova koji nisu klijenta (npr.: https://mail.office365.com), pojavit će se na koje nisu brandiranog stranica za prijavu u. u ovom slučaju vaše brendiranje pojavljuje se nakon što unesete svoj korisnički ID ili odaberete korisničke pločice.

> [AZURE.NOTE]
>
- Naziv domene mora biti naveden kao "Aktivno" u dijelu **domene** portala Azure u koju ste konfigurirali brendiranje. Dodatne informacije potražite u članku [Dodavanje prilagođenih naziva domena](active-directory-domains-add-azure-portal.md).
- Stranica za prijavu u brendiranje na stranici potrošača prijava programa Microsoft ne prenijeti. Ako se prijavite pomoću Microsoftova računa, vidjet ćete brandiranog popis korisnika pločica iscrtati Azure AD, ali vizualnom identitetu vaše tvrtke ili ustanove ne primjenjuju se na Microsoftov račun prijava stranice.

Na stranici prijava potvrdni okvir **Zapamti moju prijavu** korisnicima omogućuje da biste ostali prijavljeni kada ih zatvorite i ponovno otvorite njihovu pregledniku. 

   ![Zapamti moju prijavljeni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Ne efekta sesiju života. Možete sakriti potvrdnog okvira na stranicu za prijavu u Azure Active Directory.
Potvrdni okvir prikazuje li se ovisi o postavkama sustava **Zapamti moju prijavu je onemogućen**.

   ![Zapamti moju prijavljeni](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Da biste sakrili potvrdni okvir, konfigurirati ovu postavku u **da**. 

> [AZURE.NOTE] Neke značajke sustava SharePoint Online i Office 2010 ovise o korisnici moći potvrdite taj okvir. Ako konfigurirate ta postavka omogućuje skriven, korisnici možda vidjeti dodatne i neočekivana upute za prijavu.




**Da biste dodali tvrtke brendiranje direktorija:**

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **nove servise**, u tekstni okvir unesite **korisnici i grupe** , a zatim **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. Na plohu **korisnici i grupe** odaberite **brendiranje tvrtke**.

4. Na na **korisnicima i grupama – tvrtke brendiranje** plohu, odaberite naredbu za **Uređivanje** .

    ![Uređivanje Prilagođeno brendiranje](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Izmjena elemenata koji želite prilagoditi. Svi elementi nisu obavezni.

6. Kliknite **Spremi**.

To može potrajati prema gore za jedan sat za sve promjene na stranicu za prijavu u brendiranje da se pojavi.

## <a name="next-steps"></a>Daljnji koraci

[Dodavanje jezično specifične tvrtke brendiranje](active-directory-branding-localize-azure-portal.md)
