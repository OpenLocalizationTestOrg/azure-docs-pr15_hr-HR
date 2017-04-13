<properties
    pageTitle="Konfiguracija prostora za pohranu za SQL Server VMs | Microsoft Azure"
    description="U ovoj se temi opisuje kako Azure konfigurira prostor za pohranu za SQL Server VMs prilikom dodjele resursa (model implementacije Voditelj resursa). Također objašnjava kako konfigurirati za pohranu za svoje postojeće VMs SQL Server."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>Konfiguracija prostora za pohranu za SQL Server VMs

Kada konfigurirate sliku virtualnog računala sustava SQL Server u Azure, na portalu pomaže da biste automatizirali konfiguraciju prostora za pohranu. To obuhvaća priložite prostora za pohranu na VM, taj prostor za pohranu Povećanje pristupačnosti na SQL Server i konfiguriranju ga da biste optimizirali za preduvjeti za određeni performansi.

U ovoj se temi objašnjava kako Azure konfigurira prostor za pohranu za sustava SQL Server VMs prilikom dodjele resursa i za postojeće VMs. Tu konfiguraciju temelji se na [performanse najbolje prakse](virtual-machines-windows-sql-performance.md) za Azure VMs izvodi SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model.

## <a name="prerequisites"></a>Preduvjeti
Da biste koristili konfiguracijske postavke automatskog prostora za pohranu, virtualnog računala Traži sljedeće značajke:

- Resursi [sustava SQL Server galerije](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing).
- Koristi [model implementacije Voditelj resursa](../resource-manager-deployment-model.md).
- Služi [za pohranu Premium](../storage/storage-premium-storage.md).

## <a name="new-vms"></a>Novi VMs
U sljedećim odjeljcima opisuju kako konfigurirati za pohranu za nove virtualnim računalima sustava SQL Server.

### <a name="azure-portal"></a>Portal za Azure
Prilikom dodjele resursa za VM Azure pomoću galerije za SQL Server, možete je automatski konfigurirao prostora za pohranu za svoje nove VM. Navodite veličina prostora za pohranu, ograničenja performanse i radno opterećenje vrsta. Sljedeće snimka zaslona prikazuje plohu konfiguracije prostora za pohranu koji koristi tijekom SQL VM dodjele resursa.

![Konfiguriranje VM SQL Server za pohranu prilikom dodjele resursa](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Na temelju vaših odabira Azure izvodi sljedeće zadatke konfiguracije prostora za pohranu nakon stvaranja na VM:

- Stvara i pridružuje premium prostora za pohranu podataka diskova virtualnog računala.
- Konfigurira diskova podataka da bi mogao pristupiti na SQL Server.
- Konfigurira diskova podataka na temelju navedenim veličina i performanse (IOPS i propusnost) zahtjevima za pohranu grupe aplikacija.
- Prostor za pohranu skup pridružuje novi pogon na virtualnog računala.
- Optimizira novi pogonu ovisno o vrsti navedeni radno opterećenje (skladištenju, Transactional obrade ili Općenito).

Dodatne informacije o kako Azure konfigurira postavke za pohranu potražite u [sekciji konfiguracija prostora za pohranu](#storage-configuration). Potpuna objašnjenje kako stvoriti VM za poslužitelj SQL Azure portalu potražite u članku [Vodič za dodjelu resursa](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Predlošci za upravljanje resursa
Ako koristite sljedeće predloške Voditelj resursa, dva diskova podataka premium pridružuju se prema zadanim postavkama ne konfiguracija zajedničkog prostora za pohranu. Međutim, možete prilagoditi te predloške da biste promijenili broj od premium podataka diskova koje su priložene virtualnog računala.

- [Stvaranje VM pomoću automatskog sigurnosnog kopiranja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [Stvaranje VM pomoću automatiziranog zakrpa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [Stvaranje VM s AKV Integracija](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Postojeće VMs
Za postojeće VMs SQL Server, možete promijeniti neke postavke za pohranu na portalu za Azure. Odaberite vaše VM, idite na područje postavke, a zatim odaberite konfiguracije SQL poslužitelja. Plohu konfiguracije SQL poslužitelja prikazuje trenutnu korištenje prostora za pohranu na VM. Svi pogoni koji postoje na vašem VM prikazuju se u grafikonu. Za svaki pogon prostora za pohranu prikazuju se u četiri sekcije:

- SQL podacima
- SQL zapisnika
- Ostalo (-SQL spremište)
- Dostupna

![Konfiguriranje prostora za pohranu za postojeće SQL Server VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Da biste konfigurirali prostor za pohranu da biste dodali novi pogon ili proširivanje postojeći pogon, kliknite vezu za uređivanje iznad grafikona.

Konfiguracija mogućnosti koje vidite razlikuje se ovisno o tome koju ste koristili ovu značajku prije. Kada koristite prvi put, možete odrediti preduvjeti za pohranu za novi pogon. Ako ste prethodno koristili tu značajku da biste stvorili pogonu, možete odabrati da biste proširili taj pogon prostora za pohranu.

### <a name="use-for-the-first-time"></a>Koristite prvi put
Ako je to prvi put za korištenje ove značajke, možete odrediti veličina i performanse ograničenja prostora za pohranu za novi pogon. Slično što želite vidjeti na dodjeljivanje vrijeme je ovog problema. Glavna razlika je da nije dopuštena da biste naveli vrstu radno opterećenje. To ograničenje sprječava ometa bilo koje postojeće konfiguracije SQL Server na virtualnog računala.

![Konfiguriranje klizača prostora za pohranu za SQL Server](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure stvara novi pogon na osnovu specifikacija. U ovom scenariju Azure izvršava sljedeće zadatke konfiguracije prostora za pohranu:

- Stvara i pridružuje premium prostora za pohranu podataka diskova virtualnog računala.
- Konfigurira diskova podataka da bi mogao pristupiti na SQL Server.
- Konfigurira diskova podataka u grupe aplikacija za pohranu na navedeni veličina i performanse (IOPS i propusnost) potrebama.
- Prostor za pohranu skup pridružuje novi pogon na virtualnog računala.

Dodatne informacije o kako Azure konfigurira postavke za pohranu potražite u [sekciji konfiguracija prostora za pohranu](#storage-configuration).

### <a name="add-a-new-drive"></a>Dodajte novi pogon
Ako ste već konfigurirali prostor za pohranu na sustava SQL Server VM, proširivanje prostora za pohranu pojavljuju se dva nove mogućnosti. Prva mogućnost je da biste dodali novi pogon koji možete povećati razinu performansi sustava VM.

![Dodajte novi pogon SQL VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Međutim, kad dodate pogon, morate izvršiti neke vrlo ručna konfiguracija da biste postigli Povećava performanse.

### <a name="extend-the-drive"></a>Proširivanje pogon
Na drugu mogućnost za proširivanje prostora za pohranu je da biste proširili postojeći pogon. Ta mogućnost povećava dostupan prostor za pohranu za pogon, ali poboljšati performanse. Nakon stvaranja grupe aplikacija za pohranu s grupe za pohranu, ne možete mijenjati broj stupaca. Broj stupaca određuje broj paralelno pisanja koje možete Prugasta preko diskova podataka. Dakle, sve diskova dodane podataka ne možete poboljšati performanse. Možete samo pružaju dodatnog prostora za pohranu podataka koje se zapisuju. To ograničenje i znači da je kada proširivanje pogon, broj stupaca određuje najmanji broj diskova podataka koje možete dodati. Pa ako stvorite grupe aplikacija za pohranu s diskova četiri podataka, broj stupaca ujedno četiri. Bilo kojem trenutku proširivanje prostor za pohranu, morate dodati barem četiri podataka diskova.

![Proširivanje na pogon za SQL VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Konfiguriranje prostora za pohranu
U ovom se odjeljku objašnjava promjene konfiguracije prostora za pohranu koji Azure automatski izvodi tijekom SQL VM dodjele resursa ili konfiguracije na portalu za Azure.

- Ako ste odabrali manje od dva TBs prostora za pohranu za vaše VM, Azure nije moguće stvoriti grupe aplikacija za pohranu.
- Ako ste odabrali najmanje dva TBs prostora za pohranu za vaše VM, Azure konfigurira grupe aplikacija za pohranu. U sljedećem odjeljku u ovoj se temi navode pojedinosti konfiguracija zajedničkog prostora za pohranu.
- Automatsko spremanje konfiguracije uvijek koristi [pohranu premium](../storage/storage-premium-storage.md) P30 podataka diskova. Zbog toga postoji 1:1 mapiranja između odabranih broj terabajta i broj podataka diskova priložiti vaše VM.

Informacije o cijenama, potražite u članku [cijene za pohranu](https://azure.microsoft.com/pricing/details/storage) stranica na kartici **Prostora za pohranu na disku** .

### <a name="creation-of-the-storage-pool"></a>Stvaranje grupe aplikacija za pohranu
Azure koristi sljedeće postavke da biste stvorili grupu za pohranu na SQL Server VMs.

| Postavka | Vrijednost |
|-----|-----|
| Veličina pruga  | 256 KB (skladištenju); 64 KB (transakcijskih) |
| Veličina na disku | 1 TB |
| Predmemoriju | Čitanje |
| Dodjela veličina | 64 KB NTFS dodijeljeni jedinične veličine |
| Pokretanje izravne datoteka | Omogućeno |
| Zaključavanje stranice u memoriji | Omogućeno |
| Oporavak | Jednostavan oporavak (bez otpornost) |
| Broj stupaca | Broj podataka diskova<sup>1</sup> |
| Mjesto TempDB | Pohranjene na diskova podataka<sup>2</sup> |

<sup>1</sup> nakon stvaranja grupe aplikacija za pohranu, ne možete mijenjati broj stupaca u prostor za pohranu.

<sup>2</sup> Ova postavka primjenjuje se samo prvi pogon stvarate pomoću značajke konfiguracije prostora za pohranu.

## <a name="workload-optimization-settings"></a>Postavke optimizacije radno opterećenje
U sljedećoj tablici opisane su dostupne mogućnosti vrste tri radno opterećenje i njihovih odgovarajućih optimizacije:

| Vrsta radno opterećenje | Opis | Optimizacije |
|-----|-----|-----|
| **Općenito** | Zadane postavke koje podržava većina radnih opterećenja | Ništa |
| **Transakcijskih obrada** | Optimizira prostora za pohranu za radnih opterećenja OLTP običnoj bazi podataka | Praćenje oznaka 1117<br/>Praćenje oznaka 1118 |
| **Podaci koji se skladištenja** | Optimizira prostora za pohranu za analitički i izvješćivanja radnih opterećenja | Praćenje oznaka 610<br/>Praćenje oznaka 1117 |

>[AZURE.NOTE] Samo možete odrediti vrstu radno opterećenje prilikom Dodjela SQL virtualnog računala tako da ga odaberete u koraku konfiguracije prostora za pohranu.

## <a name="next-steps"></a>Daljnji koraci
Druge teme vezane uz izvodi SQL Server u Azure VMs, potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
