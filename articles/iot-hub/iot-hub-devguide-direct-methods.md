<properties
 pageTitle="Vodič za razvojne inženjere – Izravni metode | Microsoft Azure"
 description="Azure IoT koncentrator za razvojne inženjere vodič – korištenje Izravni metode pozivanje kod na uređajima"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Pozivanje metode izravno na uređaju (pretpregled)

## <a name="overview"></a>Pregled

Koncentrator IoT omogućuje pozivanje metode na uređajima iz oblaka. Načini predstavljaju zahtjev za odgovor interakcije s uređajem koji je slična poziv HTTP iz tog njihovu uspjeti ili neće uspjeti (čim korisnik navedena ograničenja) da bi korisnik znao status poziva. To je korisno za scenarije gdje tečaja trenutne akcije razlikuje se ovisno o tome je li uređaj mogao odgovorili, kao što su slanje programa SMS buđenje uređaja ako je uređaj izvanmrežne (SMS više skupi od metoda poziv u tijeku).

Od metode možete smatrati poziva udaljene procedure izravno na uređaju. Samo metode koje implementirana na uređaju moguće je pozvati iz oblaka. Ako je u oblak pokuša pozivanje metode na uređaju koji nema taj način definiran, neće uspjeti pozivanje metode.

Svaki uređaj način pronalaze jedan uređaj. [Poslovi] [ lnk-devguide-jobs] omogućuje pozivanje metode na više uređaja, a red čekanja način poziva za uređaje prekinuta veza.

Svi koji imaju dozvole za **Povezivanje servisa** na središte IoT možda pozivanje metode na uređaju.

### <a name="when-to-use"></a>Kada se koristi

Uređaj metode su slične [poruke oblaka uređaj] [ lnk-devguide-messages] iz tog i omogućite oblaka pozadinske za prosljeđivanje podataka na uređaj, ali se razlikuju u osnovnim načinima. Pojmovno, metode su slika, a ne durable, a poruke oblaka uređaj asinkronog s do 48 sati rok trajanja.

Načini uzorcima odgovor na zahtjev i nisu durable. Nedostatak rok trajanja nudi dva odmah pogodnosti kada su naredbeno uređaji:

- **Trenutnu povratnu informaciju na način izvođenja** znači da nema potrebe za upravljanje korelacije između zahtjeva i odgovora.
- **Veću propusnost** znači operacije može izvoditi brže jer IoT koncentrator ne pruža sve rok trajanja. Koncentrator IoT omogućuje način pozive po jedinici od poruke oblak na uređaj.

Oblak uređaj poruke nisu nužno naredbi na uređaju, ali umjesto se odnositi na neki servis u oblaku prosljeđivanje neke bitne podatke o uređaju da bi obraditi na njegov Preferirana i koji uređaj možda ili možda neće reagirati. Oblak uređaj poruke imati više vremenskog ograničenja vrijeme (do 48 sati) tijekom metode istječe mnogo brže.

Pomoću metode uređaj za odmah naredba poziva na uređaj i zadatke za zakazano poziva naredbe na uređaju.

## <a name="method-lifecycle"></a>Životni ciklus način

Načini primjenjuju se na uređaju, a možda ćete morati nula ili više unosa u način opseg pravilno stvoriti instancu. Pozivanje Izravni metode putem servisa dostupnog URI (`{iot hub}/twins/{device id}/methods/`). Na uređaju prima Izravni metode kroz MQTT temu specifične za uređaj (`$iothub/methods/POST/{method name}/`). Možda podržavamo metode na dodatne uređaj strani mrežnih protokola u budućnosti.

> [AZURE.NOTE] Kada se poziva izravno metode na uređaju, svojstvo nazive i vrijednosti može sadržavati samo US-ASCII ispisivo alfanumerički osim bilo u sljedećeg kompleta: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Metode su sinkrono i ili uspjeti ili neće funkcionirati nakon isteklo (zadani: 30 sekundi moguće postaviti tako da biste 3600 sekundi). Metode korisne su u interaktivne scenariji gdje želite da se uređaj djelovanje ako i samo ako je uređaj online primanja i slanja naredbi, kao što su uključivanjem svijetlo s telefona. U sljedećim scenarijima želite vidjeti na odmah uspjelo ili nije tako da je servis u oblaku mogu poslužiti na rezultat čim. Uređaj može vratiti neke tijela poruke uslijed način, ali nije potreban za metodu da biste to učinili. Postoji jamstva na redoslijed ili bilo koji semantiku istodobnosti na način pozive.

Uređaj način pozive su samo za HTTP sa strane oblaka i samo za MQTT sa strane uređaja.

## <a name="reference-topics"></a>Pregled tema:

U sljedećim temama reference sadrže dodatne informacije o korištenju Izravni metode.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Pozivanje metode izravno iz pozadinske aplikacije

### <a name="method-invocation"></a>Način poziva

Izravni način instanci na uređaju se pozivi HTTP koje čine:

- *URI* specifične za uređaj (`{iot hub}/twins/{device id}/methods/`)
- POST *metode*
- *Zaglavlja* koji sadrže autorizacija, zahtjev ID, vrsta sadržaja i sadržaja kodiranja
- Prozirni JSON *tijelo* u sljedećem obliku:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Vremensko ograničenje je u sekundama. Ako nije postavljeno vremensko ograničenje, po zadanom je odabrana 30 sekundi.
  
### <a name="response"></a>Odgovor

Pozadinske prima odgovor koji se sastoji od:

- *Šifra stanja HTTP-a*, koji se koristi za pogreške koji dolaze iz koncentratora IoT, uključujući o pogrešci 404 za uređaje koji su trenutno povezani
- *Zaglavlja* koji sadrže e-oznake, zahtjev ID, vrsta sadržaja i sadržaja kodiranja
- JSON *tijelo* u sljedećem obliku:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Oba `status` i `body` su nudi uređaja, a služe za odgovor na poruke uz uređaja vlastite Šifra stanja i/ili opis.

## <a name="handle-a-direct-method-on-a-devcie"></a>Držač Izravni način na na devcie

### <a name="method-invocation"></a>Način poziva

Uređaji prima zahtjeve izravne način o temi MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`

Tijelo čiji prima uređaj je u sljedećem obliku:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Zahtjevi za metodu su QoS 0.

### <a name="response"></a>Odgovor

Uređaj pošalje odgovore na `$iothub/methods/res/{status}/?$rid={request id}`, pri čemu:

 - Na `status` svojstvo je status uređaja navedeni način izvođenja.
 - Na `$rid` svojstvo je ID zahtjeva iz poziva način poslao IoT koncentratora.

Tijelo postavljen tako da na uređaju, a može biti bilo koji status.

## <a name="additional-reference-material"></a>Dodatne reference materijala

Druge teme referencu u vodiču za razvojne inženjere obuhvaćaju sljedeće:

- [Krajnje točke koncentrator IoT] [ lnk-endpoints] opisuju razne izlaže svaki IoT koncentrator za upravljanje i izvođenje operacija krajnje točke.
- [Ograničavanje i kvotama] [ lnk-quotas] u članku se opisuje kvote koji se odnose na servis IoT koncentratora i regulacije ponašanje očekivati prilikom korištenja servisa.
- [Koncentrator IoT uređaja i servisa SDK-ovi] [ lnk-sdks] popis različitih jezika SDK-ovi koje koristi u prilikom razvoja uređaja i servisa aplikacije kompatibilne s IoT koncentratora.
- [Upit jezik za twins, postupcima i zadacima] [ lnk-query] u članku se opisuje jezik upita možete koristiti za dohvaćanje informacija iz centra za IoT o uređaju twins, metode i zadacima.
- [Podrška IoT koncentrator MQTT] [ lnk-devguide-mqtt] navedene su dodatne informacije o podršci za IoT koncentrator za protokol MQTT.

## <a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako koristiti Izravni metode, možda ćete u temi vodič za razvojne inženjere:

- [Zakazivanje zadataka na više uređaja][lnk-devguide-jobs]

Ako želite isprobati neke koncepta opisane u ovom članku, možda ćete zanimati na središte IoT Praktični vodič:

- [Pomoću metode oblaka uređaja][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
