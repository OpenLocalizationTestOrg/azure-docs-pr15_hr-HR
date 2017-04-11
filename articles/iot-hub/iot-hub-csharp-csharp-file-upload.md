<properties
    pageTitle="Prijenos datoteka s uređaja pomoću IoT koncentrator | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste saznali kako prenijeti datoteke s uređaja pomoću Azure IoT koncentrator C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Praktični vodič: Kako prenijeti datoteke s uređaja s oblakom pomoću IoT koncentratora

## <a name="introduction"></a>Uvod

Azure IoT koncentrator je potpuno Upravljani servis koji omogućuje pouzdanog i sigurne dvosmjerni komunikaciju između milijune IoT uređaji i aplikacije sigurnosno završetka. Prethodni vodiče ([Početak rada s IoT koncentratora] i [slanje poruka oblak na uređaj s IoT koncentrator]) ilustrirali osnovni uređaj-na-oblaka i oblaka uređaj razmjenu funkcionalnost IoT koncentratora i vodič [postupak uređaj-na-oblaka poruke] u članku se opisuje način pouzdano pohranjivati poruke uređaj u oblak u spremište blobova platforme Azure. Međutim, u nekim slučajevima ne možete jednostavno mapiranja podataka uređajima Pošalji u razmjerno malo uređaj u oblak poruke koje prihvaća IoT koncentratora. Primjeri velikih datoteka koja sadrži slike, videozapise, vibracije podataka uzorkovanja pri velika učestalost ili sadrže neki oblik skupljaju podataka. Te su datoteke obično obradu obrade u oblak pomoću alata kao što su [Tvorničke Azure podataka] ili [Hadoop] stog. Kad Preferirani slanju događaje prijenosima datoteka s uređaja, i dalje možete koristiti IoT koncentrator sigurnosti i pouzdanosti funkcije.

Pomoću ovog praktičnog vodiča sastavlja na kod u vodiču [slanje poruka oblak na uređaj s IoT koncentrator] da bi se prikazala kako koristiti mogućnosti prijenos datoteka IoT koncentratora. Pokazuje kako omogućuju sigurno uređaj programa Azure bloba URI-JA za prijenos datoteka i kako koristiti prijenosima datoteka IoT koncentrator za pokretanje obrade datoteke u pozadinskih vaše aplikacije.

Na kraju ovog praktičnog vodiča pokrenite dvije aplikacije konzole za Windows:

* **SimulatedDevice**, izmijenjenu verziju aplikacije stvorene u vodič [slanje poruka oblak na uređaj s IoT koncentrator] , koji se prenese datoteka pomoću SAS URI nudi koncentratora za IoT prostora za pohranu.
* **ReadFileUploadNotification**, koja prima datoteku prijenosima iz koncentratora za IoT.

> [AZURE.NOTE] Koncentrator IoT podržava mnoge uređaj platforme i jezika (uključujući C, Java i Javascript) putem uređaja Azure IoT SDK-ovi. Pogledajte [Razvojni centar za Azure IoT] korak po korak upute o povezivanju uređaja kôda prikazanog ovog praktičnog vodiča i obično Azure IoT koncentratora.

Da biste dovršili ovaj Praktični vodič, potrebne su vam sljedeće:

+ Microsoft Visual Studio 2015,

+ Aktivni Azure račun. (Ako nemate račun, možete stvoriti [pomoću računa] [ lnk-free-trial] u samo nekoliko minuta.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Povezivanje poslovnog prostora za pohranu Azure IoT koncentrator

Jer Simulirani uređaj prenese datoteka blob, morate imati račun [Za pohranu Azure] povezane IoT koncentrator. Kada povežete račun za pohranu Azure s koncentratora za IoT, središtu mogu uzrokovati SAS URI koji uređaj omogućuju sigurno spremniku blob prijenos datoteke. Servis IoT koncentratora i uređaja SDK-ovi koordiniranje postupak koji stvara SAS URI i postaje dostupan za uređaj da biste koristiti da biste prenijeli datoteku.

Slijedite upute u [datoteku Konfiguriraj prenosi pomoću portala za Azure] [ lnk-configure-upload] povezivati s računom za pohranu Azure koncentratora za IoT.

## <a name="upload-a-file-from-a-simulated-device"></a>Prijenos datoteke s Simulirani uređaja

U ovom ćete odjeljku izmjena Simulirani uređaj aplikaciju koju ste stvorili u oblak uređaj poruka iz koncentratora IoT [slanje poruka oblak na uređaj s IoT koncentratora] .

1. U Visual Studio, desnom tipkom miša kliknite **SimulatedDevice** projekta, kliknite **Dodaj**, a zatim kliknite **Postojeću stavku**. Dođite do slikovne datoteke i uključiti ga u projektu. Pomoću ovog praktičnog vodiča pretpostavlja pod nazivom sliku `image.jpg`.

2. Desnom tipkom miša kliknite sliku, a zatim kliknite **Svojstva**. Provjerite je li je **kopirajte izlaz direktorij** postavljena na **uvijek Kopiraj**.

    ![][1]

3. U datoteci **Program.cs** dodajte sljedeće naredbe pri vrhu datoteke:

        using System.IO;

4. Dodajte na sljedeći način predmete **Program** :
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    Na `UploadToBlobAsync` postupak vodi u datoteci naziv i strujanje izvor datoteke je moguće prenijeti i rukuje prijenos prostora za pohranu. Aplikacija konzole prikazuje vrijeme potrebno za prijenos datoteka.

5. Dodajte sljedeće metoda u način **glavne** desno prije nego na `Console.ReadLine()` redak:

        SendToBlobAsync();

> [AZURE.NOTE] Za sake jednostavnosti, pomoću ovog praktičnog vodiča implementirati svih pravila Ponovi. U kodu radnog trebali biste provesti pravila Ponovi (kao što su eksponencijalne backoff), kao što je predloženo u članku MSDN [Zadužen za tranzitne kvara].

## <a name="receive-a-file-upload-notification"></a>Primanje obavijesti za prijenos datoteka

U ovom ćete odjeljku pišete aplikacije konzole za Windows koja prima poruke obavijesti za prijenos datoteka iz koncentratora IoT.

1. U trenutnom rješenju Visual Studio stvorite novi projekt Visual C# Windows pomoću predloška **Aplikacije konzole za** projekt. Naziv projekta **ReadFileUploadNotification**.

    ![Novi projekt u Visual Studio][2]

2. U pregledniku rješenja, desnom tipkom miša kliknite **ReadFileUploadNotification** projekta, a zatim **Upravljanje NuGet paketa**.

    Prikazat će se prozor za upravljanje NuGet paketa.

2. Traženje `Microsoft.Azure.Devices`, kliknite **Instaliraj**i prihvatite uvjete korištenja. 

    To preuzimanja, instalira i dodaje referenca [Azure IoT - paket servisa SDK NuGet] u programu project **ReadFileUploadNotification** .

3. U datoteci **Program.cs** dodajte sljedeće naredbe pri vrhu datoteke:

        using Microsoft.Azure.Devices;

4. Dodajte sljedeća polja u **Program** predmet. Zamijenite vrijednost rezervirano mjesto s nizom za povezivanje koncentrator IoT iz [Početak rada s IoT koncentratora]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Dodajte na sljedeći način predmete **Program** :
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Imajte na umu da uzorak primanje isti onog koji je korišten primati poruke oblaka uređaja iz aplikacije za uređaj.

6. Na kraju, dodajte sljedeće retke **Glavna** načina:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Pokretanje aplikacije

Sada ste spremni za pokretanje aplikacije.

1. U Visual Studio, desnom tipkom miša kliknite rješenje i odaberite **Postavljanje pokretanja projekata**. Odaberite **više projekata pokretanje**, a zatim odaberite akciju **pokrenuti** za **ReadFileUploadNotification** i **SimulatedDevice**.

2. Pritisnite **F5**. Obje aplikacije trebala. Trebali biste vidjeti prijenos u jedan konzole aplikacija i prijenos poruke s obavijesti prima drugih aplikacija konzolu. Možete koristiti [Azure portal] ili Visual Studio poslužitelja Explorer da biste provjerili prisutnosti prenesenih datoteka na vašem računu Azure prostora za pohranu.

  ![][50]


## <a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču naučili kako pod utjecajem datoteke prijenos mogućnosti koncentrator IoT da biste pojednostavnili prijenosima datoteka s uređaja. Možete i dalje Istražite IoT koncentrator značajke i scenariji s u sljedećim člancima:

- [Stvaranje koncentratora za IoT programski][lnk-create-hub]
- [Uvod u C SDK][lnk-c-sdk]
- [Koncentrator IoT SDK-ovi][lnk-sdks]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Portal za Azure]: https://portal.azure.com/

[Tvorničke Azure podataka]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Slanje poruka oblak na uređaj s IoT koncentratora]: iot-hub-csharp-csharp-c2d.md
[Obrada poruke uređaj u oblak]: iot-hub-csharp-csharp-process-d2c.md
[Početak rada s IoT koncentratora]: iot-hub-csharp-csharp-getstarted.md
[Razvojni centar za Azure IoT]: http://www.azure.com/develop/iot

[Rukovanje tranzitne kvara]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure prostora za pohranu]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - paket servisa SDK NuGet]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


