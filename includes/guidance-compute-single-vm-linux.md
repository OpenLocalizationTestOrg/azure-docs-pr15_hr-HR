U ovom se članku navode skup dokazana prakse za pokretanje Linux virtualnog računala (VM) na Azure, planiranju skalabilnost, dostupnost, mogućnost upravljanja i sigurnost. Azure podržava izvodi različite popularne Linux distribucija, uključujući CentOS, Debian, crveno je vaša Enterprise, Ubuntu i FreeBSD. Dodatne informacije potražite u članku [Azure i Linux][azure-linux].

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Resursima] [ resource-manager-overview] i classic. U ovom se članku koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Ne preporučujemo korištenje jedne VM za radni radnih opterećenja, jer nema gore vrijeme razine ugovor o usluzi (SLA) za jedan VMs na Azure. Da biste na SLA, morate uvesti više VMs u programa [dostupnost postavljanje][availability-set]. Dodatne informacije potražite u članku [pokretanje više VMs na Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Dodjeljivanje VM u Azure obuhvaća više premještanje dijelova od VM sam. Postoje računalnim, mreže i pohranu elemente koje treba uzeti u obzir.

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram uključen u "računalnim – jedan VM" stranice.

![[0]][0]

- **Grupa resursa.** [_Grupa resursa_] [ resource-manager-overview] je spremnik koja sadrži povezane resurse. Stvorite grupu resursa držite resurse za ovu VM.

- **VM**. Možete Dodjela VM objavljene slike s popisa ili iz datoteke virtualne na tvrdom disku (VHD) koje prenijeti na blobova platforme Azure.

- **Disk je OS.** OS disk je VHD pohranjene u [Azure prostora za pohranu][azure-storage]. To znači da nastavi čak i ako se glavno računalo funkcionira. OS disk je `/dev/sda1`.

- **Disk je privremeni.** Na VM stvara se privremene disku. Disk je pohranjena na fizički pogon na glavno računalo. Je _ne_ sprema Azure prostora za pohranu i može sam otkloniti tijekom ponovna i druge događaje VM životni ciklus. Koristite ovaj disk samo za privremene podatke, kao što su stranice ili Zamjena datoteke. Privremeni disk je `/dev/sdb1` te je postavljen na `/mnt/resource` ili `/mnt`.

- **Diskova podataka.** [Podatkovni disk] [ data-disk] je stalni VHD koristi za podataka aplikacije. Diskova podataka spremaju se u Azure prostor za pohranu, kao što su disku OS.

- **Virtualne mreže (VNet) i podmreži.** Svaki VM u Azure je implementiran u (VNet,) koji je daljnje podijeljen podmreže.

- **Javna IP adresa.** Javnu IP adresu nužan za komunikaciju na VM&mdash;, primjerice putem SSH.

- **Mrežnog sučelja (NIC)**. NIC-a omogućuje VM možete komunicirati s virtualne mreže.

- **Mrežni sigurnosne grupe (NSG)**. [NSG] [ nsg] koristi se za mrežni promet za podmreži Dopusti/odbijanje. Na NSG možete povezati s pojedinačne NIC ili podmreži. Ako se povežete s podmreži, pravila NSG će se primijeniti na sve VMs u tom podmreže.
 
- **Dijagnostika.** Zapisivanje dijagnostičkih podataka je presudne za Upravljanje projektom i otklanjanje poteškoća s VM.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji ne stane i preporuke. 

### <a name="vm-recommendations"></a>Preporuke VM

Preporučujemo DS - a Oznaka – nizu jer te veličine strojno podržavaju [Pohranu Premium][premium-storage]. Odaberite jednu od ovih stroj veličine osim ako imate specijalizirane radno opterećenje kao što su računalstvo visokih performansi. Detalje potražite u članku [virtualnog računala veličine][virtual-machine-sizes]. Prilikom pomicanja postojeće radno opterećenje za Azure, započnite s veličina VM koji je najsličniji onome poslužitelja na lokaciji. Izmjerite performanse sustava stvarni radno opterećenje procesora koji se odnose, memorije i disk unos izlaznih operacija sekundi (IOPS), a ako je potrebno prilagoditi veličinu. Osim toga, ako trebate više NIC-ovi, imajte na umu ograničenja NIC za svaki veličinu.  

Prilikom dodjele resursa u VM i drugi resursi, morate navesti mjesto. Općenito govoreći, odaberite mjesto na najbliži internim korisnicima ili klijente. Međutim, nije svih veličina VM dostupni su sva mjesta. Detalje potražite u članku [Servisi po regijama][services-by-region]. Da biste dobili popis veličina VM na određenom mjestu, pokrenite sljedeću naredbu Azure sučelje naredbenog retka (EŽA):

```
    azure vm sizes --location <location>
```

Informacije o odabiru objavljene VM slike potražite u članku [Kretanje i slike odaberite Azure virtualnog računala][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Na disku i pohranu preporuke

Za najbolje performanse diska/i, preporučujemo da [Premium prostora za pohranu][premium-storage], koji pohranjuje podatke na solid-state pogonima (SSDs). Trošak temelji se na veličinu dodijeljenu diska. IOPS i propusnost (to jest, brzina prijenosa podataka) ovisiti i na disku veličina pa prilikom Dodjela resursa na disku, imajte na umu sva tri čimbenika (kapacitetu, IOPS i propusnost). 

Jednog računa za pohranu podržavaju VMs 1 do 20.

Dodajte jedan ili više diskova. Kada stvorite na VHD je Neoblikovani. Prijavite se VM formatiranje diska. Prikaz diskova podataka u obliku `/dev/sdc`, `/dev/sdd`i tako dalje. Možete pokrenuti `lsblk` popis uređaja za blok, uključujući na diskova. Da biste koristili na disku podataka, stvorite particija i datoteka sustava i postavljanje diska. Ako, na primjer:

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

Ako imate mnogo podataka diskova, imajte na umu od ukupne/i ograničenja prostora za pohranu računa. Dodatne informacije potražite u članku [Ograničenja na disku virtualnog računala][vm-disk-limits].

Kada dodate podatkovni disk, ID (LUN) broj logičke jedinice je dodijeljen disk. Po želji možete navesti Identifikator LUN &mdash; na primjer, ako ste zamjene na disku i želite zadržati isti LUN ID ili imate aplikaciju koja traži određene LUN ID. Međutim, imajte na umu LUN ID-a mora biti jedinstvena za svaki disk.

Preporučujemo vam da biste promijenili raspored/i, da biste optimizirali za performanse solid-state pogona (SSDs) (koristi pohranu Premium). Uobičajeni preporuka je da biste koristili alat za zakazivanje NOOP SSDs, ali koristite alat kao što su [iostat] praćenje disk/i performansi za svoje radno opterećenje određeni.

- Ostvarili najbolje performanse, stvorite račun zasebnom prostora za pohranu na čuvanje dijagnostičke zapisnike. Standardni račun lokalno suvišnih prostora za pohranu (LRS) je dovoljni za dijagnostičke zapisnike.


### <a name="network-recommendations"></a>Preporuke o mreže

Na javnu IP adresu može biti dinamičke ili statičke. Zadano je dinamički.

- Rezerviranje [statičke IP adrese] [ static-ip] ako vam je potrebna fiksnim IP adresu koja se ne mogu se promijeniti &mdash; na primjer, ako je potrebno da biste stvorili A zapis u DNS-a ili potrebna na IP adresu da biste se whitelisted.

- Možete stvoriti i na potpuno kvalificirani naziv domene (FQDN) traže IP adresu. Zatim možete registrirati [CNAME zapis] [ cname-record] u DNS koja upućuje na FQDN. Dodatne informacije potražite u članku [stvaranje potpuno kvalificirani naziv domene na portalu za Azure][fqdn].

Sve NSGs sadrže skupu [Zadana pravila][nsg-default-rules], uključujući pravilo koje blokira sve ulaznog internetski promet. Zadana pravila nije moguće izbrisati, ali ih možete nadjačati drugih pravila. Da biste omogućili internetski promet, stvorite pravila koja omogućuju dolazni promet na određene priključke &mdash; , na primjer, priključak 80 za HTTP.  

Da biste omogućili SSH, dodavanje pravila NSG koji omogućuje dolazni promet TCP priključka 22.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Na VM možete skaliranje prema gore ili prema dolje tako da [promijenite veličinu VM][vm-resize]. 

Da biste skalirali vodoravno, staviti dva ili više VMs u raspoloživost postavite iza raspoređivača opterećenja. Detalje potražite u članku [pokretanje više VMs na Azure][multi-vm].

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Kao što je naznačeno iznad, postoji bez SLA za jednu VM. Da biste na SLA, morate uvesti više VMs u skupu dostupnost.

Vaš VM može utjecati [Planirano održavanje] [ planned-maintenance] ili [neplanirano održavanje][manage-vm-availability]. Možete koristiti [VM ponovno zapisnika] [ reboot-logs] da biste utvrdili je li planiranog održavanja posljedica VM ponovno pokrenite računalo.

VHDs se sigurnosno po [Prostora za pohranu Azure][azure-storage], koji je replicirati rok trajanja i dostupnost.

Da biste zaštitili od gubitka podataka slučajne tijekom uobičajenih postupaka (na primjer, zbog pogreške korisnika), trebali biste implementirati točke u vrijeme sigurnosne kopije pomoću [snimaka blob] [ blob-snapshot] ili neki drugi alat.

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

**Grupa resursa.** Stavite čvrsto povezano resursi koji se zajednički koristiti iste životnog ciklusa u istoj [grupi resursa][resource-manager-overview]. Grupa resursa omogućuju implementaciju praćenje resursi grupno i zajednički troškove prema grupi resursa za naplatu. Možete i izbrisati resurse kao skup, što je vrlo koristan i za testiranje implementacije. Dodijelite smisleni nazivi resursa. Koji olakšava pronalaženje određenog resursa i razumijevanje njegova uloga. Pogledajte [preporučene konvencije imenovanja za Azure resurse][naming conventions].

**SSH**. Prije stvaranja Linux VM generirati 2048-bitni RSA javno privatni ključ par. Korištenje javnog ključa datoteke prilikom stvaranja na VM. Dodatne informacije potražite [u]članku korištenje SSH s Linux i Mac na Azure[ssh-linux].

**Dijagnostika VM.** Omogući nadzor i Dijagnostika, uključujući metriku osnovni stanja, zapisnika infrastrukture za dijagnostiku i [Pokretanje dijagnostike][boot-diagnostics]. Pokretanje dijagnostike omogućuju dijagnosticiranje neuspješno ako vaš VM dobiti u koje nisu izraditi stanje. Dodatne informacije potražite u članku [Omogućivanje nadzor i dijagnostici][enable-monitoring].  

Sljedeća naredba EŽA omogućuje dijagnostiku:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Zaustavljanje na VM.** Azure čini razliku između "Zaustavljanja" i "Deallocated" stanju. Vam se naplatiti nakon što VM status je "prestao". Vam se naplatiti kada se VM deallocated.

Pomoću sljedeće naredbe EŽA deallocate na VM:

```
    azure vm deallocate <resource-group> <vm-name>
```

- Gumb **Zaustavi** na portalu za Azure deallocates i na VM. Međutim, nakon isključivanja kroz OS-a dok prijavljeni, na VM je zaustavljena, ali _ne_ deallocated, tako da vam i dalje naplatit će vam.

**Brisanje na VM.** Ako ste izbrisali s VM, u VHDs ne brišu se. To znači da se VM možete izbrisati sigurno bez gubitka podataka. Međutim, koji i dalje teretiti za pohranu. Da biste izbrisali na VHD, izbrisati datoteku iz [spremišta blobova][blob-storage].

Da biste onemogućili nenamjerno brisanje, upotrijebite [resursa Zaključaj] [ resource-lock] da biste zaključali resursa za cijelu grupu ili zaključavanje pojedinačnih resursa, kao što su u VM. 

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Automatizirati OS ažuriranja pomoću VM nastavak [OSPatching] . Prilikom dodjele resursa u VM, instalirajte ovaj kućni broj. Možete odrediti koliko često treba instalacije zakrpa i želite li da ponovno nakon zakrpa.

Korištenje [Kontrola pristupa na temelju uloga] [ rbac] (RBAC) za kontrolu pristupa Azure resursa koji implementacije. RBAC možete dodijeliti uloge autorizacije članovima tima DevOps. Ako, na primjer, ulozi čitač možete prikaz Azure resursa, ali ne stvaranje, upravljanje ili ih izbrisati. Neke uloge su specifične za određeni Azure resurs vrste. Na primjer, ulogu suradnika virtualnog računala možete ponovno pokrenite ili deallocate na VM, ponovno postavljanje lozinke administratora, stvorite na VM i tako dalje. Druge [ugrađene RBAC uloge] [ rbac-roles] koji bi mogli biti korisni za ovu referenca arhitektura obuhvaćaju [DevTest Labs korisnika] [ rbac-devtest] i [Mreža suradnika][rbac-network]. 

Korisnik može biti dodijeljena uloga za više, a možete stvoriti prilagođene uloge za još više preciznije dozvole.

> [AZURE.NOTE] RBAC ograničiti akcije koje je korisnik prijavljen na VM može izvoditi. Dozvole ovise o vrsti računa gosta OS.   

Korištenje [zapisnika nadzora] [ audit-logs] da biste vidjeli dodjeljivanje akcije i druge događaje VM.

- Razmislite o [Šifriranje Azure] [ disk-encryption] ako vam je potrebna za šifriranje diskova OS i podatke. 

## <a name="solution-deployment"></a>Uvođenje rješenja

[Uvođenje] [ github-folder] za referencu dostupna arhitektura koji pokazuje najbolje prakse. Arhitektura ovu referenca sadrži virtualne mreže (VNet), mreže sigurnosne grupe (NSG) i jedan virtualnog računala (VM).

Implementacija arhitektura ovu referenca na nekoliko načina. Najjednostavniji način tako da slijedite ove korake: 

1. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru".
[![Implementacija Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Kada se veza otvorila na portalu za Azure, morate unijeti vrijednosti za neke postavke: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-single-vm-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivati **Predložak korijenski Uri** ili tekstne okvire **Parametar korijenski Uri** .
    - Odaberite **Vrstu Os** na padajućem okviru **linux**.
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

4. Datoteka parametar uključuje programiranih administrator korisničko ime i lozinku, a preporučljivo je znači da odmah promijeniti oba. Kliknite na VM pod nazivom `ra-single-vm0 `na portalu za Azure. Zatim kliknite na **ponovno postavljanje lozinke** u odjeljku **podrška + otklanjanje poteškoća** . U okviru **način** padajućeg izbornika odaberite **ponovno postavljanje lozinke** , a zatim odaberite novo **korisničko ime** i **lozinku**. Kliknite gumb **Ažuriraj** održati novo korisničko ime i lozinku.

Informacije o dodatnim načinima za implementaciju ovu referenca arhitektura, potražite u članku datoteke readme u [upute, jednom i vm] [ github-folder] Github mapu.

## <a name="customize-the-deployment"></a>Prilagodba implementaciju

Da biste promijenili implementacije tako da odgovara vašim potrebama, slijedite upute u [upute, jednom i vm] [ github-folder] stranice.

## <a name="next-steps"></a>Daljnji koraci

Da bi [SLA za virtualnim strojevima] [ vm-sla] da biste primijenili, morate uvesti dva ili više instanci u skup dostupnost. Dodatne informacije potražite u članku [pokretanje više VMs na Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Jedan arhitektura Linux VM servisu Azure"

