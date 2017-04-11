<properties
    pageTitle="Stvaranje indeksa pretraživanja Azure pomoću portala za Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Stvaranje indeksa pomoću portala za Azure."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Stvaranje indeksa pretraživanja Azure pomoću portala za Azure
> [AZURE.SELECTOR]
- [Pregled](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [OSTALE](search-create-index-rest-api.md)

U ovom se članku će vas voditi kroz postupak stvaranja Azure pretraživanje [indeksa](search-what-is-an-index.md) pomoću portala za Azure.

Prije nego što slijedi ovaj vodič i stvaranje indeksa, imat ćete već [stvorene na servis za pretraživanje Azure](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>Li. Idite na vašem plohu Azure pretraživanja
1. Kliknite na "Svi resursi" na izborniku na lijevoj strani [Portal za Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Odaberite servis za pretraživanje Azure

## <a name="ii-add-and-name-your-index"></a>II. Dodavanje i nazovite indeks
1. Kliknite gumb "Dodaj indeks"
2. Dodijelite naziv indeksu pretraživanja Azure. Budući da ne možemo stvarate kazalo da biste potražili Hoteli u ovom vodiču, ne možemo sadrže pod nazivom naš indeks "Hoteli".
  * Naziv indeksa morate počinju slovima i sadrže samo mala slova, brojki ili crtice ("-").
  * Slično naziva servisa, naziv indeksa koji ste odabrali bit će dio URL-a krajnjoj točki gdje ćete poslati HTTP zahtjeva za pretraživanje API Azure
3. Kliknite na stavku "Polja" da biste otvorili novi plohu

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Stvaranje i definirati polja indeksa
1. Odabirom stavke "Polja" novi plohu će se otvoriti s obrascem da biste unijeli definicije indeksa.
2. Dodajte polja za indeks pomoću obrasca.

  * *Ključ* polje vrste Edm.String je obavezna za svaki indeks pretraživanja Azure. Ovo polje ključa se stvara po zadanome naziv polja "ID". Ne možemo ste promijenili je "hotelId" u naš indeks.
  * Neka svojstva sheme indeksa može se postaviti samo jednom i ne može se ažurirati u budućnosti. Zbog toga sheme promjene koje je potrebno ponovno indeksiranje kao što su Promjena vrste polja nisu trenutno moguće nakon početnog konfiguracije.
  * Ne možemo pažljivo odabrali ste vrijednosti svojstava za svako polje na temelju kako ćemo mislite da će se koristiti u aplikaciji. Potrebama pretraživanje korisničko sučelje i business Imajte na umu pri dizajniranju indeks, kao što je svako polje mora biti dodijeljena [odgovarajuća svojstva](https://msdn.microsoft.com/library/azure/dn798941.aspx). Ove svojstva upravljanje prikazanim pretraživanje značajke (filtriranje faceting, sortiranje, pretraživanje po cijelom tekstu, itd.) primjenjuju se na koja se polja. Na primjer, je vjerojatno da osobe koje pretražuju za Hoteli mogla zanimati ključnu riječ rezultata na polje "Opis" pa ćemo Omogućivanje pretraživanja cijelog teksta za to polje tako da svojstvo "Može se pretraživati".
    * [Alat za analizu jezik](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) za svako polje možete postaviti i tako da kliknete na kartici "Alata za analizu" pri vrhu na plohu. Vidjet ćete ispod da ne možemo ste odabrali francuski analyzer za polje u našem indeksu namijenjen teksta na francuskom jeziku.

3. Kliknite **u redu** na plohu "Polja" da biste potvrdili definicija polja
4. Kliknite **u redu** plohu "Indeks Dodaj" da biste spremili i stvorite indeks koju ste upravo definirali.

U nastavku snimke, vidjet ćete kako ćemo pod nazivom i definirana polja za naše "Hoteli" indeks.

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Sljedeći
Kada stvorite indeks pretraživanja Azure, vi ćete biti spremni [prenijeti sadržaj u indeks](search-what-is-data-import.md) da bi se pokrenuti pretraživanje podataka.
