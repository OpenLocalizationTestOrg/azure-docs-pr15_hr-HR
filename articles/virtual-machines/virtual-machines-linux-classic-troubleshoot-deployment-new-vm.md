<properties
   pageTitle="Otklanjanje poteškoća prilikom implementacije klasičan Linux VM | Microsoft Azure"
   description="Otklanjanje poteškoća s uvođenje klasičnog prilikom stvaranja novog Linux virtualnog računala u Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Otklanjanje poteškoća s klasični implementaciju sa stvaranjem novog Linux virtualnog računala u Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Prikupite nadzornog zapisnika

Da biste započeli otklanjanje poteškoća, prikupljanje zapisnika nadzora za prepoznavanje pogreške pridružene problem.

Na portalu Azure kliknite **Pregledaj** > **virtualnim strojevima** > *sustava Windows virtualnog računala* > **Postavke** > **zapisnika nadzora**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Ako OS-a je Linux GRG generalizirano pa je prenijeti ili zabilježene generalizirano postavku, zatim tamo neće se sve pogreške. Isto tako, ako je OS-a oblici sadrže Linux, i je prenijeti ili zabilježene specijalizirane postavku, a zatim neće biti sve pogreške.

**Prijenos pogrešaka pri:**

**N<sup>1</sup>:** Ako je s operacijskim Sustavom Linux GRG generalizirano i prijenosa kao oblici sadrže, dobit ćete dodjele resursa Prekoračenje jer se VM se zamrzne u fazi dodjele resursa.

**N<sup>2</sup>:** Ako je OS-a oblici sadrže Linux i prijenosa kao GRG generalizirano, dobit ćete dodjele resursa pogreške nije uspjelo jer novi VM radi s izvornom naziv računala, korisničko ime i lozinku.

**Razlučivost:**

Da biste riješili obje te pogreške, prenesite izvorne VHD dostupan lokalno, s iste postavke kao za OS (GRG generalizirano/oblici sadrže). Da biste prenijeli GRG generalizirano, imajte na umu da biste pokrenuli - deprovision prvi put. Dodatne informacije potražite [stvoriti i prenijeti virtualne tvrdi Disk koji sadrži operacijski sustav Linux](virtual-machines-linux-classic-create-upload-vhd.md) .

**Snimite pogreške:**

**N<sup>3</sup>:** Ako je s operacijskim Sustavom Linux GRG generalizirano i on se hvata kao oblici sadrže, dobit ćete dodjele resursa Prekoračenje jer izvornom VM nije moguće koristiti kao što je označena kao GRG generalizirano.

**N<sup>4</sup>:** Ako je OS-a oblici sadrže Linux pa je zabilježene kao GRG generalizirano, dobit ćete dodjele resursa pogreške nije uspjelo jer novi VM radi s izvornom naziv računala, korisničko ime i lozinku. Osim toga, u izvorno stanje VM nije moguće koristiti jer je označena kao specijalizirane.

**Razlučivost:**

Da biste otklonili obje te pogreške, izbrišite trenutnu sliku iz portal i [oslobodili iz trenutne VHDs](virtual-machines-linux-classic-capture-image.md) s iste postavke kao za OS (GRG generalizirano/oblici sadrže).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Prilagođeno / Galerija / slika trgovine; dodjeli
Ta se pogreška nastaje u slučajevima kada se novi zahtjev za VM pošalje klaster koji nema dostupnog slobodnog prostora kako bi odgovarao zahtjev, ili ne podržava veličina VM tražena. Nije moguće kombinirati različitih grupa VMs u istom servis u oblaku. Tako da ako želite stvoriti novi VM od iste veličine kao što može podržati servis u oblaku, zahtjev za računalnim neće uspjeti.

Ovisno o ograničenjima servisa u oblaku koristite da biste stvorili novi VM, može se pojaviti pogreška uzrokovana dvije situacije.

**Uzrokovati 1:** Servis u oblaku je fiksiran na određene klaster ili ga je povezana afinitet grupi i zato prikvačena na određene klaster dizajnom. Stoga novi resurs računalnim zahtjeve u tom se u istu klaster gdje se nalaze postojećih resursa pokuša afinitet grupe. Međutim, iste klaster možda ne podržava tražene veličina VM ili imati dovoljno slobodnog prostora rezultira pogreškom dodijeljeni. To vrijedi li se stvaraju nove resurse putem servis u oblaku novi ili postojeći oblaku.

**Razlučivost 1:**

- Stvorite novi servis u oblaku i povezivanje s područja ili regije sustavom virtualne mreže.
- Stvaranje nove VM u novi servis u oblaku.
  Ako nailazite na pogrešku prilikom da biste stvorili novi servis u oblaku, pokušajte ponovno kasnije ili promijenite područje za servis u oblaku.

> [AZURE.IMPORTANT] Ako pokušavate stvoriti novi VM u postojeće oblaku, ali nije moguće i imali da biste stvorili novi oblaku za svoje nove VM, možete odabrati konsolidirati sve VMs u istom servis u oblaku. Da biste to učinili, izbrišite VMs u postojeći servis u oblaku i ih oslobodili iz svoje diskova u novi servis u oblaku. Međutim, nije važno Imajte na umu da novi servis u oblaku će imati novi naziv i VIP, bit će potrebno ažuriranje tih ovisnost koji trenutno koristite ove informacije za postojeće servis u oblaku.

**Uzrokovati 2:** Servis u oblaku povezan je s virtualne mreže koja je povezana s granu afinitet tako da je fiksiran na određene klaster dizajnom. Novi resurs računalnim zahtjeve iz tog u istom klaster gdje se nalaze postojećih resursa stoga pokuša afinitet grupe. Međutim, iste klaster možda ne podržava tražene veličina VM ili imati dovoljno slobodnog prostora rezultira pogreškom dodijeljeni. To vrijedi li se stvaraju nove resurse putem servis u oblaku novi ili postojeći oblaku.

**Razlučivost 2:**

- Stvorite novi regionalnim virtualne mreže.
- Stvaranje nove VM u novi virtualne mreže.
- [Povezivanje postojećeg virtualne mreže](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) da biste novi virtualne mreže. Potražite dodatne informacije o [regionalnim virtualne mreže](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Umjesto toga možete [migrirati afinitet-grupne virtualne mrežu da biste regionalne virtualne mreže](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), a zatim stvorite novi VM.

## <a name="next-steps"></a>Daljnji koraci
Ako naiđete na probleme pri pokretanju prestao VM Linux ili promjena veličine postojeće Linux VM u Azure, potražite u članku [Otklanjanje poteškoća s uvođenje klasičnog problemi vezani uz ponovno pokretanje i promjenu veličine na postojeće Linux virtualnog računala u Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md).
