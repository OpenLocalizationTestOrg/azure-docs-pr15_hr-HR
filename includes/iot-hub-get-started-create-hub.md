## <a name="create-an-iot-hub"></a>Stvaranje koncentratora za IoT

Stvaranje koncentratora za IoT za Simulirani uređaj za povezivanje s. Sljedeći koraci pokazati kako obaviti taj zadatak pomoću portala za Azure.

1. Prijava na [portal za Azure][lnk-portal].

2. U Jumpbar, kliknite **Novo** > **Internet stvari** > **Azure IoT koncentrator**.

    ![Portal za Azure Jumpbar][1]

3. U plohu **IoT koncentrator** odaberite konfiguraciju za koncentratora za IoT.

    ![Plohu IoT koncentratora][2]

    * U okvir **naziv** unesite naziv koncentratora za IoT. Ako je **naziv** valjanih i dostupnih, u okvir **naziv** pojavit će se Zelena kvačica.
    * Odaberite [sloj cijene i skaliranje][lnk-pricing]. Pomoću ovog praktičnog vodiča zahtijevaju određene sloju. Za ovaj vodič pomoću besplatnog sloju F1.
    * U **grupi resursa**, stvorite novu grupu resursa ili odaberite postojeći. Dodatne informacije potražite u članku [Korištenje resursa grupe da biste upravljali Azure resursa][lnk-resource-groups].
    * **Mjesto**, odaberite mjesto za hostiranje vaše IoT središte. U ovom ćete praktičnom vodiču odaberite najbliže mjesto.

4. Nakon što ste odabrali koncentratora za IoT mogućnosti konfiguracije, kliknite **Stvori**.  To može potrajati nekoliko minuta za Azure da biste stvorili koncentratora za IoT. Da biste provjerili status, možete pratiti napredak u Startboard ili na ploči obavijesti.

    ![Novi status IoT koncentratora][3]

5. Kada središtu IoT uspješno je stvorena, kliknite pločicu novi za vaše IoT središte na portalu Azure da biste otvorili plohu za novi koncentrator IoT. Zabilježite **naziv glavnog računala**, a zatim kliknite **pravila za zajedničko korištenje programa access**.

    ![Novi plohu IoT koncentratora][4]

6. U plohu **pravilnike za pristup zajednički se koristi** kliknite pravila **iothubowner** i kopirati i zabilježite niz za povezivanje plohu **iothubowner** . Dodatne informacije potražite u članku [Upravljanje pristupom] [ lnk-access-control] u "Azure IoT koncentrator za razvojne inženjere vodiču."

    ![Zajednički pristup pravila plohu][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
