
<properties
   pageTitle="Početak rada prilikom stvaranja internetsku nasuprotne opterećenja u modelu uvođenje klasičnog pomoću portala za Azure klasični | Microsoft Azure"
   description="Saznajte kako stvoriti internetsku nasuprotne opterećenja u modelu uvođenje klasičnog pomoću portala za Azure klasični"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Početak rada prilikom stvaranja internetsku nasuprotne opterećenja (klasični) na portalu za Azure klasični

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]U ovom se članku opisuje model klasični implementacije. Možete i [Saznajte kako stvoriti internetsku nasuprotne opterećenja pomoću upravitelja resursa Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Postavljanje mjesto na Internetu opterećenja za virtualnim strojevima

Da biste učitali saldo mrežnog prometa s Interneta na virtualnim računalima sustava servis u oblaku, morate stvoriti skup uravnoteženja. Ovaj postupak pretpostavlja da ste već stvorili virtualnim strojevima i jesu li sve unutar iste servis u oblaku.

**Da biste konfigurirali uravnoteženja skup za virtualnim strojevima**

1. Azure klasični portalu kliknite **virtualnim računalima sustava**, a zatim naziv virtualnog računala u skupu uravnoteženja.

2. Kliknite **krajnje točke**, a zatim kliknite **Dodaj**.

3. Na stranici **Dodavanje krajnje točke za virtualnog računala** , kliknite strelicu desno.

4. Na stranici **Odredite detalje o krajnjoj** :

    * U odjeljak **naziv**upišite naziv za krajnju točku ili odaberite ime s popisa unaprijed definiranih krajnje točke za uobičajenih protokola.
    * U **protokol**odaberite protokol potrebnih vrste krajnje točke, TCP i UDP, po potrebi.
    * U **priključak javne i privatne priključak**unesite priključak brojeve koje želite virtualnog računala da biste koristili, po potrebi. Privatni priključak i pravila vatrozida možete koristiti na virtualnog računala za preusmjeravanje prometa na način koji odgovara aplikacije. Privatni priključak može biti isti kao javno priključak. Ako, na primjer, krajnje točke za promet web (HTTP), nije moguće dodijelite priključak 80 u javnim i privatnim priključak.

5. Odaberite **Stvaranje skupa uravnoteženja**, a zatim kliknite strelicu desno.

6. Na stranici **Konfiguracija skupa uravnoteženja** upišite naziv skupa uravnoteženja, a zatim dodijelite vrijednosti za probni ponašanja Azure raspoređivača opterećenja. Raspoređivača opterećenja koristi probes da bi se utvrdilo ako virtualnim strojevima u skupu uravnoteženja dostupni za primanje promet.

7. Kliknite kvačicu da biste stvorili krajnju točku uravnoteženja. Vidjet ćete **da** u stupcu **Naziv skupa uravnoteženja** na stranici **krajnje točke** za virtualnog računala.

8. Na portalu kliknite **virtualnim strojevima**, kliknite naziv dodatne virtualnog računala u skupu uravnoteženja, kliknite **krajnje točke**, a zatim **Dodaj**.

9. Na stranici **Dodavanje krajnje točke za virtualnog računala** kliknite **Dodaj krajnja točka za postojeći skup uravnoteženja**, odaberite naziv skupa uravnoteženja i zatim kliknite strelicu desno.

10. Na stranici **Odredite detalje o krajnjoj** upišite naziv za krajnju točku, a zatim kvačicu.

Za dodatne virtualnim strojevima u skupu uravnoteženja, ponovite korake od 8 10.



## <a name="next-steps"></a>Daljnji koraci

[Početak rada konfiguriranje Interna opterećenja](load-balancer-get-started-ilb-arm-ps.md)

[Konfiguriranje načina raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md)

[Konfiguriranje neaktivnosti TCP postavki vremenskog ograničenja za raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md)

