<properties
 pageTitle="Pregled Azure IoT koncentrator | Microsoft Azure"
 description="Pregled servisa Azure IoT koncentrator: što je iot koncentrator, povezivanje s uređaja, a zatim internet stvari komunikacijskom i uzorak servisa Potpomognuta komunikacije"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Što je koncentrator IoT Azure?

Dobro došli u Azure IoT koncentratora. Ovaj članak sadrži pregled Azure IoT koncentratora i u članku se opisuje zašto trebali biste koristiti taj servis za implementaciju rješenja programa Internet stvari (IoT). Azure IoT koncentrator je potpuno Upravljani servis koji omogućuje pouzdanog i sigurne Dvosmjeran komunikaciju između milijune IoT uređaji i pozadinskih rješenja. Azure koncentrator IoT:

- Nudi pouzdanog uređaj-na-oblaka i oblaka-na-uređaj poruka na razini.
- Omogućuje sigurnu komunikaciju pomoću po uređaj sigurnosne vjerodajnice i pristup kontrolu.
- Nudi proširenom nadzor za povezivanje s uređaja i događaje upravljanje identitetom uređaja.
- Uključuje biblioteke uređaj za najčešće korištene jezika i platformama.

U članku [koncentrator za usporedbu IoT i događaja koncentratora] [ lnk-compare] u članku se opisuje ključne razlike između tih dvaju servisa i ističe prednosti korištenja IoT koncentratora u vašem IoT rješenja.

![Azure IoT koncentrator kao oblak pristupnika u internet stvari rješenja][img-architecture]

> [AZURE.NOTE] Detaljnije rasprave IoT arhitektura, potražite u članku [Microsoft Azure IoT referenca arhitektura][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT uređaj povezivanje izazove

Koncentrator IoT i biblioteke uređaj vam pomoći da zadovoljavaju izazove kako pouzdano i sigurno povezivanje uređaja s pozadinska rješenja. IoT uređaji:

- Često su ugrađene sustave bez Ljudski operatora.
- Može biti udaljenog mjesta, pri čemu je skupi fizički pristup.
- Mogu se samo dostupno putem pozadinskih rješenja.
- Možda ste ograničeni power i resursi za obradu.
- Možda Povremeni, sporo ili skupi mrežne veze.
- Trebali biste koristiti protokole vlasničkih, prilagođenu ili industrijskih specifične aplikacije.
- Moguće stvoriti pomoću velikog skupa popularnih hardvera i softvera platformama.

Osim preduvjeta iznad rješenjem IoT morate i izlaganje mjerilo, sigurnost i pouzdanost. Rezultirajući skup preduvjeti za povezivanje je teško i vremenski implementirati kada koristite tradicionalni tehnologije, kao što su spremnika web i razmjenu Brokerske djelatnosti.

## <a name="why-use-azure-iot-hub"></a>Zašto koristiti Azure IoT koncentratora

Azure IoT koncentrator adrese izazove uređaj povezivanje na sljedeće načine:

-   **Provjera autentičnosti po uređaja i sigurno povezivanje**. Možete Dodjela svakom uređaju s vlastitom [sigurnosni ključ] [ lnk-devguide-security] da biste omogućili da biste se povezali IoT koncentrator. [Koncentrator IoT identiteta registra] [ lnk-devguide-identityregistry] pohranjuje uređaj identiteta i ključevi rješenja. Pozadinska rješenje možete dodati pojedinačne uređajima dopustiti ili zabraniti popisa da biste omogućili potpunu kontrolu nad pristup putem uređaja.

-   **Nadzor operacije povezivanja uređaja**. Možete primati detaljni postupak zapisnike o uređaju identiteta upravljanje operacije i događaje povezivanje uređaja. Tu mogućnost nadzora omogućuje IoT rješenje za prepoznavanje problema s povezivanjem, kao što su uređaje koji se pokušaja povezivanja s pogrešne vjerodajnice previše često slali poruke ili Odbaci sve poruke oblak na uređaj.

-   **Opsežan skup biblioteke uređaja**. [Azure IoT uređaj SDK-ovi] [ lnk-device-sdks] dostupni i podržanim za različite jezike i platforme – C za mnoge Linux distribucija i Windows te u stvarnom vremenu operacijske sustave. Azure IoT uređaj SDK-ovi podržava i upravljani jezicima, na primjer C#, Java i JavaScript.

-   **IoT protokoli i proširenja**. Ako rješenje ne koristite uređaj biblioteke, IoT koncentrator izlaže javno protokol koji omogućuje uređaje za nativno MQTT v3.1.1, HTTP 1.1 ili AMQP 1.0 protokola. Možete proširiti i IoT koncentrator nudi podrška za prilagođene protokoli po:

    - Stvaranje pristupnika za polje s [Azure IoT pristupnika SDK] [ lnk-gateway-sdk] koji pretvara protokol za prilagođene u jedan od tri protokola razumjele IoT koncentratora. 
    - Prilagodba [Azure IoT protokol pristupnika][protocol-gateway], otvaranje izvorišnu komponentu koji se izvodi u oblak.

-   **Promjena veličine**. Azure IoT koncentrator mijenja veličinu milijune istodobno povezanih uređaja i milijune događaje u sekundi.

Tih prednosti su generički za mnoge komunikacijskom. Koncentrator IoT trenutno omogućuje implementirati sljedeće određene komunikacijskom:

-   **Ingestion događaja uređaj u oblak.** Koncentrator IoT pouzdano možete primati milijune događaje u sekundi s uređaja. Ga možete zatim obraditi ih na vašem tipkovni putu pomoću mehanizam procesor događaj. Ga ih pohranite i na vašem Hladna putu za analizu. Koncentrator IoT zadržava podaci o događaju za najviše sedam dana jamči pouzdanog obrada i apsorbira peaks u opterećenje.

-   * *Pouzdanog oblaka uređaj poruka (ili *naredbe*). ** pozadinska rješenje možete koristiti IoT koncentrator za slanje poruka pomoću programa pri najmanjih-jednom isporuke jamstva pojedinačne uređaja. Svaka poruka sadrži pojedinačne vrijeme važenja postavku, a pozadinska možete zatražiti potvrde o isporuci i istek. Te potvrde provjerite je li puni uvid u životnog ciklusa poruke oblak na uređaj. Zatim možete implementirati poslovne logike koji uključuje postupaka koji se izvode na uređajima.

-   **Prijenos datoteke i senzor predmemorirane podatke u oblak.** Uređaji mogu prenositi datoteke za pohranu Azure pomoću SAS ji upravlja IoT koncentrator za vas. Koncentrator IoT mogu uzrokovati obavijesti prilikom dolaska datoteka u oblaku da biste omogućili pozadinska ih obrađivati.

## <a name="gateways"></a>Pristupnika

Pristupnik u IoT rješenju najčešće ili [protokol pristupnika] [ lnk-gateway] koji je implementiran u oblak ili [polje pristupnika] [ lnk-field-gateway] koji je lokalno implementiran s uređajima. Pristupnik za protokol izvodi protokol prijevod, na primjer MQTT AMQP. Polje pristupnika možete pokrenuti analize na rubu, donošenje odluka vrijeme osjetljivi smanjiti Latencija, ponudite usluge za upravljanje uređajima, nametnuti sigurnosti i privatnosti ograničenja i izrađivati protokol prijevod. Obje vrste pristupnika poslužiti kao intermediaries između uređajima i koncentratora za IoT.

Pristupnik za polje razlikuje se od jednostavnih promet usmjeravanje uređaju (kao što je mrežni adresu prijevoda uređaj ili vatrozid) jer je obično provodi aktivna uloga u upravljanju pristup i informacije o tijeku u rješenje.

Rješenje može obuhvaćati protokol i polje pristupnik.

## <a name="how-does-iot-hub-work"></a>Kako funkcionira koncentrator IoT?

Azure IoT koncentrator implementira [servis Potpomognuta komunikacije] [ lnk-service-assisted-pattern] u mediate interakcije između uređajima i pozadinskih vašeg rješenja. Cilj servisa Potpomognuta komunikacije je da biste uspostavili pouzdanih, Dvosmjeran putova komunikacije između kontrole sustav, kao što su IoT koncentratora i uređaji za posebne svrhe koje su uvedene u nepouzdanoj fizički prostor. Uzorak uspostavlja na sljedeći način:

- Sigurnost ima prednost pred druge mogućnosti.
- Uređaji prihvatiti informacije o neželjene mreže. Na uređaju uspostavlja sve veze i usmjerava programa izlaznog samo način. Za uređaj naredbu primanje pozadinska, uređaj mora redovito pokretanje veze da biste provjerili naredbi na čekanju za obradu.
- Uređaji treba povezati ili samo uspostaviti usmjerava poznati servisima oni su peered, kao što su IoT koncentratora.
- Put komunikacije među uređaja i usluga ili uređaj i pristupnik je osigurani na aplikacijskom sloju protokol.
- Razinu sustava autorizacije i provjeru autentičnosti temelje se na identiteta po uređaja. Omogućavanje pristupa vjerodajnice i dozvole gotovo trenutačno revocable.
- Dvosmjeran komunikacije za uređaje koji se povezuju povremeno zbog power ili povezivanje opasnosti olakšano je tako da držite naredbe i obavijesti o uređaju dok je uređaj povezuje primati. Koncentrator IoT održava redova specifične za uređaj za naredbe šalje.
- Podataka aplikacije tereta zaštićenim zasebno za zaštićeni prijenosa putem pristupnika na određeni servis.

Mobilni industrijskih ovo koristio uzorak servisa Potpomognuta komunikacije na razini Ogromno implementaciju automatske obavijesti servise kao što je [Windows automatske obavijesti Services][lnk-wns], [Google Cloud poruka][lnk-google-messaging], [Servis za Apple automatske obavijesti]i[lnk-apple-push].

Koncentrator IoT podržano je putem ExpressRoute na javno peering put.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako Azure IoT koncentrator omogućuje utemeljenih na standardima upravljanje uređajima IoT za daljinsko upravljanje, konfiguriranje i ažuriranje uređaja, potražite u članku [Pregled programa Azure IoT koncentratora za upravljanje uređajima][lnk-device-management].

Možete implementirati klijentske aplikacije na velik broj platforme uređaja hardver i operacijskim sustavima, možete pomoću uređaja IoT SDK-ovi. Uređaj IoT SDK-ovi obuhvaćaju biblioteke koje olakšavaju slanje telemetriju koncentratora za IoT i primanje naredbe oblak na uređaj. Kada koristite na SDK-ovi, možete odabrati iz različitih mrežne protokole za komunikaciju s IoT koncentratora. Dodatne informacije potražite u članku [informacije o uređaju SDK-ovi][lnk-device-sdks].

Za početak pisanja koda za neke i pokretanje nekih uzoraka potražite u članku [Početak rada s IoT koncentrator] [ lnk-get-started] vodič.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Servis Potpomognuta komunikacije, Clemens Vasters objava na blogu"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
