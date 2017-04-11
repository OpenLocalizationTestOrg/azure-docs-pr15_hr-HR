<properties
    pageTitle="Stvaranje servis za pretraživanje Azure pomoću portala za Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
    description="Saznajte kako Dodjela resursa za servis za pretraživanje Azure pomoću portala za Azure."
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

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Stvaranje servis za pretraživanje Azure pomoću portala za Azure

Ovaj vodič će vas voditi kroz postupak stvaranja (ili dodjele resursa) servis za pretraživanje Azure pomoću [Portala za Azure](https://portal.azure.com/).

Ovaj vodič pretpostavlja da već imate pretplatu na Azure i prijavite se u Portal za Azure.

## <a name="find-azure-search-in-the-azure-portal"></a>Pronalaženje Azure pretraživanja na portalu za Azure
1. Idite na [Portal za Azure](https://portal.azure.com/) i prijavite se u.
1. Kliknite znak plus ("+") u gornjem lijevom kutu.
2. Odaberite **podatke + prostor za pohranu**.
3. Odaberite **pretraživanje Azure**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Odaberite naziv usluge i URL krajnje točke servisa
1. Naziv servisa će biti dio URL ADRESU krajnje točke servisa Azure pretraživanja prema kojima će upućivati pozive API-JA za upravljanje i korištenje servisa za pretraživanje.
2. Upišite naziv servisa u polje **URL** . Naziv usluge:
  * mora sadržavati samo mala slova, brojki ili crtice ("-")
  * ne možete koristiti crticu ("-") kao prva 2 znakove ili zadnji znak
  * ne smije sadržavati uzastopne crtice (",")
  * ograničen između 2 i 60 znakova


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Odaberite pretplatu koju će zadržati na servisu
Ako imate više pretplata, možete odabrati koji će neće sadržavati stavku ovaj servis za pretraživanje Azure.

## <a name="select-a-resource-group-for-your-service"></a>Odaberite grupu resursa za uslugu
Stvorite novu grupu resursa ili odaberite postojeći. Grupa resursa je zbirka web-mjesto servisa Azure i u okvir za resurse koji se zajednički koriste. Na primjer, ako koristite Azure pretraživanja indeksirati SQL baze podataka, zatim od tih servisa mora biti dio iste grupe resursa.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Odaberite mjesto na kojem će se nalaziti na servisu
Kao Azure service Azure pretraživanja da bi se nalaziti u podatkovnim centrima diljem svijeta. Napomena taj [cijene mogu se razlikovati](https://azure.microsoft.com/pricing/details/search/) za Zemljopis.

## <a name="select-your-pricing-tier"></a>Odaberite vaše cijene sloju
[Pretraživanje Azure trenutno nude na više razine cijena](https://azure.microsoft.com/pricing/details/search/): slobodno, osnovni ili Standard. Svaki sloju ima vlastite [kapacitetu i ograničenja](search-limits-quotas-capacity.md). Upute potražite u članku [Odabir cijene sloju ili SKU-om](search-sku-tier.md) .

U ovom slučaju ne možemo ste odabrali standardni sloju za naših usluga.

## <a name="select-the-create-button-to-provision-your-service"></a>Odaberite gumb "Stvori" Dodjela usluge

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Promjena veličine na servisu

Kada na servisu dodijeljeni resursi, možete mjerilo da bi odgovarao vašim potrebama. Ako ste odabrali standardni sloju servisa za pretraživanje Azure, skaliranje uslugu u dvije dimenzije: replike i particije. Ako ste odabrali osnovni sloju, možete dodati samo replike.

*__Particije__* omogućuju usluge za pohranu i pretraživati više dokumenata.

*__Replike__* omogućuju usluge za rukovanje veći učitavanje upita za pretraživanje – [servis potrebna 2 replike da biste postigli SLA koja je samo za čitanje, a zahtijeva 3 replike da biste postigli SLA za čitanje/pisanje](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Idite na plohu upravljanje Azure pretraživanje servisa na portalu za Azure.
2. U plohu **Postavke** odaberite **Skala**.
3. Na servisu mogu mijenjati veličinu dodavanjem replike ili particije.
  * Nije moguće skaliranje na servisu proteklih jedinice 36 pretraživanja. Ukupan broj jedinica pretraživanje je proizvod replike i particije (replike * particije = ukupan jedinice za pretraživanje).
  * Ako ste odabrali osnovni sloju, samo možete skaliranje 3 replike. Osnovne servise povezane su s jedna particija.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Sljedeći
Nakon dodjele resursa za servis za pretraživanje Azure, vi ćete biti spreman za [Definiranje indeks pretraživanja Azure](search-what-is-an-index.md) da biste mogli prenijeti i pretraživanje podataka.

Potražite u članku [Početak rada s Azure pretraživanja na portalu](search-get-started-portal.md) za Brzi vodič.
