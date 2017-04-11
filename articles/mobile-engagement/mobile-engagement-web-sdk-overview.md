<properties
    pageTitle="Azure mobilne radnje Web SDK pregled | Microsoft Azure"
    description="Najnovija ažuriranja i postupci Web SDK za Azure Mobile radnje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure mobilne radnje Web SDK

Dodatne informacije o integraciji Azure Mobile radnje u web-aplikaciji, počnite ovdje. Ako želite isprobajte sami prije početak rada sa web aplikacije, potražite u članku naš [15-minutni vodič](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Integracija postupaka
1. Saznajte [kako integrirati Mobile radnje u web-aplikaciji](mobile-engagement-web-integrate-engagement.md).

2. Planiranje implementacije oznaka Saznajte [kako koristiti dodatne radnje Mobile označavanje API-JA u web-aplikaciji](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Napomene

### <a name="202-10182016"></a>2.0.2 (18/10/2016)

-   FIXED pad na InPrivate (Safari).
-   FIXED rušenje u preglednicima s kolačići onemogućene.

Sve verzije potražite [Dovršeno napomene](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Nadogradnja postupaka

### <a name="upgrade-from-121-to-200"></a>Nadogradnja s 1.2.1 2.0.0

U sljedećim odjeljcima opisuju kako migrirati Mobile radnje Web SDK integraciju sa servisom Capptain nudi Capptain SAS radnje Azure mobilne aplikacije. Ako premještate iz verzije starije od 1.2.1, obratite se Capptain web-mjesta da biste migrirali 1.2.1 prvi put, a zatim primijenite sljedećih postupaka.

Ova verzija Mobile radnje Web ne podržava Samsung pametne TV, Opera TV, webOS ili značajku razgovor.

>[AZURE.IMPORTANT] Web-mjesto Capptain i u okvir za Azure Mobile radnje nisu isti servisa i sljedećih postupaka isticanje kako će se migrirati aplikaciju klijenta. Migracija SDK Web Mobile radnje u aplikaciji će nije moguće migrirati podatke s poslužitelja Capptain na poslužitelju Mobile radnje.

#### <a name="javascript-files"></a>JavaScript datoteke

Zamjena datoteke capptain-sdk.js datoteke azure-engagement.js, a zatim sukladno tome ažurirajte vaše skripte uvozi.

#### <a name="remove-capptain-reach"></a>Uklanjanje Capptain razgovor

Ova verzija Mobile radnje Web ne podržava značajku razgovor. Ako ste integriran Capptain razgovor u aplikaciji, morate ga ukloniti.

Uklonite uvoz dosegne CSS sa stranice i brisanje povezanih .css datoteke (capptain-reach.css, po zadanom).

Brisanje u sljedećim resursima za razgovor: Zatvori slika (capptain-close.png, po zadanom) i ikona robne marke (capptain--ikona obavijesti, po zadanom).

Uklanjanje dosegne korisničko Sučelje za obavijesti u aplikaciji. Zadani izgled izgleda ovako:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Uklanjanje dosegne korisničko Sučelje za tekst i objave web i ankete. Zadani izgled izgleda ovako:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Uklanjanje na `reach` objekt s konfiguracijom, ako postoji. Izgleda ovako:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Uklonite sve ostale razgovor prilagodbu, kao što su kategorije.

#### <a name="remove-deprecated-apis"></a>Uklanjanje zastarjeli API-ji

Neki API-ji iz Capptain izostavljen je iz SDK Web Mobile radnje.

Uklonite sve pozive na sljedeće API-ji: `agent.connect`, `agent.disconnect`, `agent.pause`, i `agent.sendMessageToDevice`.

Uklanjanje bilo koji od sljedećih pozive s konfiguracijom Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, i `onPushMessageReceived`.

#### <a name="configuration"></a>Konfiguracija

Korištenje mobilne koristi niz za povezivanje da biste konfigurirali SDK identifikatora, na primjer, identifikator aplikacije.

ID aplikacije zamijenite niz za povezivanje. Imajte na umu da globalni objekt za konfiguraciju SDK mijenja iz `capptain` da biste `azureEngagement`.

Prije migracije:

    window.capptain = {
      appId: ...,
      [...]
    };

Nakon migracije:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Niz za povezivanje za svoju aplikaciju prikazuje se na portalu za Azure.

#### <a name="javascript-apis"></a>JavaScript API-ji

Globalni objekt JavaScript `window.capptain` preimenovana `window.azureEngagement`, ali možete koristiti u `window.engagement` pseudonim za pozive API-JA. Da biste definirali konfiguracije SDK nećete moći koristiti taj pseudonim.

Ako, primjerice, `capptain.deviceId` postaje `engagement.deviceId`, `capptain.agent.startActivity` postaje `engagement.agent.startActivity`i tako dalje.

Ako ste već integrirali starijoj verziji programa Azure SDK za mobilne radnje Web u aplikaciji, pročitajte o [nadogradnje postupke](mobile-engagement-web-upgrade-procedure.md).
