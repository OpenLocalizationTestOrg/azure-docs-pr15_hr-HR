<properties 
   pageTitle="Ograničenja sustava StorSimple | Microsoft Azure"
   description="U članku se opisuje ograničenja sustava i preporučene veličine za StorSimple 8000 niz komponente i veze."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="storsimple-system-limits"></a>Ograničenja sustava StorSimple

## <a name="overview"></a>Pregled

StorSimple nudi skalabilni i prilagodljivo prostora za pohranu za vaše podatkovnog centra. No postoje neka ograničenja koja treba imati na umu prilikom planiranje, uvođenje i upravljati StorSimple rješenje. U sljedećoj su tablici opisuje ta ograničenja i sadrži neke preporuke da bi mogli pristupiti na najbolji mogući način StorSimple rješenje.

| Ograničenje identifikator | Ograničenje | Komentari |
|----------------- | ------|--------- |
| Maksimalni broj vjerodajnice za pohranu računa | 64 | |
| Maksimalan broj jedinica spremnika | 64 | |
| Maksimalan broj jedinica | 255 | |
| Maksimalni broj lokalno prikvačene količine | 32 | |
| Maksimalni broj rasporede po propusnosti predloška | 168 | Raspored za svaki sat, svaki dan u tjednu (24 * 7). |
| Maksimalna veličina tiered glasnoću na fizičkih uređaja | 64 TB za 8100 i 8600 | 8100 i 8600 su fizičkih uređaja. |
| Maksimalna veličina tiered jedinice na virtualne uređaja Azure | 30 TB za 8010 <br></br> 64 TB za 8020 | 8010 i 8020 su virtualne uređaja u Azure koji koriste Standard prostora za pohranu i Premium prostora za pohranu. |
| Maksimalna veličina lokalno prikvačene glasnoću na fizičkih uređaja | 8,5 TB za 8100 <br></br> 22.5 TB za 8600 | 8100 i 8600 su fizičkih uređaja. |
| Maksimalni broj iSCSI veza | 512 | |
| Maksimalni broj iSCSI veze iz Pokretači | 512 | |
| Maksimalan broj zapisa za kontrolu pristupa po uređaja | 64 | |
| Maksimalan broj jedinica po sigurnosne kopije pravila | 24 | |
| Maksimalni broj kopija zadržavaju raspored (u sigurnosne kopije pravila) | 64 | |
| Maksimalni broj rasporede po sigurnosne kopije pravila | 10 | |
| Maksimalni broj snimaka bilo koje vrste koje se zadržavaju po jedinici | 256 | Taj broj sadrži lokalni snimke i brze snimke u oblaku. |
| Maksimalni broj snimke koje se mogu postojati u bilo kojem uređaju | 10 000 | |
| Maksimalan broj jedinica koje mogu biti obuhvaćene paralelno za sigurnosne kopije, vraćanje ili Kloniraj | 16 |<ul><li>Ako postoji više od 16 količine, oni se obrađuju sekvencijalno tijekom obrade slobodnih postaju dostupne.</li><li>Novi sigurnosne kopije na kloniranu ili vraćene tiered jedinica ne može se pojavljivati dok ne završi postupak. Međutim, za lokalnu jedinicu sigurnosne kopije dopušteno kada je glasnoće na Internetu.</li></ul>|
| Vraćanje i Kloniraj oporavak vremena za tiered jedinice | < dvije minute | <ul><li>Glasnoću postane dostupna u dvije minute postupak vraćanja ili Kloniraj, bez obzira na to veličina jedinice.</li><li>Na glasnoću prethodno može usporiti od uobičajenog kao što je većina podaci i metapodaci i dalje se nalazi u oblaku. Performanse mogu povećati kao tokova podataka iz oblaka StorSimple uređaj.</li><li>Ukupno vrijeme da biste preuzeli metapodataka ovisi o veličini dodijeljene glasnoću. Metapodaci automatski uvezeni u uređaj u pozadini brzinom od pet minuta po TB dodijeljene glasnoću podataka. Ovaj tečaj može utjecati propusnosti internetske veze s oblakom.</li><li>Vraćanje ili Kloniraj postupak je dovršena kada je sve metapodataka na uređaju.</li><li>Operacije sigurnosnog kopiranja nije moguće izvesti dok ne vrati ili Kloniraj dovršetka operacije potpuno.|
| Vraćanje oporavak vremena za lokalno prikvačene jedinice | < dvije minute | <ul><li>Glasnoću postane dostupna u dvije minute postupak vraćanja, bez obzira na to veličina jedinice.</li><li>Na glasnoću prethodno može usporiti od uobičajenog kao što je većina podaci i metapodaci i dalje se nalazi u oblaku. Performanse mogu povećati kao tokova podataka iz oblaka StorSimple uređaj.</li><li>Ukupno vrijeme da biste preuzeli metapodataka ovisi o veličini dodijeljene glasnoću. Metapodaci automatski uvezeni u uređaj u pozadini brzinom od pet minuta po TB dodijeljene glasnoću podataka. Ovaj tečaj može utjecati propusnosti internetske veze s oblakom.</li><li>Za razliku od tiered jedinice za lokalno prikvačene količine podataka glasnoću i preuzimaju se lokalno na uređaju. Svi podaci glasnoću sadrži su uvezeni na uređaju je dovršena postupak vraćanja.</li><li>Operacije vraćanja može biti duljine. Ukupno vrijeme da biste dovršili obnavljanja ovise o veličine dodijeljenu lokalnu jedinicu, internetska propusnost i postojećih podataka na uređaju. Sigurnosno kopiranje operacije na lokalno prikvačene jedinici dopuštene dok je u tijeku je postupak vraćanja.|
|Obrada stopu za oblak snimke| 15 minuta/TB | <ul><li>Minimalno da biste u oblak snimke jeste li spremni za prijenos po TB dodijeljene glasnoću podataka u sigurnosnu kopiju. </li><li> Ukupna oblak snimke vrijeme izračunava dodavanjem trenutno vrijeme učitavanja snimke koju utjecaja propusnosti internetske veze na cloud.
| Maksimalno klijentsko čitanje/pisanje propusnost (kada je poslužena iz SSD sloju) * | 920/720 MB/s s jednom 10 GbE mrežno sučelje | Dva sučelja mreže i veličine do 2 x s MPIO. |
| Maksimalno klijentsko čitanje/pisanje propusnost (kada je poslužena iz sloju podizanje tvrdog diska) * | 120/250 MB/s |
| Maksimalno klijentsko čitanje/pisanje propusnost (kada je poslužena iz oblaka sloju)* za ažuriranje 3 ili noviji** | 40/60 MB/s tiered jedinicama<br><br>60-80 MB/s tiered količine s odabranom tijekom stvaranja glasnoću arhiviranje mogućnosti | Čitanje propusnost ovisi o klijentima za generiranje i održavanje dovoljno dubina reda čekanja/i. <br><br>Brzina postići ovisi o brzinu podlozi račun za pohranu koji se koristi. | 

& #42; Maksimalna propusnost po vrsti/i mjeri je sa 100 posto čitanje i pisanje 100 posto scenariji. Stvarni propusnost može biti manji i ovisi o/i kombinirati i mrežnih uvjeta.

& #42; & #42; Performanse brojeve prije 3 ažuriranje može biti manji.

## <a name="next-steps"></a>Daljnji koraci

Pregled [StorSimple sistemske preduvjete](storsimple-system-requirements.md). 
