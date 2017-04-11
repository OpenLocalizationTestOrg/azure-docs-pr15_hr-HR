<properties
   pageTitle="Stvaranje indeksa za dokumente na više jezika u pretraživanju Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka"
   description=" Azure pretraživanja podržava 56 jezika, korištenje jezika analyzers Lucene i obradu prirodnim jezikom tehnologije od Microsofta."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Stvaranje indeksa za dokumente na više jezika u pretraživanju Azure
> [AZURE.SELECTOR]
- [Portal](search-language-support.md)
- [OSTALE](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Unleashing power analyzers jezik je jednostavno: potrebno je postavka jedno svojstvo koje je moguće polje u definicija indeksa. Sada možete učiniti ovaj korak na portalu.

U nastavku su snimke zaslona blades Azure Portal za Azure pretraživanje koje korisnicima omogućuje definiranje shemom indeksa. Iz ovog plohu korisnici mogu stvoriti sva polja i postavite svojstvo alata za analizu za svaki od njih.

> [AZURE.IMPORTANT] Možete postaviti samo za analizu jezika tijekom definicija polja, kao u prilikom stvaranja novog indeksa na prema gore ili prilikom dodavanja novog polja u postojeći indeks. Provjerite jesu li potpuno sve atribute, uključujući alata za analizu, prilikom stvaranja polja. Ih nećete moći uređivati atribute ili promijeniti vrstu alata za analizu nakon što spremite promjene.

## <a name="define-a-new-field-definition"></a>Definiranje nove definicije polja

1. Prijava na [Portal za Azure](https://portal.azure.com) i otvorite plohu servisa servisa za pretraživanje.
2. Kliknite **Dodaj indeksa** na naredbenoj traci pri vrhu nadzorne ploče servisa da biste započeli novi indeks ili otvorite postojeći indeks upute za postavljanje programa za analizu nova polja koje dodajete u postojeći indeks.
3. Pojavit će se plohu polja, mogućnosti za koji definira shemu indeks, uključujući kartici alata za analizu služi za odabir jezika alata za analizu.
4. U odjeljku polja najprije definicija polja koja omogućuje naziv, odabir vrste podataka i postavljanje atributa da biste označili polje kao cijeli tekst pretraživanja, veličina u rezultatima pretraživanja, upotrebljivosti u fasete strukture navigacije, koje je moguće sortirati i itd.. 
5. Prije no što isprobate sljedeće polje, otvorite karticu **alata za analizu** . 

   
![][1]
*Da biste odabrali do alata za analizu, kliknite karticu za analizu na plohu polja*

## <a name="choose-an-analyzer"></a>Odabir programa za analizu

6. Pomaknite se do polja definirate. 
7. Ako još niste označili polje pretraživanja, potvrdite okvir sada da biste ga označili kao **može se pretraživati**.
8. Kliknite područje za analizu da bi se prikazao popis dostupnih analyzers.
9. Odaberite Alat za analizu koji želite koristiti.

![][2]
*Odaberite jednu od podržanih analyzers za svako polje*

Prema zadanim postavkama, sva polja pretraživanja pomoću [alata za analizu standardni Lucene](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) koji je agnostic jezik. Da biste pogledali čitavog popisa podržanih analyzers, potražite u članku [Podršku u pretraživanju Azure](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Kada alat za analizu jezik je odabran za polje, će se koristiti uz svaki zahtjev za indeksiranje i pretraživanja za to polje. Kada na temelju više polja pomoću različitih analyzers se izda upita, upit obrađuju neovisno tako da desnom analyzers za svako polje.

Mnoge web i mobilnim aplikacijama posluživanje korisnici oko globusa na različitim jezicima. Moguće je definirati i indeks za scenarij ovako tako da stvorite polje za svaki jezik koji podržava.

![][3]
*Definicija indeksa s poljem opis za svaki jezik koji podržava*

Ako je poznato jezik agent izdavanja upita, zahtjev za pretraživanjem može biti iz djelokruga određenom polju pomoću parametra upita **searchFields** . Sljedeći upit će izdati samo na temelju opisa u poljski:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

Upit možete poslati indeks, na portalu pomoću **programa explorer za pretraživanje** da biste zalijepili u upitu slično onome u gore navedenoj sintaksi. Pretraživanje explorer nije dostupna na naredbenoj traci u plohu servisa. Detalje potražite u članku [upita indeksu pretraživanja Azure na portalu](search-explorer.md) .

Ponekad jezik agent izdavanja upita nije poznat, u tom slučaju upit možete izdati protiv sva polja istodobno. Ako je potrebno, preferencama rezultate na određenim jezicima može se definirati pomoću [bilježenje rezultata profila](https://msdn.microsoft.com/library/azure/dn798928.aspx). U primjeru u nastavku pronađene u opisu na engleskom će biti testu dobije višu odnosu podudaranja poljski i francuski:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Ako ste .NET za razvojne inženjere, imajte na umu da možete konfigurirati jezika analyzers pomoću [SDK za .NET Azure pretraživanja](http://www.nuget.org/packages/Microsoft.Azure.Search). Najnovije izdanje obuhvaća podršku za na Microsoft jezik analyzers kao i.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
