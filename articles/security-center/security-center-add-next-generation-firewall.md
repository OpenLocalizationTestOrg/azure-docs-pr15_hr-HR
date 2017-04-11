<properties
   pageTitle="Dodajte sljedeće Vatrozid za generiranje u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Dodaj sljedeći Vatrozid za generiranje** "i" **traffice usmjeravanje kroz samo NGFW**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Dodajte sljedeće Vatrozid za generiranje u centru za sigurnost Azure

Centar za sigurnost Azure preporučuje se da dodate sljedeći Vatrozid za generiranje (NGFW) iz programa Microsoft partner da biste povećali vaše zaštiti sigurnosti. Ovaj dokument vodit će vas kroz primjera kako to učiniti.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **Dodaj sljedeći Vatrozid za generiranje**.
![Dodajte sljedeće Vatrozid za generiranje][1]

2. U plohu **Dodaj sljedeći Vatrozid za generiranje** odaberite krajnje točke.
![Odaberite krajnje točke][2]

3. Otvorit će se drugi plohu **Dodaj sljedeći Vatrozid za generiranje** . Možete koristiti postojeće rješenje ako je dostupna ili stvorite novi. U ovom primjeru dostupnih bez postojeća rješenja pa ćemo ćete stvoriti novi NGFW.
![Stvaranje nove sljedeće generacije vatrozid][3]

4. Da biste stvorili novi NGFW, odaberite rješenje na popisu integrirani partnera. U ovom primjeru ćemo će odabir **Potvrdite točke**.
![Odaberite dalje Vatrozid za generiranje rješenje][4]

5. Otvorit će se plohu **Potvrdite točku** dajući informacije o rješenja partnera. Odaberite **Stvori** plohu informacije.
![Plohu informacije vatrozid][5]

6. Otvorit će se plohu **Stvaranje virtualnog računala** . Ovdje možete unijeti podatke potrebne za Okretni gore virtualnog računala (VM) koji će se izvoditi na NGFW. Slijedite korake i navedite potrebne podatke NGFW. Odaberite u redu da biste primijenili.
![Stvaranje virtualnog računala da biste pokrenuli NGFW][6]

## <a name="route-traffic-through-ngfw-only"></a>Usmjeravanje prometa NGFW samo

Vratite se u plohu **preporuke** . Nova stavka generirana nakon dodali NGFW putem centra za sigurnost, pod nazivom **usmjeravanje prometa samo NGFW**. U ovom preporuke stvara se samo ako ste instalirali na NGFW putem centra za sigurnost. Ako imate krajnje točke mjesto na Internetu, centar za sigurnost će preporučujemo da konfigurirate mreže sigurnosne grupe pravila koja prisilno dolazni promet na vaše VM putem vaše NGFW.

1. **Preporuke plohu**odaberite **usmjeravanje prometa samo NGFW**.
![Usmjeravanje prometa NGFW samo][7]

2. Otvorit će se plohu **usmjeravanje prometa NGFW samo** s popisom VMs koji mogu usmjeravati promet. S popisa odaberite na VM.
![Odaberite na VM][8]

3. Otvorit će se plohu za odabrani VM prikaz povezanih ulazna pravila. Opis pruža dodatne informacije o moguće sljedeće korake. Odaberite **Uređivanje ulazna pravila** da biste nastavili s uređivanjem ulazna pravila. Na očekivanja je da **izvor** nije postavljena na **bilo kojem** za krajnje točke mjesto na Internetu povezane s na NGFW. Dodatne informacije o svojstvima ulazna pravila potražite u članku [NSG pravila](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Konfiguriranje pravila da biste ograničili pristup][9]
![ulazna pravila za uređivanje][10]

## <a name="see-also"></a>Vidi također

Ovaj dokument prikazivao kako implementirati preporuke centar za sigurnost "Dodajte sljedeće Vatrozid za generiranje." Da biste saznali više o NGFWs i rješenja partnera potvrdite zareza, pogledajte sljedeće:

- [Vatrozid dalje generacije](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Provjera vSEC točka](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – Saznajte kako konfigurirati sigurnosna pravila.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – blog traženje članaka o Azure sigurnost i usklađenost

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
