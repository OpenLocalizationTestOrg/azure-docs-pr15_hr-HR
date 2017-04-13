<properties
 pageTitle="Postavljanje sustava Windows RDMA klaster za pokretanje aplikacije MPI | Microsoft Azure"
 description="Saznajte kako stvoriti paketa Windows HPC klaster s veličinom H16r, H16mr, A8 ili A9 VMs koristi Azure RDMA mreže za izvođenje MPI aplikacije."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Postavljanje Windows RDMA klaster HPC paket za pokretanje aplikacije MPI

Postavite Windows RDMA klaster u Azure s [Paket Microsoft HPC](https://technet.microsoft.com/library/cc514029) i [H niz ili računalnim ćete morati usko instance odgovora niz](virtual-machines-windows-a8-a9-a10-a11-specs.md) za pokretanje aplikacije paralelno sučelja prosljeđivanje poruke (MPI). Prilikom postavljanja RDMA zaslonom, Windows Server čvorove programa klasteru HPC paketa aplikacije MPI komunikaciju učinkovito niske latencije visoke propusnost mreže Azure koji se temelji na udaljenom izravnu memorije tehnologije pristupa (RDMA).

Ako želite pokrenuti radnih opterećenja MPI na Linux VMs koji pristup mreži Azure RDMA potražite u članku [Postavljanje Linux RDMA klaster da biste pokrenuli MPI aplikacije](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>Mogućnosti implementacije klaster HPC paketa
Paket Microsoft HPC alat je pod uvjetom da pri bez dodatnih troškova da biste stvorili HPC klaster lokalnog ili servisu Azure za pokretanje sustava Windows ili Linux HPC aplikacije. HPC paket sadrži runtime okruženja za Microsoftovu implementaciju poruke prosljeđivanje sučelje za Windows (MS-MPI). Kada se koristi sa zaslonom RDMA instance podržani operacijski sustav Windows Server, paket HPC omogućuje učinkovito mogućnost za pokretanje aplikacije Windows MPI koje pristup mreži Azure RDMA. 

U ovom se članku predstavlja dva scenarija i veze na detaljne upute za postavljanje Windows RDMA klaster Microsoft HPC paketom. 

* Scenarij 1. Implementacija instance ulogu suradnika računalnim ćete morati usko (PaaS)

* Scenarij 2. Implementacija računalnim čvorove u računalnim ćete morati usko VMs (IaaS)

Općenito preduvjeti za korištenje računalnim ćete morati usko instance sa sustavom Windows potražite u članku [o H niza i računalnim ćete morati usko VMs odgovora niz](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Scenarij 1. Implementacija instance ulogu suradnika računalnim ćete morati usko (PaaS)

Iz postojećeg paketa HPC klaster dodajte dodatni računalnim resursa u Azure tempiranja uloga instance (Azure čvorove) radi u oblaku (PaaS). Ta značajka nazivaju "brzi niz za Azure" iz paketa HPC podržava izbor veličina instanci ulogu suradnika. Prilikom dodavanja čvorove Azure, samo jednu RDMA zaslonom veličine navedite.

Slijede sljedeća pitanja i upute za brzi niz za zaslonom RDMA Azure instance iz postojećeg (obično lokalnog) klaster. Da biste dodali instance ulogu suradnika čvor glavni programa HPC paket koji je implementiran u programa Azure VM koristite slične postupke.

>[AZURE.NOTE] Vodič za brzi niz za Azure s HPC paket, potražite u članku [Postavljanje klaster hibridne HPC paketom](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Imajte na umu pitanja vezana uz u koracima u nastavku koji se odnose samo na RDMA zaslonom Azure čvorove.

![Brzi niz za Azure][burst]

### <a name="steps"></a>Koraci

4. **Implementacija i konfiguriranje programa čvor glavni HPC paket 2012 R2**

    Preuzmite najnovije instalacijski paket HPC paketa iz [Microsoftova centra za preuzimanje](https://www.microsoft.com/download/details.aspx?id=49922). Preduvjeti i upute za pripremu implementacije programa Azure neprekidnog rada potražite u članku [HPC paket priručnik za početak rada](https://technet.microsoft.com/library/jj884144.aspx) i [neprekidnog rada da biste tempiranja instance HPC paket sustava Microsoft Azure](https://technet.microsoft.com/library/gg481749.aspx).

5. **Konfiguriranje upravljanja certifikata u Azure pretplate**

    Konfiguriranje certifikata za sigurnu vezu između glavni čvor i Azure. Mogućnosti i postupke potražite u članku [Konfiguriranje certifikata za HPC paket za upravljanje Azure scenarija](http://technet.microsoft.com/library/gg481759.aspx). U slučaju implementacije test HPC paket instalira zadani Microsoft HPC Azure upravljanja certifikat brzo možete prenijeti u pretplatu za Azure.

6. **Stvaranje nove servise u oblaku i račun za pohranu**

    Pomoću portala za Azure klasični da biste stvorili neki servis u oblaku i račun za pohranu za implementaciju u području koje su dostupne zaslonom RDMA instance.

7. **Stvaranje predloška Azure čvor**

    Korištenje na čarobnjaka za stvaranje čvor predložak u HPC klaster Manager. Upute potražite u članku [Stvaranje predloška Azure čvor](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) u "Korake za implementaciju Azure čvorove s HPC paket Microsoft".

    Za početnu testira predlažemo da konfiguriranje dostupnost ručnog pravila u predlošku.

8. **Dodavanje čvorove klaster**

    Korištenje u čarobnjak za dodavanje čvor u HPC klaster Manager. Dodatne informacije potražite u članku [Dodavanje čvorove Azure klasteru HPC sustava Windows](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Prilikom određivanja veličina čvorove, odaberite jednu od RDMA zaslonom veličine instance.
    
    >[AZURE.NOTE]U neprekidnog rada u svakom za Azure implementacija s računalnim ćete morati usko instance HPC paket automatski uvodi najmanje 2 zaslonom RDMA instanci (kao što su A8) kao čvorove proxy poslužitelj, osim instance uloga Azure tempiranja koju navedete. Čvorovi proxy pomoću jezgri koji su dodijeliti pretplate i plaćati troškova uz uloga instance Azure tempiranja.

9. **Pokretanje (dodjele) čvorove a ih putem Interneta da biste pokrenuli poslova**

    Odaberite čvorove i pomoću akcije **pokretanje** u HPC klaster Manager. Po dovršetku dodjele resursa odaberite čvorove i akcijom **Premjesti Online** u HPC klaster Manager. Čvorove spremni za izvođenje zadataka.

10. **Slanje zadataka klaster**

    Pomoću alata za predavanje posla HPC paket da biste pokrenuli klaster zadatke. U odjeljku [paket Microsoft HPC: zadatak upravljanja](http://technet.microsoft.com/library/jj899585.aspx).

11. **Prekid (deprovision) čvorove**

    Kada ste gotovi izvodi zadatke, izvanmrežni čvorove i koristiti akciju **Zaustavi** u HPC klaster Manager.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Scenarij 2. Implementacija računalnim čvorove u računalnim ćete morati usko VMs (IaaS)

U ovom scenariju implementacija čvor glavni HPC paket i klaster izračunati čvorove na VMs se pridružite se domene servisa Active Directory u Azure virtualne mreže. Paket HPC nudi brojne [Mogućnosti uvođenja Azure VMs](virtual-machines-linux-hpcpack-cluster-options.md), uključujući automatiziranog implementacije skripte i predloške Azure brzi početak rada. Na primjer, sljedeća pitanja i korake u nastavku će vas voditi da biste koristili [HPC paket IaaS implementacijsku skriptu](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) da biste automatizirali Većina ovog postupka.

![Klaster u Azure VMs][iaas]



### <a name="steps"></a>Koraci

1. **Stvorite glavni čvor klaster i izračunavanje VMs čvor tako da pokrenete implementacijsku skriptu IaaS HPC paket na klijentskom računalu**

    Preuzmite paket HPC paket IaaS Implementacijsku skriptu iz [Microsoftova centra za preuzimanje](https://www.microsoft.com/download/details.aspx?id=49922).

    Da biste pripremili klijentsko računalo, stvaranje konfiguracijska datoteka skripte i pokrenuti skriptu, potražite u članku [Stvaranje programa klasteru HPC s implementacijsku skriptu HPC paket IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Da biste implementirali zaslonom RDMA čvorove računalnim, imajte na umu dodatne Imajte na umu sljedeće:
    
    * **Virtualna mreže** - Navedite novi virtualne mreže u regiji u kojoj je dostupan RDMA zaslonom veličine instancu koju želite koristiti.

    * **Operacijski sustav Windows Server** – podržavaju povezivanje RDMA, navedite operacijski sustav Windows Server 2012 R2 ili Windows Server 2012 za čvor računalnim VMs.

    * **Servisi u oblaku** – preporučujemo implementacija web-mjesto na glavni čvor u jednom u oblaku i u okvir za vašeg računalnim čvorove u drugu u oblaku.

    * **Veličina čvor glave** – u ovom scenariju, razmislite o veličine od najmanje A4 (vrlo velik) za glavni čvor.

    * **Proširenje HpcVmDrivers** - implementacijsku skriptu Azure VM Agent i nastavak HpcVmDrivers automatski instalira ako pokrenete veličinu A8 ili A9 izračunati čvorove s operacijskim sustavom Windows Server. HpcVmDrivers instalira upravljačke programe na čvor računalnim VMs tako da se mogu povezati s RDMA mrežom.

    * **Klaster mrežna konfiguracija** – implementacijsku skriptu automatski postavlja klaster HPC paketa u topologiji 5 (sve čvorove na mrežu tvrtke). Za sve implementacije klaster HPC paketa u VMs potreban je ovaj topologije. Naknadno promijeniti mrežna topologija klaster.

2. **Da biste pokrenuli poslove Premjesti čvorove računalnim na Internetu**

    Odaberite čvorove i akcijom **Premjesti Online** u HPC klaster Manager. Čvorove spremni za izvođenje zadataka.

3. **Slanje zadataka klaster**

    Povezivanje čvor glavni slanje zadataka ili postavite je na lokalno računalo da biste to učinili. Informacije potražite u članku [Slanje zadataka programa klasteru HPC u Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Izvanmrežni čvorove i zaustavljanje (deallocate) ih**

    Kada ste gotovi izvodi zadatke, potrebno čvorove izvanmrežno u HPC klaster Manager. Nakon toga pomoću alata za upravljanje Azure da biste ih isključili.



## <a name="run-mpi-applications-on-the-cluster"></a>Pokretanje aplikacije MPI na klaster

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Primjer: Pokrenuti mpipingpong programa paketa HPC klaster

Da biste provjerili implementacije sustava HPC paket zaslonom RDMA instanci, pokrenite naredbu za **mpipingpong** HPC paket na klaster. **mpipingpong** šalje pakete podataka između upareni čvorove pritišćite tipku TAB da biste izračunali Latencija i mjere propusnost i Statistika RDMA je omogućen mreže. U ovom se primjeru prikazuje uzorak standardne za pokretanje programa MPI posao (u ovom slučaju **mpipingpong**) pomoću naredbe **mpiexec** klaster.

U ovom se primjeru pretpostavlja da ste dodali Azure čvorove u konfiguraciji "neprekidnog rada za Azure" ([scenarij 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Ako je paket HPC implementiran na klaster Azure VMs, morat ćete da biste izmijenili Sintaksa naredbe možete navesti različite čvor grupe i postavljanje varijable dodatne okruženja usmjeravanja mrežnog prometa RDMA mrežu.


Da biste pokrenuli mpipingpong na skupine:


1. Na glavni čvor ili pravilno konfiguriranog klijentskom računalu otvorite naredbeni redak.

2. Za procjenu latencije između parova čvorove u implementacije sustava Azure neprekidnog rada od 4 čvorove, upišite sljedeću naredbu za slanje pokretanje mpipingpong s veličinu small paketa i velik broj iteracija:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Naredba vraća Identifikator posla koji se šalje.

    Ako implementiran klaster HPC paket koji je implementiran na Azure VMs navesti čvor grupu koja sadrži izračunati čvor VMs implementiran u jednom u oblaku i izmjena naredbu **mpiexec** na sljedeći način:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Nakon dovršetka posla, da biste pogledali izlaz (u ovom slučaju, izlazna zadatak 1 posla) upišite sljedeće

    ```
    task view <JobID>.1
    ```

    gdje &lt; *JobID* &gt; je ID zadatka koji je poslana.

    Izlaz neće sadržavati rezultate Latencija otprilike ovako.

    ![Ping pong Latencija][pingpong1]

4. Za procjenu propusnost između parova čvorove Azure neprekidnog rada, upišite sljedeću naredbu za slanje pokretanje **mpipingpong** s veličinu velike paketa i mali broj iteracija:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Naredba vraća Identifikator posla koji se šalje.

    Na paket HPC klaster koji se implementiran na Azure VMs, izmijenite naredbu kao što je naznačeno u koraku 2.

5. Nakon dovršetka posla, da biste pogledali izlaz (u ovom slučaju, izlazna zadatak 1 posla) upišite sljedeće:

    ```
    task view <JobID>.1
    ```

  Izlaz neće sadržavati rezultate propusnost otprilike ovako.

  ![Ping pong propusnost][pingpong2]


### <a name="mpi-application-considerations"></a>Razmatranja MPI aplikacije


Slijede zahtjevi za pokretanje aplikacija MPI paketom HPC u Azure. Neke se odnose samo na implementacijama Azure čvorove (tempiranja uloga instance dodali u konfiguraciji "neprekidnog rada za Azure").

* Instance uloga suradnika u oblaku se povremeno brišu bez prethodne najave po Azure (na primjer, za održavanje sustava ili u slučaju da ne uspije instance). Ako instance se brišu dok se izvodi na MPI posla, instancu gubi njegove podatke, te se vraća u stanje kada je najprije uveden, što može izazvati MPI posla uvoza. Pokreće dodatne čvorove koje koristite za jedan zadatak MPI i dulje posla, vjerojatnije da jedan od instance se brišu tijekom posao je pokrenut. Preporučujemo to i ako ste postavili jedan čvor implementacije kao datotečnom poslužitelju.


* Da biste pokrenuli MPI zadatke u Azure, ne morate koristiti zaslonom RDMA instance. Možete koristiti bilo koje veličine instancu koju podržava HPC paket. Međutim, zaslonom RDMA slučajevima preporučuje se za izvođenje relativno veliki MPI zadataka koji su osjetljive latenciju i propusnosti mreže koja se povezuje čvorove. Ako koristite druge veličine da biste pokrenuli Latencija i propusnosti osjetljivih MPI zadataka, preporučujemo da koristite small zadataka, u kojem se jedan zadatak pokreće na samo nekoliko čvorove.

* Aplikacija primijenjenim na instance Azure podliježe uvjete licenciranja povezan s aplikacijom. Obratite se dobavljača bilo koje komercijalnog aplikacije za licenciranje ili nekog drugog ograničenja za pokretanje u oblaku. Svi proizvođači nude pay-as-you-go licenciranje.


* Azure instance potrebno daljnje postavljanje pristupa lokalnim čvorove, zajedničko korištenje i poslužitelji licence. Ako, na primjer, da biste omogućili Azure čvorove da biste pristupili na lokalni poslužitelj licence, možete konfigurirati Azure virtualne mreže web-mjesto.


* Da biste pokrenuli MPI aplikacije na Azure instance, registrirate svaku aplikaciju MPI Vatrozid za Windows na instance tako da pokrenete naredbu **hpcfwutil** . Time se omogućuje komunikaciju MPI odvijati priključka vatrozidu dodijeljenog dinamički.

    >[AZURE.NOTE] Za neprekidnog rada Azure implementacije, možete konfigurirati i naredbe iznimku Vatrozid za automatsko pokretanje na sve nove čvorove Azure koje su dodane na klaster. Nakon pokretanja naredbe **hpcfwutil** i provjerite funkcionira li vaša aplikacija, dodajte naredbu skriptu za pokretanje za vaše Azure čvorove. Dodatne informacije potražite u članku [Korištenje skriptu za pokretanje za Azure čvorove](https://technet.microsoft.com/library/jj899632.aspx).



* Paket HPC koristi varijablu okruženja CCP_MPI_NETMASK klaster da biste odredili raspon prihvatljiva adrese za MPI komunikaciju. Pokretanje u HPC paket 2012 R2, varijablu okruženja CCP_MPI_NETMASK klaster utječe samo na MPI komunikaciju između domene pridruženo klaster računalnim čvorove (bilo lokalno ili na Azure VMs). Varijabla bit će zanemarene čvorovi dodane u na neprekidnog rada Azure konfiguracije.


* MPI poslove ne može pokrenuti kroz sve instance Azure koje su uvedene u različitim oblaka services (na primjer, u neprekidnog rada Azure implementacijama s različitim čvor predložaka ili Azure VM računalnim čvorove implementiran u većem broju servisa u oblaku). Ako imate više implementacijama Azure čvor koji su pokrenuti s različitim čvor predlošcima, MPI posao mora se izvoditi na samo jedan skup Azure čvorove.


* Kada Azure čvorove dodati svoj klaster, a ih online, servis Raspored za HPC odmah pokuša pokrenuti zadatke na čvorove. Ako samo dio svoje radno opterećenje mogu se izvoditi na Azure, provjerite je li ažurirati ili stvaranje predložaka za posao da biste odredili što vrste posla mogu se izvoditi na Azure. Ako, na primjer, da biste bili sigurni da poslove koje su poslane s predloškom posao izvoditi samo na Azure čvorove, dodavanje svojstvo čvor grupe predložak posla i odaberite AzureNodes kao obaveznu vrijednost. Da biste stvorili prilagođene grupe za vaše Azure čvorove, koristite cmdlet za dodavanje HpcGroup HPC PowerShell.


## <a name="next-steps"></a>Daljnji koraci

* Umjesto toga pomoću HPC paket, razviti sa servisom Azure grupe da biste pokrenuli MPI aplikacije za upravljane grupe računalnim čvorove u Azure. Potražite u članku [Korištenje više instanci zadaci za pokretanje aplikacije sučelja prosljeđivanje poruke (MPI) u grupe za Azure](../batch/batch-mpi.md).

* Ako želite da biste pokrenuli Linux MPI aplikacije koje pristup mreži Azure RDMA, potražite u članku [Postavljanje Linux RDMA klaster da biste pokrenuli MPI aplikacije](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png