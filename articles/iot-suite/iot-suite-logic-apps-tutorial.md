<properties
  pageTitle="Paket Azure IoT i logike aplikacije | Microsoft Azure"
  description="Praktični vodič za priključiti logike aplikacija u paketu IoT Azure poslovni proces."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Praktični vodič: Povezivanje logike aplikacije rješenje Azure IoT paket udaljene nadzor unaprijed konfigurirane

[Paket programa Microsoft Azure IoT] [ lnk-internetofthings] udaljene konfiguriranog rješenja za nadzor sjajan je način da biste brzo započeli skupa završetka do kraja značajke koje exemplifies rješenja programa IoT. Pomoću ovog praktičnog vodiča vodit će vas kroz kako dodati logiku aplikacije sustava Microsoft Azure IoT paket udaljene konfiguriranog rješenja za nadzor. Ove koraci objašnjavaju kako možete poduzeti rješenje IoT još više povezivanjem poslovni proces.

_Ako tražite vodič za dodjeljivanje udaljenom nadzor unaprijed konfigurirane rješenja, pročitajte članak [Praktični vodič: početak rada s rješenja IoT unaprijed konfigurirane][lnk-getstarted]._

Prije početka ovog praktičnog vodiča, trebali biste:

- Dodjela resursa za udaljenom nadzor unaprijed konfigurirane rješenja za Azure pretplatu.

- Stvaranje računa SendGrid da biste omogućili slanje e-pošte koji se pokreće poslovni proces. Možete se prijaviti za besplatnu probnu račun pri [SendGrid](https://sendgrid.com/) tako da kliknete **pokušajte besplatno**. Nakon što ste se registrirali za besplatnu probnu račun, potrebnih za stvaranje [ključ za API-JA](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) u SendGrid koji daje dozvole za slanje pošte. Morate ovaj ključ za API kasnije u ovom praktičnom vodiču.

Ako ste već dodjeli udaljene nadzor unaprijed konfigurirane rješenja, idite u grupu resursa tog rješenja [Azure portal][lnk-azureportal]. Grupa resursa ima isti naziv kao naziv rješenja koje ste odabrali pri dodjeli udaljene nadzora rješenje. U grupi resursa možete vidjeti svih resursa dodijeljenu Azure za rješenje osim Azure Active Directory aplikacije koje možete pronaći na portalu klasični Azure. Sljedeće snimka zaslona prikazuje se primjer **grupa resursa** plohu udaljene nadzora konfiguriranog rješenja:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Da biste započeli, postavite aplikaciju logike za uporabu konfiguriranog rješenja.

## <a name="set-up-the-logic-app"></a>Postavljanje aplikacije logike

1. Kliknite __Dodaj__ pri vrhu vaše plohu grupu resursa na portalu za Azure.

2. Traženje __Logike aplikacije__, odaberite ga, a zatim kliknite **Stvori**.

3. Unesite __naziv__ i koristite istu **pretplatu** i **grupa resursa** koje ste koristili prilikom dodjeli udaljene nadzora rješenje. Kliknite __Stvori__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Kada se dovrši implementaciju sustava, vidjet ćete aplikaciju logike prikazuju u obliku resursa u grupu resursa.

5. Kliknite aplikaciju logike dođite do plohu logike aplikacije, odaberite predložak **Prazne logike aplikacije** da biste otvorili **Dizajner logike aplikacije**.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Odaberite __zahtjev__. Ova akcija određuje dolazni zahtjev HTTP s određenim JSON oblikovani tereta činovi kao okidač.

7. Zahtjev za tijelo JSON shemi zalijepite sljedeće:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] URL za HTTP post možete kopirati nakon što spremite logike aplikacije, ali najprije morate dodati akciju.

8. Kliknite __+ Novi korak__ u odjeljku okidač za ručno. Zatim kliknite **Dodaj akciju**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Traženje **SendGrid – slanje e-pošte** i kliknite je.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Unesite naziv veze, kao što su **SendGridConnection**, unesite **Ključ za API SendGrid** koji ste napravili kad je postavila račun SendGrid i kliknite **Stvori**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. Dodavanje adrese e-pošte koje ste vlasnik i **od** i **do** polja. Da biste polje **Subject** dodajte **udaljenom nadzor upozorenje [DeviceId]** . U polje **Tijela e-pošte** dodajte **uređaj [DeviceId] prijavio [measurementName] s vrijednošću [measuredValue].** Klikom na u odjeljku **možete umetnuti podatke iz prethodnih koraka** možete dodati **[DeviceId]**, **[measurementName]**i **[measuredValue]** .

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. Kliknite __Spremi__ u gornji izbornik.

13. Kliknite **zahtjev za** pokretanje i kopirajte vrijednost __Http Post na ovaj URL__ . U nastavku ovog praktičnog vodiča trebate ovaj URL.

> [AZURE.NOTE] Logika aplikacijama omogućuju vam da biste pokrenuli [različite vrste akcija] [ lnk-logic-apps-actions] uključujući akcije u sustavu Office 365. 

## <a name="set-up-the-eventprocessor-web-job"></a>Postavljanje Web posao EventProcessor

U ovom ćete odjeljku povezati konfiguriranog rješenje logike aplikaciju koju ste stvorili. Da biste dovršili ovaj zadatak, dodajte URL-a da biste pokrenuli aplikaciju logike da biste akciju koja se aktivira se kada je vrijednost senzor uređaj premašuje praga.

1. Pomoću klijent brojka Kloniraj najnoviju verziju sustava [azure iot – alat za analizu daljinske-nadgledanje github spremište][lnk-rmgithub]. Ako, na primjer:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. U Visual Studio, otvorite __RemoteMonitoring.sln__ iz lokalne kopije u spremištu.

3. Otvorite datoteku __ActionRepository.cs__ u na **infrastrukture\\spremište** mapu.

4. Ažurirajte rječnika **actionIds** __Http Post na ovom URL__ koji ste zabilježili iz aplikacije programa logike na sljedeći način:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Spremiti promjene u rješenje, a zatim zatvorite Visual Studio.

## <a name="deploy-from-the-command-line"></a>Implementacija iz naredbenog retka

U ovom ćete odjeljku implementirati ažuriranu verziju udaljene nadzora rješenja da biste zamijenili trenutno pokrenuto u Azure verziju.

1. Praćenje [razvojni postaviti] [ lnk-devsetup] upute za postavljanje okruženja za implementaciju.

2.  Da biste implementirali lokalno, slijedite [lokalnoj implementaciji] [ lnk-localdeploy] upute.

3.  Implementacija u oblak i ažuriranje postojećih uvođenja oblaka, slijedite [oblaka implementaciju] [ lnk-clouddeploy] upute. Nazovite implementaciju sustava izvorni naziv implementacije. Za primjer ako izvorni implementacije je pozvan **demologicapp**, koristite sljedeću naredbu:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Kada se pokrene skripte Sastavi, obavezno koristite isti račun za Azure, pretplatu, regija i instanca servisa Active Directory koji ste koristili kada dodjeli rješenja.

## <a name="see-your-logic-app-in-action"></a>U odjeljku logika aplikacije u akciji

Udaljena nadzora konfiguriranog rješenja ima dva pravila postavljanje prema zadanim postavkama prilikom dodjele resursa rješenje. Na uređaju **SampleDevice001** su se oba pravila:

* Temperatura > 38.00
* Humidity > 48.00

Pravilo temperatura pokrene **Potenciranje alarma** akciju, a zatim pravilo Humidity pokrene **Otprema** akciju. Pod pretpostavkom da koristi iste URL ADRESE za obje akcije klase **ActionRepository** , logike aplikacije pokrene ili pravilo. Oba pravila pomoću SendGrid da biste poslali poruku e-pošte s adrese **Da biste** s pojedinostima upozorenja.

> [AZURE.NOTE] Aplikaciju logike nastavlja se pokrenuti svaki put kada se ispuni praga. Da biste izbjegli nepotrebne poruke e-pošte, možete onemogućiti pravila portalu za rješenja ili onemogućiti aplikaciju logike [Azure portal][lnk-azureportal].

Osim primati poruke e-pošte, možete vidjeti kada se pokrene aplikaciju logike na portalu:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste iskoristili logike aplikacije povezati konfiguriranog rješenje poslovni proces, možete saznati više o mogućnostima za prilagodbu konfiguriranog rješenja:

- [Korištenje dinamičke telemetrijskih s udaljenog nadzor unaprijed konfigurirane rješenja][lnk-dynamic]
- [Uređaj informacije metapodataka u udaljene nadzor unaprijed konfigurirane rješenja][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
