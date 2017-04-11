<properties
    pageTitle="Upute za izvore podataka profila"
    description="Upute u članku se isticanje kako uključiti tablice i stupca razina podataka profila prilikom registracije izvora podataka u katalogu podataka Azure te kako koristiti profili podataka da biste shvatili izvora podataka."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Izvore podataka profila

## <a name="introduction"></a>Uvod

**Katalog podataka za Microsoft Azure** je servis potpuno upravljanih oblaka koja služi kao sustav Registracija i sustava otkrivanje za izvore podataka tvrtke. Drugim riječima, **Katalog podataka Azure** je pomoć osobe otkrivanje, razumijevanje i korištenje izvora podataka i tvrtkama ili ustanovama pomoć da biste dobili dodatne vrijednost iz svoje postojeće podatke. Kada je izvor podataka registriran za **Katalog podataka Azure**, njegove metapodatke je kopirali i indeksirati servis, ali priče ne postoji Završi.

Značajka **Podataka Profiliranje** **Katalog podataka Azure** pregledava podatke iz podržani izvori podataka u katalogu i prikuplja statistike i informacije o tim podacima. Da biste uključili profila imovine podataka jednostavno je. Kada se registrirate podataka resursa, odaberite **Uključi profila podataka** u alatu za registraciju izvora podataka.

## <a name="what-is-data-profiling"></a>Što je Profiliranje podataka

Profiliranje podataka pregledava podatke u izvoru podataka registracije i prikuplja statistike i informacije o tim podacima. Tijekom otkrivanje izvora podataka, te statistike pojednostavljuju određivanje prikladnosti podataka da biste riješili problem svoje tvrtke.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Sljedećih izvora podataka podržava Profiliranje podataka:

- Tablice sustava SQL Server (uključujući baze podataka SQL Azure i Azure SQL Data Warehouse) i prikaza
- Oracle tablice i prikaze
- Teradata tablice i prikaze
- Grozd tablice

Obuhvaća profili podataka prilikom registracije imovine podataka korisnicima olakšava odgovarati na pitanja o izvorima podataka, uključujući:

-   Može se koristiti da biste riješili problem tvrtke?
-   Ne podatke odgovaraju određenom Standardi i uzorcima?
-   Što su anomalies izvora podataka?
-   Što su moguća izazove integracije tih podataka u moje aplikacije?

> [AZURE.NOTE] Dokumentacija možete dodati i na imovinu opisuje kako se podataka nije moguće integrirati u aplikaciju. Saznajte [kako dokumenta izvora podataka](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Kako uključiti podataka profila prilikom registracije izvora podataka

Jednostavno je uključiti profila izvora podataka. Kada se registrirate izvor podataka u oknu **objekata Registriranje** alata za registraciju izvora podataka, odaberite **Uključi podataka profila**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Dodatne informacije o registraciji izvora podataka potražite u članku [kako registrirati izvore podataka](data-catalog-how-to-register.md) i [Početak rada s Azure kataloga podataka](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtriranje podataka resursi koji sadrže podatke profila
Da biste otkrili imovine podatke koji sadrže podatke profila, možete uključiti `has:tableDataProfiles` ili `has:columnsDataProfiles` kao jedan od pojmova za pretraživanje.

> [AZURE.NOTE] Odabir **Uključiti profila podataka** u alatu za registraciju izvora podataka obuhvaća tablice i informacije o profilu razini stupca. Međutim, API katalog podataka omogućuje imovinu podataka koju želite biti registriran u samo jedan skup informacije o profilu uključena.

## <a name="viewing-data-profile-information"></a>Prikaz podataka profila

Kada pronađete odgovarajući izvor s profilom, možete pogledati detalje o profilu podataka. Da biste vidjeli profil podataka, odaberite podatke resursa, a zatim **Profil podataka** u prozoru portala kataloga podataka.

![](media\data-catalog-data-profile\data-catalog-view.png)

Profil podataka u **Katalogu podataka Azure** prikazuje tablice i stupca profila informacije uključujući:

### <a name="object-data-profile"></a>Objekt podataka profila

-   Broj redaka
-   Promjena veličine tablice
-   Kada je objekt zadnje ažuriranje

### <a name="column-data-profile"></a>Stupac podataka profila

- Vrsta podataka stupca
- Broj različitih vrijednosti
- Broj redaka s vrijednosti NULL
- Minimum, maksimum, average i standardne devijacije za vrijednosti stupaca

## <a name="summary"></a>Sažetak
Podaci Profiliranje nudi Statistika i podatke o imovini registriranih podataka da biste utvrdili prikladnosti podatke koje želite riješiti probleme tvrtke. Uz Opaske i dokumentiranje izvora podataka, profili podataka možete korisnicima obuhvaćaju razumijevanje podataka.


## <a name="see-also"></a>Vidi također
-   [Kako registrirati izvora podataka](data-catalog-how-to-register.md)
-   [Početak rada s Azure kataloga podataka](data-catalog-get-started.md)
