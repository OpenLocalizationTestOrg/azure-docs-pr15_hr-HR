<properties
    pageTitle="Planiranje kapaciteta za zaštitu virtualnim strojevima i fizičke poslužitelji u oporavak web-mjesta Azure | Microsoft Azure"
    description="Oporavak Azure web-mjesta koordinate replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužitelja koja se nalazi na lokalni Azure ili sekundarni lokalnog web-mjesta." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Planiranje kapaciteta za zaštitu virtualnim strojevima i fizičke poslužitelji u oporavak Azure web-mjesta

Alat za Azure web-mjesta oporavak kapaciteta planer pomaže vam da biste utvrdili kapacitetom za zaštitu Hyper-V VMs, VMware VMs i Windows/Linux fizičke poslužitelji oporavak Azure web-mjesta.


## <a name="overview"></a>Pregled

Analiza okruženje izvora i radnih opterećenja i utvrđivanje potrebe propusnosti, morat ćete u izvornom mjestu resurse poslužitelja i resursa (virtualnim strojevima i pohranu itd), koje morate odredišnog mjesta pomoću planer kapaciteta za oporavak web-mjesta. 

Možete pokrenuti alat na nekoliko načina:

- **Brzog planiranja**: pokretanje alata u tom načinu rada da biste dobili projekcija mreža i server na temelju Prosječan broj VMs, diskova, za pohranu i promjena stopa.
- **Planiranje detaljnom**: pokretanje alata u tom načinu rada i detalje o svakom radno opterećenje na razini VM. Analiza VM kompatibilnosti i pronađite projekcija mreža i poslužitelj.

## <a name="before-you-start"></a>Prije početka

Prije nego što pokrenete alat za:

1. Prikupite informacije o okruženju, uključujući VMs, diskova po VM, prostor za pohranu po disk.
2. Odredite dnevnih stopa promjena (churn) za repliciranu podatke. Da biste to učinili:

    - Ako ste replikaciju Hyper-V VMs, zatim preuzmite [Alat za planiranje kapaciteta Hyper-V tako](https://www.microsoft.com/download/details.aspx?id=39057) da biste dobili ratu promjena. [Dodatne informacije](site-recovery-capacity-planning-for-hyper-v-replication.md) o alatu. Preporučuje se da pokrenete alat putem tjedan dana da biste snimili prosjeka.
    - Ako ste replikaciju VMware virtualnim strojevima, korištenje [kapaciteta vSphere planiranje uređaj](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) da biste utvrdili stopa churn.
    - Ako ste replikaciju fizičke poslužitelje morate ručno procjene.

## <a name="run-the-quick-planner"></a>Planer za brzo pokretanje
1.  Preuzmite i otvorite alat za [Azure web-mjesta oporavak kapaciteta planer](http://aka.ms/asr-capacity-planner-excel) . Morat ćete pokretati makronaredbe pa odaberite da biste omogućili uređivanje i omogućivanje sadržaja kada se to od vas zatraži. 
2.  U **Odabir vrste planer** odaberite **Brzi planer** iz okvira popisa.

    ![Početak rada](./media/site-recovery-capacity-planner/getting-started.png)

3.  Na radnom listu **Kapaciteta planer** unesite potrebne informacije. Ispunite sva polja u kružnici crvenom bojom u nastavku snimku zaslona.

    - U **Odaberite scenariju** odaberite **Hyper-V tako da Azure** ili **VMware/fizičke za Azure**.
    - U **Average dnevno podaci mijenjaju stopa (%)** stavite podatke prikupite pomoću [alatu za planiranje kapaciteta Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) ili [uređaj za planiranje kapaciteta, vSphere](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance).  
    - **Sažimanje** odnosi se samo na sažimanje nudi kada replikaciju VMware VMs ili fizičke poslužitelje Azure. Ne možemo procjenu 30% ili više, ali možete izmijeniti postavke prema potrebi. Za replikaciju Hyper-V VMs za Azure sažimanja možete koristiti proizvođača potražite kao što su Riverbed. 
    -  U **Zadržavanja unosa** Navedite koliko zadržati replike. Ako ste replikaciju VMware ili fizičke poslužitelji za unos vrijednosti u danima. Ako ste replikaciju Hyper-V pritisak sata.
    -  U **broj sati u koje početne replikacije za obradu virtualnim strojevima mora dovršiti** i **broj virtualnim strojevima po početne replikacije obradu** unos postavke koje se koriste za izračun početne replikacije preduvjeti.  Kada je oporavak web-mjesta implementiran cijeli skup podataka za početne treba prenijeti. 

    ![Unosa](./media/site-recovery-capacity-planner/inputs.png)

2.  Nakon što ste postavili vrijednosti okruženju izvora, prikazana je obuhvaćen:

    - **Propusnost potrebnu za replikaciju delta** (MB/sec). Propusnost mreže za replikaciju delta izračunava Prosječni dnevni tečaj promjena podataka.
    - **Propusnost potrebnu za početne replikacije** (MB/sec). Propusnost mreže za replikaciju početni se izračunava replikacije početne vrijednosti spremate u. 
    - **Prostor za pohranu potrebno (u GBs)** je ukupna Azure spremište potrebno.
    - **Ukupna IOPS standardne prostora za pohranu računa** se izračunava na temelju 8 K IOPS jedinične veličine računa ukupnu standardne prostora za pohranu.  Brzi planer se izračunava broj koji se temelji na sve izvora VMs diskova i dnevnih podataka promijeniti stopa. Za detaljne planer se izračunava broj ukupan broj VMs koji su mapirani na standardni Azure VMs i podataka na temelju promijeniti učestalost te VMs. 
    - **Broj računa standardnu prostora za pohranu** daje ukupan broj računa standardnu prostora za pohranu potrebno da biste zaštitili na VMs. Imajte na umu da standardne prostora za pohranu račun može sadržavati do 20000 IOPS preko VMs u standardni prostora za pohranu i najveći 500 IOPS podržani po disk. 
    - **Potreban broj diskova s blob** daje broj diskova koji će se stvoriti na Azure prostora za pohranu.
    - **Broj računa za pohranu premium obavezno** nudi ukupan broj od premium prostora za pohranu računa potrebno da biste zaštitili na VMs. Imajte na umu izvor VM s visoke IOPS (veće od 20000) potrebno je račun za pohranu premium. Račun za pohranu premium može sadržavati do 80000 IOPS.
    - **Ukupna IOPS na pohranu premium** izračunato na temelju 256 K IOPS jedinične veličine prostora za pohranu računa ukupnu premium.  Brzi planer se izračunava broj koji se temelji na sve izvora VMs diskova i dnevnih podataka promijeniti stopa. Za detaljne planer se izračunava broj ukupan broj VMs koja su mapirana premium Azure VM (niz DS i Oznaka) i podataka na temelju promijeniti učestalost te VMs. 
    - **Potreban broj konfiguraciju poslužitelja** prikazuje koliko konfiguraciju poslužitelja su potrebni za implementaciju (1)
    - **Potreban broj dodatne postupak poslužitelja** prikazuje jesu li dodatne postupak poslužitelje potrebni osim poslužitelj za postupak koji je konfiguriran na poslužitelj za konfiguraciju po zadanom.
    - **100% dodatni prostor za pohranu na izvoru** prikazuje je li potreban dodatan prostor za pohranu u izvornom mjestu.
            
    ![Izlaz](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>Pokretanje detaljne planer


1.  Preuzmite i otvorite alat za [Azure web-mjesta oporavak kapaciteta planer](http://aka.ms/asr-capacity-planner-excel) . Morat ćete pokretati makronaredbe pa odaberite da biste omogućili uređivanje i omogućivanje sadržaja kada se to od vas zatraži. 
2.  U **Odabir vrste planer** odaberite **Detaljne planer** iz okvira popisa.

    ![Početak rada](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  Na radnom listu **Radno opterećenje kvalifikacije** unesite potrebne informacije. Ispunite sva označena polja.

    - U **jezgri procesora** navesti ukupan broj jezgri izvorni poslužitelj.
    - U **dodjelu memorije u MB** odredite veličinu RAM-a izvorišnog poslužitelja. 
    - **Broj od NIC-ovi** navedite broj mrežnih prilagodnika na izvorni poslužitelj. 
    -  U **ukupan prostor za pohranu (u GB)** navedite ukupnu veličinu VM prostora za pohranu. Ako je s izvorišnim poslužiteljem 3 diskova s 500 GB, a zatim veličina ukupan prostor za pohranu, primjerice je 1500 GB.
    -  Broj **diskova priložene** navedite ukupan broj diskova izvorni poslužitelj.
    -  U **Disk kapaciteta Upotreba** navedite average Upotreba.
    -  U **svakodnevno promjena stopa (%)** navedite dnevni podaci mijenjaju rata izvorni poslužitelj.
    -  Veličina **Mapiranja Azure** unesite veličina Azure VM koje želite mapirati. Ako ne želite da biste to učinili ručno kliknite**Izračunati VMs IaaS**. Imajte na umu da ako ručno postavku za unos, a zatim kliknite Izračun VMs IaaS ručno postavku može prebrisati jer postupka računalnim automatski prepoznaje najbolje odgovaraju na veličina Azure VM.

    ![Diploma radno opterećenje](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Ako kliknete **Izračunati VMs IaaS** ovdje je funkcija:

    - Provjeri valjanost obavezno unosa.
    - Izračunava IOPS i predlaže najbolje VM Azure aize podudaranje za svaku VMs koji ispunjava uvjete za replikaciju za Azure. Ako odgovarajuću veličinu Azure VM nije moguće otkriti izdaje pogrešku. Primjer broj diskova priložene u 65 pojavila se pogreška ako je problema od najveće veličina Azure VM iznosi 64.
    - Predlaže račun za pohranu koji se mogu koristiti za programa Azure VM.
    - Izračunava ukupni broj standardne prostora za pohranu računi i računi servisa za pohranu premium potrebne za povećavaju. Pomaknite se prema dolje s desne strane da biste pregledali vrstu Azure pohrane i račun za pohranu koji se mogu koristiti za izvorni poslužitelj
    - Dovršava i sortira ostatak tablice na temelju potreban prostor za pohranu vrsta (standardni ili premium) na VM i broj diskova priložene. Prikazuje da za sve VMs koji zadovoljava preduvjete za sigurnosnom najviše Azure, stupac A (je VM pogodni?). Ako je VM ne može sigurnosno do prikazan je Azure pojavila se pogreška.

Stupci AA lt su izlazne i unesite podatke za svaku VM.

![Diploma radno opterećenje](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Primjer
Na primjer, za šest VMs s vrijednosti prikazane u tablici alat izračunava i dodjeljuje najbolje odgovaraju Azure VM i preduvjeti za Azure prostora za pohranu.

![Diploma radno opterećenje](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- U rezultatu navedeni primjer Imajte na umu sljedeće:
    
    - Prvi stupac sadrži stupac provjere valjanosti za VMs diskova i churn.
    - Dvije standardne prostora za pohranu računa i račun za pohranu jedan premium su vam potrebne za pet VMs. 
    -  VM3 ne ispunjavate uvjete za zaštitu jer je jedan ili više diskova više od 1 TB.
    -  VM1 i VM2 mogu koristiti prvi račun standardne prostora za pohranu
    -  VM4 možete koristiti drugi račun standardne prostora za pohranu.
    -  VM5 i VM6 potreban je račun za pohranu premium, a možete koristiti jedan račun.

    >[AZURE.NOTE]  IOPS na standardni i premium pohranu izračunavaju se na razini VM, a ne na razini disk. Standardni virtualnog računala možete rukovati do 500 IOPS po disk. Ako je vrijednost veća od 500 IOPS za disk morat ćete premium prostora za pohranu. No ako IOPS za disk su više od 500, ali IOPS za disketa VM su unutar podršku standardne Azure VM ograničenja (VM veličina, broj diskova, broj prilagodnika, procesora, memorije) pa planer izdvajanja standard VM i ne DS ili Oznaka nizova. Morat ćete ručno ažuriranje mapiranja Azure Veličina ćelije s odgovarajućim DS ili Oznaka nizova VM.

5. Kada sve pojedinosti će se prikazati, kliknite **Pošalji podatke u planer alat** da biste otvorili **Kapaciteta planer** radnih opterećenja istaknute su da bi se prikazala hoće li se ispunjava uvjete za zaštitu ili ne.


### <a name="submit-data-in-the-capacity-planner"></a>Slanje podataka u planer kapaciteta

1.  Kada otvorite list **Kapaciteta planer** ga se popunjava ovisno o postavkama koje ste naveli. Riječ "Radno opterećenje" pojavljuje se u **Infra unosa izvorne** ćelije da biste prikazali koji unos **Radno opterećenje kvalifikacije** radnog lista. 
2.  Ako želite da biste unijeli promjene morat ćete izmjena **Radno opterećenje kvalifikacije** radnog lista, a zatim ponovno kliknite Pošalji podatke u planer alat.  

    ![Planer za kapaciteta](./media/site-recovery-capacity-planner/capacity-planner.png)


