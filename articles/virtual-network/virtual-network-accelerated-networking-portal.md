<properties 
   pageTitle="Ubrzana umrežavanje za virtualnog računala – Portal | Microsoft Azure"
   description="Saznajte kako konfigurirati ubrzana umrežavanje Azure virtualnog računala pomoću portala za Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Ubrzanom mreže za virtualnog računala

> [AZURE.SELECTOR]
- [Portal za Azure](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Ubrzanom umrežavanje omogućuje jedan korijenski/i virtualizacije (SR IOV) da biste virtualnog računala (VM) znatno unaprjeđenje njegov umrežavanje. Ovaj put visokih performansi zaobilazi glavno računalo s datapath smanjenje Latencija, razrješava zahtjeve i procesora za korištenje s najzahtjevnijih radnih opterećenja mreže na podržane vrste VM. U ovom se članku objašnjava kako pomoću portala za Azure konfiguriranje ubrzana umrežavanje u model implementacije Azure Voditelj resursa. Možete stvoriti i u VM s ubrzana povezivanje s mrežom pomoću komponente PowerShell Azure. Da biste saznali kako to učiniti, kliknite okvir PowerShell pri vrhu ovog članka.

Sljedeća slika prikazuje komunikaciju između dva virtualnim strojevima (VM) sa ili bez ubrzana umrežavanje:

![Usporedba](./media/virtual-network-accelerated-networking-portal/image1.png)

Bez ubrzana umrežavanje sav mrežni promet i na VM morate prolaziti glavno računalo, a parametar virtualne. Parametar virtualne nudi sve Uspostava pravila, kao što su mreže sigurnosne grupe, popise kontrole pristupa, odvajanja i ostale usluge mreže virtualiziranom za mrežni promet. Da biste saznali više, pročitajte članak [virtualizacije Hyper-V mreže i virtualne promjenu](https://technet.microsoft.com/library/jj945275.aspx) .

Ubrzana umrežavanje mrežni promet stigne na kartici mreže (NIC), a zatim proslijediti na VM. Sva pravila mrežom virtualne skretnica primjenjuje bez ubrzana umrežavanje su prenijeti i primijeniti u hardveru. Primjena pravila u hardveru omogućuje NIC-a za prosljeđivanje mrežni promet izravno u VM, zaobilaženje glavno računalo, a parametar virtualne uz zadržavanje svih pravila primjenjuju na glavnom računalu.

Prednosti ubrzana umrežavanje primjenjuju se samo na VM koji je omogućen na. Da biste postigli najbolje rezultate, idealna da biste omogućili ovu značajku na najmanje dva VMs povezan s istom VNet je. Kada komuniciram preko VNets ili povezivanje lokalnih, ta značajka ima minimalnog utjecaj na ukupna Latencija.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Prednosti

- **Donjem kašnjenje / veći pakete po drugoj (pps):** Uklanjanjem virtualne prebaci na datapath uklanja vrijeme pakete potrošiti na glavnom računalu za obradu pravila i povećava broj pakete koji može obraditi unutar na VM.
- **Smanjene razrješava zahtjeve:** Obrada virtualne promjenu ovisi o količinu pravilo koje valja zatvoriti i radno opterećenje procesora koji se način obrade. Rasteretite Uspostava pravila za hardver uklanja te raznolikosti tako da isporučuje pakete izravno u VM, uklanjanjem voditelju VM komunikacije i sve interrupts softver i parametri konteksta.
- **Smanjiti procesora:** Zaobilaženje parametar virtualne u glavnom potencijalnih klijenata za manje procesora za obradu mrežni promet.

## <a name="limitations"></a>Ograničenja

Sljedeća ograničenja postoje kada koristite ovu mogućnost:
 
- **Mreže stvaranje sučelja:** Ubrzanom mreže je moguće omogućiti samo za novo sučelje mreže.  Ne može se omogućiti na postojeće mrežnom sučelju.
- **Stvaranja VM:** Mrežno sučelje s ubrzanom mrežom omogućeno mogu samo priložiti u VM stvaranja na VM. Mrežno sučelje ne može se priložiti postojeće VM.
- **Regije:** Koje se nude Zapad središnje NAM i Zapad Europe Azure područja samo. Skup regija u budućnosti će se proširiti.
- **Podržani operacijski sustav:** Microsoft Windows Server 2012 R2 i Windows Server 2016 Tehnički pretpregled 5. Uskoro će biti dodan službe za podršku Linux i Windows Server 2012.
- **Veličina VM:** Standard_D15_v2 i Standard_DS15_v2 su jedini podržane VM instancu veličine. Dodatne informacije potražite u članku [Windows VM veličine](../virtual-machines/virtual-machines-windows-sizes.md) . Postavljanje veličine podržani VM instanci će se proširiti u budućnosti.

Promjene ove ograničenja će se objaviti na stranici [Azure virtualne umrežavanje ažuriranja](https://azure.microsoft.com/updates/accelerated-networking-in-preview) .

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Stvaranje Windows VM s ubrzanom mrežom

1. Registrirajte se za pretpregled slanjem poruke e-pošte [Ubrzana umrežavanje pretplate](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) s ID pretplate i korištenje. Dovršite preostale korake tek kada primite poruku e-pošte koja obavještava da ste prihvatili u pretpregledu.
2. Prijavite se na Portal sustava Azure na http://portal.azure.com.
3. Stvaranje na VM s povećanim korake u članku [Stvaranje Windows VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) odabir sljedeće mogućnosti:
    - Odaberite operacijski sustav na popisu u odjeljku ograničenja ovog članka.
    - Odaberite mjesto (regija) na popisu u odjeljku ograničenja ovog članka.
    - Odaberite veličinu VM navedene u odjeljku ograničenja ovog članka. Ako neku od podržanih veličine nije naveden, kliknite **Prikaži sve** u plohu **Odaberite veličinu** da biste odabrali veličinu s popisa za prošireni.
    - U plohu **Postavke** kliknite *omogućeno* za **umrežavanje Accelerated**, kao što je prikazano na sljedećoj slici:

        ![Postavke](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] Mogućnost ubrzana umrežavanje bit će vidljiv samo ako ste:
    >
    >- Prihvaćen u pretpregledu
    >- Odabrana podržani operacijski sustav, mjesto i veličine VM spomenute u odjeljku ograničenja ovog članka.

5. Nakon stvaranja na VM preuzimanje [ubrzana umrežavanje upravljački program](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), povezivanje i prijavite se na VM i pokrenite instalacijski program upravljački program unutar na VM.
6. Desnom tipkom miša pritisnite gumb Windows, a zatim kliknite **Upravitelj uređaja**. **Mellanox ConnectX 3 virtualne funkcija Ethernet prilagodnika** provjerite prikazuje li se u odjeljku mogućnosti **mrežni** kada Proširi, kao što je prikazano na sljedećoj slici:

    ![Upravitelj uređaja](./media/virtual-network-accelerated-networking-portal/image2.png)