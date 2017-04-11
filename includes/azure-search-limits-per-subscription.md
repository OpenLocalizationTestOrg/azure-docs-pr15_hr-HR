Možete stvoriti više usluga pretplate, svaki od njih dodjeli na određene sloju ograničene samo Reporting services dopušten svaki sloju. Na primjer, mogli biste stvoriti do 12 pri osnovni sloju i drugi 12 services u sloju S1 unutar iste pretplate. Dodatne informacije o razine, pročitajte članak [Odabir SKU ili sloju za pretraživanje Azure](../articles/search/search-sku-tier.md).

Maksimalna servisa ograničenja se mogu potenciju nakon zahtjev. Ako vam je potrebna više servisa unutar iste pretplate, obratite se podršci za Azure.

Resurs|Besplatni|Osnovni|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Maksimalna services |1 |12 |12  |6 |6 |6 
Maksimalno skaliranje SU <sup>2</sup>|N/d <sup>3</sup>|3 SU <sup>4</sup> |36 SU|36 SU|36 SU|12 SU, 3 SU <sup>5</sup>

<sup>1</sup> S3 HD ne podržava [indexers](../articles/search/search-indexer-overview.md) trenutno. 

<sup>2</sup> pretraživanja (SU) su jedinice naplatu jedinica po servis, a zatim dodijeliti kao *replike* ili *particije*. Potrebno i resursa za pohranu, indeksiranja i operacije upita. Da biste saznali više o valjani kombinacije koje Ostanite u odjeljku Maksimalna ograničenja, potražite u članku [Skaliranje razine resursa za radnih opterećenja upita i indeksa](../articles/search/search-capacity-planning.md). 

<sup>3</sup> slobodno temelji se na zajedničkim resursima koristi više pretplatnika. U ovom sloju nema namjenski resursa za pojedinačne pretplatnika. Zbog toga maksimalno skaliranje nosi nije primjenjivo.

<sup>4</sup> basic ima fixed particija. U ovom sloju dodatne SUs koriste se za dodjeljivanje više replike za radnih opterećenja povećana upita.

<sup>5</sup> S3 HD različite dodijeljeni struktura je pomoću kombinacije dopuštene. Za replike, može sadržavati najviše od 12. Za particije Maksimalna je 3.




