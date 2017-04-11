<properties
   pageTitle="Azure Voditelj resursa podrške za opterećenja | Microsoft Azure "
   description="Pomoću ljuske powershell za opterećenja s Azure Voditelj resursa. Korištenje predložaka za opterećenja"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Podrška za Azure resursima pomoću Azure opterećenja

Azure Voditelj resursa je Preferirani management framework za servise u Azure. Azure opterećenja može upravljati utemeljen na Voditelj resursa Azure API-ji i alati.

## <a name="concepts"></a>Koncepti

S Voditelj resursa Azure opterećenja sadrži u sljedećim resursima podređeni:

- Pristupne IP konfiguracija – raspoređivača opterećenja mogu sadržavati jedan ili više sučelje IP adrese, u suprotnom naziva na virtualne IP-ovi (VIPs). Ove IP adrese služe kao ingress za promet.

- Skupna pozadinsku adresa – to su IP adresu povezanu s virtualnog računala mreže sučelja kartica (NIC-a) koji će se raspodijeliti učitavanja.

- Učitavanje ujednačavanje pravila – svojstvu pravilo mape IP navedeni sučelje i priključak kombinacija skup pozadinskih IP adresa i kombinacija priključak. Jedan opterećenja može imati više pravila za ujednačavanje opterećenja. Svako pravilo kombinacija sučelja IP i priključak i pozadinske IP i priključak pridružene VMs je.

- Probes – probes omogućuju vam da biste pratili stanje instance VM. Stanje probni ne uspije, instancu VM uzimati iz zakretanja automatski.

- Ulazna pravila NAT – NAT pravila definiranje dolazni promet slijedi kroz ispred završili IP i distribuirati IP pozadinskih.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Predlošci za brzi početak rada

Azure Voditelj resursa omogućuje dodjeljivanje aplikacije pomoću predloška za deklarativno. U jedan predložak možete implementirati većem broju servisa uz njihove ovisnosti. Koristiti isti predložak za više puta implementaciju aplikacije tijekom svakoj fazi o životnom ciklusu aplikacije.

Predlošci mogu sadržavati definicije za virtualnim strojevima, virtualne mreže, skupovima dostupnost, mreže sučelja (NIC-ovi), račune za pohranu, učitavanja Balancers, mreže sigurnosne grupe i javnu IP-ovi. S predlošcima možete stvoriti sve što vam treba složene aplikacije. Datoteka predloška mogu se provjeriti u sustavu za upravljanje sadržajem za kontrolu i suradnju.

[Dodatne informacije o predlošcima](http://go.microsoft.com/fwlink/?LinkId=544798)

[Dodatne informacije o mrežnim resursima](../virtual-network/resource-groups-networking.md)

Pomoću Azure opterećenja predložaka za brzi početak rada pronaći ćete u [spremištu GitHub](https://github.com/Azure/azure-quickstart-templates) hosting čitav niz predložaka iz zajednice koje generira.

Primjeri predložaka:

- [2 VMs u raspoređivača opterećenja i pravila za ujednačavanje opterećenja](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 VMs u VNET s web-mjesto Interna opterećenja i u okvir za opterećenja pravila](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 VMs u raspoređivača opterećenja i konfiguriranje pravila NAT na na LB](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Postavljanje Azure opterećenja PowerShell ili EŽA

Početak rada s Voditelj resursa Azure cmdleti za alate naredbenog retka i REST API-ji

- [Cmdleti za povezivanje s mrežom na Azure](https://msdn.microsoft.com/library/azure/mt163510.aspx) se može koristiti za stvaranje raspoređivača opterećenja.
- [Kako stvoriti raspoređivača opterećenja pomoću upravitelja resursa za Azure](load-balancer-get-started-ilb-arm-ps.md)
- [Azure EŽA pomoću upravljanja Azure resursa](../xplat-cli-azure-resource-manager.md)
- [Učitavanje opterećenja REST API-ji](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Daljnji koraci

Možete i [Početak rada prilikom stvaranja internetsku nasuprotne opterećenja](load-balancer-get-started-internet-arm-ps.md) i konfiguriranje koju vrstu [način distribucije](load-balancer-distribution-mode.md) za određene Učitaj ponašanje opterećenja mrežni promet.

Informirajte se o upravljanju [neaktivnosti TCP vremenskog ograničenja postavke raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md). To je važno kada aplikacija treba da biste zadržali veze aktivnosti za poslužitelje iza raspoređivača opterećenja.
