Resurs|Zadano ograničenje
---|---
Broj računa za pohranu po pretplati|200<sup>1</sup>
TB po račun za pohranu|500 TB
Maksimalan broj blob spremnika, blob-ova, zajedničke datoteke, tablice, redove, entiteti ili poruke po računu za pohranu|Samo ograničenje je račun 500 TB kapaciteta
Maksimalna veličina blob jedan spremnik, tablice ili reda čekanja|500 TB
Maksimalni broj blokovi u blob blok ili dodati blobova platforme|50.000
Maksimalni veličina bloka u blob blok ili dodati blobova platforme|4 MB
Maksimalni veličina blob blok ili dodati blobova platforme|MB 50.000 x 4 (approx. 195 GB) 
Maksimalna veličina blob stranice |1 TB
Maksimalna veličina tablice entitet|1 MB
Maksimalan broj svojstava u tablici entitet|252
Maksimalna veličina poruke u redu čekanja|64 KB
Maksimalna veličina zajedničko korištenje datoteka|5 TERABAJTA
Maksimalna veličina datoteke u zajedničko korištenje datoteka|1 TB
Maksimalan broj datoteka u zajedničko korištenje datoteka|Samo ograničenje je 5 Terabajta ukupni kapacitet zajedničko korištenje datoteka
Max 8 KB IOPS po zajedničko korištenje|1000
Maksimalan broj datoteka u zajedničko korištenje datoteka|Samo ograničenje je 5 Terabajta ukupni kapacitet zajedničko korištenje datoteka
Maksimalan broj blob spremnika, blob-ova, zajedničke datoteke, tablice, redove, entiteti ili poruke po računu za pohranu|Samo ograničenje je račun 500 TB kapaciteta
Maksimalan broj pravilnike pohranjene pristup po spremnik, zajedničko korištenje datoteka, tablice ili reda čekanja|5
Ukupna zahtjev stopa (Ako je veličina 1KB objekta) račun za pohranu|Do 20 000 IOPS entiteti sekundi ili poruke u sekundi
Cilj propusnost za jednu blobova platforme|Do 60 MB po za drugu ili do 500 u sekundi
Cilj propusnost jedan red (1 KB poruke)|Do 2000 poruke u sekundi
Cilj propusnost za jednu tablicu particija (1 KB entiteti)|Do 2000 entiteti u sekundi
Cilj propusnost za zajedničko korištenje jedne datoteke|Do 60 MB u sekundi
Max ingress<sup>2</sup> po računu za pohranu (NAM regije)|Ako je omogućeno GRS/ZRS<sup>3</sup> , 20 Gbps za LRS 10 Gbps
Max izlazne<sup>2</sup> po računu za pohranu (NAM regije)|Ako je omogućeno RA-GRS/GRS/ZRS<sup>3</sup> , 30 Gbps za LRS 20 Gbps
Max ingress<sup>2</sup> po računu za pohranu (Europske i područja Azije)|Ako je omogućeno GRS/ZRS<sup>3</sup> , 10 Gbps za LRS 5 Gbps
Max izlazne<sup>2</sup> po računu za pohranu (Europske i područja Azije)|Ako je omogućeno RA-GRS/GRS/ZRS<sup>3</sup> , 15 Gbps za LRS 10 Gbps

<sup>1</sup> To obuhvaća standardnu i Premium račune za pohranu. Ako je potrebno više od 200 račune za pohranu, provjerite zahtjeva putem [Azure podrška](https://azure.microsoft.com/support/faq/). Tim za pohranu Azure će pregledajte slučaj tvrtke i mogu odobriti do 250 račune za pohranu. 

<sup>2</sup> *Ingress* odnosi se na sve podatke (zahtjeve) koja se šalje s računom za pohranu. *Izlazne* odnosi se na sve podatke (odgovore) Prima s računa za pohranu.  

<sup>3</sup> Azure mogućnosti pohrane replikacijom obuhvaćaju sljedeće:

- **RA GRS**: pristup za čitanje zemlj suvišnih prostora za pohranu. Ako je omogućeno RA GRS, izlazne ciljevi za sekundarne mjesto su jednake onima za primarno mjesto.
- **GRS**: zemlj suvišnih prostora za pohranu. 
- **ZRS**: Zone suvišnih prostora za pohranu. Dostupna je samo za blokiranje blob-ova. 
- **LRS**: lokalno suvišnih prostora za pohranu. 

