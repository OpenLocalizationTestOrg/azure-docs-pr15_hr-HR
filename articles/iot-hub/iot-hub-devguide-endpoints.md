<properties
 pageTitle="Vodič za razvojne inženjere - krajnje točke IoT koncentrator | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – referentne informacije o IoT koncentrator krajnje točke"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="reference---iot-hub-endpoints"></a>Referenca - IoT koncentrator krajnje točke

## <a name="list-of-iot-hub-endpoints"></a>Popis IoT koncentrator krajnje točke

Azure IoT koncentrator je servis više klijentu izlaže njegove funkcije različite Glumci. Sljedeći dijagram prikazuje različite krajnje točke izlaže IoT koncentratora.

![Koncentrator IoT krajnje točke][img-endpoints]

Slijedi opis krajnjih točaka:

* **Davatelja resursa**. Davatelja resursa IoT koncentrator izlaže [Voditelj resursa Azure] [ lnk-arm] sučelja koja omogućuje vlasnici Azure pretplate za stvaranje i brisanje IoT koncentratora i ažuriranje svojstava IoT koncentratora. Svojstva IoT koncentrator upravljaju [pravila za sigurnost na razini koncentrator][lnk-accesscontrol], umjesto kontrola pristupa na razini uređaja i funkcionalne mogućnosti oblaka uređaj, a uređaj u oblak razmjene poruka. Davatelja resursa IoT koncentrator omogućuje vam da biste [izvezli identiteta uređaj][lnk-importexport].
* **Upravljanje identitetom uređaja**. Svaki IoT koncentrator izlaže skup HTTP OSTALE krajnje točke za upravljanja uređaj (Stvaranje, dohvatiti, ažuriranje i brisanje). [Uređaj identiteta] [ lnk-device-identities] se koriste za upravljanje uređajima provjeru autentičnosti i pristup.
* **Upravljanje uređajima twin**. Svaki IoT koncentrator izlaže skup krajnje točke HTTP OSTALE usluge dostupnog upita i ažuriranje [uređaja twins] [ lnk-twins] (Ažuriraj oznake i svojstva).
* **Upravljanje zadacima**. Svaki IoT koncentrator izlaže postavljanje krajnje točke servisa dostupnog OSTALE HTTP-a za slanje upita i upravljanje [zadacima][lnk-jobs].
* **Krajnje točke uređaja**. Za svaki uređaj dodjeli u registru identiteta uređaj, IoT koncentrator izlaže skup krajnje točke koje uređaja možete koristiti za slanje i primanje poruka:
    - *Slanje poruka uređaj u oblak*. Koristite ovaj krajnje točke slanja [poruka uređaj u oblak][lnk-d2c].
    - *Primanje poruka oblak na uređaj*. Na uređaju koristi ovaj krajnjoj točki primaju ciljano [oblaka uređaj poruke][lnk-c2d].
    - *Pokretanje prijenosima datoteka*. Na uređaju koristi ovaj krajnjoj točki primanje programa Azure prostora za pohranu SAS URI-JA IoT koncentrator za [Prijenos datoteke][lnk-upload].
    - *Dohvaćanje i ažuriranje twin svojstva*. Na uređaju koristi ovaj krajnje točke za pristup [twin uređaj][lnk-twins]na svojstva.
    - *Primanje zahtjeva za Izravni metode*. Uređaj koristi ovaj krajnje točke da biste preslušali [Izravni metode][lnk-methods]na zahtjeve.

    Te krajnje točke ponudit će se pomoću [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 i [AMQP 1.0] [ lnk-amqp] protokola. Imajte na umu da AMQP je dostupan putem [WebSockets] [ lnk-websockets] na priključak 443.
    
    Twins' i metode endpoins su dostupne samo pomoću [MQTT v3.1.1][lnk-mqtt].

* **Krajnje točke servisa**. Svaki IoT koncentrator izlaže skup krajnje točke pozadinskih vašeg računala pomoću možete komunicirati s uređajima. Te krajnje točke su trenutno samo prikazanog koristite [AMQP] [ lnk-amqp] protokol, osim krajnje točke način poziva predstavljeni putem HTTP 1.1.
    - *Primanje poruka uređaj u oblak*. Ovaj krajnje kompatibilan s [Azure događaj koncentratora][lnk-event-hubs]. Pozadinske servis pomoću njega možete čitati [poruke uređaj u oblak] [ lnk-d2c] poslao uređajima.
    - *Oblak uređaj poruke slati i primati potvrda o isporuci*. Te krajnje točke omogućiti vaše aplikacije pozadinskih slanje pouzdanog [oblaka uređaj poruke][lnk-c2d], i dobili odgovarajuće isporuke ili potvrda isteka.
    - *Primanje obavijesti za datoteke*. Ova poruka krajnjoj točki omogućuje vam da biste primali obavijesti kada uređajima uspješno prenijeli datoteku. 
    - *Izravni način poziva*. Ovaj krajnjoj točki omogućuje pozadinske servisa pozivanje [Izravni način] [ lnk-methods] na uređaju.

Na [IoT koncentrator API -ji i SDK-ovi] [ lnk-sdks] se članku opisuju se razni načini za pristup te krajnje točke.

Na kraju, važno je Imajte na umu da sve krajnje točke IoT koncentrator koristiti [TLS] [ lnk-tls] protokol i bez krajnjoj točki ikad predstavljeni na nešifrirani/nesigurnim kanala.

## <a name="field-gateways"></a>Polje pristupnika

U rješenju IoT *pristupnika polja* nalazi između uređajima i vaše IoT koncentrator krajnje točke. Obično nalazi blizu uređajima se. Uređajima izravno komunicirati s pristupnika polja pomoću protokola podržava uređaje. Pristupnik za polje povezuje krajnje koncentrator IoT putem protokola koji podržavaju IoT koncentratora. Pristupnik za polje može biti vrlo specijalizirane hardver ili malo energije računala sa sustavom softver koja izvršava završetka do kraja scenarij za koji je namijenjen pristupnika.

Možete koristiti [Azure IoT pristupnika SDK] [ lnk-gateway-sdk] implementaciju pristupnik za polje. U ovom SDK nudi određene funkcije kao što je mogućnost multiplex komunikacije s više uređaja na istu vezu IoT koncentrator.

## <a name="next-steps"></a>Daljnji koraci

Druge teme referencu u ovom vodiču za razvojne inženjere IoT koncentrator obuhvaćaju sljedeće:

- [Jezik upita za twins, postupcima i zadacima][lnk-devguide-query]
- [Kvota i ograničavanje][lnk-devguide-quotas]
- [Podrška za MQTT IoT koncentratora][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md