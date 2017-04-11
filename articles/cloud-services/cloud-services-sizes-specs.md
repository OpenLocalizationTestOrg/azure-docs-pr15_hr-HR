<properties
 pageTitle="Veličina za servise u oblaku | Microsoft Azure"
 description="Popis veličine različite virtualnog računala (i ID-a) za Azure oblaka servisa web- a tempiranja uloge."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Veličina za servise u Oblaku

U ovoj se temi opisuju dostupna veličine i mogućnosti za servis u Oblaku uloga instance (web uloge i uloge suradnika). Također nudi implementaciju što je potrebno imati na umu prilikom planiranja korištenja ove resurse. Svaki veličina sadrži ID-a koji će se staviti u [datoteku definicije servisa](cloud-services-model-and-package.md#csdef).

Servisi u oblaku je jedan od nekoliko vrsta računalnim resursa nudi Azure. Kliknite [ovdje](cloud-services-choose-me.md) za dodatne informacije o servisima u Oblaku.

> [AZURE.NOTE]Da biste vidjeli povezane Azure ograničenja, pročitajte članak [Azure pretplate i ograničenja servisa, kvote, i ograničenjima](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Veličina web- a tempiranja uloga instanci

Nema više standardnih veličina možete birati na Azure. Zahtjevi za neke od tih veličine obuhvaćaju sljedeće:

* D niz VMs dizajnirani za pokretanje aplikacije koje potražnje veći računalnim power i performanse privremene diska. D niz VMs pružaju procesora koji se brže, veća omjer memorije core i solid-state pogon (SSD) za privremene disk. Detalje potražite u članku objava na blogu za Azure, [Nove veličine D niz virtualnog računala](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2 nizom, follow-on izvornu D-niz, značajke jače procesora. CPU Dv2 niz je otprilike 35% brže nego procesora D niz. Se temelji na najnovijih 2,4 GHz Intel Xeon® E5-2673 v3 procesor (Haswell) i s tehnologije 2.0 Intel Turbo isticanja možete prijeći do 3.1 GHz. Dv2 niz ima isti konfiguracije memorije i disk kao D niz.

*   G niz VMs nude najviše memorije i pokrenuti hosts koji imaju obiteljski procesori Intel Xeon E5 V3.

*   VMs odgovora niz može uvesti na različite vrste hardvera i procesori. Veličina je ograničio vrijeme, na temelju hardver nudi dosljedan procesor izvedbe pokrenute instance, bez obzira na to hardver je u upotrebi na. Da biste utvrdili fizički hardverski na koji je implementiran ovu veličinu, upit virtualne hardver iz unutar virtualnog računala.

*   Veličina A0 je previše ste pretplaćeni na fizičke hardveru. Za ovaj određene veličine samo, na druge korisnike implementacijama mogu utjecati performanse sustava izvodi radno opterećenje. Relativna performanse obrubljena je ispod kao očekivani osnovne crte, primjenjuju djelomičnog raznolikosti od 15 posto.


Veličina virtualnog računala utječe na cijene. Veličina utječe i obradi dokumenata, memorije i prostora za pohranu kapacitet virtualnog računala. Troškovi spremanja su izračunato zasebno na temelju korištenih stranica na računu za pohranu. Detalje potražite u članku [Virtualnim strojevima s pojedinosti o cijene](https://azure.microsoft.com/pricing/details/virtual-machines/) i [Cijene za Azure prostora za pohranu](https://azure.microsoft.com/pricing/details/storage/). 


Imajte na umu sljedeće možda će vam pomoći da odlučite na veličinu:


* Veličina A8 A11 i H niz se nazivaju i *računalnim ćete morati usko instance*. Hardverski koja se pokreće te veličine je osmišljeno i optimiziran za računalnim ćete morati usko, a mreže ćete morati usko aplikacije, uključujući računalno visokih performansi (HPC) skupine aplikacija, Modeliranje i simulations. A8 A11 niz koristi Xeon E5 2670 Intel @ 2.6 GHZ i H niza koristi v3 Xeon E5 2667 Intel @ 3.2 GHz. Detaljne informacije i pitanja o korištenju ove veličine potražite u članku [o odgovora niz VMs H niza i računalnim ćete morati usko](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-serije, D G nizom idealne su za aplikacije koje potražnje brže CPU-ovi, bolje lokalni disk performanse ili ste instalirali veća memorija zahtjeva.  Oni nude Napredna kombinaciju za mnoge poslovne klase aplikacije.

*   Neke od fizička domaćini u Azure podataka možda ne podržava virtualnog računala veće, kao što su A5 – A11. Kao rezultat, vidjet ćete poruku o pogrešci **Konfiguriranje virtualnog računala {naziv računala} nije uspjelo** ili **nije uspjelo stvaranje virtualnog računala {naziv računala}** prilikom promjene veličine postojeće virtualnog računala da biste novu veličinu; Stvaranje novog virtualnog računala u virtualne mreže stvorene prije Travanj 16 2013; ili u postojeće oblaku Dodavanje novog virtualnog računala. Potražite u članku [pogreška: "Nije uspjela konfiguriranje virtualnog računala"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) na forumu podrška za zaobilazna rješenja za svaki scenarij implementacije.  

* Pretplate i može ograničiti broj jezgri možete implementirati u linije određene veličine. Da biste povećali ograničenja, obratite se podršci za Azure.


## <a name="performance-considerations"></a>Pitanja vezana uz performanse

Stvorili smo pojam od na Azure izračun jedinica (ACU) možete unijeti način Usporedba performanse računalnim (CPU) preko Azure SKU-ove. To će vam pomoći lakše prepoznali koji se SKU najvjerojatnije zadovoljili vaše potrebe performansi.  ACU trenutno standardizirani priručniku na mali (Standard_A1) VM pa se 100 i sve druge SKU-ove predstavljaju približno koliko brže SKU možete pokrenuti standardne usporednih. 

>[AZURE.IMPORTANT] U ACU je samo većinom.  Rezultati za svoje radno opterećenje se mogu razlikovati. 

<br>

|Obitelj SKU |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4](#a-series) |100 |
|[Standard_A5 7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1 14](#d-series) |160 |
|[D1 15v2](#dv2-series) |210 - 250 *|
|[G1-5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

ACUs koji je označen u * pomoću tehnologije Intel® Turbo povećanju učestalost procesora i pribaviti povećanje performansi.  Iznos u pojačavanje može se razlikovati ovisno o veličini VM, radno opterećenje i drugih radnih opterećenja radi na istom računalu koje hostira.

## <a name="size-tables"></a>Veličina tablice

U sljedećoj tablici prikazane veličine i kapaciteta pružaju.

* Kapacitet pohrane prikazuje se u jedinicama GiB ili 1024 ^ 3 bajtova. Usporedba diskova kada izmjerenu u GB (1000 ^ 3 bajtova) da biste diskova izmjerenu u GiB (1024 ^ 3) Imajte na umu brojeva kapaciteta izražen u GiB pojaviti manje. Na primjer, GiB 1023 = 1098.4 GB

* Propusnost diska mjeri se u ulaza i izlaza operacije sekundi (IOPS) i MB/s kojima MB/s = 10 ^ 6 bajtova/sec.

* Diskova podataka možete raditi u načinu prikaza spremanje predmemoriranih ili uncached. Za operaciju na disku predmemorirane podatke, glavno računalo predmemorirani način rada postavljen na **samo za čitanje** ili **ReadWrite**.  Za operaciju disk uncached podataka, glavno računalo predmemorirani način rada postavljen na **ništa**.

* Propusnost mreže najveći je maksimalna Zbrojeno propusnost dodijeliti i dodijeljeni po vrsti VM. Maksimalna propusnost sadrži upute za odabir ispravnu vrstu VM da biste bili sigurni odgovarajuće mrežnom kapacitetu dostupna. Prilikom pomicanja između Nisko, umjerene, Visoko i vrlo visoka, propusnost povećava sukladno tome. Stvarni mrežnih performansi ovisi o mnogo je čimbenika uključujući mreža i opterećenje aplikacije i postavke aplikacije mreže.


## <a name="a-series"></a>A niza

| Veličina        | Jezgri procesora | Memorija: GiB | Lokalni podizanje tvrdog diska: GiB | Max podataka diskova | Max podataka na disku propusnost: IOPS | NIC-ovi Max / propusnost mreže |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / niske                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / moderiranje              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / moderiranje              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / visoka                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / visoka                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / moderiranje              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / visoko                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / visoka                  |

## <a name="a-series---compute-intensive-instances"></a>A nizom-računalnim ćete morati usko instanci

Informacije i pitanja o korištenju ove veličine potražite u članku [o odgovora niz VMs H niza i računalnim ćete morati usko](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Veličina         | Jezgri procesora | Memorija: GiB | Lokalni podizanje tvrdog diska: GiB | Max podataka diskova | Max podataka na disku propusnost: IOPS | NIC-ovi Max / propusnost mreže |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / visoka                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / vrlo visoka             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / visoka                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / vrlo visoka             |

* RDMA koje je moguće povezati

## <a name="d-series"></a>D niza


| Veličina         | Jezgri procesora | Memorija: GiB | Lokalni SSD: GiB | Max podataka diskova | Max podataka na disku propusnost: IOPS | NIC-ovi Max / propusnost mreže |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / moderiranje              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / visoka                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / visoka                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / visoka                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / visoka                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / visoka                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / visoka                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / vrlo visoka             |

## <a name="dv2-series"></a>Dv2 niza

| Veličina            | Jezgri procesora | Memorija: GiB | Lokalni SSD: GiB | Max podataka diskova | Max podataka na disku propusnost: IOPS | NIC-ovi Max / propusnost mreže |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / moderiranje              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / visoka                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / visoka                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / visoka                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / iznimno visoka        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / visoka                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / visoka                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / visoka                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / iznimno visoka        |
| Standard_D15_v2 | 20        | 140          | 1000                | 40             | 40 x 500             | 8 / iznimno visoka        |

## <a name="g-series"></a>G niza

| Veličina        | Jezgri procesora | Memorija: GiB  | Lokalni SSD: GiB  | Max podataka diskova | Max propusnost diska: IOPS | NIC-ovi Max / propusnost mreže |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / visoka                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / visoka                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 x 500           | 4 / vrlo visoka             |
| Standard_G4 | 16        | 224          | 3,072                | 32             | 32 x 500           | 8 / iznimno visoko        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / iznimno visoka        |


## <a name="h-series"></a>H niza

Azure H niz virtualnim strojevima su sljedeći generiranja visoke performanse računalstvo VMs usmjerenih na visok završetka računalne potrebama, kao što su molecular Modeliranje i računalne tekuća dynamics. Ove 8 i 16 core VMs utemeljena je na tehnologije procesor Intel Haswell E5 2667 V3 sa svojstvima DDR4 memorije i lokalno spremište SSD temelje. 

Uz znatno power procesora H niz nudi raznih mogućnosti za niske latencije RDMA mreže pomoću FDR InfiniBand i nekoliko memorije konfiguracije podržava memorije zahtjevne računalne preduvjete.


| Veličina           | Jezgri procesora | Memorija: GiB | Lokalni SSD: GiB | Max podataka diskova | Max propusnost diska: IOPS | NIC-ovi Max / propusnost mreže |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / visoka                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / vrlo visoka                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / visoka                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / vrlo visoka                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / vrlo visoka                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / vrlo visoka                  |


* RDMA koje je moguće povezati

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Napomene: Standardne A0 – A4 pomoću EŽA i komponente PowerShell 

U modelu uvođenje klasičnog neki nazivi veličina VM razlikuju malo EŽA i PowerShell:

* Standard_A0 je ExtraSmall 
* Standard_A1 je male
* Standard_A2 je srednja
* Standard_A3 je veliko
* Standard_A4 je ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Konfiguriranje veličine za servise u Oblaku

Veličina virtualnog računala uloga instance možete odrediti kao dio modela servisa opisan [datoteka za definiciju](cloud-services-model-and-package.md#csdef). Veličina uloge određuje broj procesora jezgri, kapacitet memorije i veličinu sustava lokalne datoteke koju je dodijeljen pokrenute instance. Odaberite veličinu uloga na temelju vašeg računala resursa zahtjev.

Slijedi primjer za postavljanje veličine uloga biti [Standard_D2](#general-purpose-d) Web uloga instance:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o [azure pretplate i ograničenja servisa, kvote, i ograničenja](../azure-subscription-service-limits.md).
- Saznajte više [o odgovora niz VMs H niza i računalnim ćete morati usko](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) za radnih opterećenja kao što su najviša-performansi (HPC).

