<properties
   pageTitle="Korištenje SAP-a na virtualnim strojevima sa sustavom Windows | Microsoft Azure"
   description="Očisti o korištenju SAP-a na Windows virtualnim strojevima (VMs) u Microsoft Azure"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Korištenje SAP-a na virtualnim računalima sustava Windows u Azure

Cloud računalstvo široku pojam koji se pokušavaju više važnost unutar industrijskih IT s malim tvrtkama najviše velikih i multinacionalnoj korporacije. Microsoft Azure je u oblak usluge tvrtke Microsoft koji nudi širok razni načini nove mogućnosti. Sada su korisnici moći hitro Dodjela resursa i njezini Dodjela aplikacije kao servise u Oblaku, tako da se ne nalaze ograničeni na tehnička ili izrade proračuna ograničenja. Umjesto morate uložiti vrijeme i proračun u infrastrukture hardver, tvrtke možete fokus na aplikaciju, poslovnih procesa i njegov pogodnosti Kupci i korisnicima.

Microsoft Azure virtualnim računalima sustava Microsoft nudi potpun infrastrukture kao platforma servisa (IaaS). Podržane su aplikacija SAP NetWeaver koji se temelji na virtualnim računalima sustava Azure (IaaS). Studije u nastavku opisuju kako planiranje i implementacija aplikacije NetWeaver SAP-a koji se temelji na virtualnim računalima sustava Windows u Azure. Mogli implementirati i aplikacije NetWeaver SAP-a koji se temelji na [virtualnim strojevima Linux](virtual-machines-linux-classic-sap-get-started.md).

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver na Azure - SLIKA

Naslov: SAP NetWeaver na Azure - Klasteriranje SAP ASCS/SCS instance pomoću poslužiteljskom klasteru prebacivanje sustava Windows Azure s SIOS DataKeeper

Sažetak: "ovaj dokument opisuje način korištenja SIOS DataKeeper da biste postavili iznimno dostupna konfiguracije ASCS SCS SAP-a na Azure. SAP štiti njihove jednu točku komponente pogreške, kao što su ASCS SCS SAP-a ili usluge za replikaciju Enqueue s konfiguracijama Windows Server prebacivanje klaster koje je potrebno zajedničke diskova. Te komponente SAP su ključne funkcionalnosti sustava SAP-a. Stoga visoku dostupnost funkcija mora staviti na mjestu da biste bili sigurni da možete te komponente nastavka rješenja pogreška poslužitelja ili VM kao dovršen s konfiguracijama Windows klaster Gola metal i Hyper-V okruženja. Nakon kolovoza 2015 Azure na samom ne može dati zajedničke diskova koji će biti potrebni za Windows temelji iznimno dostupna konfiguracije potrebne za te ključne komponente SAP-a. No uz pomoć proizvoda DataKeeper po SIOS, Windows Server prebacivanje klaster konfiguracije prema potrebi za SAP ASCS/SCS možete biti utemeljena na platforme Azure IaaS. Ovog članka u korak po korak pristup opisuje kako instalirati Windows Server prebacivanje klaster konfiguriranje zajedničke disku nudi SIOS Datakeeper u Azure. Papira će objašnjavaju pojedinosti u konfiguracijama na strani Azure, Windows i SAP, što pojednostavnjuje konfiguracije visoke dostupnosti radi optimalnog način. Papira dopuna dokumentaciju o instalaciji SAP i bilješke SAP-a koji predstavljaju primarni resursi za instalacijama i implementacijama SAP softvera na navedeni platformama.

Ažurirano: Kolovoz 2015.

[Odmah preuzmite ovaj vodič](http://go.microsoft.com/fwlink/?LinkId=613056)
