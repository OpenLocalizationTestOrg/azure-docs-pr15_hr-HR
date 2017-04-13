<properties
   pageTitle="Vodič za brzi početak rada za Ručna instalacija SAP HANA na Azure VMs | Microsoft Azure"
   description="Vodič za brzi početak rada za Ručna instalacija SAP HANA na Azure VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Vodič za brzi početak rada za Ručna instalacija jednokratni SAP HANA na Azure VMs

## <a name="introduction"></a>Uvod

Da biste postavili na jednokratni SAP HANA predložak/pokazni videozapis sustav na Azure VMs Ručna instalacija SAP NetWeaver 7.5 i SAP HANA SP12 pomoći će ovaj vodič za brzi početak rada.
Vodič presumes da je čitač upoznati s osnovama Azure IaaS kao što su kako implementirati virtualnim strojevima ili virtualne mreže ili putem portala za Azure ili Powershell/EŽA uključujući mogućnost za korištenje json predložaka. Osim toga očekuje da je čitač poznato SAP HANA, SAP NetWeaver i kako ga instalirati na lokalni.

Očekuje se da je čitač umu Općenito dokumentacije SAP Azure spomenute u odjeljku općenite informacije na kraju članka.

Zbog ograničenja u koje nisu produkcijske sustave ovaj vodič će pokrivaju teme kao što su HA, sigurnosno kopirati, DR, visoke performanse i posebnih sigurnosna pitanja vezana uz.

Postavljanje oglednih je gotovo pomoću dva virtualnih računala da biste izvršili raspodijeljeno instalacije SAP NetWeaver putem model upravljanja resursima Azure kao SAP Linux Azure je podržana samo na Voditelj resursa Azure i ne klasični modela. Veze na dodatne informacije o Azure Voditelj resursa pronaći ćete u odjeljku općenite informacije na kraju ovog članka.

Te su VMs dva testa koji se koristi za instalaciju uzorka:

* hana appsrv (Vrsta DS3) za hostiranje instancu NW 7.5 ASCS + PAS
* hana-dbsrv (Vrsta GS4) glavno računalo za HANA SP12
* oba VMs pokazali jedan Azure virtualne mrežom (azure-hana-test-vnet)
* OS u oba slučaja je SLES 12 SP1


Imajte na umu činjenica kroz da od 2016 srpanj SAP HANA je samo u potpunosti podržane za OLAP (BW) produkcijske sustave tipu Azure VM GS5. Za testiranje gdje nešto ne očekuje službeni SAP podršku je precizno da biste koristili nešto manje kao što je npr GS4.
Što uvijek želite koristiti za SAP HANA na Azure je Azure premium prostor za pohranu HANA podataka i datoteka zapisnika - potražite u članku "Postavljanje Disk" sekcije daljnje dolje. O detaljima koje SAP proizvodi podržava na Azure provjerite je li u odjeljku općenite informacije na kraju ovog članka.


Vodič opisuju dva načina da biste ručno instalirali HANA SAP-a na Azure VMs:

* Instalacija SAP HANA putem SAP softver dodjeljivanje Manager (SWPM) kao dio raspodijeljeno instalacije NetWeaver u koraku "instanca baze podataka"
* Instalacija HANA SAP-a pomoću alata hdblcm HANA životnog ciklusa upravitelj, a zatim instalirajte NetWeaver

Također je moguće koristiti SWPM i instalirajte sve komponente (HANA SAP, poslužitelj aplikacije za SAP, ASCS instance GUI SAP-a) u jedan jedan VM. Ta mogućnost nije opisano u ovom članku, ali stavke koje ste smatraju jednake.

Prije pokretanja instalacije u odjeljku nakon kontrolne popise ispod o postavljanju VMs Azure test trebali biste pročitati izbjegavanja nekoliko osnovnih pogrešaka koje će se dogoditi kada se koristi samo zadanu konfiguraciju Azure VM.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Kontrolni popis za SAP HANA instalaciju putem SWPM SAP-a

To je jednostavan popis za provjeru ključ instalaciju pojedinačnih stavki koje su povezane Ručna instalacija SAP HANA jednokratni za pokazni videozapis ili prototyping pursposes putem SWPM SAP način raspodijeljeno 7.5 NW SAP-a. Pojedinačne stavke su objašnjenje i prikazati u obliku snimke zaslona detaljno cijeloj u članku:

* Stvaranje Azure virtualne mreže koja će sadržavati VMs dva testa 
* Implementacija dva Azure VMs sa servisnim paketom SP1 OS SLES 12 putem upravitelja resursa Azure modela 
* Prilaganje dvije standardne prostora za pohranu diskova poslužitelj app VM (npr. 75GB i 500GB)
* Prilaganje četiri diskova poslužitelja HANA DB VM - 2 standardnih prostora za pohranu diskova kao što su za poslužitelj aplikacije VM + 2 premium prostora za pohranu diskova (npr. 2x512GB)
* Ovisno o veličina i/ili propusnost preduvjeti priložiti više diskova i stvaranje prugastim količine ili pomoću lvm ili mdadm na razini OS unutar na VM
* Stvaranje XFS datotečnih priložene diskova / logičke jedinice
* Postavljanje novog XFS datotečnih sustava na razini OS. Zadržavanje sve programe za SAP i drugu npr za sapmnt direktorija i možda sigurnosne kopije pomoću jedne datotečnom sustavu. Na SAP HANA DB server postavljanja na XFS datotečnih sustava na diskova za pohranu premium kao /hana i /usr/sap to je sve potrebno da biste izbjegli da datotečnom sustavu korijenski koji nije prevelika na Linux Azure VMs puni
* Unesite lokalna ip adrese testa VMs u/itd/domaćini
* unos parametra nofail u /etc/fstab
* Postavljanje parametara otklanjanje prema bilješke SAP HANA SLES 12 (pogledajte detalje niže u odjeljku otklanjanje paramters)
* Dodavanje prostora zamjena
* Ako istaknuti - instalirati grafički radne površine na test VMs. U suprotnom koristite udaljene sapinst instalacije
* preuzimanje softvera SAP-a web-mjestu servisa marketplace SAP-a
* Instalacija instancu ASCS SAP-a na poslužitelju aplikacije VM
* zajedničko korištenje direktorija sapmnt putem NFS među test VMs (poslužitelj app VM je NFS server)
* Instalacija uključujući HANA putem SWPM na poslužitelju DB VM instanci baze podataka
* Instalacija sustava PAS na poslužitelju aplikacije VM
* Pokrenite MC SAP-a i povezivanje – primjerice putem SAP GUI / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Kontrolni popis za SAP HANA instalaciju putem hdblcm

To je jednostavan popis za provjeru ključ instalaciju pojedinačnih stavki koje su povezane Ručna instalacija SAP HANA jednokratni za pokazni videozapis ili prototyping pursposes putem SWPM SAP način raspodijeljeno 7.5 NW SAP-a. Pojedinačne stavke su objašnjenje i prikazati u obliku snimke zaslona detaljno cijeloj u članku:

* Stvaranje Azure virtualne mreže koja će sadržavati VMs dva testa 
* Implementacija dva Azure VMs sa servisnim paketom SP1 OS SLES 12 putem upravitelja resursa Azure modela 
* Prilaganje dvije standardne prostora za pohranu diskova poslužitelj app VM (npr. 75GB i 500GB)
* Prilaganje četiri diskova poslužitelja HANA DB VM - 2 standardne kao što je prostor za pohranu poslužitelj app VM + 2 premium prostora za pohranu diskova (npr. 2x512GB)
* Ovisno o veličina i/ili propusnost preduvjeti priložiti više diskova i stvaranje prugastim količine ili pomoću lvm ili mdadm na razini OS unutar na VM
* Stvaranje XFS datotečnih priložene diskova / logičke jedinice
* Postavljanje novog XFS datotečnih sustava na razini OS. Zadržavanje sve programe za SAP i drugu npr za sapmnt direktorija i možda sigurnosne kopije pomoću jedne datotečnom sustavu. Na SAP HANA DB server postavljanja na XFS datotečnih sustava na diskova za pohranu premium kao /hana i /usr/sap to je sve potrebno da biste izbjegli da datotečnom sustavu korijenski koji nije prevelika na Linux Azure VMs puni
* Unesite lokalna ip adrese testa VMs u/itd/domaćini
* unos parametra nofail u /etc/fstab
* Postavljanje parametara otklanjanje prema bilješke SAP HANA SLES 12 (pogledajte detalje niže u odjeljku otklanjanje paramters)
* Dodavanje prostora zamjena
* Ako istaknuti - instalirati grafički radne površine na test VMs. U suprotnom koristite udaljene sapinst instalacije
* preuzimanje softvera SAP-a web-mjestu servisa marketplace SAP-a
* Stvaranje grupe "sapsys" s id grupe 1001 na VM za poslužitelj HANA DB
* Instalacija HANA SAP-a na poslužitelju DB VM pomoću hdblcm
* Instalacija instancu ASCS SAP-a na poslužitelju aplikacije VM
* zajedničko korištenje direktorija sapmnt putem NFS među test VMs (poslužitelj app VM je NFS server)
* Instalacija uključujući HANA putem SWPM na poslužitelju DB VM instanci baze podataka
* Instalacija sustava PAS na poslužitelju aplikacije VM
* Pokrenite MC SAP-a i povezivanje – primjerice putem SAP GUI / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Priprema Azure VMs za Ručna instalacija HANA SAP-a

Ovo poglavlje o pripremi Azure VMs radi Ručna instalacija SAP HANA sastoji se od pet sekcije koje pokrivaju u sljedećim temama:

* Postavljanje na disku
* Otklanjanje parametara
* Filesystems
* / itd/domaćini
* / itd/fstab


### <a name="disk-setup"></a>Postavljanje na disku

Datotečnom sustavu korijenski u Linux VM na Azure je ograničeno veličine. Stoga je potrebno da biste priložili dodatni prostor na disku na VM za pokretanje SAP-a. U slučaju SAP server aplikacije VM koristi u okruženju čisto predložak/pokazni videozapis je u redu da biste koristili diskova Azure standardne prostora za pohranu. Dok za SAP HANA DB podatke i zapisnika datoteke - diskova za pohranu Azure Premium želite koristiti čak i u koje nisu radnog vodoravno.

U odjeljku Detalji o kako priložiti diskova Linux VM [ovdje](virtual-machines-linux-add-disk.md)

Kada je riječ o Azure disk predmemoriranje – jedan morate koristiti "Nema" za diskova koji će se koristiti za spremanje zapisnika transakcije HANA. HANA podatkovne datoteke je u redu da biste koristili pročitajte predmemoriranje. Kao što je HANA baze podataka u memoriji ovisi o cjelokupan korištenje uzorka koliko čitanja predmemoriju na razini Azure disk će poboljšati performanse (npr. početni HANA i čitanje podataka s diska u memorije).

Prikaz detalja o Azure Premium pohranu [ovdje](../storage/storage-premium-storage.md)

[Ovdje](https://github.com/Azure/azure-quickstart-templates) pronaći Ogledni predlošci json da biste stvorili VMs.
U "101-vm – jednostavno-linux" prikazan je Izgled Osnovni predložak kao što su uključujući odjeljak prostora za pohranu koji se dodaje na disku 100GB podataka.

[Ovaj članak](virtual-machines-linux-sap-on-suse-quickstart.md) sadrži neke informacije o traženju slika SUSE putem komponente Powershell ili EŽA i važnost koju želite priložiti na disku putem UUID.


Ovisno o veličini i propusnost preduvjetima možda će biti potrebno priložiti više diskova umjesto jednog i kasnije pri stvaranju pruga postavljena na te diskova na razini OS. Ovo su dva aspekte Zašto nešto stvorile pruga postavite na više Azure diskova:

* povećajte propusnost
* potrebno je jedan datotečnom sustavu > 1TB trenutno ograničenje veličine Azure disk je 1TB (na 2016 srpanj)


Dodatne informacije o dva glavna alata za konfiguriranje Podjela nalazi se ovdje:

[Članak o korištenju mdadm da biste konfigurirali Linux raid softver na VM programa Azure](virtual-machines-linux-configure-raid.md)

[Članak o konfiguriranju Upravitelj logičkih glasnoće na VM za Azure Linux](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

U testu okruženje dva Azure standardne prostora za pohranu diskova su priložiti aplikacija SAP server VM. Jedan je korišten za pohranu sve programe SAP-a za instalaciju (npr. NetWeaver 7.5, SAP GUI SAP HANA... ) i drugu imati dovoljno prostora za ono što se može zahtijevati (primjerice sigurnosno kopirati, test podataka) te direktorija sapmnt (npr. SAP profili) da biste zajednički koristiti sve VMs koji pripadaju isti vodoravno SAP-a.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

Contrary to poslužitelj app diskova VM četiri su priložiti SAP HANA server VM. Kao što su prije dva diskova korišten za održavanje SAP softver (nešto nije moguće zajednički koristiti softver na disku SAP putem NFS) i imate dovoljno prostora npr za sigurnosno kopiranje. Dodatnih dva diskova su Azure Premium prostora za pohranu diskova da biste zadržali SAP HANA podataka i datoteka zapisnika, kao i /usr/sap direktorija.


### <a name="kernel-parameters"></a>Otklanjanje parametre


SAP HANA zahtijeva određene postavke otklanjanje Linux koji nisu dio slike standardnih Azure Galerija i morati ručno postavljanje. Nema određenu bilješku SAP-a koji opisuje postavke. 


Napomena SAP SAP HANA DB: Preporučenih postavki OS za SLES 12 / SLES za SAP aplikacije 12: [2205917 bilješke SAP-a](https://launchpad.support.sap.com/#/notes/2205917)

Jedan dodatnih tema reagrding-predmemorije stranice povezani s pokretanjem HANA SAP-a na SLES nalazi se [ovdje](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) poglavlja 6.1 otklanjanje: ograničenje predmemorije stranice

Postoji i SAP bilješke vezane uz ograničenje predmemorije stranice [1557506 bilješke SAP-a](https://service.sap.com/sap/support/notes/1557506)

SLES 12 sadrži novi alat koji zamjenjuje stari utility sapconf. Je "počeo gledati-Admin" i nema dostupnog posebno SAP HANA profila. Jedan možete pronaći dodatne informacije o alatu pratiti dvije veze u nastavku.

Moguće je pronaći SLES dokumentaciju o počeo gledati Admin profila sap-hana [ovdje](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

SLES dokumentaciju o počeo gledati Admin profila sap-hana - poglavlja 6.2 usklađivanje sustavi za SAP radnih opterećenja s počeo gledati-Admin – pronaći ćete [ovdje](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Ovdje nešto vidjeti kako promijeniti "počeo gledati-Admin" na transparent_hugepage, kao i vrijednosti numa_balancing prema potrebne postavke HANA SAP-a.


Da biste postavke otklanjanje SAP HANA trajna jedno je korištenje grub2 na SLES 12. Dodatne informacije o grub2 nalazi se [ovdje](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Snimka zaslona prikazuje način na koji su postavke otklanjanje promijenjen u konfiguracijskoj datoteci i zatim "prevedena" putem grub2 mkconfig


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Da biste promijenili postavke putem Yast i parametar postavke za pokretanje Loader otklanjanje bilo bi neku drugu mogućnost.


### <a name="filesystems"></a>Filesystems 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Ovdje nešto možete vidjeti dva datotečnih koji su stvoreni na poslužitelju aplikacija SAP VM pri vrhu dva diskova priložene Azure standardne prostora za pohranu. Oba filesystems su vrste XFS i postavljen da /sapdata i /sapsoftware.

Nije obavezno to učiniti na taj način. Dostupne su različite mogućnosti kako sturcture prostora na disku.
Najvažnije razmjer je da biste izbjegli datotečnom sustavu korijenski ponestane prostora. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Vezane uz DB VM za SAP HANA važno je znati da tijekom instalacije baze podataka putem sapinst (swpm) i koristite mogućnost jednostavne "standardne" Instalacija je instalirat će sadržaji prema zadanim postavkama u odjeljku /hana i /usr/sap. Zadane postavke za sigurnosno kopiranje zapisnika SAP HANA npr je u odjeljku /usr/sap.
Slično prije nego što je ključ da biste izbjegli datotečnom sustavu korijenski ponestane prostora. Stoga nešto provjerite je li ima li dovoljno slobodnog prostora u odjeljku /hana i /usr/sap prije instalacije SAP HANA putem swpm.

[U ovom se članku](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) iz SAP opisuje izgled standardne datotečnom sustavu HANA SAP-a 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Prilikom instaliranja NetWeaver SAP-a na standardni SLES 12 Azure galerije pojavit će se poruka da nema prostora Zamijeni. Možete riješiti ovu poruku nešto npr. ručno dodati Zamjena datoteke kao što je opisano u ovom dokumentu putem dd, mkswap i swapon. Samo potražite "Dodavanje samo Zamjena datoteke ručno" u [ovom članku](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Druga je mogućnost da biste konfigurirali prostor zamjena putem agent Linux VM. Dodatne informacije možete pronaći [ovdje](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ itd/domaćini

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Druge aspekte važne prije početka da biste instalirali SAP je uključiti nazivi glavnog računala i IP adrese VMs SAP-a u datoteku /etc/hosts. Jedan treba uvesti sve VMs SAP unutar jedne mreže Azure virtualne i koristiti interne IP adrese.

### <a name="etcfstab"></a>/ itd/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Tijekom testiranja faze ga izgleda biti dobro dodavanja parametra nofail fstab. Ako nešto pošlo po redu s na diskova na VM će će i dalje pojavljuju i nije "smrzavanje" u postupku pokretanja. Ali jedno je da biste pogledali kao u ovom slučaju na dodatni prostor na disku možda neće biti dostupne i procesa ispuni datotečnom sustavu korijen. U slučaju da bi /hana nedostaje HANA SAP-a ne može pokrenuti iako uopće.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Instalacija grafički Gnome radne površine na SLES 12

Ovo poglavlje sastoji se od dva secitions koji pokrivaju u sljedećim temama:

* Instalacija Gnome radne površine i xrdp na SLES 12
* Pokretanje utemeljenih na SLES 12 pomoću preglednika Firefox MC SAP-a

Nešto nije moguće koristiti alternativa kao što su Xterminal, VNC, ali od Sep 2016 samo u članku xrdp.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Instalacija Gnome radne površine i xrdp na SLES 12

Posebno za osobe koje imaju pozadinu Microsoft Windows, a želite koristiti grafičkog radnu površinu izravno unutar Linux VMs SAP-a da biste pokrenuli Firefox, Sapinst, SAP GUI, MC SAP-a ili HANA Studio i možda povezati VM putem RDP Microsoft Windows s računala sa sustavom postoji jednostavan način da biste to postigli. Dok je možda nije odgovarajuće npr za poslužitelj baze podataka za radni je u redu za okruženje čisto predložak/pokazni videozapis. Evo nekoliko koraka da biste instalirali Gnome radne površine na programa VM za Azure SLES 12:

Instalirajte gnome radne površine na sljedeću naredbu (npr. u prozoru za putty):

   zypper u -t uzorak gnome basic

Instalirajte xrdp omogućuje povezivanje VM putem RDP:

   zypper u xrdp

Uređivanje /etc/sysconfig/windowmanager i postavite o zadanom upravitelju windows Gnome:

   DEFAULT_WM = "gnome"

Pokrenite chkconfig da biste provjerili te xrdp pokreće se automatski nakon ponovnog pokretanja: 

  chkconfig-xrdp razinu 3 na

u slučaju da mora biti problem s vezom za RDP pokušajte ponovno pokrenuti (možda izvan putty prozor):

  ponovno pokretanje /etc/xrdp/xrdp.SH

u slučaju da ponovno pokretanje xrdp kao spomenutih ne funkcioniraju potvrdite ako postoji datoteka .pid i uklonite je:

  Provjera /var/run i pogledajte xrdp.pid   
  Uklanjanje i ponovno pokušajte ponovno pokretanje



### <a name="sap-mc"></a>SAP MC


Da biste započeli s grafički utemeljena na SAP MC iz Firefox izvodi u do 12 VM za Azure SLES nakon instalacije na Gnome radne površine opisan u prethodnom odjeljku nešto će dođe do pogreške zbog nedostaje dodatak za Java preglednika.

URL adresa da biste pokrenuli SAP MC je <server>: 5 < instance_number > 13

Dodatne informacije možete pronaći [ovdje](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Na snimku zaslona koja se nalazi iznad nešto možete vidjeti kako se poruka o pogrešci može izgledati kada nedostaje dodatak za Java preglednika. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Jednostavno je da biste instalirali dodatak za nedostaje putem Yast jedan mogućnost da biste riješili taj problem.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Ponavljajuća SAP MC URL će otvorili malo dijaloški prvi put koja vas pita da biste aktivirali dodatak.


Jedan dodatni problem koji može iskakati se poruka o pogrešci o nedostaje datoteka: javafx.properties to vrlo je vjerojatno povezani s instalacijom Java 1.8 koje je potrebno za 7,4 GUI SAP-a

Verzija IBM Java vidjeti putem Yast ne sadrži tu datoteku. Rješenje je preuzeti Java iz Oracle.
Članak koji govori o ovom konkretan problem se može pronaći [ovdje](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Ručna instalacija SAP HANA putem SWPM kao dio NetWeaver 7.5 instalacije


Sljedeći popis snimke zaslona prikazuje ključne korake prilikom instalacije SAP NetWeaver 7.5 i SAP HANA SP12 putem SWPM (sapinst). Kao dio NW 7.5 instalacije SWPM sadrži mogućnosti za i instalirate HANA bazu podataka kao jednokratan.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Za testiranje uzorka okruženje samo jedan ABAP aplikacije poslužitelj je bio instaliran. Mogućnost "Distributed sustava" korišten za instalaciju instancu ASCS i instanca poslužitelja primarni aplikacije u jedan Azure VM i SAP HANA kao sustav baze podataka u drugu VM za Azure.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Kada instancu ASCS je instalirana na poslužitelju aplikacije VM i postavljeno na "zeleno" u u SAP MC direktorija sapmnt koja obuhvaća npr. imeniku profila SAP je za zajedničko korištenje s poslužiteljem SAP HANA DB VM.
DB koraka instalacije je potreban pristup tim informacijama. Najbolji način je da biste koristili NFS koji se može konfigurirati pomoću Yast.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Na aplikaciju poslužitelja VM direktorija sapmnt trebali zajednički koristiti putem NFS pomoću mogućnosti "rw" i "no_root_squash". Zadana vrijednost je "redaka" i "root_squash", što može uzrokovati poteškoće s prilikom instalacije instanca baze podataka.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

Na poslužitelju SAP HANA DB VM na sapmnt zajednički koristite s poslužiteljem aplikacije VM je konfigurirati putem "NFS klijenta" (npr. uz pomoć Yast)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Zatim jednu mora prijaviti na poslužitelj za SAP HANA DB VM i počnite SWPM da biste izvršili sljedeći korak raspodijeljeno instalacija NW 7.5 – "Instanca baze podataka".


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Povezani s instalacijom HANA SAP-a koji se ne postoji zapravo prevelik da biste unijeli nakon odabira "standardne" instalacija. Osim toga put do medija installatiom nešto mora unijeti DB SID, naziv glavnog računala, broj instanci i lozinke za administratore sistemskim DB.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Sljedeći je korak unesite lozinku za DBACOCKPIT shemu.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Zatim dolazi pitanje za lozinku sheme SAPABAP1.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Na kraju mora biti samo zeleni kvačicama ispred svakog faza procesa instalacije DB i okvir malo poruke koja se pojavljuje trebalo bi pisati "izvođenja od... Instanca baze podataka je dovršen".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Nakon uspješne instalacije SAP MC treba prikazati instancu DB kao "zelenoj" i cijeli popis SAP HANA procesa (npr. hdbindexserver, hdbcompileserver)


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Snimka zaslona prikazuje dijelove struktura datoteke u odjeljku /hana/shared koji je stvorio tijekom instalacije HANA SWPM. Pojavila se nema mogućnosti da biste naveli drugi put. Stoga je tako važan postavljanja dodatni prostor na disku u odjeljku /hana prije instalacije SAP HANA putem SWPM da biste izbjegli datotečnom sustavu korijenski ponestane slobodnog prostora.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Ovdje nešto možete vidjeti na isti način kao što je opisano prije direktorij /usr/sap.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Posljednji korak raspodijeljeno instalacije ABAP je "Primarni aplikacije instanca poslužitelja"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Kada imate instaliran PAS, kao i SAP GUI nešto možete provjeriti putem transakcije "dbacockpit", koji se sve čini izravno s instalacijom HANA SAP-a.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

U posljednjem prvog koraka nije instalirati SAP HANA Studio poslužitelju aplikacija za SAP VM te povezivanje s instancom HANA SAP-a koji se izvodi na poslužitelju DB VM.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Ručna instalacija SAP HANA putem HANA životnog ciklusa Upravitelj alata hdblcm


Osim instalacije SAP HANA kao dio raspodijeljeno instalaciju putem SWPM je possibe najprije instalirajte HANA samostalne pomoću hdblcm a zatim instalirati npr SAP NetWeaver 7.5 naknadno. Na popisu ispod snimke zaslona prikazuje kako to funkcionira.

Ovo su tri izvora podataka o alatu za hdblcm HANA:

[Odabir HDBLCM HANA točan SAP zadatka](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[Alati za upravljanje SAP HANA životni ciklus](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[Instalacija poslužitelja HANA SAP i vodič za ažuriranje](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Da biste izbjegli pokrenut na probleme s kasnije zadana postavka id grupe za u \<HANA SID\>Admin korisnički (alatom za hdblcm) nešto morate definirati nove grupe naziva "sapsys" s id grupe 1001 prije instalacije HANA SAP-a pomoću hdblcm.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Pokretanje hdblcm prvi put prikazat će se izbornik jednostavne start gdje nešto ima da biste odabrali stavku 1 "koji instalirati novi sustav"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Snimka zaslona nešto možete pogledati sve ključne mogućnosti koje su unijeli prije. Važno - direktorija koji su pod nazivom HANA jedinicama zapisnika i podataka, kao i put instalacije (/ hana/zajednički se koristi u ovom primjeru), a/korsnik pitanje/sap ne smije biti dio datotečnom sustavu korijen, ali pripadaju diskova Azure podataka koje su priložene na VM kao što je opisano u odjeljku Postavljanje Azure VM. To će izbjeći rizika koji datotečnom sustavu korijenski moglo bi vam ponestati prostora.
Jedan, možete vidjeti da korisnik administrator HANA korisnički id 1005 i dio grupe sapsys (id 1001) koja je definirana prije instalacije.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Jedan možete provjeriti na HANA \<HANA SID\>Detalji o korisniku Admin (azdadm u ovom primjeru) u/itd/passwd



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Nakon instalacije HANA SAP-a pomoću hdblcm može se vidjeti u Studio HANA SAP-a. Shema SAPABAP1 koja obuhvaća npr sve SAP NetWeaver tablice još nije dostupno.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Nakon instalacije SAP HANA nešto možete instalirati NetWeaver SAP-a nudi. U ovom primjeru ga je učinjeno putem "raspodijeljeno instalacije" putem SWPM kao što je opisano u odjeljku odgovarajuću iznad.
Prilikom instalacije instanca baze podataka putem SWPM nešto samo mora unos jednakih podataka kao prije s hdblcm (primjerice naziv glavnog računala, ID HANA, broj instanci). SWPM pa se korištenje postojeće instalacije HANA i dodajte dodatne sheme.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

To je slika koraka instalacije SWPM gdje jedna sadrži za unos podataka o shemi DBACOCKPIT.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Zatim SWPM očekuje da biste unijeli podatke o shemi SAPABAP1.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Po dovršetku instance instalacija za SWPM baze podataka jedan možete vidjeti shemom SAPABAP1 u HANA Studio.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

I na kraju nakon instalacije poslužitelja aplikacija SAP i SAP GUI nešto trebali biste moći provjerite je li instanca HANA DB s transakcije "dbacockpit".




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Općenite informacije vezane uz certifications SAP Azure SAP HANA sustavom Azure i SAP, a preuzimanje softvera

* Općenito SAP Azure oznake strukture doku o pokretanju SAP-a na Azure s operacijskim Sustavom Windows na klasičan način rada: [Korištenje SAP-a na virtualnim računalima sustava Windows u Azure](virtual-machines-windows-classic-sap-get-started.md)

* informacije o postojeće predloške SAP-a za korištenje po klijentima: [Azure predložaka brzi početak rada SAP-a](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* Općenito SAP Azure oznake strukture doku o pokretanju SAP-a na Azure s operacijskim Sustavom Linux u modelu Voditelj resursa Azure: [Korištenje SAP-a na Linux virtualnim strojevima (VMs)](virtual-machines-linux-sap-get-started.md)

* potvrđenog direktorija hardver HANA SAP-a koji navodi koje vrste Azure VM podržani su za radni: [Potvrđeni HANA SAP® hardver direktorija](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* informacije o veličinama virtualnog računala posebno za radnih opterećenja Linux: [veličine za virtualnim strojevima servisu Azure](virtual-machines-linux-sizes.md)

* SAP bilješke koje se nalaze se sve podržane SAP proizvoda na Azure i podržane vrste Azure VM za SAP: [1928533 bilješke SAP-a](https://launchpad.support.sap.com/#/notes/1928533/E)

* SAP bilješku o SAP "poboljšan nadzor" Linux VMs na Azure: [2191498 bilješke SAP-a](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP HANA koja nudi na Azure "Velike instance". Važno je da razumijete nije o pokretanju HANA SAP-a na Azure VMs, ali u hibridnoj okruženje gdje poslužitelji aplikacija SAP pokreću se u Azure VMs, ali SAP HANA pokreće na poslužiteljima Gola metal: [2316233 bilješke SAP-a](https://launchpad.support.sap.com/#/notes/2316233/E)

* Napomena SAP-a s informacijama o SAPOSCOL na Linux: [1102124 bilješke SAP-a](https://launchpad.support.sap.com/#/notes/1102124/E)

* Ključ nadzor metriku za SAP-a na Microsoft Azure: [SAP bilješke 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Informacije o upravitelju za Azure resursa: [Pregled upravitelja resursa za Azure](../azure-resource-manager/resource-group-overview.md)

* Informacije o implementaciji Linux VMs putem predložaka: [uvođenje i upravljanje virtualnim strojevima pomoću predložaka Voditelj resursa Azure i EŽA Azure](virtual-machines-linux-cli-deploy-templates.md)

* Usporedba implementacije modelima između Voditelj resursa Azure i klasičnu: [Voditelj resursa Azure nasuprot uvođenje klasičnog: razumjeti implementacije modela i stanje resursa](../resource-manager-deployment-model.md)

* Preuzmite NetWeaver 7.5 za Linux/HANA iz trgovine sustava SAP-a servisa:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* Preuzmite HANA SP12 platformu izdanje iz trgovine sustava SAP-a servisa:![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

