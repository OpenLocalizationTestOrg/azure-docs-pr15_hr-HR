<properties
    pageTitle="Konfiguriranje prilagođenog naziva domene za web-aplikaciju u aplikacije servisa Azure koji koristi Upravitelj promet za opterećenja."
    description="Koristite naziv prilagođene domene u web-aplikacijama u aplikacije servisa Azure koja obuhvaća Upravitelj promet za opterećenja."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfiguriranje prilagođenog naziva domene za web-aplikacije u Azure aplikacije servisa za pomoću upravitelja promet

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Ovaj članak sadrži generički upute za korištenje prilagođenog naziva domene za aplikacije servisa za Azure pomoću upravitelja promet za opterećenja.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Objašnjenje DNS zapisa

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Konfiguriranje web aplikacije za standardnom načinu rada

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Dodavanje DNS zapisa za prilagođenu domenu

> [AZURE.NOTE] Ako ste kupili domenu kroz Azure aplikacije servisa web-aplikacije preskočite korake u nastavku i pogledajte završnom koraku članka [Kupiti domenu za web-aplikacije](custom-dns-web-site-buydomains-web-app.md) .

Želite pridružiti web app u aplikacije servisa za Azure prilagođene domene, morate dodati novu stavku u tablici DNS za svoju prilagođenu domenu pomoću alata za koje ste dobili od registrara domena koje ste kupili naziva domene s. Poduzmite sljedeće korake da biste pronašli i koristiti alate za DNS-a.

1. Prijavite se na svoj račun kod registrara domena, a zatim pronađite stranicu za upravljanje DNS zapisima. Potražite veze ili područja web-mjesta koje su označene kao **Naziv domene**, **DNS-a**ili **Upravljanje naziv poslužitelja**. Često je moguće pronaći veze na ovoj stranici se prikaz podataka o računu i tražite veza kao što su **Moje domene**.

1. Kada pronađete na stranicu Upravljanje za naziv domene, potražite vezu koja vam omogućuje uređivanje DNS zapisa. To može biti navedena kao **datoteku Zone**, **DNS zapise**ili vezom za konfiguraciju **Dodatno** .

    * Stranici najvjerojatnije će nekoliko zapisa koji su već stvorili, kao što su stavke zastavicom "**@**"ili"\*" sa stranicom "parking domene". Može sadržavati i zapisi za uobičajene poddomene kao što su **"www"**.
    * Na stranici će spomenuti **CNAME zapisa**ili pružaju s padajućeg popisa odaberite vrstu zapisa. Možda spomenuti i ostale zapise, kao što **je** i **MX zapisa**. U nekim slučajevima CNAME zapisi će se pozivati tako da druge nazive kao što su **Pseudonima zapisa**.
    * Na stranici dodijelit će se polja koje omogućuju **karte** na **naziv glavnog računala** ili **naziv domene** na neki drugi naziv domene.

1. Dok specifičnosti svaki registrara razlikuju se Općenito mapiranja *iz* prilagođeni naziv domene (primjerice **contoso.com**,) *Da biste* promet Upravitelj naziva domene (**contoso.trafficmanager.net**) koja se koristi za web-aplikacije.

    > [AZURE.NOTE] Osim toga, ako zapis već se koristi i trebali biste preemptively i povezati aplikacije, možete stvoriti dodatnog CNAME zapisa. Na primjer, povezati preemptively **www.contoso.com** na web-aplikaciju, stvorite CNAME zapis iz **awverify.www** za **contoso.trafficmanager.net**. Zatim možete dodati "www.contoso.com" na web-aplikaciju bez promjene "www" CNAME zapis. Dodatne informacije potražite u članku [Stvaranje DNS zapisa za web-aplikacije u prilagođenu domenu][CREATEDNS].

1. Kada završite s dodavanjem ili izmjena DNS zapisa kod registrara, spremite promjene.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Omogućivanje Upravitelj promet

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije potražite u [Centru za razvojne inženjere Node.js](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
