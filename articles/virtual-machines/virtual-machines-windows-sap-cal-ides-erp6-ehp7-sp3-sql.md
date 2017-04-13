<properties 
pageTitle="Implementacija SAP IDES EHP7 SP3 za SAP ERP 6.0 Microsoft Azure | Microsoft Azure" 
description="Implementacija SAP IDES EHP7 SP3 za SAP ERP 6.0 Microsoft Azure" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/> 
<tags 
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/> 


# <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a>Implementacija SAP IDES EHP7 SP3 za SAP ERP 6.0 Microsoft Azure 

U ovom se članku opisuje kako implementirati IDES SAP raditi s SQL Server i s operacijskim Sustavom Windows na Microsoft Azure putem SAP oblaka potražite biblioteku 3.0. U snimke zaslona prikazuje postupak korak po korak. Implementacija druga rješenja na popisu funkcionira na isti način kao iz perspektive postupak. Jedno samo je da biste odabrali drugu rješenja.

Započeti s otvorite SAP oblaka potražite biblioteku (Pokrećite SAP-a) [u nastavku](https://cal.sap.com/). Postoji bloga iz SAP o novi [SAP oblaka potražite biblioteku 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Sljedeće snimke zaslona Pokaži detaljne kako implementirati IDES SAP-a na Microsoft Azure. Postupak funkcionira na isti način za druga rješenja.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Prvu sliku prikazuje sve rješenja koje su dostupne na Microsoft Azure. U istaknuta je utemeljen na sustavu Windows IDES SAP rješenja koja je dostupna na Azure samo odabrali prođite kroz postupak.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

Najprije na novi račun SAP Pokrećite ima će biti stvoren. Trenutno postoje dvije mogućnosti za Azure - standardni Azure i Azure na Kini Kine koji je kojim upravlja 21vianet partnera.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Zatim jedno je da biste unijeli ID Azure pretplate koje možete pronaći na portalu Azure – i pogledajte daljnje dolje Kupnja. Naknadno potvrdu Azure upravljanja mora se preuzeti.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

U novi Azure portala jedan pronalazi stavke "Pretplate" na lijevoj strani. Kliknite da biste prikazali sve aktivne pretplate za korisniku.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Odabirom jedne od pretplate, a zatim odaberete "Upravljanje certifikata" u članku se objašnjava koji ima je novi pojam pomoću "servisa upravitelji" za novi model upravljanja resursima Azure.
SAP Pokrećite nije prilagođen još ovaj novi model, a i dalje potreban je "klasični" modela i bivšem Azure portal za rad s potvrdama upravljanje.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

Ovdje nešto možete vidjeti bivši Azure portal. Prijenos certifikat upravljanje daje SAP Pokrećite dozvole za stvaranje virtualnim strojevima kupca pretplate. U odjeljku "PRETPLATE" kartica nešto možete pronaći ID pretplate koje je moguće unijeti na portalu Pokrećite SAP.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

Na kartici drugi pa je moguće prenijeti upravljanje certifikat preuzet prije s Pokrećite SAP-a.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Mali dijaloški okvir pojavljuje da biste odabrali datoteku preuzete certifikata.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Kada je učitana certifikat veze između Pokrećite SAP i kupca Azure pretplate možete testirati unutar pokrećite SAP-a. Mali poruke treba iskakati koji upućuje na to vrijedi vezu.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

Nakon postavljanja računa jedno je da biste odabrali rješenje koje treba uvesti i stvoriti instancu.
"Osnovni" način je zaista trivial. Unesite naziv instance, odaberite Azure regija i definirati glavna lozinka rješenja.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Nakon nekog vremena ovisno o veličini i složenosti rješenja (u procjeni je zadan Pokrećite SAP-a) prikazan "aktivne" i jeste li spremni za korištenje. To je vrlo jednostavne.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Pogled na Detalji rješenja nešto možete vidjeti koja će se vrsta VMs su implementiran. U ovom slučaju postoji jedan jedan Azure VM veličine D12 stvoren pomoću Pokrećite SAP-a.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

Na portalu Azure virtualnog računala pronaći ćete počevši s istim nazivom instancu koji je odobren u Pokrećite SAP-a.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

Sada je moguće rješenje putem gumba za povezivanje na portalu Pokrećite SAP-a za povezivanje. Dijaloški okvir malo sadrži vezu na korisničkom priručniku koji opisuje svih zadanih vjerodajnica za rad s rješenja.
[Ovdje](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) je veza na vodič za IDES rješenja.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Druga mogućnost je prijava Windows VM i počnite, primjerice unaprijed konfigurirana GUI SAP-a.





