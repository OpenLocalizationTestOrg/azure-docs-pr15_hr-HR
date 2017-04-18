## <a name="create-a-device-management-enabled-iot-hub"></a>Stvaranje uređaja upravljanje omogućeno IoT koncentrator

Budući da je upravljanje uređajima IoT koncentratora u pretpregledu, potrebnih za stvaranje IoT koncentrator omogućiti upravljanje uređaja. Kad upravljanje uređajima IoT koncentrator dostigne Općenito dostupan, ažurirat će se ovog praktičnog vodiča. Sljedeći koraci vam pokazati kako obaviti taj zadatak pomoću portala za Azure.

1.  Prijavite se na [portal za Azure].
2.  U Jumpbar, kliknite **Novo**, a zatim kliknite **Internetske mogućnosti**pa kliknite **Azure IoT koncentratora**.

    ![][img-new-hub]

3.  U plohu **IoT koncentrator** odaberite konfiguraciju za koncentratora za IoT.

    ![][img-configure-hub]

  -   U okvir **naziv** unesite naziv koncentratora za IoT. Ako je **naziv** valjanih i dostupnih, u okvir **naziv** pojavit će se Zelena kvačica.
  -   Odaberite **sloj određivanje cijena i promjena veličine**. Pomoću ovog praktičnog vodiča zahtijevaju određene sloju.
  -   U **grupi resursa**, stvorite novu grupu resursa ili odaberite postojeći. Dodatne informacije potražite u članku [Korištenje grupa resursa za upravljanje Azure resurse].
  -   Potvrdite okvir **Omogući Device**Management - PRETPREGLED.
  -   **Mjesto**, odaberite mjesto za hostiranje vaše IoT središte. Upravljanje uređajima koncentrator IoT dostupna je samo u SAD-a za istočnoazijske, Sjeverna Europa i istočnoazijski tijekom javno pretpregleda.

    > [AZURE.NOTE]Ako ne potvrdite okvir **Omogući Device**Management, uzorcima ne funkcioniraju.<br/>Potvrđivanjem **Omogući upravljanje uređajima**stvorite pretpregled koncentrator IoT podržani samo u SAD-a za istočnoazijske, Sjeverna Europa i istočnoazijski i nije namijenjen radni scenariji. Uređaji nije moguće migrirati u i iz koncentratora omogućiti upravljanje uređaja.

4.  Nakon što ste odabrali koncentratora za IoT mogućnosti konfiguracije, kliknite **Stvori**. To može potrajati nekoliko minuta za Azure da biste stvorili vaše IoT središte. Da biste provjerili status, možete pratiti napredak **Startboard** ili na ploči **obavijesti** .

    ![][img-monitor]

5.  Kada središtu IoT uspješno je stvorena, plohu za vaše središte će automatski otvoriti. Zabilježite **naziv glavnog računala**, a zatim kliknite **pravila za zajedničko korištenje programa access**.

    ![][img-keys]

6.  Kliknite pravila **iothubowner** , a zatim kopirajte i zabilježite niz za povezivanje plohu **iothubowner** . Kopirajte ga na mjesto možete pristupiti kasnije jer je potrebno za dovršetak ovog praktičnog vodiča.

    > [AZURE.NOTE] U slučajevima radnog provjerite ne smije pomoću vjerodajnica **iothubowner** .

    ![][img-connection]

Sada ste stvorili na uređaju upravljanje omogućeno IoT koncentratora. Potrebno je niz za povezivanje za dovršetak ovog praktičnog vodiča.

## <a name="create-a-device-identity"></a>Stvaranje uređaja identiteta

U ovom odjeljku koristite alat za čvor naziva [IoT koncentrator Explorer] [ iot-hub-explorer] da biste stvorili uređaj identiteta za ovog praktičnog vodiča.

1. Pokrenite sljedeće u okruženju sustava naredbenog retka:

    Instalacija npm -giothub-explorer@latest

2. Zatim, pokrenite sljedeću naredbu da biste se prijavili koncentrator, plaćanju zamjenu `{service connection string}` s nizom za povezivanje IoT koncentratora koje ste prethodno kopirali:

    Prijava iothub explorer "{servisa niz za povezivanje}"

3. Na kraju, stvorite novi uređaj identitet pod nazivom `myDeviceId` pomoću naredbe:

    iothub explorer stvaranje myDeviceId – niz za povezivanje

Zabilježite niz za povezivanje uređaja iz rezultata. Niz za povezivanje koristi aplikaciju uređaj za povezivanje s koncentratora za IoT kao uređaj.

![][img-identity]

Pogledajte [Uvod u rad s IoT koncentrator] [ lnk-getstarted] je način programski Stvori identitete uređaja.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Portal za Azure]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Upravljanje resursi za Azure pomoću grupa resursa]: ../articles/azure-portal/resource-group-portal.md
