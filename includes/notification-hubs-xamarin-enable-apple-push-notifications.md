

Da biste registrirali aplikacije za slanje obavijesti putem Apple automatske obavijesti servisa (APN-ove), morate stvoriti novi automatske certifikat, ID aplikacije i dodjele resursa profila za projekt na Appleova portala za razvojne inženjere. ID-a aplikacija će sadržavati konfiguracijske postavke koje omogućuju aplikacije za slanje i primanje automatske obavijesti. Te postavke neće sadržavati stavku certifikat automatske obavijesti potreban za provjeru autentičnosti s Apple automatske obavijesti servisa (APN-ove) pri slanju i primanju automatske obavijesti. Dodatne informacije o koncepte potražite u članku službeni dokumentaciju [Apple automatske obavijesti servisa](http://go.microsoft.com/fwlink/p/?LinkId=272584) .


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Generiranje datoteke zahtjev za potpisivanje certifikata za slanje certifikata

Sljedeće će vas voditi kroz stvaranje zahtjev za potpisivanje certifikata. To će se koristiti za stvaranje certifikata automatske mogla koristiti za APN-ove.

1. Na računalu Mac, pokrenite alat za pristup spremišta lozinki. Mogu se otvoriti iz mape **uslužni programi** ili u **druge** mape na pločici za pokretanje.

2. Kliknite **Pristup spremišta lozinki**, proširite **Pomoćnik za potvrdu**, a zatim kliknite **zahtjev za potvrdom ovlaštene...**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Odaberite **Adresu e-pošte za korisnika** i **Uobičajeni naziv** , obavezno da **spremljeni na disk** , a zatim **Nastavi**. **Adresa e-pošte CA** polje ostavite prazno kao nije obavezno.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Upišite naziv za datoteku potvrda potpisa zahtjev službe za Korisnike u **Spremi kao**, odaberite mjesto na **kojem**, a zatim kliknite **Spremi**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Time CSR datoteku na odabranom mjestu; Zadano mjesto je na radnoj površini. Imajte na umu mjesto za tu datoteku.


####<a name="register-your-app-for-push-notifications"></a>Registracija aplikacije za slanje obavijesti

Stvaranje nove eksplicitnih aplikacije ID-a za svoju aplikaciju s Apple i i konfigurirajte je za slanje obavijesti.  

1. Dođite do [iOS Provisioning Portal](http://go.microsoft.com/fwlink/p/?LinkId=272456) u centru za razvojne inženjere Apple, prijavite se pomoću Apple ID, kliknite **identifikatora**, kliknite **ID-ove aplikacije**i na kraju kliknite na **+** prijavite se da biste registrirali nove aplikacije.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Ažurirajte sljedeća tri polja za novu aplikaciju, a zatim kliknite **Nastavi**:

    * **Naziv**: upišite opisni naziv aplikacije u polje **naziv** u odjeljku **Opis ID aplikacije** .

    * **Paket identifikator**: u odjeljku **Eksplicitnih ID aplikacije** unesite **Identifikator paket** u obliku `<Organization Identifier>.<Product Name>` kao što je rečeno [Vodič za raspodjelu aplikacije](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Time se mora podudarati što služi i u programu project XCode, Xamarin ili Cordova aplikacije.

    * **Automatske obavijesti**: Potvrdite mogućnost **Automatske obavijesti** u odjeljku **Aplikacije servisa** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Na stranici Potvrda zaslonu aplikacije ID, pregledajte postavke, a kada ih kliknite **Pošalji**

4.  Nakon što ste poslali novi ID aplikacije, vidjet ćete zaslonu **registracije dovršeno** . Kliknite **gotovo**.

5. U centru za razvojne inženjere u odjeljku ID-ove aplikacije, pronađite ID aplikacije upravo stvorili, a na recima kliknite. Klikom na retku ID aplikacija će se prikazati detalje aplikacije. Kliknite gumb **Uređivanje** pri dnu.

6. Pomaknite se do dna zaslona, a zatim kliknite gumb **Stvaranje certifikata …** u odjeljku **Razvoj automatske SSL certifikata**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Time će se prikazati Pomoćnik za "Dodaj iOS certifikata".

    > [AZURE.NOTE] Pomoću ovog praktičnog vodiča koristi certifikat za razvoj. Isti postupak koristi prilikom registriranja radnog certifikata. Samo provjerite je li koristiti istu vrstu certifikata prilikom slanja obavijesti.

7. Kliknite **Odaberite datoteku**, dođite do mjesta na kojem ste spremili predstavniku službe za Korisnike za slanje certifikata. Kliknite **Generiraj**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Nakon potvrde stvorio portal, kliknite gumb za **Preuzimanje** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Preuzimanje potpisnog certifikata i sprema na računalo u mapi preuzimanja.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Prema zadanim postavkama preuzetu datoteku certifikat za razvoj pod nazivom **aps_development.cer**.

9. Dvokliknite certifikat preuzete automatske **aps_development.cer**. Instalira taj novi certifikat u lozinki, kao što je prikazano u nastavku:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] Naziv u certifikata mogu se razlikovati, ali će biti mjestu s **razvoj Apple iOS automatske usluge:**.

10. U programu Access spremišta lozinki, desnom tipkom miša kliknite novi automatske certifikat koji ste upravo stvorili u kategoriji **certifikata** . Kliknite **Izvoz**, dajte naziv datoteci, odaberite oblik **.p12** i zatim kliknite **Spremi**.

    Imajte na umu naziv datoteke i mjesto izvezene .p12 slanje certifikata. Će se koristiti za provjeru autentičnosti s APN-ove tako da prenesete na portalu klasični Azure.



####<a name="create-a-provisioning-profile-for-the-app"></a>Stvaranje profila za dodjelu resursa za aplikaciju

1. Vratite se u <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning portala</a>odaberite **Profila za dodjelu resursa**, odaberite **sve**, a zatim na **+** gumb za stvaranje novog profila. Time se pokreće čarobnjak za **Dodavanje iOS Provisiong profila**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Odaberite **iOS razvoja aplikacija** u **razvoju** kao vrstu profila provisiong pa kliknite **Nastavi**.


3. Nakon toga odaberite aplikaciju ID-a koji ste upravo stvorili s padajućeg popisa **ID aplikacije** pa kliknite **Nastavi**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. Na zaslonu **potvrde** odabrali razvoj certifikat za potpisivanje koda, a zatim kliknite **Nastavi**. Ovo je potpisnog certifikata ne automatske certifikat koji ste upravo stvorili.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Nakon toga odaberite **uređaji** za testiranje pa kliknite **Nastavi**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Naposljetku, odaberite naziv profila u **Naziv profila**, kliknite **Generiraj**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
