<!--author=alkohli last changed: 12/15/15-->

| Ograničenje identifikator | Ograničenje | Komentari |
|----------------- | ------|--------- |
| Maksimalni broj za pohranu vjerodajnica računa | 64 | |
| Maksimalan broj jedinica spremnika | 64 | |
| Maksimalan broj jedinica | 255 | |
| Maksimalni broj rasporede po propusnosti predloška | 168 | Raspored za svaki sat, svaki dan u tjednu (24 * 7). |
| Maksimalna veličina tiered glasnoću na fizičkih uređaja | 64 TB za 8100 i 8600 | 8100 i 8600 su fizičkih uređaja. |
| Maksimalna veličina tiered jedinice na virtualne uređaja Azure | 30 TB za 8010 <br></br> 64 TB za 8020 | 8010 i 8020 su virtualne uređaja u Azure koji koriste Standard prostora za pohranu i Premium prostora za pohranu. |
| Maksimalna veličina lokalno prikvačene glasnoće na fizičkih uređaja | 9 TB za 8100 <br></br> 24 TB za 8600 | 8100 i 8600 su fizičkih uređaja. |
| Maksimalni broj iSCSI veza | 512 | |
| Maksimalni broj iSCSI veze iz Pokretači | 512 | |
| Maksimalan broj zapisa za kontrolu pristupa po uređaja | 64 | |
| Maksimalan broj jedinica po sigurnosne kopije pravila | 24 | |
| Maksimalni broj kopija zadržavaju sigurnosne kopije pravila | 64 | |
| Maksimalni broj rasporede po sigurnosne kopije pravila | 10 | |
| Maksimalni broj snimaka bilo koje vrste koje se zadržavaju po jedinici | 256 | To obuhvaća lokalne snimke i brze snimke u oblaku. |
| Maksimalni broj snimke koje se mogu postojati u bilo kojem uređaju | 10 000 | |
| Maksimalan broj jedinica koje mogu biti obuhvaćene paralelno za sigurnosne kopije, vraćanje ili Kloniraj | 16 |<ul><li>Ako postoji više od 16 količine, oni obrađuju sekvencijalno tijekom obrade slobodnih postaju dostupne.</li><li>Novi sigurnosne kopije na kloniranu ili vraćene tiered glasnoće ne može se pojavljivati dok ne završi postupak. Međutim, za lokalnu jedinicu sigurnosne kopije dopušteno kada je glasnoće na Internetu.</li></ul>|
| Vraćanje i Kloniraj oporavak vremena za tiered jedinice | < dvije minute | <ul><li>Glasnoću postane dostupna u dvije minute postupak vraćanja ili Kloniraj, bez obzira na to veličina jedinice.</li><li>Na glasnoću prethodno može usporiti od uobičajenog kao što je većina podaci i metapodaci i dalje se nalazi u oblaku. Performanse mogu povećati kao tokova podataka iz oblaka StorSimple uređaj.</li><li>Ukupno vrijeme da biste preuzeli metapodataka ovisi o veličini dodijeljene glasnoće. Metapodaci automatski uvezeni u uređaj u pozadini brzinom od pet minuta po TB dodijeljene glasnoće podataka. U ovom stopa može utjecati propusnosti internetske veze s oblakom.</li><li>Vraćanje ili Kloniraj postupak je dovršena kada je sve metapodataka na uređaju.</li><li>Operacije sigurnosnog kopiranja nije moguće izvesti dok ne vrati ili Kloniraj dovršetka operacije potpuno.|
| Vraćanje oporavak vremena za lokalno prikvačene jedinice | < dvije minute | <ul><li>Glasnoću postane dostupna u dvije minute postupak vraćanja, bez obzira na to veličina jedinice.</li><li>Na glasnoću prethodno može usporiti od uobičajenog kao što je većina podaci i metapodaci i dalje se nalazi u oblaku. Performanse mogu povećati kao tokova podataka iz oblaka StorSimple uređaj.</li><li>Ukupno vrijeme da biste preuzeli metapodataka ovisi o veličini dodijeljene glasnoće. Metapodaci automatski uvezeni u uređaj u pozadini brzinom od pet minuta po TB dodijeljene glasnoću podataka. U ovom stopa može utjecati propusnosti internetske veze s oblakom.</li><li>Za razliku od tiered količine, u slučaju lokalno prikvačene količine podataka glasnoću i preuzimaju se lokalno na uređaj. Svi podaci glasnoće sadrži su uvezeni na uređaju je dovršena postupak vraćanja.</li><li>Operacije vraćanja možda dugo i ukupno vrijeme da biste dovršili obnavljanja ovise o veličine dodijeljenu lokalnu jedinicu, internetska propusnost i postojećih podataka na uređaju. Sigurnosno kopiranje operacije na lokalno prikvačene glasnoće dopuštene dok je u tijeku je postupak vraćanja.|
| Tanki vraćanja dostupnosti | Zadnji prebacivanje | |
| Maksimalno klijentsko čitanje/pisanje propusnost (kada je poslužena iz SSD sloju) * | 920/720 MB/s s jednom 10GbE mrežnog sučelja | Dva sučelja mreže i veličine do 2 x s MPIO. |
| Maksimalno klijentsko čitanje/pisanje propusnost (kada je poslužena iz sloju podizanje tvrdog diska) * | 120/250 MB/s |
| Maksimalno klijentsko čitanje/pisanje propusnost (kada je poslužena iz oblaka sloju) * | 11/41 MB/s | Čitanje propusnost ovisi o klijentima za generiranje i održavanje dovoljno dubina reda čekanja/i. |

& #42; Maksimalna propusnost po vrsti/i mjeri je sa 100 posto čitanje i pisanje 100 posto scenariji. Stvarni propusnost može biti manji i ovisi o/i kombinirati i mrežnih uvjeta.
