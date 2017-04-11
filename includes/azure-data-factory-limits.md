Tvorničke podataka je servis više klijentu koji ima sljedeća ograničenja zadano mjesto da biste bili sigurni iz svih ostalih radnih opterećenja koji su zaštićeni kupca pretplate. Mnoge od ograničenja mogu se jednostavno potenciju za vašu pretplatu do maksimalno ograničenje tako da se obratite podršci. 

**Resurs** | **Zadano ograničenje** | **Maksimalno ograničenje**
-------- | ------------- | -------------
factories podataka u pretplatu za Azure | 50 | [Kontaktiranje službe za podršku](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
kanali unutar podataka tvorničke | 2500 | [Kontaktiranje službe za podršku](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
skupove podataka unutar podataka tvorničke | 5000 | [Kontaktiranje službe za podršku](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Istodobni isječke po skupu podataka | 10 | 10
bajta po objekt za kanal objekata <sup>1</sup> | 200 KB | 2000 KB
bajtova po objektu za skup podataka i objekata povezanih servisa <sup>1</sup> | 100 KB | 2000 KB
HDInsight klaster osvježavati jezgri unutar pretplate <sup>2</sup> | 48 | [Kontaktiranje službe za podršku](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Oblak podataka premještanja jedinica <sup>3</sup> | 8 | [Kontaktiranje službe za podršku](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Ponovite broj za kanal aktivnosti | 1000 | MaxInt (32-bitna)

<sup>1</sup> kanal, skup podataka i objekata povezanih servisa predstavljaju logičke grupiranje svoje radno opterećenje. Ograničenja za sljedeće objekte odnose se na količinu podataka možete premjestiti i obrada sa servisom Azure podataka tvorničke. Tvorničke podataka osmišljene su da biste skalirali učiniti petabytes podataka.

<sup>2</sup> na zahtjev HDInsight jezgri dodijeliti su iz pretplate koja sadrži tvorničke podataka. Zbog toga iznad ograničenje je tvorničke podataka nametnuo ograničenja core jezgri HDInsight na zahtjev i razlikuje se od ograničenja core povezanog s pretplatom na Azure.

<sup>3</sup> oblaka podataka premještanja jedinica (DMU) koristi se u operaciji kopiju oblaka u oblak. Je mjera koja predstavlja power (kombinacija procesora i memorije Dodjela resursa za mrežni) jedna jedinica u tvorničke podataka. Veću propusnost kopiju možete postići tako da više DMUs nekim scenarijima za korištenje. Pogledajte odjeljak [jedinice za premještanje podataka oblaka](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) detalja.

**Resurs** | **Zadani donje granice** | **Minimalna ograničenja**
-------- | ------------------- | -------------
Interval za planiranje rasporeda | 15 minuta | 15 minuta
Interval između pokušaja Ponovi | 1 sekunde | 1 sekunde
Ponovite vrijednost vremenskog ograničenja | 1 sekunde | 1 sekunde


### <a name="web-service-call-limits"></a>Ograničenja poziva servisa za web

Azure Voditelj resursa ima ograničenja za API poziva. Možete upućivati pozive API brzinom unutar [ograničenja Azure resursima API -JA](../azure-subscription-service-limits.md#resource-group-limits). 


