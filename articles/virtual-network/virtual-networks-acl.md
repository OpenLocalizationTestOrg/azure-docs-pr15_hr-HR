<properties
   pageTitle="Što je na mreži pristup kontrola popisa (ACL)?"
   description="Dodatne informacije o ACL-a"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Što je krajnje popis za kontrolu pristupa (ACL)?

Krajnje pristup kontrola popisa (ACL) je poboljšanje sigurnosti za implementaciju sustava Azure. ACL omogućuje selektivno dopustiti ili zabraniti promet za krajnju točku virtualnog računala. Tu mogućnost filtriranja paket sadrži dodatnu razinu zaštite. Možete navesti mreže ACL-a za krajnje točke samo. Ne možete navesti ACL virtualne mreže ili određene podmreže sadržani u virtualne mreže.

> [AZURE.IMPORTANT] Preporučuje se da biste koristili mrežom sigurnosnih grupa (NSGs) umjesto ACL-a kad god je moguće. Da biste saznali više o NSGs, pročitajte članak [što je sigurnosne grupe mreže?](virtual-networks-nsg.md).

ACL-a mogu se konfigurirati tako da pomoću programa PowerShell ili Portal za upravljanje. Da biste konfigurirali mreže ACL pomoću komponente PowerShell, potražite u članku [Upravljanje pristupom kontrola popise (ACL) za krajnje točke pomoću komponente PowerShell](virtual-networks-acl-powershell.md). Da biste konfigurirali mreže ACL pomoću portala za upravljanje, potražite [u](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)članku postavljanje točke za virtualnog računala.

Koristite mrežni ACL-a, možete učiniti sljedeće:

- Selektivno dopustiti ili zabraniti dolazne prometa koji se temelji na udaljenom podmreži rasponu IPv4 adresa da biste krajnju točku unosa virtualnog računala.

- Blacklist IP adrese

- Stvaranje više pravila po krajnjoj točki virtualnog računala

- Odredite do 50 ACL pravila po krajnjoj točki virtualnog računala

- Korištenje pravila redoslijed da biste bili sigurni pravilan skup pravila primjenjuju se na krajnje navedeni virtualnog računala (od najniže prema najvećem)

- Navedite ACL za određene udaljene podmreži IPv4 adresa.

## <a name="how-acls-work"></a>Kako funkcionira ACL-a

ACL je objekt koji sadrži popis pravila. Kada stvorite ACL i primijenite ga na krajnjoj točki virtualnog računala, filtriranje paketa odvija na glavno računalo čvora vaše VM. To znači da promet s udaljenog IP adrese filtriran prema čvor glavno računalo za ACL pravila podudaranja umjesto na vašem VM. To sprječava vaše VM trošite dragocjeno ciklusa procesora o filtriranju paketa.

Pri stvaranju virtualnog računala zadani ACL se smješta u mjesto da biste blokirali sve dolazne promet. Međutim, ako krajnje je stvorio za (priključak 3389), zatim zadani ACL mijenja se da biste omogućili sav dolazni promet za taj krajnju točku. Dolazni promet s bilo kojeg udaljene podmreže pa je dopušteno te krajnjoj točki i bez Vatrozid za dodjelu resursa potrebno je. Svi priključci su blokirane za dolazni promet osim ako krajnje točke stvaraju te priključke. Izlazni promet je dopušteno prema zadanim postavkama.

**Primjer zadane ACL tablice**

| **Pravilo #** | **Udaljena podmreže** | **Krajnja točka** | **Dopustite/odbijanje** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Dopustite      |

## <a name="permit-and-deny"></a>Dopustite i odbijanje

Možete selektivno dopustiti ili uskratiti mrežni promet za krajnju točku unosa virtualnog računala stvaranjem pravila koja sadrže "dopušta" ili "uskratiti". Nije važno Imajte na umu da po zadanom prilikom stvaranja krajnje sav promet je dopušteno krajnju točku. Zbog toga je važno je znati kako stvoriti pravila dopustite/odbijanje i njihovo stavljanje proper redoslijedom prvenstva ako želite zrnastog kontrolu nad mrežnog prometa koji odaberete da biste omogućili dosegne krajnju točku virtualnog računala.

Upućuje na umu:

1. **Nema ACL –** Prema zadanim postavkama pri stvaranju krajnje smo dopušta sve za krajnju točku.

1. **Dopustite-** Kada dodate jedan ili više "dopušta" raspona, po zadanom su odbijanju sve raspone. Samo pakete dopuštene IP raspona moći možete komunicirati s krajnju točku virtualnog računala.

1. **Nemoj dopustiti-** Kada dodate jedan ili više "Nemoj dopustiti" raspona, po zadanom su dopuštate svi rasponi prometa.

1. **Kombinacija dopušta i odbijanje-** Koristite kombinaciju "dopušta" i "Nemoj dopustiti" kada želite carve izvan određenog raspona IP dopuštenom ili zabranjen.

## <a name="rules-and-rule-precedence"></a>Pravila i prvenstva pravila

Mrežni ACL-a mogu se postaviti na određeni virtualnog računala krajnje točke. Na primjer, možete odrediti mrežu ACL-a za krajnje RDP stvoreno virtualnog računala koje zaključavanja dolje pristup za određene IP adrese. Sljedeća tablica prikazuje način da biste omogućili pristup javnim virtualne IP-ovi (VIPs) određenog raspona dopustiti pristup za RDP. Sve ostale Udaljena IP-ovi su zabranjen. Ne možemo slijedite pravilo redosljed *najniže ima prednost* .

### <a name="multiple-rules"></a>Više pravila

U primjeru u nastavku ako želite dopustiti pristup krajnju točku RDP samo s dva javna IPv4 adresa raspona (65.0.0.0/8 i 159.0.0.0/8), to možete postići tako da navedete dva *dopustiti* pravila. U ovom slučaju, budući da je po zadanom za virtualnog računala stvaraju RDP, želite zaključati pristup RDP priključka koji se temelji na udaljenom podmreži. U sljedećem primjeru pokazuje na način da biste omogućili pristup javnim virtualne IP-ovi (VIPs) određenog raspona dopustiti pristup za RDP. Sve ostale Udaljena IP-ovi su zabranjen. To funkcionira jer je mreža ACL-a mogu se postaviti za krajnju točku određene virtualnog računala i pristup odbijen je prema zadanim postavkama.

**Primjer – višestruka pravila**

| **Pravilo #** | **Udaljena podmreže** | **Krajnja točka** | **Dopustite/odbijanje** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Dopustite      |
| 200    | 159.0.0.0/8   | 3389     | Dopustite      |

### <a name="rule-order"></a>Redoslijed pravila

Budući da možete navesti više pravila za krajnje točke, mora biti načina za organiziranje pravila da biste utvrdili koje pravilo ima prednost. Redoslijed pravila određuje prednost. Mrežni ACL-a slijedite pravilo redosljed *najniže ima prednost* . U primjeru u nastavku krajnje točke na priključak 80 selektivno je omogućen pristup samo određenim rasponi IP adresa. Da biste konfigurirali, imamo Nemoj dopustiti pravila (pravila \# 100) za adrese 175.1.0.1/24 prostor. Drugo pravilo zatim navesti s prioritetom 200 koji omogućuje pristup svim adresama u odjeljku 175.0.0.0/8.

**Primjer – prvenstva pravila**

| **Pravilo #** | **Udaljena podmreže** | **Krajnja točka** | **Dopustite/odbijanje** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Nemoj dopustiti        |
| 200    | 175.0.0.0/8   | 80       | Dopustite      |

## <a name="network-acls-and-load-balanced-sets"></a>Mrežni ACL-a i učitavanje Balansirani skupova

Možete navesti mreže ACL-a na krajnje za postavljanje (LB postavljene) rasporediti opterećenje. Ako je ACL navedena za skup LB, ACL mreže primjenjuje se na sve virtualnim strojevima u tom skupu LB. Ako, na primjer, ako skup LB stvara se pomoću "Priključak 80", a zatim postavite LB sadrži 3 VMs, ACL mreže stvara na krajnjoj točki "Priključka 80" jedne VM automatski će se primijeniti na druge VMs.

![Mrežni ACL-a i učitavanje Balansirani skupovima](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Daljnji koraci

[Kako upravljati pristup kontrola popisa (ACL) za krajnje točke pomoću komponente PowerShell](virtual-networks-acl-powershell.md)
