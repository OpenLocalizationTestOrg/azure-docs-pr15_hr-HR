U ovom se članku navode skup dokazana prakse za pokretanje sustava Windows virtualnog računala (VM) na Azure, planiranju skalabilnost, dostupnost, mogućnost upravljanja i sigurnost. 

> [AZURE.NOTE] Azure sadrži dva različitoj implementaciji modela: [Voditelj resursa Azure] [ resource-manager-overview] i classic. U ovom se članku koristi Voditelj resursa koji Microsoft preporučuje za nove implementacije.

Ne preporučujemo korištenje jedne VM za radni radnih opterećenja, jer nema gore vrijeme razine ugovor o usluzi (SLA) za jedan VMs na Azure. Da biste na SLA, morate uvesti više VMs u programa [Postavljanje dostupnosti][availability-set]. Dodatne informacije potražite u članku [pokretanje više VMs Windows na Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Arhitektura dijagrama

Dodjeljivanje VM u Azure obuhvaća više premještanje dijelove od VM sam. Postoje računalnim, mreže i prostora za pohranu elemenata.

> Visio dokument koji sadrži ovaj dijagram arhitektura dostupna je za preuzimanje na [Microsoftov centar za preuzimanje][visio-download]. Ovaj dijagram uključen u "računalnim - jednostranični VM.

![[0]][0]


- **Grupa resursa.** [_Grupa resursa_] [ resource-manager-overview] je spremnik koja sadrži povezane resurse. Stvorite grupu resursa držite resurse za ovaj VM.

- **VM**. Možete Dodjela VM iz popisa objavljene slike ili virtualne na tvrdom disku (VHD) datoteke koju prenosite u spremište blobova platforme Azure.

- **Disk je OS.** OS disk je VHD pohranjene u [Azure prostora za pohranu][azure-storage]. To znači da nastavi čak i ako se glavno računalo funkcionira.

- **Privremeni disk.** Na VM stvara se pomoću privremene na disku (u `D:` pogon u sustavu Windows). Disk je pohranjena na fizički pogon na glavno računalo. Nije _ne_ sprema Azure prostora za pohranu i može sam otkloniti tijekom ponovna i druge događaje VM životni ciklus. Koristite ovaj disk samo za privremene podatke, kao što su stranice ili Zamjena datoteke.

- **Diskova podataka.** [Podatkovni disk] [ data-disk] je stalni VHD koristi za podataka aplikacije. Diskova podataka spremaju se u Azure prostor za pohranu, baš kao i OS disk.

- **Virtualne mreže (VNet) i podmreže.** Svaki VM u Azure je uveden u VNet koji dalje se dijeli podmreže.

- **Javna IP adresa.** Javnu IP adresu nužan za komunikaciju na VM&mdash;, primjerice putem udaljene radne površine (RDP).

- **Mrežnog sučelja (NIC)**. NIC-a omogućuje VM možete komunicirati s virtualne mreže.

- **Mrežni sigurnosne grupe (NSG)**. [NSG] [ nsg] koristi se za mrežni promet za podmreži Dopusti/odbijanje. Na NSG možete povezati s pojedinačne NIC ili podmreži. Ako se povežete s podmreži, pravila NSG će se primijeniti na sve VMs u tom podmreže.
 
- **Dijagnostika.** Zapisivanje dijagnostičkih podataka je presudne za Upravljanje projektom i otklanjanje poteškoća s VM.

## <a name="recommendations"></a>Preporuke

Azure nudi mnogo različitih resurse i vrste resursa, tako da ovu referenca arhitekture može dodjeli brojne načine. Ne možemo je navesti predložak Voditelj resursa Azure da biste instalirali arhitektura reference koja slijedi te preporuke. Ako se odlučite za stvaranje vlastite arhitektura referenca slijedite ove preporuke u osim ako imate određene zahtjeva koji preporuku ne stane u njega.

### <a name="vm-recommendations"></a>Preporuke VM

Preporučujemo DS - a Oznaka – nizu jer te veličine strojno podržavaju [Pohranu Premium][premium-storage]. Odaberite jednu od tih računala veličine osim ako imate specijalizirane radno opterećenje kao što su računalstvo visokih performansi. Detalje potražite u članku [virtualnog računala veličine][virtual-machine-sizes]. Prilikom pomicanja postojeće radno opterećenje za Azure, započnite s veličina VM koji se nalazi najbliže poslužiteljima na lokaciji. Zatim mjera performanse stvarni radno opterećenje vezana uz procesora i memorije na disku unos izlaznih operacija sekundi (IOPS), a ako je potrebno prilagoditi veličinu. Osim toga, ako trebate više NIC-ovi, imajte na umu ograničenja NIC za svaki veličinu.  

Prilikom dodjele resursa u VM i drugi resursi, morate navesti mjesto. Općenito govoreći, odaberite mjesto na najbliži internim korisnicima i kupcima. Međutim, nije svih veličina VM dostupni su sva mjesta. Detalje potražite u članku [Servisi po regijama][services-by-region]. Da biste dobili popis veličina VM na određenom mjestu, pokrenite sljedeću naredbu Azure sučelje naredbenog retka (EŽA):

```
    azure vm sizes --location <location>
```

Informacije o odabiru objavljene VM slike potražite u članku [Kretanje i slike odaberite Azure virtualnog računala][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Na disku i pohranu preporuke

Za najbolje performanse diska/i, preporučujemo da [Premium prostora za pohranu][premium-storage], koji pohranjuje podatke na pogonima pune stanje (SSDs). Trošak temelji se na veličinu dodijeljenu diska. IOPS i propusnost i ovisi o veličini diska, pa prilikom dodjele resursa na disku, imajte na umu sva tri čimbenika (kapacitetu, IOPS i propusnost). 

Jednog računa za pohranu podržavaju VMs 1 do 20.

Dodajte jedan ili više diskova. Prilikom stvaranja novog VHD je Neoblikovani. Prijavite se u VM da biste oblikovali disk.

Ako imate velik broj diskova podataka, imajte na umu od ukupne/i ograničenja prostora za pohranu računa. Dodatne informacije potražite u članku [Ograničenja na disku virtualnog računala][vm-disk-limits].

Ostvarili najbolje performanse, stvorite račun zasebnom prostora za pohranu na čuvanje dijagnostičke zapisnike. Standardni račun lokalno suvišnih prostora za pohranu (LRS) je dovoljni za dijagnostičke zapisnike.

Kada je to moguće, instalirajte aplikacije na disku podataka, ne na disku OS. Međutim, neke naslijeđene aplikacije možda morati instalirati komponente na pogon C:. U tom slučaju možete [promijeniti veličinu na disku OS] [ resize-os-disk] pomoću komponente PowerShell.

### <a name="network-recommendations"></a>Preporuke o mreže

Na javnu IP adresu može biti dinamičke ili statičke. Zadano je dinamički.

- Rezerviranje [statičke IP adrese] [ static-ip] ako vam je potrebna fiksnim IP adresu koja se ne mogu se promijeniti &mdash; na primjer, ako je potrebno da biste stvorili A zapis u DNS ili potrebna na IP adresu da biste se whitelisted.

- Možete stvoriti i na potpuno kvalificirani naziv domene (FQDN) traže IP adresu. Zatim možete registrirati [CNAME zapis] [ cname-record] u DNS koja upućuje na FQDN. Dodatne informacije potražite u članku [stvaranje potpuno kvalificirani naziv domene na portalu za Azure][fqdn].

Sve NSGs sadrže skupu [Zadana pravila][nsg-default-rules], uključujući pravilo koje blokira sve ulaznog internetski promet. Zadana pravila nije moguće izbrisati, ali ih možete nadjačati drugih pravila. Da biste omogućili internetski promet, stvorite pravila koja omogućuju dolazni promet na određene priključke &mdash; , na primjer, priključak 80 za HTTP.  

Da biste omogućili RDP, dodajte pravilo NSG koje dopušta unutarnje promet TCP priključka 3389.

## <a name="scalability-considerations"></a>Razmatranja skalabilnost

Na VM možete skaliranje prema gore ili prema dolje tako da [promijenite veličinu VM][vm-resize]. 

Da biste skalirali vodoravno, staviti dvije ili više VMs u raspoloživost postavljanje iza raspoređivača opterećenja. Detalje potražite u članku [pokretanje više VMs Windows na Azure][multi-vm].

## <a name="availability-considerations"></a>Razmatranja dostupnosti

Kao što je naznačeno iznad, postoji bez SLA za jednu VM. Da biste na SLA, morate uvesti više VMs u skupu dostupnost.

Vaš VM može utjecati [Planirano održavanje] [ planned-maintenance] ili [neplanirano održavanje][manage-vm-availability]. Možete koristiti [VM ponovno zapisnika] [ reboot-logs] da biste utvrdili je li planiranog održavanja posljedica VM ponovno pokrenite računalo.

VHDs se sigurnosno po [Prostora za pohranu Azure][azure-storage], koji je replicirati rok trajanja i dostupnost.

Da biste zaštitili od gubitka podataka slučajne tijekom uobičajenih postupaka (na primjer, zbog pogreške korisnika), trebali biste implementirati točke u vrijeme sigurnosne kopije pomoću [snimaka blob] [ blob-snapshot] ili neki drugi alat.

## <a name="manageability-considerations"></a>Pitanja vezana uz mogućnost upravljanja

**Grupa resursa.** Stavite čvrsto povezano resursi koji se zajednički koristiti iste životnog ciklusa u istoj [grupi resursa][resource-manager-overview]. Grupa resursa omogućuju implementaciju i resursima grupno i praćenje zajednički troškove prema grupi resursa za naplatu. Možete i izbrisati resurse kao skup, što je vrlo koristan i za testiranje implementacije. Dodijelite smisleni nazivi resursa. Koji olakšava pronalaženje određenog resursa i razumijevanje njegova uloga. Pogledajte [preporučene konvencije imenovanja za Azure resurse][naming conventions].

**Dijagnostika VM.** Omogući nadzor i Dijagnostika, uključujući metriku osnovni stanja, zapisnika infrastrukture za dijagnostiku i [Pokretanje dijagnostike][boot-diagnostics]. Pokretanje dijagnostike omogućuju Dijagnosticiranje pogreške prilikom pokretanja Ako vaš VM dobiti u koje nisu izraditi stanje. Dodatne informacije potražite u članku [Omogućivanje nadzor i dijagnostici][enable-monitoring]. Korištenje [Prikupljanje zapisnika Azure] [ log-collector] proširenje za prikupljanje zapisnika platforme Azure i prenesite ih Azure prostora za pohranu.   

Sljedeća naredba EŽA omogućuje dijagnostiku:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Zaustavljanje na VM.** Azure čini razliku između "Zaustavljanja" i "Deallocated" stanju. Vam se naplatiti nakon što VM status je "prestao". Vam se naplatiti kada se VM deallocated.

Pomoću sljedeće naredbe EŽA deallocate na VM:

```
    azure vm deallocate <resource-group> <vm-name>
```

Gumb **Zaustavi** na portalu za Azure deallocates i na VM. Međutim, nakon isključivanja kroz OS-a dok prijavljeni, na VM je zaustavljena, ali _ne_ deallocated, tako da vam i dalje naplatit će vam.

**Brisanje na VM.** Ako ste izbrisali s VM, u VHDs ne brišu se. To znači da se VM možete izbrisati sigurno bez gubitka podataka. Međutim, koje će i dalje se naplatiti prostora za pohranu. Da biste izbrisali na VHD, izbrisati datoteku iz [spremišta blobova][blob-storage].

Da biste onemogućili nenamjerno brisanje, upotrijebite [resursa Zaključaj] [ resource-lock] da biste zaključali resursa za cijelu grupu ili zaključavanje pojedinačnih resursa, kao što su u VM. 

## <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Korištenje [Centra za sigurnost Azure] [ security-center] da biste dobili središnje prikaz stanja sigurnosti Azure resurse. Centar za sigurnost nadzire potencijalne sigurnosnim problemima kao što su sustav ažurira, antimalware, i omogućuje potpun slika stanju sigurnost implementaciju sustava. 

- Centar za sigurnost je konfiguriran po Azure pretplate. Omogućivanje sigurnost prikupljanje podataka kao što je opisano u [Centru za sigurnost koristi].

- Kada je omogućen prikupljanje podataka, centar za sigurnost automatski pregledava sve VMs stvorena u odjeljku tu pretplatu.

**Upravljanje zakrpu.** Ako je omogućeno, centar za sigurnost provjerava je li sigurnost i kritičnih ažuriranja nedostaju. Koristite [postavke pravilnika grupe] [ group-policy] na VM da biste omogućili ažuriranja automatsko sustava.

**Antimalware.** Ako je omogućeno, centar za sigurnost provjerava je li instaliran softver antimalware. Centar za sigurnost možete koristiti i da biste instalirali softver antimalware unutar Azure portal.

Korištenje [Kontrola pristupa na temelju uloga] [ rbac] (RBAC) za kontrolu pristupa Azure resursa koji implementacije. RBAC možete dodijeliti uloge autorizacija članovima tima DevOps. Ako, na primjer, ulozi čitač možete prikaz Azure resursa, ali ne stvaranje, upravljanje ili ih izbrisati. Neke uloge su specifične za određeni Azure resurs vrste. Na primjer, ulogu suradnika virtualnog računala možete ponovno pokrenite ili deallocate na VM, ponovno postavljanje lozinke administratora, stvorite novi VM i tako dalje. Druge [ugrađene RBAC uloge] [ rbac-roles] koji bi mogli biti korisni za ovu referenca arhitektura obuhvaćaju [DevTest Labs korisnika] [ rbac-devtest] i [Mreža suradnika][rbac-network]. Korisnik može biti dodijeljena uloga za više, a možete stvoriti prilagođene uloge za još više preciznije dozvole.

> [AZURE.NOTE] RBAC ograničiti postupaka koji se prijavili u VM korisnik može izvoditi. Dozvole ovise o vrsti računa gosta OS.   

Da biste ponovno postavili lozinku za lokalni administrator, pokrenite na `vm reset-access` naredba Azure EŽA.

```
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Korištenje [zapisnika nadzora] [ audit-logs] da biste vidjeli dodjeljivanje akcije i druge događaje VM.

Razmislite o [Šifriranje Azure] [ disk-encryption] ako vam je potrebna za šifriranje diskova OS i podatke. 

## <a name="solution-deployment"></a>Uvođenje rješenja

[Uvođenje] [ github-folder] za referencu dostupna arhitektura koji pokazuje najbolje prakse. Arhitektura ovu referenca sadrži virtualne mreže (VNet), mreže sigurnosne grupe (NSG) i jedan virtualnog računala (VM).

Implementacija arhitektura ovu referenca na nekoliko načina. Najjednostavniji način tako da slijedite ove korake: 

1. Desnom tipkom miša kliknite donji gumb i odaberite neki "Otvori vezu na novoj kartici" ili "Otvori vezu u novom prozoru".  
[![Implementacija Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Kada na portalu za Azure otvorio vezu, morate unijeti vrijednosti za neke postavke: 
    - Naziv **grupe resursa** već definiran u datoteci parametar, pa odaberite **Stvori novo** i unesite `ra-single-vm-rg` u tekstni okvir.
    - **Mjesto** padajućem okviru odaberite regiju.
    - Uređivanje **Uri korijenski predložak** ili tekstne okvire **Parametar korijenski Uri** .
    - Odaberite **Vrstu Os** na padajućem okviru **sustava windows**.
    - Pregledajte uvjete i odredbe, a zatim potvrdite okvir **se slažete uvjete i odredbe naveden iznad** .
    - Kliknite gumb **za kupnju** .

3. Pričekajte implementaciju da biste dovršili.

4. Datoteke za parametar uključuju programiranih administrator korisničko ime i lozinku, a preporučljivo je znači da odmah promijeniti oba. Kliknite na VM pod nazivom `ra-single-vm0 `na portalu za Azure. Zatim na **ponovno postavljanje lozinke** u plohu **podršku + otklanjanje poteškoća** . U okviru **način** padajućeg izbornika odaberite **ponovno postavljanje lozinke** , a zatim odaberite novo **korisničko ime** i **lozinku**. Kliknite gumb **Ažuriraj** održati novo korisničko ime i lozinku.

Informacije o dodatnim načinima za implementaciju ovu referenca arhitektura, potražite u članku datoteke readme u [upute, jednom i vm][github-folder]] Github mapu. 

## <a name="customize-the-deployment"></a>Prilagodba uvođenje

Ako morate promijeniti implementacije tako da odgovara vašim potrebama, slijedite upute u [readme][github-folder]. 

## <a name="next-steps"></a>Daljnji koraci

Da bi [SLA za virtualnim strojevima] [ vm-sla] da biste primijenili, morate uvesti dva ili više instanci u skupu dostupnost. Dodatne informacije potražite u članku [pokretanje više VMs na Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Korištenje centra za sigurnost]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Jedan Windows VM arhitektura servisu Azure"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
