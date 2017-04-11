<properties
 pageTitle="Tarife i naplata u Azure rasporeda"
 description="Tarife i naplata u Azure rasporeda"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Tarife i naplata u Azure rasporeda

## <a name="job-collection-plans"></a>Tarife zbirke posla

Zadatak zbirke su naplatu entitet u Azure raspored. Posao zbirke sadrže broj zadataka i Dođite na tri tarife – slobodno, standardna i Premium – koji su opisani u nastavku.

|**Planiranje zbirke posla**|**Maksimalni broj zadataka po zbirci posla**|**Max ponavljanja**|**Max posao zbirke po pretplati**|**Ograničenja**|
|:---|:---|:---|:---|:---|
|**Besplatni**|5 zadataka po zbirci posla|Svaki sat. Nije moguće izvršavanje češće od jednom na sat poslova|Pretplata je dopušteno do 1 zbirke besplatne posla|Ne možete koristiti [HTTP izlaznog autorizacije objekta](scheduler-outbound-authentication.md)
|**Standardna**|50 zadataka po zbirci posla|Jednom u minuti. Nije moguće izvršavanje češće od svake minute poslova|Pretplata je dopušteno do 100 zbirke standardne posla|Pristup cjelokupan skup značajki sustava rasporeda|
|**P10 Premium**|50 zadataka po zbirci posla|Jednom u minuti. Nije moguće izvršavanje češće od svake minute poslova|Pretplata je dopušteno do 10 000 zbirki P10 Premium posao. Dodatne informacije <a href="mailto:wapteams@microsoft.com">obratite nam</a> se.|Pristup cjelokupan skup značajki sustava rasporeda|
|**P20 Premium**|Poslovi 1000 po zbirci posla|Jednom u minuti. Nije moguće izvršavanje češće od svake minute poslova|Pretplata je dopušteno do 10 000 zbirki P20 Premium posao. Dodatne informacije <a href="mailto:wapteams@microsoft.com">obratite nam</a> se.|Pristup cjelokupan skup značajki sustava rasporeda|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Nadogradnji i Downgrades posao zbirke tarife

Možete nadograditi ili prijeći na nižu posao zbirke plan bilo kojem trenutku među tarife slobodno, standardna i Premium. Međutim, kad prijelaz na stariju besplatne posao zbirku, na nadogradnju na stariju verziju možda neće uspjeti zbog nekog od sljedećih razloga:

- Zbirka besplatne posla već postoji u pretplate
- Zadatak u zbirci posla sadrži veći ponavljanja od dopuštenog za zadatke u zbirkama besplatne posao. Maksimalno ponavljanja dopušteno u zbirci besplatne posao se svaki sat
- Nema više od 5 zadacima u zbirci posla
- Zadatak u zbirci posao ima HTTP ili HTTPS akciju koja koristi [HTTP izlaznog autorizacije objekta](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Tarife za naplatu i Azure

Pretplate se naplaćuju besplatno zbirke posao. Ako imate više od 100 zbirke standardne posla (10 standardne naplate jedinice), tada je bolje dijeli da bi se sve zbirke posla u planu premium.

Ako imate jednu zbirku standardne posla i jednu zbirku premium posla, su naplate standardne naplate jedinica _i_ jedan premium naplate jedinice. Računi servisa rasporeda ovisno o broju zbirki aktivan posao koja su postavljena na standardnu ili premium; To je objašnjena u sljedeća dva odjeljka.

## <a name="standard-billable-units"></a>Standardni jedinice za naplatu

Standardni jedinica za naplatu možete uključiti do 10 zbirke standardni zadatak. Jer zbirka standardni zadatak možete imati do 50 zadataka po zbirci posla, standardne naplate jedinice omogućuje pretplatu na imaju najviše 500 posla – najviše gotovo 22 milijuna posao izvršavanja mjesečno.

Ako imate između 1 i 10 zbirke standardne posla, koji će naplatiti 1 standardne jedinice za naplatu. Ako imate između 11 i 20 zbirki standardne posla, će se naplatiti za 2 standardne jedinice za naplatu. Ako imate između 21 i zbirki 30 standardne posla, će naplatiti za 3 standardne jedinice za naplatu i tako dalje.

## <a name="p10-premium-billable-units"></a>P10 Jedinice za naplatu Premium

Utrošeno jedinica premium P10 možete uključiti do 10 000 zbirki P10 premium posao. Budući da zbirku P10 premium posao možete imati do 50 zadataka po zbirci posla, premium naplate jedinice omogućuje pretplatu na imaju najviše 500 000 posla – najviše gotovo 22 milijarde posao izvršavanja mjesečno.

Ako imate između 1 i 10 000 zbirki premium posla, prikazat će se naplatiti 1 P10 premium naplate jedinice. Ako imate između 10,001 i zbirki 20 000 premium posla, prikazat će se naplatiti za 2 P10 premium naplate jedinice i tako dalje.

Dakle, P10 premium posao zbirke imaju isti funkcionalnost kao zbirke standardne posla, ali omogućuje prijeloma cijena u slučaju aplikacije potreban je mnogo posao zbirke.

## <a name="p20-premium-billable-units"></a>P20 Jedinice za naplatu Premium

P20 jedinica naplatu premium može sadržavati do 5000 P20 premium posao zbirke. Budući da zbirku P20 premium posao možete imati do 1000 zadataka po zbirci posla, premium naplate jedinice omogućuje pretplatu na imaju najviše 5,000,000 posla – najviše gotovo 220 milijarde posao izvršavanja mjesečno.

P20 premium posao zbirke nudi iste mogućnosti kao P10 premium posao zbirke, ali podržava veći broj zadataka po zbirci posla i veći ukupni broj zadataka cjelokupan od premium P10 omogućujući vam da bi više skalabilnost.

## <a name="billing-and-active-status"></a>Naplatu i aktivni Status

Zadatak zbirke uvijek su aktivni osim ako ih ima cijelu pretplatu prešla u neke privremeno onemogućene stanje zbog problema za naplatu. Jedini način da biste bili sigurni da posao zbirka je naplaćeno u postavljen je plan _slobodno_ ili brisanje zbirke posao.

Iako možete onemogućiti sve zadatke u zbirci posla u jednoj operaciji, promijenite stanje naplate zbirke posao – zbirke posao će _i dalje_ naplaćivati. Isto tako, prazan posao zbirke smatraju aktivne i neće naplaćivati.

## <a name="pricing"></a>Cijene

Cijene detalje potražite u članku [Raspored cijene](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Vidi također


 [Što je raspored?](scheduler-intro.md)

 [Azure koncepata raspored, terminologija i entitet hijerarhije](scheduler-concepts-terms.md)

 [Početak rada s raspored na portalu za Azure](scheduler-get-started-portal.md)

 [Referenca za Azure raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Azure referenca cmdleta ljuske PowerShell za raspored](scheduler-powershell-reference.md)

 [Azure visoku dostupnost raspored i pouzdanosti](scheduler-high-availability-reliability.md)

 [Azure ograničenja raspored, zadanih vrijednosti i šifre pogreške](scheduler-limits-defaults-errors.md)

 [Azure izlaznu provjeru autentičnosti rasporeda](scheduler-outbound-authentication.md)
