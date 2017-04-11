<properties
    pageTitle="Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije | Microsoft Azure"
    description="Konceptualni reference i pregled o značajci sinkroniziranja izvanmrežne podatke za Azure mobilne aplikacije"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Sinkronizacija izvanmrežnih podataka u Azure mobilne aplikacije

## <a name="what-is-offline-data-sync"></a>Što je sinkronizacija izvanmrežne podatke?

Sinkronizacija izvanmrežnih podataka je postupak klijentske i poslužiteljske SDK značajka Azure mobilne aplikacije koja olakšava inženjerima omogućuje stvaranje aplikacije koje su funkcionalne bez mrežne veze.

Kada je aplikacija u izvanmrežnom načinu rada, korisnici i dalje možete stvarati i mijenjati podatke koji će biti spremljene u lokalnom spremištu. Kada je aplikaciju vratite u mrežni je s pozadinskom vaše Azure mobilnu aplikaciju sinkronizirajte lokalne promjene. Značajka obuhvaća i podršku za otkrivanje sukoba nakon isti zapis o klijentu i pozadinski. Sukobe se zatim može riješiti ili na poslužitelju ili klijent.

Izvanmrežna sinkronizacija sadrži brojne prednosti:

* Predmemoriranje podatke server lokalno na uređaj poboljšati odziv aplikacije
* Stvaranje robusne aplikacije koje ostaju korisna kad postoje problemi s mrežom
* Dopusti krajnji korisnici možete stvarati i mijenjati podatke, čak i kad je pristup mreži nema podrške scenariji s povezivanjem malo ili nimalo
* Sinkronizacija podataka na više uređaja i otkriti sukobe prilikom isti zapis je izmijenio dva uređaja
* Ograničite korištenje mreže na Visoka Latencija ili s ograničenim prometom mreža

Sljedeći vodiči za prikaz kako dodati izvanmrežnu sinkronizaciju mobilnim klijentima pomoću Azure mobilne aplikacije:

* [Android: Omogućivanje izvanmrežne sinkronizacije]
* [iOS: Omogućivanje izvanmrežne sinkronizacije]
* [Xamarin iOS: Omogućivanje izvanmrežne sinkronizacije]
* [Xamarin Android: Omogućivanje izvanmrežne sinkronizacije]
* [Xamarin.Forms: Omogućivanje izvanmrežne sinkronizacije](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Univerzalni platformu sustava Windows: Omogućivanje izvanmrežne sinkronizacije]

## <a name="what-is-a-sync-table"></a>Što je tablica sinkronizaciju?

Da biste pristupili krajnju točku "/ tablice", klijent za Azure Mobile SDK-ovi pružaju sučelja kao što su `IMobileServiceTable` (.NET klijent SDK) ili `MSTable` (iOS klijent). Ove API-ji izravno povezivanje s pozadinskom Azure mobilne aplikacije i neće uspjeti ako klijentskog uređaja povezani s mrežom.

Podržava izvanmrežno korištenje aplikacije trebali biste umjesto toga koristite *sinkronizaciju tablice* API-ji, kao što su `IMobileServiceSyncTable` (.NET klijent SDK) ili `MSSyncTable` (iOS klijent). Iste CRUD operacija (Stvaranje, čitanje, ažuriranje, brisanje) radi odnosu tablica sinkronizaciju API-ji, osim što sada će čitanje ili pisanje *lokalno spremište*. Prije nego što se može izvršiti sve operacije sinkronizaciju tablice, lokalno spremište potrebno pokrenut.

## <a name="what-is-a-local-store"></a>Što je lokalno spremište?

Lokalno spremište je sloj postojanost podacima na klijentskog uređaja. Azure mobilne aplikacije klijenta SDK-ovi sadrže zadane implementacije lokalno spremište. U sustavu Windows, Xamarin i Android se temelji na SQLite; na iOS, se temelji na temeljni podaci.

Da biste koristili implementaciju SQLite utemeljen na Windows Phone ili Windows 8.1 trgovine, morate instalirati nastavkom SQLite. Dodatne informacije potražite u članku [univerzalni platformu sustava Windows: Omogućivanje izvanmrežne sinkronizacije]. Android i iOS isporuka s verzijom SQLite u operacijskom sustavu uređaj, tako da nije potrebno upućuju na vlastitu verziju SQLite.

Razvojni inženjeri mogli implementirati i vlastite lokalno spremište. Na primjer, ako želite spremiti podatke u obliku šifrirane na mobilnom klijentu možete definirati lokalno spremište koji koristi SQLCipher za šifriranje.

## <a name="what-is-a-sync-context"></a>Što je kontekst za sinkronizaciju?

*Sinkronizacija kontekst* vezan uz mobilnu verziju klijentskog objekta (kao što su `IMobileServiceClient` ili `MSClient`) te prati promjene načinjene s tablicama za sinkronizaciju. Kontekst za sinkronizaciju zadržava programa *reda čekanja postupak* koji čuva numerirani popis CUD operacija (Stvaranje, ažuriranje, brisanje) koje kasnije poslat će se na poslužitelj.

Lokalno spremište je povezan s kontekst sinkronizaciju kao što su pomoću metode inicijalizaciju `IMobileServicesSyncContext.InitializeAsync(localstore)` u [klijent za .NET SDK].

## <a name="how-sync-works"></a>Izvanmrežni kako funkcionira sinkronizacija

Prilikom korištenja sinkronizacije tablice, kod klijenta kontrolira kada lokalne promjene sinkronizirat će se s pozadinskom programa Azure mobilnu aplikaciju. Ništa se šalje pozadinski sve dok se poziv *automatske* lokalne promjene. Isto tako, lokalno spremište je popunjen nove podatke samo kada je poziv za *izvlačenje* podataka.

* **Automatske**: automatske je operaciju u kontekstu sinkronizaciju i šalje sve promjene CUD od posljednjeg automatske. Imajte na umu da nije moguće poslati samo promjene pojedinačnih tablica jer u suprotnom operacije može biti poslana izvan redoslijeda. Automatske izvršava niz OSTALE pozive na vaše pozadinskog Azure mobilnu aplikaciju koja će shodno izmjena bazu podataka poslužitelja.

* **Povlačenje**: istaknuti se izvršava na temelju stvaranjem tablice i može se prilagoditi s upitom želite dohvatiti samo podskup podataka poslužitelja. Azure mobilnog klijenta SDK-ovi umetnite podatke rezultata u lokalnom spremištu.

* **Implicitno ih gura**: Ako je istaknuti se izvršava odnosu tablica koja Neriješeno lokalne ažuriranja, na istaknuti će najprije izvršiti s automatske na kontekst za sinkronizaciju. Omogućuje minimiziranje sukoba između promjene koje su već u redu čekanja i nove podatke s poslužitelja.

* **Rastuća sinkronizacije**: prvi parametar da biste istaknuti postupak je *naziv upita* koji se koristi samo na klijentskom računalu. Ako koristite naziv upite koje nisu null, Azure Mobile SDK će izvesti *rastuće sinkronizaciju*.
  Svaki put kada istaknuti postupak vraća skup rezultata, telefonski najnovijih `updatedAt` vremensku oznaku iz tog skupa rezultata su spremljeni u tablicama za lokalni sustav SDK. Operacija koja slijedi istaknuti samo dohvaćanje zapisa nakon te vremensku oznaku.

  Da biste koristili rastuće sinkronizaciju, na poslužitelju mora vratiti smisleni `updatedAt` vrijednosti i moraju također podržavati sortiranje po tom polju. Međutim, budući da u SDK dodaje vlastitu sortiranje na polju updatedAt, ne možete koristiti istaknuti upit koji ima vlastitu `$orderBy$` uvjet.

  Naziv upita može biti bilo koji niz znakova koje odaberete, ali mora biti jedinstvena za svaki logičke upit u aplikaciji.
  U suprotnom različite istaknuti operacije može prebrisati isti rastuće sinkronizaciju vremenska oznaka i upite možete vratiti netočne rezultate.

  Ako je upit parametra, jedan od načina za stvaranje upita jedinstveni naziv je za unos vrijednosti parametra.
  Na primjer, ako filtrirate na korisnički ID, naziv upita može biti na sljedeći način (u C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Ako želite da biste se uključili rastuća nesinkroniziranost, prenesite `null` kao ID upita U ovom slučaju, svi zapisi će biti dohvaćeni na svakoj poziv `PullAsync`, koji nije potencijalno.

* **Purging**: sadržaj korištenja lokalno spremište možete očistiti `IMobileServiceSyncTable.PurgeAsync`.
  To može biti potrebno ako imate zastarjele podatke u klijentskoj bazi ili ako želite da biste odbacili sve promjene na čekanju.

  Na čišćenje će izbrisati tablicu iz lokalno spremište. Ako postoje operacije na čekanju sinkronizacija s poslužiteljem baze podataka, na čišćenje će vratiti iznimku, osim ako je parametar *prisilno Pročistite* postavljen.

  Kao primjer zastarjele podatke na klijentskom računalu pretpostavimo da u primjeru "obveze popis" Device1 povlači samo stavke koje su dovršeni. Nakon toga na todoitem "Kupi mlijeko" označen dovršiti na poslužitelju neki drugi uređaj. Međutim, Device1 će i dalje imati todoitem "Kupi mlijeko" u lokalnom spremištu jer samo izvlačenja stavke koje su označene dovršeno. Na čišćenje će poništite tu zastarjele stavku.

## <a name="next-steps"></a>Daljnji koraci

* [iOS: Omogućivanje izvanmrežne sinkronizacije]
* [Xamarin iOS: Omogućivanje izvanmrežne sinkronizacije]
* [Xamarin Android: Omogućivanje izvanmrežne sinkronizacije]
* [Univerzalni platformu sustava Windows: Omogućivanje izvanmrežne sinkronizacije]

<!-- Links -->
[.NET klijent SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Omogućivanje izvanmrežne sinkronizacije]: app-service-mobile-android-get-started-offline-data.md
[iOS: Omogućivanje izvanmrežne sinkronizacije]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: Omogućivanje izvanmrežne sinkronizacije]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Omogućivanje izvanmrežne sinkronizacije]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Univerzalni platformu sustava Windows: Omogućivanje izvanmrežne sinkronizacije]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
