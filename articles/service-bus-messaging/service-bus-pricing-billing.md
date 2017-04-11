<properties 
    pageTitle="Servis Bus cijene i naplata | Microsoft Azure"
    description="Pregled servisa Bus cijene strukturu."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Servis Bus cijene i naplata

Servis Bus je ponuđen u Basic, standardna i [Premium](service-bus-premium-messaging.md) razine. Možete odabrati sloju servisa za svaki servis Bus polje naziva servisa koji ste stvorili, a taj odabir sloju primjenjuje u svi entiteti stvorene taj prostor naziva.

>[AZURE.NOTE] Detaljne informacije o trenutne cijene servisa Bus potražite u članku [Bus servisa Azure cijene stranice](https://azure.microsoft.com/pricing/details/service-bus/)i [Najčešća pitanja vezana uz servis Bus](service-bus-faq.md#service-bus-pricing).

Servis Bus koristi sljedeće dvije metar redovima i teme/pretplate:

1. **Operacije razmjene poruka**: definiran kao API poziva na temelju redu ili tema/pretplate krajnje točke servisa. U ovom metar zamijenit će poruke slati ni primati kao primarni jedinice naplatu korištenje redovima i teme/pretplate.

2. **Brokered veze**: definirani kao broj Vršna trajne veze otvorite protiv redovi, teme i pretplate tijekom razdoblja navedeni uzorkovanje jedan sat. U ovom metar primijenit će se samo u standardni sloju, u kojem možete otvoriti dodatne veze (prethodno, veze bili su ograničeni na 100 po redu čekanja/tema/pretplati) za nominal mjesečnu naknadu.

**Standardni** razina predstavlja postupnu promjenu cijene za razne operacije obavljene s redovima i teme/pretplate rezultira utemeljen na glasnoću popuste do 80% na najviše razine korištenje. Postoji standardni sloju osnovni trošak od $10 mjesečno, što vam omogućuje da izvođenje operacija do milijuna kao 12.5 mjesečno na bez dodatnih troškova.

Razina **Premium** nudi resursa odvajanja sloju opterećenje procesora i memorije tako da se pokreće svaki radno opterećenje kupca u odvajanja. Ovaj spremnik resursa naziva u *porukama jedinicu*. Prostor naziva svaki premium dodijeljeno najmanje jednu razmjenu jedinicu. Možete kupiti 1, 2 ili 4 razmjenu jedinica za svaki servis Bus Premium prostor naziva. Jedan radno opterećenje ili entitet mogu obuhvaćati više jedinica za razmjenu poruka i broj jedinica za razmjenu se mogu mijenjati po želji, iako naplata naknade 24-satnom ili dnevni stopa. Rezultat je predvidljivi te kontrolirani performanse za servis Bus rješenje. Ne samo je ovaj performanse predvidljivi i dostupan, ali preporučuje se i brže. Azure razmjenu poruka servisa Bus Premium sastavlja modul prostora za pohranu u koncentratora Azure događaj. Premium poruke, performanse Vršna je mnogo brže od standardne sloju.

Imajte na umu standardne osnovni trošak samo jednom naplatiti mjesečno po Azure pretplate. To znači da nakon što stvorite Jedna standardna ili prostor naziva za servis Bus sloju Premium, moći da biste stvorili proizvoljan broj dodatne standardni prikaz ili Premium sloju prostora naziva pod kojim želite u odjeljku te iste pretplate Azure bez dodatnih troškova osnovni nakon toga.

Svi postojeći servis Bus prostori naziva stvorena prije 1. studenom 2014 su automatski sprema u standardne sloju. Time se osigurava da imate pristup svim značajkama trenutno dostupno sa servisa Bus nastavite. Naknadno, koristite [Azure klasični portal][] za vraćanje na stariju verziju na osnovni sloju po želji.

U sljedećoj su tablici navedene funkcionalne razlike između osnovne i standardne/Premium razine.

|Mogućnost|Osnovni|Standardna/Premium|
|---|---|---|
|Reda čekanja|Da|Da|
|Planirani poruke|Da|Da|
|Teme i pretplate|ne|Da|
|Preusmjeravanje|ne|Da|
|Transakcije|ne|Da|
|De-dupliciranje|ne|Da|
|Sesije|ne|Da|
|Velike poruke|ne|Da|
|ForwardTo|ne|Da|
|SendVia|ne|Da|
|Brokered veze (uključeno)|100 po servisa Bus prostor naziva|1000 po Azure pretplati|
|Brokered veze (pokrivenost dopušteni)|ne|Da (naplatu)|

## <a name="messaging-operations"></a>Postupci za razmjenu poruka

Kao dio novog modela cijene, naplatom za redova i teme/pretplata se mijenja. Ove entiteti su pri prijelazu s naplatom po poruci na naplatu po operaciji. "Operacija" odnosi se na sve poziv API u odnosu na redu ili tema/pretplate servisa krajnjoj točki. To obuhvaća upravljanje, slanje/primanje i sesiju stanje operacija.

|Vrsti operacije|Opis|
|---|---|
|Upravljanje|Stvaranje, čitanje, ažuriranje, brisanje (CRUD) protiv redova ili teme/pretplate.|
|Razmjena poruka|Slanje i primanje poruka s redovima ili teme/pretplate.|
|Stanje sesije|Početak ili postavljanje stanja sesije na redu ili tema/pretplate.|

Cijene za sljedeće su učinkovitih početni 1. studenom 2014.:

|Osnovni|Trošak|
|---|---|
|Operacije|$0,05 po milijuna operacije|

|Standardna|Trošak|
|---|---|
|Osnovni trošak|$10/ mjesec|
|Najprije milijuna kao 12.5 operacije/mjesec|Dio|
|kao 12.5 100 milijuna operacije/mjesec|$0.80 po milijuna operacije|
|100 milijuna - 2,500 milijuna operacije/mjesec|$0.50 po milijuna operacije|
|Više 2,500 milijuna operacije/mjesec|$0,20 po milijuna operacije|

|Premium|Trošak|
|---|---|
|Dnevni|$11.13 fiksna stopa po jedinici poruke|

## <a name="brokered-connections"></a>Brokered veze

*Brokered veze* bi odgovarao uzoraka korištenja klijenta koji obuhvaćaju velik broj "persistently povezani" pošiljatelja i primatelja protiv redovi, teme ili pretplate. Persistently povezanog pošiljatelja i primatelja su onih koji se povezuju AMQP ili HTTP pomoću programa od nule primanje vremenskog ograničenja (na primjer, HTTP dugo provjere). HTTP pošiljatelja i primatelja s odmah vremenskog ograničenja generirati brokered veze.

Tema/pretplate i redova prethodno ograničenje od 100 Istodobni veze po URL-a. Trenutna naplate shema uklanja ograničenje-URL za redova i teme/pretplate i implementira kvotama i mjerenja na brokered vezama na prostor naziva Bus usluge i razine Azure pretplate.

Osnovni sloju uključuje i isključivo ograničeno je na, 100 brokered veze po prostora za naziv Bus servisa. Veza iznad taj broj će biti odbijene u osnovni sloju. Standardni sloju uklanja ograničenje po prostor naziva i broji korištenje zbrajanja brokered veze preko Azure pretplate. U standardnom sloju 1000 brokered veze po Azure pretplati imati dopuštenje pri bez extra trošak (osim osnovni trošak). Više od 1000 brokered veze pomoću preko standardna sloja servisa Bus prostora naziva u Azure pretplata će naplaćivati postupnu promjenu rasporedu, kao što je prikazano u sljedećoj tablici.

|Brokered veze (standardni sloju)|Trošak|
|---|---|
|Prvi 1000/mjesec|Uključene osnovni trošak|
|1000-100,000/mjesec|$0.03 veze/mjesec|
|100,000 500 000/mjeseca|$0.025 veze/mjesec|
|Putem 500 000/mjesec|$0.015 veze/mjesec|

>[AZURE.NOTE] 1000 brokered veze su uključeni standardne razmjenu sloju (putem osnovni trošak), a možete zajednički koristiti s redova, teme i pretplata unutar povezane Azure pretplate.

<br />

>[AZURE.NOTE] Naplata temelje se na vrh broj Istodobni veza, a je proporcionalno podijeljen zaračunava na temelju 744 sati mjesečno.

|Razina Premium
|---|
|Brokered veze se neće naplatiti u sloju Premium.|

Dodatne informacije o brokered veze u odjeljku [Najčešća pitanja vezana uz](#faq) u nastavku ovog članka.

## <a name="relay"></a>Prijenos

Preusmjeravanje dostupni su samo u standardni sloju prostora naziva. U suprotnom cijene i veze kvote za preusmjeravanje ne mijenjaju. To znači da preusmjeravanje će nastaviti naplatiti se broj poruka (ne operacije), a preusmjeravanje sati.

|Preusmjeravanje cijene|Trošak|
|---|---|
|Prijenos sata|$0.10 svakih 100 preusmjeravanja sata|
|Poruka|$0,01 svakih 10 000 poruka|

## <a name="faq"></a>NAJČEŠĆA PITANJA

### <a name="how-is-the-relay-hours-meter-calculated"></a>Način izračuna mjerača vremena preusmjeravanja?

Pogledajte [ovu temu](service-bus-faq.md#how-is-the-relay-hours-meter-calculated).

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Što su brokered veze i kako ne mogu se naplatiti ih?

Brokered veze definirana je kao nešto od sljedećeg:

1. AMQP veze sustava od klijenta servisa Bus red ili tema/pretplatu.

2. HTTP poziv primati poruke od servisa Bus temu ili reda koja ima primanje vremenskog ograničenja vrijednost veću od nule.

Servisa Bus naknade za broj Vršna Istodobni brokered veza koje premašuju uključene količina (1000 u standardni sloju). Peaks su mjeri na svaki sat temelj, proporcionalno podijeljen dijeljenjem 744 sate tijekom mjeseca i dodati putem mjesečni razdoblje naplate. Uključeni količina (1000 brokered veze mjesečno) primjenjuje se na kraju razdoblja naplate protiv zbroj Proporcionalni peaks svaki sat.

Ako, na primjer:

1. Svaki od 10 000 uređaji povezuje putem jedne AMQP veze, a prima naredbe iz servisa Bus temu. Uređaje telemetrijskih događaje poslati koncentratora za događaj. Ako se svi uređaji povezati 12 sati svakog dana, na sljedeće veze naplaćuje (osim bilo koje druge usluge Bus tema troškove): 10 000 veze *12 sati* 31 dan / 744 = 5000 brokered veze. Nakon mjesečni odbitak 1000 brokered veze bi se naplatiti 4000 brokered veze, brzina $0.03 po brokered veze, za Ukupno $120.

2. 10 000 uređaji poruka iz reda Bus servisa putem HTTP-a, pri određivanju vremensko ograničenje od nule. Ako se svi uređaji povezati 12 sati svakodnevno, vidjet ćete sljedeće veze naknada (osim bilo koje druge troškove servisa Bus): 10 000 primanje HTTP veza *12 sati dnevno* 31 dan / 744 sati = 5000 brokered veze.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Nemojte brokered veze naplaćuje redovima i teme/pretplate?

Da. Postoje bez naknade veze za slanje događaja putem HTTP, bez obzira na to koliko slanje sustavi ili uređaji. Primanje događaje s HTTP pomoću vremenskog ograničenja veći od nule, ponekad se zove "Dugi provjere", generirat će naknade za brokered veze. Veze AMQP generirati naknade brokered veze bez obzira na to jesu li koriste veze za slanje i primanje. Imajte na umu da 100 brokered veze su dopuštene bez troškova u osnovni naziva. To je i najveći je broj brokered veze dopušten za Azure pretplatu. Prvih 1000 brokered veze putem svi standardni prostori naziva u Azure pretplati su uključeni bez dodatnih troškova (osim osnovni trošak). Budući da su te dostupan dovoljno da prekrije mnogo scenariji za razmjenu servis za servis, naknade za brokered veze obično samo postaju odgovarajući ako namjeravate koristiti AMQP ili HTTP dugu-ankete s velikim brojem klijenata; Primjerice, da biste postigli učinkovitiji strujanje događaj ili omogućite dvosmjerni komunikaciju s više uređaja ili aplikacija instance.

## <a name="next-steps"></a>Daljnji koraci

- Više pojedinosti o cijene servisa Bus potražite u članku [Bus servisa Azure cijene stranice](https://azure.microsoft.com/pricing/details/service-bus/).

- Neke uobičajene najčešća pitanja oko servisa bus cijene i naplati potražite u članku [Najčešća pitanja vezana uz servis Bus](service-bus-faq.md#service-bus-pricing) .

[Azure klasični portal]: http://manage.windowsazure.com