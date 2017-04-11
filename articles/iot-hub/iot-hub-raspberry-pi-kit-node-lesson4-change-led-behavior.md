<properties
 pageTitle="Neobavezna sekcija – promjena i isključenog ponašanje u LED | Microsoft Azure"
 description="Prilagođavanje poruke da biste promijenili u LED na Uključivanje i isključivanje ponašanje."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 neobavezne sekcije: promjena i isključenog ponašanje u LED

## <a name="421-what-you-will-do"></a>4.2.1 što ćete učiniti

Prilagođavanje poruke da biste promijenili u LED na Uključivanje i isključivanje ponašanje. Ako zadovoljavate eventualne probleme, traženje rješenja [stranice za otklanjanje poteškoća](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2 što ćete saznati

Koristite dodatne funkcije Node.js da biste promijenili u LED na Uključivanje i isključivanje ponašanje.

## <a name="423-what-you-need"></a>4.2.3 što vam je potrebno

Morate uspješno ste dovršili [4.1 pokretanje aplikacije za uzorak na vaše Raspberry Pi prima oblak na uređaju poruke](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 dodavanje Node.js funkcija

1. Otvorite aplikaciju uzorak u kodu Visual Studio ponovnim pokretanjem sljedeće naredbe:

    ```bash
    cd Lesson4
    code .
    ```

2. Otvaranje u `app.js` datoteku, a zatim na kraju dodajte sljedeće funkcije:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![Ažuriranje app.js](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Dodavanje sljedeći uvjeti prije zadana u Bloku za promjenu velikih od na `receiveMessageCallback` funkcija:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Sada ste konfigurirali primjer aplikacije da biste odgovorili na dodatne upute putem poruka. Upute za "na" uključuje u LED i uputa za "isključivanje" isključuje se LED.

4. Otvorite datoteku gulpfile.js, a zatim dodajte nove funkcije prije funkciju `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![Ažuriranje gulpfile](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. U na `sendMessage` funkcije, zamijeniti redak `var message = buildMessage(sentMessageCount);` s novi redak prikazano u sljedećim isječak:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Spremite sve promjene.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 uvođenje i pokrenuti aplikaciju uzorka

Implementacija i pokrenuti aplikaciju uzorka vaše Pi ponovnim pokretanjem sljedeće naredbe:

```bash
gulp
```

Trebali biste vidjeti LED uključena u dvije sekunde, a zatim isključeno za drugi dvije sekunde. Zadnji poruku "stop" zaustavlja izvođenje aplikacije uzorka.

![Uključivanje i isključivanje](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Čestitamo! Uspješno ste prilagodili poruke koje se šalju vrijednost Pi iz centra za IoT.

### <a name="427-summary"></a>4.2.7 sažetak

U ovom se odjeljku neobavezno demos kako prilagoditi poruke tako da se primjer aplikacije možete kontrolirati i isključenog ponašanje u LED na drugačiji način.

