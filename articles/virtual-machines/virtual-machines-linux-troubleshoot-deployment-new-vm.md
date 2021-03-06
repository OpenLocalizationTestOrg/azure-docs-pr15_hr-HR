<properties
   pageTitle="Otklanjanje poteškoća s upravitelja Resursa – Linux VM implementacije | Microsoft Azure"
   description="Otklanjanje poteškoća s resursima implementacije prilikom stvaranja novog Linux virtualnog računala u Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Otklanjanje poteškoća s resursima implementaciju sa stvaranjem novog Linux virtualnog računala u Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Prikupite nadzornog zapisnika

Da biste započeli otklanjanje poteškoća, prikupljanje zapisnika nadzora za prepoznavanje pogreške pridružene problem. Na sljedećim vezama sadrži detaljne informacije o postupku za praćenje.

[Otklanjanje poteškoća s implementacijama grupu resursa pomoću portala za Azure](../resource-manager-troubleshoot-deployments-portal.md)

[Operacije nadzora s Voditelj resursa](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Ako OS-a je Linux GRG generalizirano pa je prenijeti ili zabilježene generalizirano postavku, zatim tamo neće se sve pogreške. Isto tako, ako je OS-a oblici sadrže Linux, i je prenijeti ili zabilježene specijalizirane postavku, a zatim neće biti sve pogreške.

**Prijenos pogrešaka pri:**

**N<sup>1</sup>:** Ako je s operacijskim Sustavom Linux GRG generalizirano i prijenosa kao oblici sadrže, dobit ćete dodjele resursa Prekoračenje jer se VM se zamrzne u fazi dodjele resursa.

**N<sup>2</sup>:** Ako je OS-a oblici sadrže Linux i prijenosa kao GRG generalizirano, dobit ćete dodjele resursa pogreške nije uspjelo jer novi VM radi s izvornom naziv računala, korisničko ime i lozinku.

**Razlučivost:**

Da biste riješili obje te pogreške, prenesite izvorne VHD dostupan lokalno, s iste postavke kao za OS (GRG generalizirano/oblici sadrže). Da biste prenijeli GRG generalizirano, imajte na umu da biste pokrenuli - deprovision prvi put.

**Snimanje pogreške:**

**N<sup>3</sup>:** Ako je s operacijskim Sustavom Linux GRG generalizirano i on se hvata kao oblici sadrže, dobit ćete dodjele resursa Prekoračenje jer izvorni VM nije moguće koristiti kao što je označena kao GRG generalizirano.

**N<sup>4</sup>:** Ako je OS-a oblici sadrže Linux pa je zabilježene kao GRG generalizirano, dobit ćete dodjele resursa pogreške nije uspjelo jer novi VM radi s izvornom naziv računala, korisničko ime i lozinku. Osim toga, u izvorno stanje VM nije moguće koristiti jer je označena kao specijalizirane.

**Razlučivost:**

Da biste riješili obje te pogreške, izbrišite trenutnu sliku iz portal i [oslobodili iz trenutne VHDs](virtual-machines-linux-capture-image.md) s iste postavke kao za OS (GRG generalizirano/oblici sadrže).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Prilagođeno / Galerija / slika trgovine; dodjeli
Ta se pogreška nastaje u slučajevima kada se novi zahtjev za VM prikvačiti klaster koji ne podržava veličina VM Tražena ili nema dostupnog slobodnog prostora kako bi odgovarao zahtjev.

**Uzrokovati 1:** Klaster ne podržava traženu veličina VM.

**Razlučivost 1:**

- Ponovno zahtjev korištenjem manja veličina VM.
- Ako nije moguće promijeniti veličinu tražene VM:
  - Zaustavite VMs u postavljanje dostupnosti.
  Kliknite **grupe resursa** > *grupu resursa* > **Resursi** > *Postavljanje dostupnosti* > **virtualnim strojevima** > *virtualnog računala* > **Zaustavi**.
  - Kada zaustavite sve VMs, stvorite novi VM u do željene veličine.
  - Prvo pokretanje novog VM i odabira svakog od prestao VMs i kliknite **Start**.

**Uzrokovati 2:** Klaster nema slobodnih resursa.

**Razlučivost 2:**

- Ponovno zahtjev kasnije.
- Ako se novi VM može biti dio različite dostupnosti skupa
  - Stvaranje nove VM u različitim dostupnosti (u istom području).
  - Dodajte novi VM isti virtualne mreže.

## <a name="next-steps"></a>Daljnji koraci
Ako naiđete na probleme pri pokretanju prestao VM Linux ili promjena veličine postojeće Linux VM u Azure, potražite u članku [Upravitelj resursa za otklanjanje poteškoća s implementacije problemi vezani uz ponovno pokrenuti i promjena veličine na postojeće Linux virtualnog računala u Azure](virtual-machines-linux-restart-resize-error-troubleshooting.md).
