####<a name="configuring-the-ios-project-in-xamarin-studio"></a>Konfiguriranje iOS projekta u Xamarin Studio

1. Xamarin.Studio, otvorite **Info.plist**i ažurirajte **Paket identifikator** ID paket koji koju ste ranije stvorili koristeći novi ID aplikacije.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. Pomaknite se do odjeljka **Načini pozadine** i potvrdite okvire **Omogućivanje načina rada pozadine** i **Udaljena obavijesti** . 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Dvokliknite projekta na ploči rješenja da biste otvorili **Mogućnosti projekta**.

4.  Odaberite **iOS paket potpisa** u odjeljku **Sastavljanje**i odaberite odgovarajuće **identiteta** i **Provisioning profila** imali samo postavljanje i za taj projekt. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Time se osigurava da projekt koristi novi profil za potpisivanje koda. Službeni Xamarin uređaj dokumentaciju za dodjelu resursa, potražite u članku [Dodjeljivanje uređaj Xamarin].

####<a name="configuring-the-ios-project-in-visual-studio"></a>Konfiguriranje iOS projekta u Visual Studio

1. U Visual Studio, desnom tipkom miša kliknite projekt, a zatim kliknite **Svojstva**.

2. Na stranicama svojstava kliknite karticu **iOS aplikacije** i ažuriranje **identifikator** ID koji ste prethodno stvorili.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Na kartici **iOS paket prijave** odaberite odgovarajuće **identiteta** i **Provisioning profila** imali samo postavljanje i za taj projekt. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Time se osigurava da projekt koristi novi profil za potpisivanje koda. Službeni Xamarin uređaj dokumentaciju za dodjelu resursa, potražite u članku [Dodjeljivanje uređaj Xamarin].

4. Dvokliknite Info.plist da biste ga otvorili, a zatim omogućiti **RemoteNotifications** u odjeljku načini pozadine. 



[Uređaj Xamarin dodjele resursa]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/