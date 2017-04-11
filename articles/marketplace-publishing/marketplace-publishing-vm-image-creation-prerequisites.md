<properties
   pageTitle="Tehnički preduvjeti za stvaranje virtualnog računala slika za Azure Marketplace | Microsoft Azure"
   description="Razumijevanje zahtjevima za stvaranje i implementacija sliku virtualnog računala Azure Marketplace drugima da biste kupili."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Tehnički preduvjeti za stvaranje virtualnog računala slika za Azure Marketplace
Čitanje postupka temeljito prije početka i razumijevanje gdje i zašto se izvode svakog koraka. Najveću moguću, koje treba Priprema podataka o tvrtki i ostalih podataka, preuzmite potrebne alate i/ili stvorite tehničke komponente prije početka postupak stvaranja ponudu. Te stavke mora biti iz pregleda ovog članka.  

## <a name="download-needed-tools--applications"></a>Preuzmite potrebne alata i aplikacija
Trebali biste dobiti jeste li spremni prije početka postupka sljedećih stavki:

- Ovisno o operacijskom sustavu koji birate, instalirajte [Azure PowerShell cmdleti](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) ili [Linux sučelje naredbenog retka alata](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) sa stranice za [Preuzimanje Azure](https://azure.microsoft.com/downloads/) .
- Instalirajte Explorer Azure prostora za pohranu iz CodePlex.
- Preuzmite i instalirajte alat za testiranje certifikata za Azure potvrđeni:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Potreban vam je računalo sa sustavom Windows da biste pokrenuli alat za certifikata. Ako nemate Windows računalu sa sustavom dostupna, možete pokrenuti alat pomoću VM za utemeljen na sustavu Windows Azure.

## <a name="platforms-supported"></a>Platforme koje podržava
Razviti VMs Azure utemeljen na sustavu Windows ili Linux. Neke elemente postupak objavljivanja – kao što su stvaranje programa Azure kompatibilnog virtualne na tvrdom disku (VHD) – korištenje različitih alata i korake ovisno o tome koji operacijski sustav koji koristite:  

- Ako koristite Linux, pogledajte odjeljak "Stvaranje programa Azure kompatibilnog VHD (Linux sustavom)" [Vodič za objavljivanje slika virtualnog računala](marketplace-publishing-vm-image-creation.md).
- Ako koristite Windows, pogledajte odjeljak "Stvaranje programa Azure kompatibilnog VHD (utemeljen na sustavu Windows)" [Vodič za objavljivanje slika virtualnog računala](marketplace-publishing-vm-image-creation.md).

> [AZURE.NOTE] Potreban je pristup stroj utemeljen na sustavu Windows da biste:
- Pokrenite alat za provjeru valjanosti certifikata.
- Stvaranje URL za potpis VHD zajednički pristup za slanje certifikata VHD.

## <a name="develop-your-vhd"></a>Razvoj sustava VHD
Razviti Azure VHDs u oblaku ili na lokalnim poslužiteljima:

- Razvoj oblaku znači svih koraka za razvoj daljinski se izvode na VHD koji će biti koje postoji na Azure.
- Lokalni razvoj zahtijeva preuzimanje programa VHD i razvoj koristeći lokalne infrastrukture. Iako je to moguće, ne preporučujemo to. Imajte na umu da razvoj za Windows ili SQL lokalnog morate imati licencu tipki odgovarajući lokalnog. Ne možete uključiti ni nakon stvaranja na VM instalirati SQL Server. Tu ponudu mora temeljiti i na odobrene SQL sliku s portala za Azure. Ako odlučite razvijate na lokalnim, morate izvršiti nekoliko koraka drugačije nego ako su razvoj u oblaku. Relevantne informacije možete pronaći u [Stvaranje lokalnog VM sliku](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Daljnji koraci
Sad kad pregledava preduvjete i izvršiti potrebne zadatke, možete premjestiti naprijed sa stvaranjem virtualnog računala ponude za slike u obliku detaljni [Vodič za objavljivanje slika virtualnog računala](marketplace-publishing-vm-image-creation.md).

## <a name="see-also"></a>Vidi također
- [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)
- [Stvaranje virtualnog računala sa sustavom Windows Azure pretpregled portalu](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
