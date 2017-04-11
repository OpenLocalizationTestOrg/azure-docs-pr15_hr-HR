<properties 
   pageTitle="Praćenje uređaju StorSimple | Microsoft Azure"
   description="U članku se opisuje praćenje/i performanse, Upotreba kapaciteta, propusnost mreže i performanse uređaj pomoću upravitelja StorSimple servisa."
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
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Praćenje uređaju StorSimple pomoću upravitelja StorSimple servisa 

## <a name="overview"></a>Pregled

Servis StorSimple Manager možete koristiti za praćenje određenim uređajima unutar StorSimple rješenje. Možete stvoriti prilagođeni grafikone na temelju/i performanse, Upotreba kapaciteta, propusnost mreže i metriku performanse uređaja. 

Da biste vidjeli informacije o praćenju za određeni uređaj, na portalu za Azure klasični, odaberite StorSimple Upravitelj servisa. Kliknite karticu **Monitor** , a zatim odaberite na popisu uređaja. Stranica **monitora** sadrži sljedeće podatke.

## <a name="io-performance"></a>/ I performanse 

Metrika **prati/i performanse** povezani broj čitanje i pisanje operacije između ili pokretač sučelja iSCSI na poslužitelj glavnog računala i uređaja ili uređaj u oblak i. Ovaj performanse mjeri se za određene glasnoće, spremniku određene glasnoću ili sve spremnike glasnoću.

Grafikon u nastavku prikazuje/i pokretača na uređaj za sve jedinice za radni uređaj. Metrika iscrtavanja su za čitanje i pisanje bajtova sekundi, čitanje i pisanje IO operacije sekundi, pročitajte i pisanje latencies.

![Performanse IO iz pokretača uređaj](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Za istom uređaju operacije/i se iscrtavaju za podatke s uređaja s oblakom za sve spremnike glasnoću. Na ovom uređaju podaci su samo u linearnom sloju i ništa ne sadrži spilled u oblak. Postoje bez operacija čitanja i pisanja koje su se pojavile s uređaja s oblakom. Stoga su peaks na grafikonu u intervalu s pet minuta koji odgovara učestalost potvrđen veza između uređaja i usluge. 

![Performanse IO s uređaja na cloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Za istom uređaju snimku oblaka izvršena glasnoću podataka počevši od 2:00 pm. Rezultirala podataka slijedi s uređaja s oblakom. Čitanje zapisivanja su poslužena u oblak u ovom trajanje. Grafikon IO prikazuje na vrh u različite metriku odgovara vrijeme kada je potrebno snimke. 

![Performanse IO za uređaj na cloud nakon oblaka snimke](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Upotreba kapaciteta 

**Upotreba kapaciteta** prati metriku vezanih uz količinu prostora za pohranu podataka koji se koristi količine, glasnoću spremnika ili uređaj. Možete stvarati izvješća na temelju Upotreba kapaciteta primarni prostora za pohranu, oblaka prostora za pohranu ili uređaj prostora za pohranu. Upotreba kapaciteta mjeri se na određene glasnoće, spremniku određene glasnoću ili sve spremnike glasnoću.


Primarni, oblaka, uređaj za pohranu kapacitet moguće je opisano na sljedeći način:

###<a name="primary-storage-capacity-utilization"></a>Upotreba kapaciteta primarni prostora za pohranu
 
Ove grafikoni prikazuju količinu podataka napisanih StorSimple količine prije nego što se podaci deduplicated i sažeti. Upotreba primarni prostora za pohranu možete pogledati tako da sva količine ili jedna jedinica.

Kada prikaz Upotreba grafikona primarni prostora za pohranu kapaciteta jedinica za sve jedinice nasuprot svaki pojedinačne količine i zbroji primarni podataka u oba slučaja, možda postoji nepodudaranje između dvaju brojeva. Ukupna primarni podatke na sve jedinice mogu dodati na ukupni zbroj primarnu podatkovnu pojedinačne količine. Razlog može biti nešto od sljedećeg:

- **Snimka podatke koji su uključeni za sve jedinice**: takvo ponašanje je vidjeti samo ako imate li verziju stariju od 3 ažuriranja. Primarni za sve jedinice prikazuju podaci zbroj primarni podatke za svaku jedinicu i podatke snimke. Primarni podaci prikazani za dani jedinicu odgovara samo količinu podataka koji se dodijeliti jedinice (i ne obuhvaća odgovarajuće podatke snimke jedinice).

    To možete i objašnjena po sljedeće jednadžbe:

    *Primarni podataka (sve jedinice) = Sum (primarni podataka (promet i) + veličina snimku podataka (promet i))*
    
    *gdje, primarni podataka (promet i) = veličina primarni podataka dodijeliti glasnoću i*
 
    Ako se snimki izbrišu pomoću usluge, brisanje obavlja asinkrono u pozadini. Ga može potrajati neko vrijeme za veličinu podataka glasnoću ažurirati nakon brisanja snimke. 

    Ako se izvodi ažuriranje 3 ili noviji, zatim podatke snimke nije obuhvaćen glasnoću podataka. I primarni Upotreba se izračunava na sljedeći način:

    * Primarni podataka (sve jedinice) = Sum (primarni podataka (promet i)
    
    *gdje, primarni podataka (promet i) = veličina primarni podataka dodijeliti glasnoću i*
 
- **Količine s nadzor onemogućena obuhvatiti sve jedinice**: Ako imate količine na uređaju koji je nadzor li isključena, nadzor podataka za te pojedinačne jedinice neće biti dostupne na grafikonima. Međutim, podaci za sve jedinice na grafikonu obuhvaća količine čijim nadzor isključen. 
 
- **Izbrisane količine s uživo pridružene sigurnosne kopije sadrži za sve jedinice**: Ako se brišu količine koje sadrže podatke snimke, ali pridružene snimke stanja i dalje postoji, a zatim možda ćete vidjeti ne podudaraju.

- **Izbrisani količine sadrži za sve jedinice**: U nekim slučajevima stare količine možda postoji čak i ako se to su izbrisane. Efekt brisanja ne vide, a uređaj može prikazati nižu dostupan kapacitet. Morate se obratiti Microsoftovoj Support da biste uklonili te količine.

Sljedeći grafikoni prikaz kapaciteta Upotreba prostora za pohranu primarnog uređaja StorSimple prije i nakon što se snimka oblaka snimljena. Budući da je samo glasnoću podataka, snimku oblaka trebate promijeniti primarni prostora za pohranu. Kao što vidite, grafikon prikazuje razlike u primarni kapaciteta Upotreba uslijed izradi oblaka snimke. Snimka oblaka rada na oko 2:00 prikazano na tom uređaju.

![Upotreba primarni kapaciteta prije oblaka snimke](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Upotreba primarni kapacitet nakon snimku oblaka](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Ako koristite ažurirajte 2 ili noviji, možete prekinuti dolje kapaciteta Upotreba primarni prostora za pohranu pojedinačne glasnoće, sve jedinice, sve tiered jedinicama i sve lokalno prikvačene jedinicama kao što je prikazano u nastavku. Raščlaniti po sve lokalno prikvačene jedinice će vam omogućiti da biste brzo provjerili koliko lokalne sloju koristi prema gore.

![Upotreba primarni kapaciteta za sve lokalno prikvačene jedinice](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Upotreba kapaciteta za pohranu oblaka

Ove grafikoni prikazuju količinu za pohranu u oblaku koristi. Deduplicated te sažete podatke. Iznos obuhvaća snimke oblak koji mogu sadržavati podatke koji ne odražavaju u bilo koje primarni jedinice i se drži svrhe naslijeđene i je li potreban zadržavanja. Možete usporediti primarnih i pohranu potrošnje slika da biste dobili ideju smanjenja rate podataka u oblaku iako neće biti točan broj. Sljedeće grafikoni prikazuju oblaka za pohranu kapaciteta korištenjem uređaja StorSimple prije i nakon snimljena u oblak snimke. Snimka oblaka započeto oko 2:00 prikazano na tom uređaju pa možete vidjeti kapacitet Upotreba oblaka snimka prema gore u isto vrijeme povećanje iz 5.73 MB 4.04 GB.

![Upotreba kapaciteta oblaka prije oblaka snimke](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Upotreba kapaciteta oblaka nakon oblaka snimke](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Upotreba kapacitet pohrane za uređaj

Ove grafikonima prikazuje ukupni Upotreba za uređaj koji će biti Upotreba više primarni prostora za pohranu jer sadrži sloju linearni SSD. Ova razina sadrži količinu podataka koji se i dalje postoji na uređaj je druge razine. Kapacitet u sloju linearni SSD je ima tako da se kad najteže nove podatke, stari podaci se premješta da biste sloju podizanje tvrdog diska (koje vrijeme je deduplicated i sažeti), a potom u oblak.

S vremenom Upotreba primarni kapaciteta i upotreba kapaciteta uređaj najvjerojatnije povećava zajedno dok se ne počinje podatke da biste se tiered u oblak. U tom trenutku kapaciteta Upotreba uređaja vjerojatno će početi da biste plateau, ali primarni kapaciteta Upotreba povećava se napisan više podataka.

Sljedeći grafikoni prikaz kapaciteta Upotreba prostora za pohranu primarnog uređaja StorSimple prije i nakon što se snimka oblaka snimljena. Oblak snimku započeto 2:00 pm i upotreba kapaciteta uređaj s radom smanjivanje to. Upotreba kapaciteta za pohranu uređaja nije prema dolje s 11.58 GB 7.48 GB. To označava da najvjerojatnije koje nisu komprimirane podataka u linearnom sloju SSD je deduplicated, sažeti i premještaju se u sloju podizanje tvrdog diska. Imajte na umu da ako uređaj već ima veliku količinu podataka u razine SSD i podizanje tvrdog diska, možda nećete vidjeti ovaj smanjenje. U ovom primjeru uređaj ima malu količinu podataka.

![Upotreba kapaciteta uređaj prije snimku oblaka](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Upotreba kapaciteta uređaj nakon oblaka snimke](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Propusnost mreže

**Propusnost mreže** prati metriku vezanih uz količinu podataka koji se prenosi s mreže sučelja pokretač iSCSI na poslužitelj glavnog računala i uređaja, a između uređaja i s oblakom. Možete nadzirati ovu metriku za svaku sučelje mreže iSCSI na uređaju.

Sljedeće grafikoni prikazuju propusnost mreže za podataka 0 i 4 podataka, obje 1 sučelje mreže GbE na uređaju. U ovoj instanci podataka 0 je oblak omogućenim dok 4 podataka nije omogućena za iSCSI. Vidjet ćete na ulazni i izlazni promet za svoj uređaj StorSimple. Paušalni crte u grafikonu počevši od 3:24 pm owing je da biste činjenica da bismo prikupljanje podataka svakih pet minuta i je zanemariti. 

![Propusnost mreže za Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Propusnost mreže za Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Performanse uređaja 

**Performanse uređaja** prati metriku vezane uz performanse uređaja. Sljedeći grafikon prikazuje stat Upotreba procesora za uređaj u radnog.

![Procesora za uređaj](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [koristiti na nadzornoj ploči StorSimple Upravitelj servisa uređaja](storsimple-device-dashboard.md).

- Saznajte kako [pomoću upravitelja StorSimple servisa za administraciju uređaju StorSimple](storsimple-manager-service-administration.md).
