<properties
    pageTitle="Dodajte prilagođeni naziv domene i postavljanje vanjskog prijave Azure Active Directory | Microsoft Azure"
    description="Upute za dodavanje naziva domene vaša tvrtka Azure Active Directory i kako postaviti pridruženim prijave između Azure Active Directory i rješenje vanjski pristup lokalnog."
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
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Dodajte prilagođeni naziv domene Azure Active Directory

Prilagođeni naziv domene, primjerice 'contoso.com,' možete konfigurirati tako da korisnici u contoso.com može imati s pridruženim jedan prijave na mrežu tvrtke. Ako već imate Active Directory Federation Services (AD FS) ili drugi vanjski pristup poslužitelju koji se izvodi na mreži tvrtke, možete konfigurirati Azure AD da biste koristili prilagođeni naziv domene pomoću alata za Azure AD Connect. Možete koristiti Azure AD Connect za implementaciju novo okruženje za AD FS i konfiguracije koji za vanjsko jedinstvene prijave za Azure AD.

Ako imate i Planiranje implementacije AD FS ili neki drugi poslužitelj za vanjski pristup, slijedite ove upute: [Dodavanje prilagođenog naziva domene Azure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Dodavanje prilagođenog naziva domene u direktoriju

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com/) korisničkih računa koji je globalni administrator direktorija za Azure AD.

2. U **Servisu Active Directory**, otvorite direktorija, a zatim odaberite karticu **domene** .

3. Na naredbenoj traci odaberite **Dodaj**, a zatim unesite naziv prilagođene domene, primjerice "contoso.com". Obavezno uključite s domenom .com, .net ili druge proširenje najviše razine.

4. Potvrdite okvir **Planiranje konfiguriranje ovu domenu za jedinstvenu prijavu s lokalni Active Directory** .

5. Odaberite **Dodaj**.

Pokrenite alat Azure AD Connect da biste dobili DNS unos koji Azure AD će koristiti da biste potvrdili domenu. Vidjet ćete DNS unos u **Azure AD domene** korak u čarobnjaku. Što možete vidjeti taj korak u čarobnjaku izgleda [u ovim uputama](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Ako nemate alat za Azure AD Connect, možete [preuzeti ovdje](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Dodavanje unosa DNS-a kod registrara naziva domena za domenu

Sljedeći korak da biste koristili prilagođeni naziv domene s Azure AD je ažuriranje datoteka zone DNS za domenu. Time se omogućuje Azure AD da biste provjerili vašoj tvrtki ili ustanovi je vlasnik naziva prilagođene domene.

1. Prijavite se na web-mjestu registrara naziva domena za naziv vaše domene. Ako nemate pristup da biste to učinili, obratite se osoba ili tim u tvrtki ili ustanovi tko ima pristup da biste dovršili korak 2 i obavijesti kada se dovrši.

2. Ažuriranje datoteka zone DNS za domenu dodavanjem DNS stavke dobili od Azure AD. Stavka DNS omogućuje Azure AD za potvrdi vlasništvo domene. Unos DNS-a ne mijenja sve ponašanja, kao što su usmjeravanje pošte ili hostiranje web.

Pomoć za taj korak, pročitajte [upute o dodavanju unos u DNS-a kod popularnih registrara DNS-a](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Provjerite je li naziv domene za Azure AD

Kada dodate stavku DNS-a, spremni ste za potvrditi naziv domene s Azure AD.

Da biste potvrdili domenu, na **Azure AD domene** korak čarobnjaka za Azure AD Connect odaberite **Dalje** . Azure AD će tražiti unos DNS-a u datoteku zone DNS za domenu. Nakon što ste DNS zapisa koji su preneseni Azure AD samo provjerite naziv domene. Prijenos često traje samo sekundi, ali ponekad može potrajati sata ili više njih. Ako provjera ne uspije kada prvi put, pokušajte ponovno kasnije.

Zatim nastavite s preostale korake u čarobnjaku za Azure AD Connect. To će se sinkronizirati korisnicima s poslužitelja servisa Active Directory za Windows Azure AD. Sinkronizirani korisnici u domenu koju ste konfigurirali za vanjski pristup će se na Azure AD pristupiti s pridruženim jedan prijave mrežu tvrtke.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Ako se ne može potvrditi naziv prilagođene domene, pokušajte sljedeće. Ne možemo ćete započnite s najviše zajednički i prema dolje do najmanje zajednički rad.

1.  **Pričekajte jedan sat**. DNS zapisi morati proširiti prije Azure AD možete provjeriti domene. To može potrajati sata ili više njih.

2.  **Unesen DNS zapis, da biste bili sigurni i je li točan**. Dovršite ovaj korak na web-mjestu za registrara naziva domena za domenu. Ako stavku DNS-a ne postoji u datoteku zone DNS-a ili ako ona još nije točno podudaranje sa stavkom DNS taj Azure AD koje vam je dao Azure AD ne može potvrditi naziv domene. Ako nemate pristup ažurirati DNS zapise za domenu kod registrara naziva domena, zajednički koristiti stavku DNS-a s osoba ili tim u tvrtki ili ustanovi tko ima pristup i zamolite ga da biste dodali stavku DNS-a.

3.  **Brisanje naziva domene s drugom imeniku u Azure AD**. Naziv domene možete provjeriti u samo jednom direktoriju. Ako naziv domene je prethodno potvrdili u neki drugi direktorij, je potrebno je izbrisati li ga možete provjeriti u novi direktorij. Da biste saznali više o brisanju naziva domena, pročitajte [Upravljanje prilagođenih naziva domena](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Dodavanje više prilagođenih naziva domena

Ako vaša tvrtka ili ustanova koristi više prilagođenih naziva domena, kao što su 'contoso.com' i 'contosobank.com', možete ih dodati maksimalno 900 nazivi domena do. Koristite iste korake u ovom članku da biste dodali svaki naziva domena.

## <a name="next-steps"></a>Daljnji koraci

-   [Upravljanje nazivima prilagođene domene](active-directory-add-manage-domain-names.md)
-   [Informirajte se o koncepti upravljanja domene u Azure AD](active-directory-add-domain-concepts.md)
-   [Prikaz tvrtke je brendiranje korisnika za prijavu](active-directory-add-company-branding.md)
-   [Upravljanje nazivima domena u Azure AD pomoću komponente PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
