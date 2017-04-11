<properties 
    pageTitle="Provjeru s mobilnog radnje REST API - ručno postavljanje"
    description="U članku se opisuje kako ručno postaviti provjere autentičnosti za mobilne radnje REST API-ji" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Provjeru s mobilnog radnje REST API - ručno postavljanje

Ovo je dokumentacija za dodatak za [provjeru autentičnosti s mobilnog radnje REST API -ji](mobile-engagement-api-authentication.md). Provjerite je li čitate prvi put da biste dobili kontekstu. To opisuje drugi način za jednokratni instalacijski program za postavljanje sustava provjere autentičnosti za mobilne radnje REST API-ji pomoću portala za Azure. 

>[AZURE.NOTE] Upute u nastavku su koji se temelji na ovaj [Vodič za Active Directory](../resource-group-create-service-principal-portal.md) i prilagoditi za što je potrebno za provjeru autentičnosti za API radnje Mobile. Pa potražite je ako želite da biste shvatili korake u nastavku detaljno. 

1. Prijavite se na račun za Azure putem [klasične portal](https://manage.windowsazure.com/).

2. U lijevom oknu odaberite **Servisa Active Directory** .

     ![Odabir servisa Active Directory][1]

3. Odaberite **Zadani servisa Active Directory** portalu za Azure. 

     ![Odaberite imenik][2]

    >[AZURE.IMPORTANT] Taj se način funkcionira samo kada radite u zadanom vašeg računa servisa Active Directory i neće funkcionirati ako se taj postupak servisa Active Directory koju ste stvorili na vašem računu. 

4. Da biste pregledali aplikacije u direktoriju, kliknite **aplikacije**.

     ![Prikaz aplikacija][3]

5. Kliknite **DODAJ**. 

     ![Dodavanje aplikacije][4]

6. Kliknite **Dodavanje aplikacije je moje tvrtke ili ustanove razvoju**

     ![nove aplikacije][5]

6. Unesite naziv aplikacije, odaberite vrstu aplikacije kao **API WEB AND/OR APLIKACIJU za WEB** i kliknite gumb Dalje.

     ![Naziv aplikacije][6]

7. Možete unijeti sve URL-ove sustava za **Prijavu na URL** i **Aplikacije ID URI**. Ne koriste se za naše scenarij i URL-ovi same se provjeriti.  

     ![Svojstva aplikacije][7]

8. Na kraju ovog, imat ćete aplikaciju AAD pod nazivom ste prethodno naveli ovako. Ovo je vaš **AD\_Aplikacije\_naziv** i zabilježite ga.  

     ![Naziv aplikacije][8]

9. Kliknite naziv aplikacije, a zatim kliknite **Konfiguriraj**.

     ![Konfiguriranje aplikacije][9]

10. Zabilježite ID KLIJENTA koji će se koristiti kao **KLIJENT\_ID** vaše API poziva. 

     ![Konfiguriranje aplikacije][10]

11. Pomaknite se prema dolje do odjeljka **tipke** , dodajte ključ s kapaciteta trajanje 2 godina (isteka) i kliknite **Spremi**. 

     ![Konfiguriranje aplikacije][11]


12. Odmah kopirajte vrijednost koja je prikazana tipke samo sada prikazati i ne sprema da bi se neće prikazati ikad ponovno. Ako je izgubite zatim morat ćete stvoriti novi ključ. To će biti **CLIENT_SECRET** za pozive API-JA. 

     ![Konfiguriranje aplikacije][12]

    >[AZURE.IMPORTANT] Ovaj ključ istječe na kraju trajanje koje ste naveli pa provjerite jeste li obnoviti kada se vrijeme u suprotnom provjere autentičnosti na API neaktivna. Brisanje i ponovno stvoriti ovaj ključ ako mislite da ga je ugrožena.
 
13. Kliknite gumb za **PRIKAZ krajnje točke** sada koji će se otvoriti dijaloški okvir **Aplikacije krajnje točke** . 

    ![][13]

14. U dijaloškom okviru aplikacije krajnje točke kopirajte **TOKENA krajnjoj TOČKI 2.0 OAUTH**. 

    ![][14]

15. U sljedećem obliku pri čemu je GUID u URL-u **TENANT_ID** pa zabilježite ga bit će ovaj krajnja točka: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Sada ćemo nastavit će konfigurirati dozvole na ovu aplikaciju. Za ovaj morat ćete otvoriti [Azure portal](https://portal.azure.com). 

17. Kliknite **Grupe resursa** i pronađite grupu resursa **Mobile radnje** .  

    ![][15]

18. Kliknite grupu resursa **Radnje Mobile** , a zatim otvorite plohu **Postavke** ovdje. 

    ![][16]

19. Kliknite na **korisnike** u plohu postavke, a zatim na **Dodaj** za dodavanje korisnika. 

    ![][17]

20. Kliknite **Odaberite ulogu**

    ![][18]

21. Kliknite na **vlasnika**

    ![][19]

22. Traženje naziv vaše aplikacije **AD\_Aplikacije\_naziv** u okvir za pretraživanje. Prema zadanim postavkama ovdje nećete vidjeti to. Kada pronađete, odaberite ga i kliknite **Odaberite** pri dnu zaslona u plohu. 

    ![][20]

23. Na plohu **Dodavanje pristup** je prikazat će se kao **1 korisnik, 0 grupe**. Na ovom plohu za potvrdu promjene, kliknite **u redu** . 

    ![][21]

Sada ste izvršili potrebne AAD konfiguraciju i su sve postavite da biste uputili poziv API-ji. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



