<properties
    pageTitle="Objavljivanje aplikacija s Proxy aplikacije Azure AD | Microsoft Azure"
    description="Objavljivanje aplikacija lokalnog s oblakom pomoću Proxy aplikacije za Azure AD."
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
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Objavljivanje aplikacije koje koriste Proxy aplikacije za Azure AD

Proxy aplikacije za Azure AD pomaže vam podržava zaposlenici zaduženi za udaljene objavljivanjem na lokalni aplikacije koje će se može pristupiti putem Interneta. Točke, već imat ćete [omogućiti Proxy aplikacije na portalu za Azure klasični](active-directory-application-proxy-enable.md). U ovom se članku vodit će vas kroz korake da biste objavili aplikacije koje koristite u lokalnoj mreži i sigurne daljinski pristup iz izvan mreže. Kada dovršite ovaj članak, bit ćete spremni za konfiguriranje aplikacije s osobne informacije ili sigurnosne preduvjete.

> [AZURE.NOTE] Proxy poslužitelj aplikacije je značajka koja je dostupna samo ako ste nadogradili na Premium ili Osnovno izdanje Azure Active Directory. Dodatne informacije potražite u članku [izdanja Azure Active Directory](active-directory-editions.md).

## <a name="publish-an-app-using-the-wizard"></a>Aplikaciju pomoću čarobnjaka za objavljivanje

1. Prijavite se kao administrator [Azure klasični portal](https://manage.windowsazure.com/).
2. Idite na servisu Active Directory i odaberite direktorija kojima je omogućeno Proxy aplikacije.

    ![Active Directory - ikona](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Kliknite karticu **aplikacije** , a zatim kliknite gumb **Dodaj** pri dnu zaslona

    ![Dodavanje aplikacije](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Odaberite **Objavi aplikacije koja će se može pristupiti iz izvan mreže**.

    ![Objavljivanje aplikacije koja će se može pristupiti iz izvan mreže](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Navedite sljedeće podatke o aplikacija:

    - **Naziv**: jednostavnih naziv aplikacije. Mora biti jedinstvena u direktoriju.
    - **Interna URL**: adresu koja koristi Proxy poveznik aplikacija za pristup aplikaciji iz unutar privatne mreže. Možete unijeti određene put na pozadinskom poslužitelju za objavljivanje, dok je ostatak poslužitelj objavu. Na taj način možete objaviti različita web-mjesta na istom poslužitelju i dati svaki od njih vlastiti naziv i pristup pravila.

        > [AZURE.TIP] Ako objavite puta, provjerite je li ona sadrži sve potrebne slike, skripte i listove stilova za svoju aplikaciju. Na primjer, ako aplikaciju na https://yourapp/app i koristi slike koja se nalazi na https://yourapp/media, trebali biste objavite https://yourapp/ kao put.

    - **Preauthentication metoda**: kako Proxy aplikacije provjerava korisnike prije nego što im daje pristup aplikaciji. Odaberite jednu od mogućnosti s padajućeg izbornika.

        - Azure Active Directory: Proxy aplikacije preusmjerava korisnika da se prijavite pomoću Azure AD koja potvrđuje njihove dozvole za direktorija i aplikaciju.
        - PASSTHROUGH: Korisnici ne moraju provjeriti autentičnost za pristup aplikaciji.

    ![Svojstva aplikacije](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Da biste dovršili Čarobnjak, kliknite kvačicu pri dnu zaslona. Aplikacija definirana je sada u Azure AD.


## <a name="assign-users-and-groups-to-the-application"></a>Dodjeljivanje korisnicima i grupama u aplikaciju

Korisnici za pristup objavljenom programu, morate ih dodijeliti pojedinačno ili u grupama. (Imajte na umu da biste dodijelili sami access previše.) To zahtijeva svaki korisnik licence za Azure osnovni ili noviji. Licence možete dodijeliti pojedinačno ili grupe. Dodatne informacije potražite u članku [Dodjela korisnika u aplikaciju](active-directory-applications-guiding-developers-assigning-users.md) . 

Za aplikacije koje je potrebno prethodna to daje dozvole da biste koristili aplikaciju. Aplikacije za koje ne zahtijevaju Prethodna, korisnici mogu i dalje dodijeliti aplikaciju da bi se prikazivalo u svoj popis aplikacija, kao što su MyApps.

1. Nakon dovršetka Čarobnjak za dodavanje aplikacije, posjetite stranicu za brzi početak rada za svoju aplikaciju. Da biste upravljali tko ima pristup aplikacije, odaberite **korisnici i grupe**.

    ![Brzi početak rada Proxy aplikacije korisnicima dodijelite - snimka](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Traženje određene grupe u direktoriju ili prikazali sve korisnike. Da biste prikazali rezultate pretraživanja, kliknite kvačicu.

    ![Traženje grupa ili korisnika – snimka](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Odaberite svakog korisnika ili grupu koju želite dodijeliti aplikacije, a zatim kliknite **Dodijeli**. Se od vas zatraži da biste potvrdili ovu akciju.

> [AZURE.NOTE] Za aplikacije za integriranu provjeru autentičnosti sustava Windows, možete dodijeliti samo korisnici i grupe koji se sinkroniziraju iz lokalnog Active Directory. Prijavite se pomoću Microsoftova računa i Gosti korisnika nije moguće dodijeliti za aplikacije koje se objavljuju s Azure Active Directory Proxy aplikacije. Provjerite jesu li vaši korisnici prijavi pomoću vjerodajnica koje su dio iste domene aplikacija objavljujete.

## <a name="test-your-published-application"></a>Testiranje objavljeni program

Nakon što ste objavili aplikacije, možete ga testirati tako da odete na URL koji ste objavili. Pripazite da joj možete pristupiti, koji prikazuje se ispravno i radi li sve prema očekivanjima. Ako imate poteškoća s ili primite poruku o pogrešci, pokušajte [vodiča za otklanjanje pogrešaka](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfiguriranje aplikacija

Možete izmijeniti objavljene aplikacije ili postavite napredne mogućnosti na stranici Konfiguracija. Na ovoj stranici možete prilagoditi aplikaciju promjenom naziva ili prijenosom logotip. Možete upravljati i pravila za pristup kao što su prethodna metoda ili višestruke provjere autentičnosti.

![Napredna konfiguracija](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Nakon što ste objavili aplikacije pomoću Azure Active Directory proxy poslužitelj aplikacije, pojavljuju se na popisu aplikacija u Azure AD, a možete upravljati ako ih ima.

Ako onemogućite Proxy aplikacije servisa nakon objave aplikacije, više nisu dostupni iz izvan privatne mreže. Time nećete izbrisati aplikacije.

Da biste vidjeli aplikaciju i provjerite je li se može pristupiti, dvokliknite naziv aplikacije. Ako je servis Proxy aplikacije onemogućen, a aplikacija nije dostupna, pri vrhu zaslona pojavljuje se poruka upozorenja.

Da biste izbrisali aplikaciju, odaberite aplikaciju na popisu, a zatim kliknite **Izbriši**.

## <a name="next-steps"></a>Daljnji koraci

- [Objavljivanje aplikacije koje koriste naziv domene](active-directory-application-proxy-custom-domains.md)
- [Omogućivanje jedinstvene prijave na](active-directory-application-proxy-sso-using-kcd.md)
- [Omogućivanje pristupa uvjetno](active-directory-application-proxy-conditional-access.md)
- [Rad s programima koji su svjesni zahtjevima](active-directory-application-proxy-claims-aware-apps.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
