<properties
    pageTitle="Stvaranje indeksa pretraživanja Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Što je indeks pretraživanja Azure i kako se koristi?"
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index"></a>Stvaranje indeksa pretraživanja Azure
> [AZURE.SELECTOR]
- [Pregled](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [OSTALE](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Što je indeks?

*Indeks* je stalni spremište *dokumenata* i drugih konstrukta servis za pretraživanje Azure koristi. Dokument je jedna jedinica mogu pretraživati podatke u indeks. Na primjer, programa e-trgovine prodavača možda dokumenta za svaku stavku oni Prodaja, novosti tvrtka ili ustanova možda dokumenta za svaki članak itd. Mapiranje koncepte na ekvivalentima poznatiji baze podataka: *index* pojmovno slično kao *tablicu*, a *dokumenata* nalaze otprilike jednake *redaka* u tablici.

Kada ste dodali i prijenos dokumenata i slanje upita za pretraživanje da biste pretraživanje Azure, slanja na zahtjeve za određene indeksa u servis za pretraživanje.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Vrsta polja i atribute u indeks pretraživanja Azure

Pri definiranju sheme, morate navesti ime, vrsta i atribute svako polje indeksa. Vrsta polja raspoređuje podacima pohranjenima u tom polju. Atributi postavljene na pojedinačna polja da biste odredili kako se koristi polje. U tablicama u nastavku Nabrajanje vrste i atributi koje možete odrediti.


### <a name="field-types"></a>Vrsta polja
|Vrsta|Opis|
|------------|-----------|
|*Edm.String*|Tekst koji se po želji možete tokenized za pretraživanje po cijelom tekstu (prepoznavanje riječi korijen riječi, itd).|
|*Collection(EDM.String)*|Popis nizovi koji po želji možete tokenized za pretraživanje po cijelom tekstu. Nema theoretical gornjem ograničenja broja stavki u zbirci, ali 16 MB gornju granicu veličine tereta odnosi se na zbirke.|
|*Edm.Boolean*|Sadrže vrijednosti true i false.|
|*Edm.Int32*|32-bitnu cjelobrojnu vrijednosti.|
|*Edm.Int64*|64-bitnu cjelobrojnu vrijednosti.|
|*Edm.Double*|Dvostruke preciznosti numeričke podatke.|
|*Edm.DateTimeOffset*|Vrijeme vrijednosti prikazane u obliku OData V4 datuma (npr. `yyyy-MM-ddTHH:mm:ss.fffZ` ili `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Pokažite na koji predstavlja zemljopisni globus.|

Možete pronaći više informacija o Azure pretraživanje [podržane vrste podataka na MSDN-u](https://msdn.microsoft.com/library/azure/dn798938.aspx).



### <a name="field-attributes"></a>Atributi polja
|Atribut|Opis|
|------------|-----------|
|*Ključ*|Niz koji daje jedinstveni ID za svaki dokument, koji se koristi za potražili dokument. Svaki indeks mora imati jednu tipku. Samo jedno polje može biti tipku pa vrstu mora biti postavljeno na Edm.String.|
|*Veličina*|Određuje hoće li se polja mogu vratiti u rezultatu pretraživanja.|
|*Koje je moguće filtrirati*|Omogućuje polja koja će se koristiti u upitima filtar.|
|*Koje je moguće sortirati*|Omogućuje upita da biste sortirali rezultate pretraživanja putem to polje.|
|*Facetable*|Omogućuje polja koja će se koristiti u [slojevito navigacijske](search-faceted-navigation.md) strukture korisnika koja se sama usmjerenog filtriranja. Obično polja koji se ponavljaju vrijednostima koje možete koristiti za grupiranje većeg broja dokumenata (na primjer, većeg broja dokumenata koji ulaze u jednom marku ili kategorija servisa) najbolje funkcioniraju kao pozornici.|
|*Pretraživanja*|Označava polje kao cijelog teksta koji se može pretraživati.|

Možete pronaći dodatne informacije o Azure pretraživanje [atribute indeksa na MSDN-u](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## <a name="guidance-for-defining-an-index-schema"></a>Upute za definiranje shemom indeksa

Dizajnirate indeks, potrebno vrijeme u faze planiranja razmišljati kroz svaki odluka. Važno da zadržite potrebama pretraživanje korisničko sučelje i business na umu pri dizajniranju indeks, kao što je svako polje mora biti dodijeljena [proper atribute](https://msdn.microsoft.com/library/azure/dn798941.aspx)je. Promjena indeksa nakon što je uvedena uključuje Zatvori podatke i ponovno stvaranje.


Ako promijenite potrebama za pohranu podataka tijekom vremena, možete povećati ili smanjiti kapaciteta dodavanjem ili uklanjanjem particije. Detalje potražite u članku [Upravljanje servisa za pretraživanje u Azure](search-manage.md) ili [Ograničenja servisa](search-limits-quotas-capacity.md).
