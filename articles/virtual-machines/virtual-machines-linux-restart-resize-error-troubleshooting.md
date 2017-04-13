<properties
   pageTitle="VM pokrenite ili promjena veličine problemi | Microsoft Azure"
   description="Otklanjanje poteškoća s resursima implementacije problemi vezani uz ponovno pokretanje i promjenu veličine na postojeće Linux virtualnog računala u Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Otklanjanje poteškoća s resursima implementacije problemi vezani uz ponovno pokretanje i promjenu veličine na postojeće Linux virtualnog računala u Azure

Kada pokušate pokrenuti prestao Azure virtualnog računala (VM) ili promijenite veličinu postojeće VM za Azure, naiđete na pogrešku uobičajeni je neuspio dodijeljeni. Ta se pogreška rezultate prilikom klaster ili regija nema dostupnih resursa ili ne podržava traženu veličina VM.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Prikupite nadzornog zapisnika

Da biste započeli otklanjanje poteškoća, prikupljanje zapisnika nadzora za prepoznavanje pogreške pridružene problem. Na sljedećim vezama sadrže detaljne informacije o postupak:

[Otklanjanje poteškoća s implementacijama grupu resursa pomoću portala za Azure](../resource-manager-troubleshoot-deployments-portal.md)

[Operacije nadzora s Voditelj resursa](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Pogreška pri pokretanju prestao VM

Pokušajte pokrenuti prestao VM, ali vam neuspio dodijeljeni.

### <a name="cause"></a>Uzrok

Zahtjev za pokretanje prestao VM mora biti pokušaj na izvornom klaster koji hostira servisa u oblaku. Međutim, klaster imati slobodnog prostora za ispunjavanje zahtjeva.

### <a name="resolution"></a>Rješenja

*   Zaustavljanje VMs u odjeljku Postavljanje dostupnosti, a zatim ponovo svaki VM.

  1. Kliknite **grupe resursa** > _grupu resursa_ > **Resursi** > _Postavljanje dostupnosti_ > **virtualnim strojevima** > _virtualnog računala_ > **Zaustavi**.

  2. Kada zaustavite sve VMs, odabira svakog od prestao VMs pa kliknite Pokreni.

*   Zahtjev za ponovno pokretanje ponovno kasnije.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Pogreška prilikom promjene veličine postojeće VM

Pokušajte promijeniti veličinu postojeće VM, ali vam neuspio dodijeljeni.

### <a name="cause"></a>Uzrok

Zahtjev za promjenu veličine na VM mora biti pokušaj na izvornom klaster koji hostira servisa u oblaku. Međutim, klaster ne podržava traženu veličina VM.

### <a name="resolution"></a>Razlučivost

* Ponovno zahtjev korištenjem manja veličina VM.

* Ako nije moguće promijeniti veličinu tražene VM:

  1. Zaustavite VMs u postavljanje dostupnosti.

    * Kliknite **grupe resursa** > _grupu resursa_ > **Resursi** > _Postavljanje dostupnosti_ > **virtualnim strojevima** > _virtualnog računala_ > **Zaustavi**.

  2. Nakon zaustavili sve VMs, promijeniti veličinu željene VM s odabranom mogućnošću objavu.
  3. Odaberite veličina VM i kliknite **Start**, a pokrenuti svaki prestao VMs.

## <a name="next-steps"></a>Daljnji koraci

Ako naiđete na probleme prilikom stvaranja novog VM Linux u Azure, potražite u članku [Otklanjanje poteškoća za implementaciju sa stvaranjem novog virtualnog računala Linux u Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
