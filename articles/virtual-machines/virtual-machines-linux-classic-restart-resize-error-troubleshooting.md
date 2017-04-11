<properties
   pageTitle="VM pokrenite ili promjena veličine problemi | Microsoft Azure"
   description="Otklanjanje poteškoća klasični implementaciju s ponovnim ili promjenu veličine na postojeće Linux virtualnog računala u Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Otklanjanje poteškoća klasični implementaciju s ponovnim ili promjenu veličine na postojeće Linux virtualnog računala u Azure

> [AZURE.SELECTOR]
- [Klasični](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Voditelj resursa](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Kada pokušate pokrenuti prestao Azure virtualnog računala (VM) ili promijenite veličinu postojeće VM za Azure, naiđete na pogrešku uobičajeni je neuspio dodijeljeni. Ta se pogreška rezultate prilikom klaster ili regija nema dostupnih resursa ili ne podržava traženu veličina VM.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Prikupite nadzornog zapisnika

Da biste započeli otklanjanje poteškoća, prikupljanje zapisnika nadzora za prepoznavanje pogreške pridružene problem.

Na portalu Azure kliknite **Pregledaj** > **virtualnim strojevima** > _računalu virtualne Linux_ > **Postavke** > **zapisnika nadzora**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Pogreška pri pokretanju prestao VM

Pokušajte pokrenuti prestao VM, ali vam neuspio dodijeljeni.

### <a name="cause"></a>Uzrok

Zahtjev za pokretanje prestao VM mora biti pokušaj na izvornom klaster koji hostira servisa u oblaku. Međutim, klaster imati slobodnog prostora za ispunjavanje zahtjeva.

### <a name="resolution"></a>Razlučivost

* Stvaranje nove servise u oblaku i povezati je s ili područja ili regije sustavom virtualne mreže, ali ne granu afinitet.

* Brisanje prestao VM.

* Ponovno stvorite VM u novi servis u oblaku pomoću na diskova.

* Pokrenite ponovno stvara VM.

Ako nailazite na pogrešku prilikom da biste stvorili novi servis u oblaku, pokušajte ponovno kasnije ili promijenite područje za servis u oblaku.

> [AZURE.IMPORTANT] Novi servis u oblaku će sadržavati novi naziv i VIP, pa ćete morati promijeniti te podatke za sve ovisnosti koje koriste te informacije za postojeće servis u oblaku.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Pogreška prilikom promjene veličine postojeće VM

Pokušajte promijeniti veličinu postojeće VM, ali vam neuspio dodijeljeni.

### <a name="cause"></a>Uzrok

Zahtjev za promjenu veličine na VM mora biti pokušaj na izvornom klaster koji hostira servisa u oblaku. Međutim, klaster ne podržava traženu veličina VM.

### <a name="resolution"></a>Razlučivost

Smanjivanje veličine tražene VM pa ponovite zahtjev za promjenu veličine.

* Kliknite **Pregledaj sve** > **virtualnim strojevima (klasični)** > _virtualnog računala_ > **Postavke** > **Veličina**. Detaljne korake potražite u članku [Promjena veličine virtualnog računala](https://msdn.microsoft.com/library/dn168976.aspx).

Ako nije moguće da biste smanjili veličinu VM, slijedite ove korake:

  * Stvaranje nove servise u oblaku, zajamčiti nije povezana afinitet grupi i nije povezana ni s virtualne mreže koja je povezana s granu afinitet.

  * Stvorite novu, veća veličine VM u njoj.

Moguće je konsolidirati sve VMs u istom servis u oblaku. Ako je postojeći servis u oblaku povezana sa sustavom regija virtualne mreže, možete se povezati novi servis u oblaku za postojeće virtualne mreže.

Ako postojeće servis u oblaku nije povezana sa sustavom regija virtualne mreže, pa ćete morati izbrisati VMs u postojeći servis u oblaku i ponovno ih stvorite u novi servis u oblaku s njihovim diskova. Međutim, nije važno Imajte na umu da novi servis u oblaku će imati novi naziv i VIP, bit će potrebno ažuriranje tih ovisnost koji trenutno koristite ove informacije za postojeće servis u oblaku.

## <a name="next-steps"></a>Daljnji koraci

Ako naiđete na probleme prilikom stvaranja novog VM Linux u Azure, potražite u članku [Otklanjanje poteškoća za implementaciju sa stvaranjem novog virtualnog računala Linux u Azure](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
