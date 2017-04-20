> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Ovaj članak pruža detaljne vodič [uzorak koda Hello World] [ lnk-helloworld-sample] da biste ilustrirali temeljne komponente [Azure IoT pristupnik SDK] [ lnk-gateway-sdk] arhitekture. Uzorak koristi pristupnik SDK koncentrator IoT za izradu jednostavnih pristupnika koja prijavljuje poruku "Pozdrav svijetu" u datoteku svakih pet sekundi.

Prikaz pokriva:

- **Koncepti**: konceptualni pregled komponente koje sastavite pristupnik bilo stvaranje s pristupnika SDK IoT.  
- **Arhitektura Hello svijeta uzorak**: opisuje kako koncepti se primjenjuju na Hello svijeta uzorak i koliko stane komponente zajedno.
- **Kako izraditi uzorka**: korake potrebne za izgradnju uzorka.
- **Kako pokrenuti uzorka**: korake potrebne za pokretanje uzorka. 
- **Tipične izlaz**: primjer izlaza očekivati kada pokrenete uzorka.
- **Komadići koda**: zbirke komadići koda za pokazivanje kako uzorak Hello svijeta implementira pristupnog ključa komponente.

## <a name="azure-iot-gateway-sdk-concepts"></a>Koncepti Azure IoT pristupnik SDK

Prije Pregledajte uzorak koda ili stvaranje vlastite pristupnik polje pomoću SDK IoT pristupnik, trebalo bi da razumete ključne koncepte underpin arhitekture SDK-a.

### <a name="modules"></a>Moduli

Stvaranjem i montaža *module*izgraditi pristupnik s SDK pristupnik Azure IoT. Moduli koristite *poruke* razmjene podataka međusobno. Modul prima poruku, provodi neke akcije, po izboru pretvorbe u novu poruku i zatim objavljuje za druge module obraditi. Neki moduli možda samo proizvesti nove poruke i nikad obraditi dolazne poruke. Lanac module stvara kanal obrade podataka sa svakom modulu izvođenje transformaciju na podatke u jednoj točki u tom kanalu.

![Lanac module u pristupnik ugrađen SDK pristupnik Azure IoT][1]
 
SDK sadrži sljedeće:

- Već napisanih module koji izvršavaju funkcije zajednički pristupnik.
- Sučelja programer može koristiti za pisanje prilagođenog module.
- Infrastruktura potrebno uvesti i pokrenuti skup module.

SDK pruža sloj apstrakcije koji vam omogućuje sastavljanje pristupnici pokretanje na različite operacijske sustave i platforme.

![Sloj apstrakcije Azure IoT pristupnik SDK][2]

### <a name="messages"></a>Poruke

Iako misle o module prosleđivanje poruke međusobno je praktičan način da conceptualize kako pristupnik funkcije, ona nije ispravno odražavaju što se događa. Moduli pomoću na broker međusobno komunicirati, objavljivanje poruka broker (sabirnice, pubsub ili drugim razmjenu uzorak) i omogućuju broker Usmjeri poruku module povezan s njim.

Neki moduli koristi funkciju **Broker_Publish** za objavljivanje poruka brokera. Brokera isporučuje poruke modul po pozivanje funkciju povratnog poziva. Poruka se sastoji od skupa ključa vrijednosti svojstva i sadržaj proslijeđen kao blok memorije.

![Uloga Broker SDK pristupnik Azure IoT][3]

### <a name="message-routing-and-filtering"></a>Usmjeravanje poruka i filtriranje

Postoje dva načina directing poruke ispravan module. Skup veza se mogu proslijediti brokera tako brokera zna izvora i Sito za svaki od modula ili modul možete filtrirati na svojstva poruke. Modul treba samo primljenu poruku ako poruka je namijenjen za njega. Veze i filtriranje poruka je što učinkovito stvara poruku kanalima.

## <a name="hello-world-sample-architecture"></a>Arhitektura uzorak svijeta pozdrav

Uzorak Hello svijeta ilustrira koncepti opisane u prethodnoj sekciji. Uzorak Hello svijeta implementira pristupnika koja ima kanali sastoji se od dva modula:

-   Modul *hello svijeta* stvara poruku svakih pet sekundi i prosljeđuje modul zapisivaču.
-   Modul *zapisivaču* piše ga Prima poruke u datoteku.

![Arhitektura Hello svijeta uzorak ugrađen SDK pristupnik Azure IoT][4]

Kao što je opisano u prethodnoj sekciji modula Hello svijeta nije prenesite poruke izravno modul zapisivaču svakih pet sekundi. Umjesto toga, objavljuje poruku za brokera svakih pet sekundi.

Modul zapisivaču primi poruku iz brokera i djeluje na njemu, sadržaj poruke za pisanje u datoteku.

Modul zapisivaču samo troši poruke iz brokera, za brokera nikad objavljuje nove poruke.

![Kako preusmjerava brokera poruke između module u SDK pristupnik Azure IoT][5]

Slika iznad prikazuje arhitekturu Hello svijeta uzorak i relativni putovi izvorišne datoteke implementirati različite dijelove uzorka u [spremište][lnk-gateway-sdk]. Samostalno proučavati šifru ili kao vodič koristite komadići koda ispod.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk