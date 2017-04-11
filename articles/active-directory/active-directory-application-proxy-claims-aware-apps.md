<properties
    pageTitle="Rad s zahtjevima umu aplikacije u Proxy aplikacije"
    description="Opisuje kako započeti s radom s Proxy aplikacije za Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Rad s zahtjevima umu aplikacije u Proxy aplikacije

Zahtjevima umu aplikacije izvršiti preusmjeravanje da biste na sigurnost tokena servisa (STS), koji u nizu zahtijeva vjerodajnice korisnika gotovinu token prije preusmjeravanja korisnika u aplikaciji. Da biste omogućili Proxy aplikacije da biste radili s ove preusmjeravanja, sljedeće korake potrebne da bi se otvorila.

## <a name="prerequisites"></a>Preduvjeti
Prije ovog postupka, provjerite je li STS aplikaciju umu zahtjevima preusmjerava dostupna izvan vaše lokalne mreže.

## <a name="azure-classic-portal-configuration"></a>Azure klasični Konfiguracija portala

1. Objavite svoju aplikaciju prema uputama opisane u [aplikacijama za objavljivanje s Proxy aplikacije](active-directory-application-proxy-publish.md).
2. Na popisu aplikacija odaberite aplikaciju umu zahtjevima, a zatim kliknite **Konfiguriraj**.
3. Ako ste odabrali **Passthrough** kao **Preauthentication način**, provjerite je li da biste odabrali **HTTPS** kao shemu **Vanjski URL** .
4. Ako ste odabrali **Azure Active Directory** kao **Preauthentication način**, odaberite **ništa** kao **Interna metoda provjere autentičnosti**.


## <a name="adfs-configuration"></a>Konfiguriranje ADFS

1. Otvorite Upravljanje ADFS.
2. Idite na **Potrebe za oslanjanjem smatra pouzdanima strana**, desnom tipkom miša kliknite aplikaciju koju objavljujete Proxy aplikacije pa odaberite **Svojstva**.  
  ![Potrebe za oslanjanjem smatra pouzdanima sudionika desnom tipkom miša kliknite naziv aplikacije - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Na kartici **krajnje točke** u odjeljku **Vrsta krajnjoj točki**odaberite **WS Federacija**.
4. U odjeljku **Pouzdana URL** unesite URL koji ste unijeli u Proxy aplikacije u odjeljku **Vanjski URL** , a zatim kliknite **u redu**.  
  ![Dodavanje krajnje točke - Postavi vrijednost pouzdana URL - snimka](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Vidi također

- [Objavljivanje aplikacija pomoću Proxy aplikacije](active-directory-application-proxy-publish.md)
- [Omogućivanje jedinstvene prijave na](active-directory-application-proxy-sso-using-kcd.md)
- [Otklanjanje poteškoća s Proxy aplikacije](active-directory-application-proxy-troubleshoot.md)
- [Omogućivanje aplikacije za nativni klijent za interakciju s aplikacijama proxy poslužitelja](active-directory-application-proxy-native-client.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
