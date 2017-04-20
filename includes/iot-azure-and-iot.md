
# <a name="azure-and-internet-of-things"></a>Azure i Internet stvari

Dobro došli u Microsoft Azure i Internet stvari (IoT). Ovaj članak predstavlja arhitektura rješenje IoT koja opisuje uobičajene karakteristike možda implementirati pomoću Azure services rješenja za IoT. IoT rješenja zahtijevaju sigurne komunikacije dvosmjernim između uređaja vjerojatno numeriranje u u milijunima i pozadinskih rješenje. Pozadinska rješenje, na primjer, možda koristite analytics automatizirani, predvidljivih otkriti uvida s događaja uređaj oblaka strujati.

Koncentrator Azure IoT je ključa sastavni blok kada implementirate ovo rješenje arhitekture IoT pomoću Azure services. IoT programski pruža dovršeno, end-to-end, implementacije arhitekture za određene scenarije IoT. Na primjer: 

- Rješenje *udaljene nadzor* omogućuje nadzirati status uređaje poput vending strojeva. 
- Rješenje *predvidljivih održavanja* pomaže vam predvidjeti održavanja potrebama uređaje poput pumps u udaljeno pumping stanice i izbjegli neplanirani pauza radi.

## <a name="iot-solution-architecture"></a>Arhitektura rješenje IoT

Sljedeći dijagram prikazuje tipične arhitektura IoT rješenja. Dijagram uključuju nazive određene Azure usluge, ali opisuje ključni elementi u generički arhitekturi rješenje IoT. U toj arhitekturi uređaji IoT prikupljanje podataka poslati oblak pristupnika. Pristupnik oblaka čini podataka dostupna za obradu druge servise pozadinskim iz isporuke podataka drugih redak poslovnih aplikacija ili ljudske operatore kroz nadzorne ploče ili druge prezentacije uređaj.

![Arhitektura rješenje IoT][img-solution-architecture]

> [AZURE.NOTE] Detaljne rasprave IoT arhitekture, pogledajte [Microsoft Azure IoT referenca arhitektura][lnk-refarch].

### <a name="device-connectivity"></a>Povezivanje uređaja

U toj arhitekturi rješenje IoT uređaji poslati telemetry, što je senzor čitanja iz pumping stanica oblak krajnje točke za obradu i pohranu. U slučaju predvidljivih održavanja pozadinska možda strujanje podaci senzora koristite za određivanje kada zahtijeva određene pump održavanja. Uređaji možete primati i odgovarati na uređaj oblak naredbe čitajući poruke iz oblak krajnje točke. Ako, na primjer, u scenariju predvidljivih održavanja pozadinska rješenje možda poslati naredbe drugih pumps pumping stanica za početak rerouting tokova prije održavanja dospijeva za pokretanje provjerite inženjeru za održavanje početak rada kada ona stigne.

Jedna od najvećih izazove nasuprotne IoT projektima je način pouzdano i sigurno povezivanje uređaja pozadinska rješenje. IoT uređaji imati različite karakteristike to drugim klijentima preglednici i mobile apps. Uređaji IoT:

- Često su ugrađene sustave bez ljudske operatora.
- Može se uvesti udaljenih mjesta, gdje je najskuplji fizički pristup.
- Samo možda dostupna putem pozadinskih rješenje. Postoji drugi način za interakciju s uređajem.
- Možda imate ograničen napajanja i resurse za obradu.
- Možda Povremeni, spora ili najjeftiniji mrežne veze.
- Možda morati koristiti protokole vlasničko, prilagođene ili određenu djelatnost aplikacije.
- Moguće je kreirati pomoću velikog skupa popularne platforme hardver i softver.

Uz uvjete iznad bilo koje rešenje IoT također morate isporučiti skale, sigurnosti i pouzdanosti. Rezultirajući skup connectivity preduvjeti je tvrdi i vremenski zahtjevan implementirati pomoću tradicionalni tehnologije kao što su spremnici web i razmjenu Brokerske djelatnosti. Koncentrator Azure IoT i IoT uređaj SDK-ovi olakšati za implementaciju rješenja koja udovoljavaju tim zahtjevima.

Uređaj možete izravno komunicirati s krajnje točke pristupnik oblak ili ako uređaj ne možete koristiti protokole komunikacije koji podržava pristupnik oblaka, možete povezati putem posrednu pristupnika. Ako, na primjer, [pristupnik protokol Azure IoT] [ lnk-protocol-gateway] protokol prijevod možete izvršiti ako uređaji ne možete koristiti protokole koje podržava IoT koncentrator.

### <a name="data-processing-and-analytics"></a>Obrada podataka i analytics

U oblaka, IoT je kraj natrag rješenje je gdje većinu obradu podataka pojavljuje, primjerice filtriranje i telemetry izračunavanjem zbrojeva i usmjeravanje druge servise. Rješenje IoT natrag Završi:

- Prima telemetry u mjerilu s uređaja i određuje kako obraditi i spremiti podatke. 
- Mogu omogućiti slanje naredbi iz oblak određenog uređaja.
- Pruža mogućnosti Registracija uređaja koji omogućuju dodjele uređaja i kontrola uređaja koji dopušteno povezivanje s vaše infrastrukture.
- Omogućuje praćenje stanja uređaje i nadgledanje njihovih aktivnosti.

U slučaju predvidljivih održavanja rješenje pozadinska pohranjuje podatke povijesnog telemetry. Pozadinska ove podatke možete koristiti da biste identificirali uzorci naznačili održavanja dospijeva na određene pump.

IoT rješenja možete uključiti automatsko povratne informacije petlje. Ako, na primjer, možete identificirati analytics modul u pozadinska iz telemetry je iznad običnom operativni razine temperatura određenog uređaja. Rješenje zatim možete poslati naredbu uređaj uputom poduzmite akciju ispravljanja.

### <a name="presentation-and-business-connectivity"></a>Prezentaciju i poslovne povezivost

Povezivost sloj prezentaciju i poslovne omogućuje krajnji korisnici interakciju s IoT rješenje i uređaje. Omogućuje korisnicima prikaz i analizu podataka prikupljenih iz svoje uređaje. Tim pogledima možete poduzeti obrasca ili nadzorne ploče Dvosmjerno izvješća koja možete prikazati i povijesne podatke ili blizu podataka u stvarnom vremenu. Operator, na primjer, možete provjeriti stanje određenom pumping stanica i vidjeti bilo upozorenja Podignuto sustav. Taj sloj omogućuje integraciju IoT rješenje pozadinska s postojeći redak poslovnih aplikacija za povezivanje u enterprise poslovnih procesa ili tijekova rada. Na primjer, rješenje predvidljivih održavanja možete integrirati s zakazivanja sustava koje knjige inženjeru posjetiti pumping stanica kada rješenje identificira pump kojoj su potrebne održavanja.

![IoT rješenja nadzorne ploče][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
