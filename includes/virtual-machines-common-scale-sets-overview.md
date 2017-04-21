

Za aplikacije koje treba skaliranja računalnim resursa izgleda i u odjeljku skaliranje operacije su implicitno raspoređen preko kvara i ažuriranje domene. Uvod u VM skaliranje skupove priručniku nedavne [Azure blog objava](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/).

Pogledajte ove videozapise dodatne informacije o skupovima skaliranje VM:

 - [Označavanje Russinovich razgovara skupove skaliranje Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Promjena veličine virtualnog računala postavlja s Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Stvaranje i upravljanje VM skaliranje postavlja

Skaliranje VM skupova može se definirati i implementirati pomoću predložaka JSON i [REST API -ji](https://msdn.microsoft.com/library/mt589023.aspx) samo kao što su pojedinačne VMs Azure Voditelj resursa. Dakle, sve standardne Voditelj resursa Azure načinima može se koristiti. Dodatne informacije o predlošcima potražite u članku [Predlošci Voditelj resursa za Azure autorizaciju](../articles/resource-group-authoring-templates.md).

Skup primjer predložaka za skupove skaliranje VM pronaći ćete u Azure brzi početak rada predložaka GitHub spremište ovdje:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) – izgleda za predloške s _vmss_ u naslov.

Na stranicama Detalji za predloške tih vidjet ćete gumb koja je povezana s portala implementacije značajka. Da biste implementirali skup skaliranje VM, kliknite gumb, a zatim unesite u sve parametre koje su potrebne na portalu. Ako niste sigurni je li resurs podržava slova velika ili koriste različite verzije je sigurnije uvijek koristi vrijednosti parametara malim slovima. Također je praktičan video dissection VM skaliranje postavljanje predloška ovdje:

[Skaliranje VM postavljanje Dissection predloška](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Promjena veličine skalu VM postavljanje izgleda i u

Da biste povećali ili smanjili broj virtualnim strojevima u skupu skaliranje VM, jednostavno promijeniti svojstvo _kapaciteta_ i ponovno implementirate predložak. U ovom jednostavnosti olakšava napišite vlastiti prilagođeni skaliranja sloja ako želite definirati Prilagođeno mjerilo događaja koji se ne podržava Azure automatsko skaliranje.

Ako su redeploying predložak da biste promijenili kapacitet, možete definirati koliko manje predložak koji sadrži samo se SKU i ažurirane kapaciteta. Primjer to je prikazano ovdje: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Da biste prošli kroz korake koji Stvaranje skupa skaliranje koja je automatski neproporcionalno, potražite u članku [Automatski skaliranje strojeva u skupu na skaliranje virtualnog računala](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>Nadzor vaš skaliranje VM postavljanje

Trenutno preporučuje [Azure resursa Explorer](https://resources.azure.com) koristite da biste pogledali VM skaliranje skupove. Skupovi skaliranje VM su resursa u odjeljku Microsoft.Compute, tako da s ovog mjesta vidjet ćete ih proširenjem sljedeće veze:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Skaliranje VM postavljanje scenariji

U ovom se odjeljku navedeni neki uobičajeni scenariji skup VM mjerilo. Neke višu razinu Azure usluge (kao što su grupe, tkanina servisa, servisa Azure spremnik) će koristiti tih scenarija.

 - **RDP / SSH VM skaliranje postaviti pojave** - stvaranja skupa skaliranje odgovora VM unutar na VNET i pojedinačne VMs u su dodijeliti javnu IP adrese. To je dobro jer obično ne želite indirektni trošak i upravljanje od dodjeljivanje zasebnom IP adrese bez praćenja stanja resursima u računalnim rešetke i možete jednostavno povezati s ove VMs iz drugih resursa u svoje VNET uključujući one koji imaju javnu IP adrese kao što su balancers učitavanja ili samostalnu virtualnim računalima.

 - **Povezivanje s VMs pomoću pravila NAT** – možete stvoriti javne IP adrese, dodijeliti raspoređivača opterećenja i definirati pravila za dolazni NAT mapiranje priključak na IP adresu s priključkom na VM u skupu skaliranje VM. Npr.

    Javnu IP -> priključak 50000 -> vmss\_0 -> priključak 22 javnu IP - > priključak 50001 -> vmss\_1 -> priključak 22 javnu IP - > priključak 50002 -> vmss\_2 -> priključak 22

    Evo primjera Stvaranje skupa skaliranje VM koji koristi NAT pravila da biste omogućili SSH vezu svaki VM u skalu skupa pomoću jedne javnu IP: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Evo primjera isto s RDP i Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Povezivanje s VMs pomoću "jumpbox"** - ako stvarate skalu VM postavite i samostalni VM u istom VNET, samostalne VM i skup skaliranje VM VMs možete se povezati s međusobno pomoću njihovih interne IP adrese kako je definirano parametrom VNET/podmreže. Ako stvorite javnu IP adresa i njezino dodavanje samostalne VM možete RDP ili SSH kao samostalne VM i povezati se s tog računala na vašem instance skup VM mjerilo. Možda ćete primijetiti sada je li jednostavne postavljanje skaliranje VM čini dodatno zaštititi od jednostavne samostalne VM s javnu IP adresa njegova zadanoj konfiguraciji.

    Na primjer takvog, ovaj predložak stvara jednostavno Mesos klaster koji se sastoji od samostalnu VM matrica koji upravlja VM skaliranje postavljanje temelji Klaster od VMs: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Kružno opterećenja VM skaliranje postaviti pojave** – ako želite isporuka rad klasteru VMs pomoću "-kružnog" pristup, Azure opterećenja možete konfigurirati s ujednačavanje opterećenja pravila sukladno tome. Možete definirati i probes da biste aplikaciji provjerite je li pokrenut po pinging priključke s navedenom putu protokol, interval i zahtjev.

    Slijedi primjer čime ćete stvoriti skup skaliranje VM VMs pokrenuti IIS web-poslužitelj i koristi raspoređivača opterećenja za saldo opterećenje koje svaki VM Prima. Također koristi HTTP protokola za određeni URL na svakom VM pomoću naredbe ping: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) – vrsta resursa Microsoft.Network/loadBalancers i networkProfile i extensionProfile u na virtualMachineScaleSet.

 - **Deploying skalu VM Postavi kao klasteru klaster upravitelja PaaS** - VM skaliranje ulogu suradnika dalje generacije ponekad opisan skupovima. Je valjani opis, ali je pokrenut će se i rizika pregledniji skaliranje skup značajki značajke PaaS v1 tempiranja uloge. U smislu VM skaliranje skupove pružaju na true "tempiranja uloga" "ili" resurs radnih iz tog pružaju generalizirano računalnim resurs koji je platformu/runtime neovisno, prilagoditi i integrira u IaaS resursima Azure.

    PaaS v1 tempiranja uloge, dok je ograničeno pomoću platforme/runtime podršku (samo Windows platformu slike) obuhvaća i servise kao što su zamjena VIP, konfigurirati postavke nadogradnje runtime/uvođenja određenog postavki aplikacije koje ne _još_ dostupne u VM skaliranja skupovima i će isporuku drugih višu razinu PaaS servisa kao što je tkanina servisa. Uz to umu možete pogledati VM skaliranje skupova kao infrastruktura za koji podržava PaaS. Odnosno PaaS rješenja tkanina servisa kao što su ili upravitelji klaster kao Mesos možete izraditi pri vrhu VM skaliranje skupova kao skalabilni računalnim sloja.

    Evo primjera Mesos klaster koji implementira VM skaliranje skup u istoj VNET kao samostalan proizvod VM. Samostalne VM je Mesos matrice i skaliranje skup VM predstavlja skup podređeni čvorovi: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Budućim verzijama [Servisa Azure spremnik](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) će uvesti više kompleksni/hardened verzijama scenarij koji se temelji na VM skaliranje skupovima.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>VM skaliranje skup performanse i skaliranje upute

- Tijekom javno pretpregleda nije moguće stvoriti više od 500 VMs u više skupova skaliranje VM odjednom.
- Plan za više od 40 VMs po računu za pohranu.
- Proširenje prvih slova naziva računa za pohranu najveću moguću.  Primjer VMSS predložaka u [predlošcima Azure brzi početak rada](https://github.com/Azure/azure-quickstart-templates/) navode Primjeri kako to učiniti.
- Ako koristite prilagođeni VMs, plan za više od 40 VMs po skupu VM skaliranje na računu za jednu prostora za pohranu.  Trebat će vam slika unaprijed kopirali u račun za pohranu prije no što započnete VM skaliranje skup implementacije. Najčešća pitanja vezana uz dodatne informacije potražite u članku.
- Plan za više od 2048 VMs po VNET.  To ograničenje će se u budućnosti povećati.
- Broj VMs možete stvoriti ograničen kvotu core računalnim u bilo kojem regiji. Možda ćete se morati obratiti službi za podršku klijentima da biste povećali ograničenje kvote računalnim povećati čak i ako imate visoke ograničenje od jezgri za servise u oblaku ili IaaS v1 danas. Da biste kvotu za upite možete pokrenite sljedeću naredbu Azure EŽA: _popis korištenje azure vm_i sljedeću naredbu komponente PowerShell: _Get-AzureRmVMUsage_ (Ako je pomoću verzije programa PowerShell ispod 1.0 koristite _Get-AzureVMUsage_).

## <a name="vm-scale-set-frequently-asked-questions"></a>Skaliranje VM postavljanje često postavljana pitanja

**PITANJA.** Koliko VMs se nalaziti u skupu skaliranje VM?

**ODGOVORI.** 100 ako koristite platformu slike koje možete raspodijeliti više prostora za pohranu računa. Ako koristite prilagođeni slike, do 40, jer prilagođene slike ograničeni su na jednom prostora za pohranu računa tijekom pretpregleda.

**Pitanja** postoje koje ograničenja resursa za skupove skaliranje VM?

**ODGOVORI.** Ograničeni ste stvaranje više od 500 VMs u više skupova skaliranje po regijama tijekom razdoblja za pretpregled. Postojeći [ograničenja servisa Azure pretplate /](../articles/azure-subscription-service-limits.md) primijeniti.

**PITANJA.** Podataka diskova podržane su unutar VM skaliranje skupove?

**ODGOVORI.** Nije u prvom izdanju. Mogućnosti za spremanje podataka su:

- Azure datoteke (SMB zajednički pogona)

- OS pogon

- Temp pogon (lokalno, sigurnosnu po Azure prostora za pohranu)

- Vanjski podaci servisa (npr. udaljene DB Azure tablice, Azure blob-ova)

**PITANJA.** Koje Azure područja podrške za skupove skaliranje VM?

**ODGOVORI.** Sve regije koji podržava Voditelj resursa Azure podržava VM skaliranje skupove.

**PITANJA.** Kako stvoriti skalu VM skupove pomoću prilagođenu sliku?

**ODGOVORI.** Svojstvo vhdContainers, ostavite prazna, primjerice:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**PITANJA.** Ako smanjiti Moje skaliranje VM postaviti kapacitet od 20 15, uklonit će se VMs?

**ODGOVORI.** Virtualnim strojevima uklonjeni su iz skale postavite ravnomjerno preko nadogradnje domene i domene kvara maksimizirali dostupnost.

**PITANJA.** Kako otprilike je ako zatim povećati kapacitet 15 18?

**ODGOVORI.** Ako povećate 18, VMs s indeksom 15, 16, stvorit će se 17. U oba slučaja u VMs su raspoređen na FDs i UDs.

**PITANJA.** Kada koristite više nastavaka u skupu skaliranje VM, možete li se nametnuti programa izvođenja slijed?

**ODGOVORI.** Ne izravno, ali za proširenje customScript skriptu može proći drugo proširenje da biste dovršili (na primjer prateći proširenje prijavu – pogledajte [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**PITANJA.** VM skaliranje skupovima funkcioniraju s Azure dostupnost skupovima?

**ODGOVORI.** Da. Promjena veličine skup VM je implicitno raspoloživost postavite s 3 FDs i 5 UDs. Ne morate ništa u odjeljku virtualMachineProfile konfiguriranje. Ubuduće izdanja, ali VM skaliranje skupovima vjerojatno protežu na više klijenata, ali sada postaviti skalu je jedan dostupnosti skupa.
