<properties
    pageTitle="Kako omogućiti objavljivanje aplikacija za nativni klijent s aplikacijama proxy | Microsoft Azure"
    description="Opisuje kako omogućiti aplikacije nativni klijent za komunikaciju s Azure AD aplikacije Proxy poveznik za sigurne daljinski pristup lokalnog aplikacija."
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

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Omogućivanje aplikacije za nativni klijent za interakciju s proxy aplikacije

Azure Active Directory proxy poslužitelj aplikacije široko koristi da biste objavili preglednika aplikacija kao što je SharePoint, Outlook Web Access i redak prilagođenih poslovnih aplikacija. Može se koristiti i da biste objavili nativni klijent aplikacije koje se razlikuju se od web-aplikacijama jer su instalirani na uređaju. To možete učiniti tako da Azure AD izdavanja tokena koje se šalju u standardni autorizirali HTTP zaglavlja za podršku.

![Odnos između krajnji korisnici, Azure Active Directory i objavljenim programima](./media/active-directory-application-proxy-native-client/richclientflow.png)

Preporučeni je način da biste objavili takve aplikacije jest korištenje Azure AD provjera autentičnosti biblioteke koji vodi brigu o svim biste morali provjera autentičnosti te podržava više okruženja drugi klijent. Proxy poslužitelj aplikacije pristaju [Matičnoj aplikaciji za scenarij Web API](active-directory-authentication-scenarios.md#native-application-to-web-api). Postupak za izvršavanje to je na sljedeći način:

## <a name="step-1-publish-your-application"></a>Korak 1: Objavljivanje aplikacija

Objavljivanje proxyja aplikacije kao i na bilo kojoj aplikaciji, korisnicima dodijelite i im premium ili osnovni licence. Dodatne informacije potražite u članku [Objavljivanje aplikacija pomoću Proxy aplikacije](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Korak 2: Konfiguriranje aplikacije

Konfiguriranje matičnoj aplikaciji na sljedeći način:

1. Prijavite se na portal Azure klasični.
2. Odaberite ikonu servisa Active Directory na lijevom izborniku, a zatim odaberite direktorija.
3. Na izborniku gornji kliknite **aplikacije**. Ako nema aplikacije dodani direktorija, ova stranica će prikazati samo veze **Dodaj aplikaciju** . Kliknite na vezu ili umjesto toga možete kliknuti gumb **Dodaj** na naredbenoj traci.
4. Na stranici **što želite učiniti** , kliknite vezu da biste **dodali je moje tvrtke ili ustanove razvoju aplikacija**.
5. Na stranici **Recite nam o aplikaciji** Navedite naziv aplikacije, a zatim odaberite **aplikaciju za nativni klijent**. Kliknite ikonu sa strelicom da biste nastavili.
6. Na stranici **informacije o aplikaciji** nudi **Preusmjeravanje URI** za nativni klijent aplikacije, a zatim kliknite kvačicu da biste završili.

Dodana je aplikacija, a bit ćete usmjereni na stranicu za brzo pokretanje aplikacije.

## <a name="step-3-grant-access-to-other-applications"></a>Korak 3: Dopuštanje pristupa drugim aplikacijama

Omogućivanje matičnoj aplikaciji za izložiti drugih aplikacija u direktoriju:

1. Na izborniku gornji kliknite **aplikacije**, odaberite novi matičnoj aplikaciji pa kliknite **Konfiguriraj**.
2. Pomaknite se prema dolje do odjeljka **dozvole drugim aplikacijama** . Kliknite gumb **Dodaj aplikaciju** , odaberite proxy aplikaciju koju želite dopustiti pristup matičnoj aplikaciji i kliknite kvačicu u donjem desnom kutu. S padajućeg izbornika **Uz prijenos ovlasti dozvole** odaberite novu dozvolu.

![Dozvole za ostale aplikacije snimka – dodavanje aplikacije](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Korak 4: Uređivanje u provjera autentičnosti biblioteke imenika Active Directory

Uređivanje matičnoj aplikaciji koda u kontekstu provjeru autentičnosti sustava na Active Directory provjere autentičnosti biblioteke (ADAL) da biste obuhvaćaju sljedeće:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Varijable treba zamijeniti na sljedeći način:

- **TenantId** pronaći ćete u GUID u URL stranice za **Konfiguriranje** aplikacije, odmah nakon što "/ direktorija /".
- **Sučelju URL** je URL sučelje unesena u proxyja aplikacije i nalazi se na stranici **Konfiguracija** proxy aplikacije.
- **Id klijenta** nativni aplikacije pronaći ćete na stranici **Konfiguracija** matičnoj aplikaciji.
- **Preusmjeravanje URI nativni aplikacije** pronaći ćete na stranici **Konfiguracija** matičnoj aplikaciji.

![Snimka zaslona stranice za konfiguriranje nove matičnoj aplikaciji](./media/active-directory-application-proxy-native-client/new_native_app.png)

Dodatne informacije o tijeku matičnoj aplikaciji potražite u članku [matičnoj aplikaciji za API na webu](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Vidi također

- [Objavljivanje aplikacije koje koriste naziv domene](active-directory-application-proxy-custom-domains.md)
- [Omogućivanje pristupa uvjetno](active-directory-application-proxy-conditional-access.md)
- [Rad s programima koji su svjesni zahtjevima](active-directory-application-proxy-claims-aware-apps.md)
- [Omogućivanje jedinstvene prijave na](active-directory-application-proxy-sso-using-kcd.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
