<properties
    pageTitle="Korištenje referenci podataka i pretraživanja tablica u strujanje analize | Microsoft Azure"
    description="Korištenje referentnih podataka u upitu strujanje Analytics"
    keywords="Tablica za traženje referentnih podataka"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Korištenje web-mjesto podataka ili u okvir za pretraživanje tablica referencu u strujanje analize ulaznog toka

Referentnih podataka (poznat i kao tablica za traženje) je konačne skup podataka koji je statične ili usporavanja mijenja u prirode, koristi za izvođenje pretraživanje ili za povezivanje s vašeg strujanja podataka. Da biste pomoću podataka referencu u vaš posao Azure strujanje analize obično koristit će na [Pridruži se referenca podataka](https://msdn.microsoft.com/library/azure/dn949258.aspx) u upitu. Analitički strujanje koristi spremište blobova platforme Azure kao sloj prostora za pohranu za referentnih podataka, a s referencom Azure podataka tvorničke podatke možete biti transformacije i/ili kopirana sa spremištem blobova platforme Azure, za upotrebu kao referencu podatke, [bilo koji broj oblaka temelje i lokalnih podataka trgovine](../data-factory/data-factory-data-movement-activities.md). Kao niz blob-ova (definirano u konfiguraciji unos) uzlaznim redoslijedom od datuma/vremena naveden u nazivu blob je katalog modeliran referentnih podataka. To podržava **samo** dodavanje do kraja niza korištenjem datuma/vremena **veća** od one koja je navedena po posljednje blob u nizu.

## <a name="configuring-reference-data"></a>Konfiguriranje referentnih podataka

Da biste konfigurirali referentnih podataka, najprije morate stvoriti ulazni koje je vrste **Referentnih podataka**. U dolje navedenoj tablici objašnjavaju sva svojstva koja morat ćete unijeti prilikom stvaranja referentnih podataka za unos s njezin opis:

<table>
<tbody>
<tr>
<td>Naziv svojstva</td>
<td>Opis</td>
</tr>
<tr>
<td>Pseudonim za unos</td>
<td>Neslužbeni naziv koji će se koristiti u upitu posao referentni taj unos.</td>
</tr>
<tr>
<td>Račun za pohranu</td>
<td>Naziv računa spremišta gdje se nalaze datoteke blob. Ako je u okviru iste pretplate kao strujanje analize posla, možete je odabrati iz na padajućem izborniku prema dolje.</td>
</tr>
<tr>
<td>Ključ za račun za pohranu</td>
<td>Tajna tipku povezanu s računom za pohranu. Dohvaća unose automatski ako je račun za pohranu u okviru iste pretplate kao posla strujanje analize.</td>
</tr>
<tr>
<td>Spremnik za pohranu</td>
<td>Spremnici sadrže logičke grupiranja za pohranjene na servisu Microsoft Azure Blob blob-ova. Kada prenesete blob Blob servisa, morate navesti spremnik za taj blob.</td>
</tr>
<tr>
<td>Put uzorak</td>
<td>Put datoteke primijenjenih radi pronalaženja blob-ova unutar navedeni kontejnera. U parametru path, odaberete Navedite jednu ili više instanci sljedeće varijable 2:<BR>{date}, {time}<BR>Primjer 1: products/{date}/{time}/product-list.csv<BR>Primjer 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Oblik datuma [neobavezni]</td>
<td>Ako ste koristili {date} unutar uzorak put koji ste naveli, možete odabrati oblik datuma u kojima su organizirane datoteka s padajućeg Podržani oblici. Primjer: Gggg/MM/DD</td>
</tr>
<tr>
<td>Oblik vremena [neobavezni]</td>
<td>Ako ste koristili {time} unutar uzorak put koji ste naveli, možete odabrati oblik vremena u kojem su organizirane datoteka s padajućeg Podržani oblici. Primjer: HH</td>
</tr>
<tr>
<td>Događaj serijaliziranog oblika</td>
<td>Da biste bili sigurni upitima funkcionira kao što ste očekivali, strujanje analize treba znati koji serijalizacije oblik koristite za dolazne strujanja podataka. Podržani oblici za referentnih podataka su CSV i JSON.</td>
</tr>
<tr>
<td>Kodiranje</td>
<td>UTF-8 trenutno samo podržani oblik kodiranja</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generiranje referentnih podataka prema rasporedu

Ako je referentnih podataka sporo promjenom skupom podataka, zatim podrške za osvježavanje referenca podataka omogućen navođenjem uzorak put u konfiguraciji unos pomoću {date} i {vremena} zamjena tokena. Definicija podataka ažurirane referenca na temelju ovaj put uzorak će obraditi strujanje analize. Ako, na primjer, uzorak `sample/{date}/{time}/products.csv` obliku datuma **"gggg-MM-DD"** i jedan oblik **"Hh"** upućuje strujanje analize mogao preuzeti ažuriranu blob `sample/2015-04-16/17:30/products.csv` pri 17:30 po na Travanj 2015 16th UTC vremena zone.

> [AZURE.NOTE] Trenutno strujanje analize zadacima potražite osvježavanja blob samo kada se vrijeme na računalu prelazi vrijeme kodirana blob naziv. Na primjer će izgledati posla `sample/2015-04-16/17:30/products.csv` čim moguće, ali ne starije od 17:30 po na Travanj 2015 16th UTC vremena zone. To će *Nikad* potražite datoteku s starija od posljednjeg onu koja je otkrio je kodirana vrijeme.
> 
> Npr. Kada posao pronalazi blob-om `sample/2015-04-16/17:30/products.csv` zanemariti će sve datoteke s datumom kodiranih starija od 17:30 po 16th Travanj 2015 pa ako na najnovije stiže `sample/2015-04-16/17:25/products.csv` blob dobiva stvorena u istoj spremniku posao neće koristiti.
> 
> Isto tako ako `sample/2015-04-16/17:30/products.csv` samo proizvodi pri 10:03 PM Travanj 2015 16th, ali nema blob s ranijeg datuma postoji u spremniku, zadatak će koristiti tu datoteku počevši od 10:03 PM 16th Travanj 2015 i koristiti prethodne referentnih podataka do zatim.
> 
> Iznimka kada posao mora ponovno obradu podataka unatrag u vremenu ili pri prvom posao pokreće. Prilikom pokretanja Tražite li najnovija blob proizvodi prije no što posao posao pokrenuti vrijeme navedeno. To možete učiniti da biste bili sigurni da postoji skupa podataka **koje nisu prazne** reference prilikom pokretanja posla. Ako nije moguće pronaći nešto, posao prikazat će se sljedeće Dijagnostika: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Tvorničke Azure podataka](https://azure.microsoft.com/documentation/services/data-factory/) mogu se orkestrirali zadatka stvaranja blob-ova ažurirane potrebnih strujanje analize da biste ažurirali definicija podataka referencu. Tvorničke podataka je oblaku podataka Integracija servis orchestrates i automatizira premještanja i transformacije podataka. Tvorničke podataka podržava [povezivanja s velikog broja u oblaku i služi za pohranu lokalnih podataka](../data-factory/data-factory-data-movement-activities.md) i premještanje podataka jednostavno redovito koji navedete. Dodatne informacije i korak po korak upute za postavljanje podataka tvorničke kanal za generiranje referentnih podataka za strujanje analizu koji se osvježavaju na unaprijed definiranih rasporeda, potražite u članku [GitHub uzorka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Savjeti o osvježavanju referentnih podataka ##

1. Prebrisivanje referenca podataka blob-Ova će uzrokovati strujanje analize da biste ponovno učitali blob-om i u nekim slučajevima može uzrokovati posla uvoza. Preporučeni način da biste promijenili referentnih podataka je da biste dodali novi blob koristeći istu spremnik i uzorak put definirano u unos posla i korištenje datuma/vremena **veća** od one koja je navedena po posljednje blob u nizu.
2.  Pregled podataka blob-ova su poredane po vremenu "Zadnje promjene" s blob, ali samo po vremena i datuma navedenog u blob-om naziv pomoću {date} i {vremena} zamjene.
3.  Na nekoliko prilike posao mora se vratili u vremenu, stoga referenca podataka blob-ova mora nije moguće promijenjen ili izbrisati.

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci
Koje ste je uvedena strujanje analize, servis za upravljane tok analize podataka s Interneta stvari. Da biste saznali više o taj servis, pogledajte:

- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
