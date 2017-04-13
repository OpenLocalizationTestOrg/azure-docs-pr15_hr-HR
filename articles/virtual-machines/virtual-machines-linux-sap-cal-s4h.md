<properties 
pageTitle="Implementacija HANA S/4 ili BW/4 HANA na Azure VM | Microsoft Azure" 
description="Implementacija S/4 HANA ili HANA BW/4 na Azure VM" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>Implementacija S/4 HANA ili HANA BW/4 na Microsoft Azure 

U ovom se članku opisuje kako implementirati S/4 HANA na Microsoft Azure putem SAP oblaka potražite biblioteku 3.0.
U snimke zaslona prikazuje postupak korak po korak. Implementacija drugih rješenja utemeljena na SAP HANA kao BW/4 HANA funkcionira na isti način kao iz perspektive postupak. Jedno samo je da biste odabrali drugu rješenja.

Započeti s otvorite SAP oblaka potražite biblioteku (Pokrećite SAP-a) [u nastavku](https://cal.sap.com/). Postoji bloga iz SAP o novi [SAP oblaka potražite biblioteku 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Sljedeće snimke zaslona Pokaži detaljne kako implementirati S/4 HANA na Microsoft Azure. Postupak funkcionira na isti način za druge rješenja likeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Prvu sliku prikazuje sve SAP Pokrećite HANA rješenja utemeljena na koje su dostupne na Microsoft Azure.
Exemplarily "SAP S/4 HANA lokalnog izdanje" (rješenje pri dnu snimku zaslona) nije odabran prođite kroz postupak.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Najprije na novi račun SAP Pokrećite ima će biti stvoren. Trenutno postoje dvije mogućnosti za Azure - standardni Azure i Azure na Kina Kini koji je kojim upravlja 21vianet partnera.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Zatim jedno je da biste unijeli ID Azure pretplate koje možete pronaći na portalu Azure – i pogledajte daljnje dolje Kupnja. Naknadno potvrdu Azure upravljanja mora se preuzeti.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

U novi Azure portala jedan pronalazi stavke "Pretplate" na lijevoj strani. Kliknite da biste prikazali sve aktivne pretplate za korisniku.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Odabirom jedne od pretplate, a zatim odaberete "Upravljanje certifikata" u članku se objašnjava koji ima je novi pojam pomoću "servisa upravitelji" za novi model upravljanja resursima Azure.
SAP Pokrećite nije prilagođen još ovaj novi model, a i dalje potreban je "klasični" modela i bivšem Azure portal za rad s potvrdama upravljanje.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Ovdje nešto možete vidjeti bivši Azure portal. Prijenos certifikat upravljanje daje SAP Pokrećite dozvole za stvaranje virtualnim strojevima kupca pretplate. U odjeljku "PRETPLATE" kartica nešto možete pronaći ID pretplate koje je moguće unijeti na portalu Pokrećite SAP.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Na kartici drugi pa je moguće prenijeti upravljanje certifikat preuzet prije s Pokrećite SAP-a.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Mali dijaloški okvir pojavljuje da biste odabrali datoteku preuzete certifikata.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Kada je učitana certifikat veze između Pokrećite SAP i kupca Azure pretplate možete testirati unutar pokrećite SAP-a. Mali poruke treba iskakati koji upućuje na to vrijedi vezu.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Nakon postavljanja računa jedno je da biste odabrali rješenje koje treba uvesti i stvoriti instancu.
"Osnovni" način je zaista trivial. Unesite naziv instance, odaberite Azure regija i definirati glavna lozinka rješenja.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Nakon nekog vremena ovisno o veličini i složenosti rješenja (u procjeni je zadan Pokrećite SAP-a) prikazan "aktivne" i jeste li spremni za korištenje. To je vrlo jednostavne.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Pogled na Detalji rješenja nešto možete vidjeti koja će se vrsta VMs su implementiran. U ovom slučaju su stvorene tri VMs Azure s različitim veličinama i više nije potrebna.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Na portalu Azure virtualnim strojevima možete pronaći počevši s istim nazivom instancu koji je odobren u Pokrećite SAP-a.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Sada je moguće rješenje putem gumba za povezivanje na portalu Pokrećite SAP-a za povezivanje. Dijaloški okvir malo sadrži vezu na korisničkom priručniku koji opisuje svih zadanih vjerodajnica za rad s rješenja.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Druga mogućnost je prijava klijentskog programa Windows VM i počnite, primjerice unaprijed konfigurirana GUI SAP-a.







