<properties
    pageTitle="Azure postupke za nadogradnju Mobile radnje Web SDK | Microsoft Azure"
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
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile radnje Web SDK nadogradnje postupaka

Ako ste već integriran starijoj verziji programa Azure Mobile radnje Web SDK-a u web-aplikaciju, morate Imajte na umu sljedeće prilikom nadogradnje SDK-a.

Ako je preskočeno više verzija SDK Web radnje Mobile, možda ćete morati dovršiti nekoliko postupaka tijekom postupka nadogradnje. Ako, na primjer, ako migrirati iz 1.4.0 u 1.6.0, najprije slijedite postupke za nadogradnju s 1.4.0 1.5.0. Zatim slijedite postupke u da biste nadogradili 1.5.0 1.6.0.

Bez obzira verziju ste nadogradili zamijenite sve starije verzije programa azure datoteke-engagement.js najnoviju verziju datoteke.

## <a name="upgrade-from-121-to-200"></a>Nadogradnja s 1.2.1 2.0.0

U ovom se odjeljku opisuje kako migrirati Mobile radnje Web SDK integraciju sa servisom Capptain nudi Capptain SAS radnje Azure mobilne aplikacije. Ako premještate iz starije verzije, obratite se web-mjesto Capptain najprije migriranja u 1.2.1, a zatim primijenite sljedećih postupaka.

Ova verzija Mobile radnje Web ne podržava Samsung pametne TV, Opera TV, webOS ili značajku razgovor.

>[AZURE.IMPORTANT] Web-mjesto Capptain i u okvir za Azure Mobile radnje nisu isti servisa. Sljedeći postupak ističe samo kako će se migrirati aplikaciju klijenta. Migracija SDK Web Mobile radnje u aplikaciji će nije moguće migrirati podatke s poslužitelja Capptain na poslužitelju Mobile radnje.

### <a name="javascript-files"></a>JavaScript datoteke

Zamjena datoteke capptain-sdk.js datoteke azure-engagement.js, a zatim sukladno tome ažurirajte vaše skripte uvozi.

### <a name="remove-capptain-reach"></a>Uklanjanje Capptain razgovor

Ova verzija Mobile radnje Web ne podržava značajku razgovor. Ako je integriran Capptain razgovor u aplikaciji, morate ga ukloniti.

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

### <a name="remove-deprecated-apis"></a>Uklanjanje zastarjeli API-ji

Neki API-ji iz Capptain izostavljen je iz SDK Web Mobile radnje.

Uklonite sve pozive na sljedeće API-ji: `agent.connect`, `agent.disconnect`, `agent.pause`, i `agent.sendMessageToDevice`.

Uklonite sve instance sljedeće pozive iz konfiguraciju Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, i `onPushMessageReceived`.

### <a name="configuration"></a>Konfiguracija

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

### <a name="javascript-apis"></a>JavaScript API-ji

Globalni objekt JavaScript `window.capptain` preimenovana `window.azureEngagement` , ali možete koristiti u `window.engagement` pseudonim za pozive API-JA. Pseudonim ne možete koristiti da biste definirali SDK konfiguracije.

Ako, primjerice, `capptain.deviceId` postaje `engagement.deviceId`, `capptain.agent.startActivity` postaje `engagement.agent.startActivity`i tako dalje.
