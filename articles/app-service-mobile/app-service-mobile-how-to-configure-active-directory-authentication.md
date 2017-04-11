<properties
    pageTitle="Upute za konfiguriranje provjere autentičnosti za aplikaciju aplikacije servisa Azure Active Directory"
    description="Upute za konfiguriranje provjere autentičnosti za aplikaciju aplikacije servisa Azure Active Directory."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Upute za konfiguriranje aplikacije servisnoj aplikaciji za korištenje prijava Azure Active Directory

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

U ovoj se temi objašnjava konfiguriranje servisa za Azure aplikacije da biste koristili Azure Active Directory kao davatelj usluge provjere autentičnosti.

## <a name="express"> </a>Konfiguriranje Azure Active Directory pomoću eksplicitnih postavki

13. [Portal za Azure]dođite do aplikacija. Kliknite **Postavke**, a zatim **Provjera autentičnosti/autorizacije**.

14. Ako provjera autentičnosti / autorizacije značajka nije omogućena, uključite prijelaz na **na**.

15. Kliknite **Azure Active Directory**, a zatim u odjeljku **Način upravljanja** **Express** .

16. Kliknite **u redu** da biste registrirali aplikaciju servisa Azure Active Directory. To će stvoriti novu registracije. Ako želite da biste odabrali postojeći Registracija umjesto toga, kliknite **Odaberite postojeći aplikacije** , a zatim potražite naziv prethodno stvorenog Registracija unutar vaš klijent.
Kliknite Registracija da biste ga odabrali, a zatim kliknite **u redu**. Zatim kliknite **u redu** na postavke plohu Azure Active Directory.

    ![][0]

    Prema zadanim postavkama aplikacije servisa za omogućuje provjeru autentičnosti, ali ograničiti ovlašteni pristup sadržaju web-mjesta i API-ji. Korisnici moraju Autorizirajte u kodu aplikacije.

17. (Neobavezno) Da biste ograničili pristup web-mjestu samo korisnici autentičnost po Azure Active Directory, postavite **poduzeti kada zahtjeva nije autentičnost** da biste se **prijavili pomoću servisa Azure Active Directory**. To su potrebne da sve zahtjeve za moguće provjeriti autentičnost, a sve zahtjeve za Neprovjereni vas preusmjerava Azure Active Directory za provjeru autentičnosti.

17. Kliknite **Spremi**.

Sada ste spremni za korištenje Azure Active Directory za provjeru autentičnosti u svojoj aplikaciji.

## <a name="advanced"> </a>(Zamjenski metoda) ručno konfiguriranje Azure Active Directory pomoću naprednih postavki
Možete odabrati i omogućuju konfiguracijske postavke ručno. To je Preferirani rješenja ako se razlikuje od klijenta s kojim se prijavite u Azure AAD klijentu koji želite koristiti. Da biste dovršili konfiguraciju, najprije morate stvoriti na Registracija servisa Azure Active Directory, a zatim navedite neke detalje registracija za aplikacije servisa.

### <a name="register"> </a>Morate registrirati aplikacije Azure Active Directory

1. Prijavite se na [portal za Azure], a zatim otvorite u aplikaciji. Kopirajte **URL-a**. Time ćete koristiti za konfiguriranje aplikacije Azure Active Directory.

3. Prijava na [portal za Azure klasični] , a zatim otvorite sa **Servisom Active Directory**.

    ![][2]

4. Odaberite direktorija, a zatim odaberite karticu **aplikacija** pri vrhu. Pri dnu da biste stvorili novi Registracija aplikacije kliknite **DODAJ** .

5. Kliknite **Dodaj aplikaciju je moje tvrtke ili ustanove razvoju**.

6. U čarobnjaku za dodavanje aplikacije unesite **naziv** aplikacije, a zatim kliknite vrstu **API Web And/Or aplikaciju za Web** . Kliknite da biste nastavili.

7. U okvir **URL za prijavu na** zalijepite URL aplikacije koju ste ranije kopirali. Unesite taj isti URL u okvir **ID URI aplikacije** . Kliknite da biste nastavili.

8. Kada aplikacija je dodana, kliknite karticu **Konfiguracija** . Uređivanje **Odgovora URL-a** u odjeljku **jedinstvenu prijavu** će se URL aplikacije dodan putom, _/.auth/login/aad/callback_. Na primjer, `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Provjerite koristite li HTTPS shemu.

    ![][3]

9. Kliknite **Spremi**. Zatim kopirajte **ID klijenta** za aplikaciju. Konfigurirate aplikacije za kasnije korištenje.

10. Na dnu naredbenoj traci kliknite **Prikaz krajnje točke**, kopirajte URL **Dokumenta metapodataka za vanjski pristup** i preuzmite taj dokument i je otvoriti u pregledniku.

11. Unutar korijenski element **EntityDescriptor** mora biti atribut **entityID** obrasca `https://sts.windows.net/` slijedi GUID određene klijent sustava (pod nazivom "ID klijenta"). Kopirajte ovu vrijednost: će vam poslužiti kao **URL izdavač**. Konfigurirate aplikacije za kasnije korištenje.

### <a name="secrets"> </a>Dodavanje Azure Active Directory informacije u aplikaciji

13. Vratite se u [Azure portal]dođite do aplikacija. Kliknite **Postavke**, a zatim **Provjera autentičnosti/autorizacije**.

14. Ako se značajka provjere autentičnosti/Autorizacija nije omogućena, pretvorite prijelaz na **na**.

15. Kliknite **Azure Active Directory**, a zatim kliknite **Dodatno** u odjeljku **Način upravljanja**. Zalijepite vrijednost ID klijenta i izdavatelj URL koji ste prethodno nabavili. Zatim kliknite **u redu**.

    ![][1]

    Prema zadanim postavkama aplikacije servisa za omogućuje provjeru autentičnosti, ali ograničiti ovlašteni pristup sadržaju web-mjesta i API-ji. Korisnici moraju Autorizirajte u kodu aplikacije.

17. (Neobavezno) Da biste ograničili pristup web-mjestu samo korisnici autentičnost po Azure Active Directory, postavite **poduzeti kada zahtjeva nije autentičnost** da biste se **prijavili pomoću servisa Azure Active Directory**. To su potrebne da sve zahtjeve za moguće provjeriti autentičnost, a sve zahtjeve za Neprovjereni vas preusmjerava Azure Active Directory za provjeru autentičnosti.

17. Kliknite **Spremi**.

Sada ste spremni za korištenje Azure Active Directory za provjeru autentičnosti u svojoj aplikaciji.

## <a name="optional-configure-a-native-client-application"></a>(Neobavezno) Konfiguriranje aplikacije za nativni klijent

Azure Active Directory omogućuje da biste registrirali nativni klijenata, koji pruža veću kontrolu nad dozvole preslikavanje. Potreban vam je to ako želite izvršiti prijave pomoću biblioteke kao što je **Provjera autentičnosti biblioteke imenika Active Directory**.

1. Dođite do **Servisa Active Directory** [Azure klasični portal].

2. Odaberite direktorija, a zatim odaberite karticu **aplikacija** pri vrhu. Pri dnu da biste stvorili novi Registracija aplikacije kliknite **DODAJ** .

3. Kliknite **Dodaj aplikaciju je moje tvrtke ili ustanove razvoju**.

4. U čarobnjaku za dodavanje aplikacije unesite **naziv** aplikacije, a zatim kliknite vrstu **Nativni klijentska aplikacija** . Kliknite da biste nastavili.

5. U okvir za **Preusmjeravanje URI** unesite krajnje _/.auth/login/done_ web-mjesta, pomoću HTTPS shemu. Ta vrijednost mora biti sličan _https://contoso.azurewebsites.net/.auth/login/done_. Ako stvorite aplikacije za Windows, umjesto toga pomoću [paketa SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) kao URI.

6. Kada je dodana matičnoj aplikaciji, kliknite karticu **Konfiguracija** . Pronalaženje **ID klijenta** i zabilježite tu vrijednost.

7. Pomaknite se na stranicu prema dolje do odjeljka **dozvole drugim aplikacijama** , a zatim kliknite **Dodaj aplikaciju**.

8. Pronađite web-aplikaciju koju ste ranije registrirali i kliknite ikonu plus. Zatim kliknite Provjeri da biste zatvorili dijaloški okvir. Ako web-aplikacija nije moguće pronaći, dođite do njegova Registracija i dodajte novi odgovor URL (npr., verzija HTTP trenutni URL-a), kliknite Spremi, a zatim ponovite korake – aplikacija treba prikazuju se na popisu.

9. Na koji ste upravo dodali novu stavku, otvorite padajući izbornik **Uz prijenos ovlasti dozvole** i odaberite **pristup (appName)**. Zatim kliknite **Spremi**.

Sada ste konfigurirali nativni klijent aplikacije koje mogu pristupiti aplikacije servisa za aplikacijama.

## <a name="related-content"> </a>Povezani sadržaj

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Portal za Azure]: https://portal.azure.com/
[Azure klasični portal]: https://manage.windowsazure.com/
[alternative method]:#advanced
