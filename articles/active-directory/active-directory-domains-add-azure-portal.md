<properties
    pageTitle="Dodajte prilagođeni naziv domene s pretpregledom Azure Active Directory | Microsoft Azure"
    description="Kako dodati naziva domene vaša tvrtka Azure Active Directory te kako potvrditi naziv domene."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Dodavanje prilagođenog naziva domene s pretpregledom Azure Active Directory

> [AZURE.SELECTOR]
- [Portal za Azure](active-directory-domains-add-azure-portal.md)
- [Azure klasični portal](active-directory-add-domain.md)

Imate jedan ili više naziva domene vaša tvrtka ili ustanova koristi gradu, a vaši korisnici prijavite se na mrežu tvrtke pomoću naziva domene tvrtke. Pomoću pretpregleda Azure Active Directory (Azure AD), možete dodati svoj naziv domene tvrtke kao i Azure AD. [Novosti u pretpregledu?](active-directory-preview-explainer.md) To vam omogućuje dodjeljivanje korisničkih imena u imeniku koje su vam već poznati korisnika, kao što su ‘alice@contoso.com.’ postupak je jednostavno:

1. Dodajte naziv prilagođene domene u direktoriju
2. Dodavanje unosa DNS-a za naziv domene od registrara naziva domena
3. Provjerite je li naziv prilagođene domene u Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Kako dodati naziv domene?

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **više usluga**, unesite **Azure Active Directory** u tekstni okvir, a zatim pritisnite **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na plohu ***Naziv direktorija*** odaberite **naziva domena**.

4. Na plohu ** *Naziv direktorija* - nazivi domena** odaberite naredbu **Dodaj** .

  ![Odabir naredbe Dodaj](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Na plohu **naziv domene** u okvir, primjerice "contoso.com", unesite naziv prilagođene domene, a zatim odaberite **Dodaj domenu**. Obavezno uključite s domenom .com, .net ili druge proširenje najviše razine.

6. Na ***domainname*** plohu (to jest, na plohu koji će se otvoriti koja ima novi naziv domene u naslovu) pronađite DNS podaci stavka Azure AD pomoću kojega će provjerite je li vaša organizacija je vlasnik naziva prilagođene domene.

  ![Dohvaćanje DNS unos podataka](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Sad kad ste dodali naziv domene, Azure AD morate provjeriti vaše tvrtke ili ustanove je vlasnik naziva domene. Prije Azure AD izvršava Ova provjera, morate dodati DNS unos u datoteku zone DNS-a za naziv domene. Ovaj zadatak se izvodi na web-mjestu registrara naziva domena za naziv domene.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Dodavanje unosa DNS-a kod registrara naziva domena za domenu

Sljedeći korak da biste koristili prilagođeni naziv domene s Azure AD je ažuriranje datoteka zone DNS za domenu. Time se omogućuje Azure AD da biste provjerili vašoj tvrtki ili ustanovi je vlasnik naziva prilagođene domene.

1.  Prijavite se na registrara naziva domena za domenu. Ako nemate pristup za ažuriranje DNS-a stavke, zamolite osoba ili tim tko ima pristup da biste dovršili korak 2 i obavijesti kada se dovrši.

2.  Ažuriranje datoteka zone DNS za domenu dodavanjem DNS stavke dobili od Azure AD. Stavka DNS omogućuje Azure AD za potvrdi vlasništvo domene. Unos DNS-a ne mijenja sve ponašanja, kao što su usmjeravanje pošte ili hostiranje web.

Pomoć za ovu Dodavanje unosa DNS-a, pročitajte [upute o dodavanju unos u DNS-a kod popularnih registrara DNS-a](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Provjerite je li naziv domene za Azure AD

Kada dodate stavku DNS-a, spremni ste za potvrditi naziv domene s Azure AD.

Naziv domene možete provjeriti tek nakon što ste DNS zapisa koji su preneseni. Prijenos često traje samo sekundi, ali ponekad može potrajati sata ili više njih. Ako provjera ne uspije kada prvi put, pokušajte ponovno kasnije.

1.  Prijavite se na [portal za Azure](https://portal.azure.com) pomoću računa koji je globalni administrator za direktorij.

2.  Odaberite **Pregledaj**, unesite Upravljanje korisnicima u tekstni okvir, a zatim pritisnite **Enter**.

    ![Upravljanje korisnicima za otvaranje](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na plohu **Upravljanje korisnicima - nazivi domena** odaberite naziv Neprovjereni domene koju želite provjeriti.

4. Na plohu ***naziv domene*** (to jest, na plohu koji će se otvoriti koja ima novi naziv domene u naslovu), odaberite **Provjeri** da biste dovršili Provjera.

Sada možete [dodijeliti imena korisnika koji uključuje prilagođeni naziv domene](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Ako se ne može potvrditi naziv prilagođene domene, pokušajte sljedeće. Ne možemo ćete započnite s najviše zajednički i prema dolje do najmanje zajednički rad.

1.  **Pričekajte jedan sat**. DNS zapisi morati proširiti prije Azure AD možete provjeriti domene. To može potrajati sata ili više njih.

2.  **Unesen DNS zapis, da biste bili sigurni i je li točan**. Dovršite ovaj korak na web-mjestu za registrara naziva domena za domenu. Ako stavku DNS-a ne postoji u datoteku zone DNS-a ili ako ona još nije točno podudaranje sa stavkom DNS taj Azure AD koje vam je dao Azure AD ne može potvrditi naziv domene. Ako nemate pristup ažurirati DNS zapise za domenu kod registrara naziva domena, zajednički koristiti stavku DNS-a s osoba ili tim u tvrtki ili ustanovi tko ima pristup i zamolite ga da biste dodali stavku DNS-a.

3.  **Brisanje naziva domene s drugom imeniku u Azure AD**. Naziv domene možete provjeriti u samo jednom direktoriju. Ako naziv domene je prethodno potvrdili u neki drugi direktorij, je potrebno je izbrisati li ga možete provjeriti u novi direktorij. Da biste saznali više o brisanju naziva domena, pročitajte [Upravljanje prilagođenih naziva domena](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Dodavanje više prilagođenih naziva domena

Ako vaša tvrtka ili ustanova koristi više prilagođenih naziva domena, kao što su 'contoso.com' i 'contosobank.com', možete ih dodati maksimalno 900 nazivi domena do. Koristite iste korake u ovom članku da biste dodali svaki naziva domena.

## <a name="next-steps"></a>Daljnji koraci

[Upravljanje nazivima prilagođene domene](active-directory-domains-manage-azure-portal.md)
