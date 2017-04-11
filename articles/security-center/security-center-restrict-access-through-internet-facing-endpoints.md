<properties
   pageTitle="Ograničiti pristup putem internetskog krajnje točke u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Ograniči pristup putem Interneta dostupnog krajnje točke**."
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

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Ograničiti pristup putem internetskog krajnje točke u centru za sigurnost Azure

Centar za sigurnost Azure će vam ograničiti pristup putem internetskog krajnje točke Ako bilo koji od mreže sigurnosnih grupa (NSGs) sadrži jednu ili više ulazna pravila koja omogućuju pristup s "sve" IP adresu izvora. Otvaranje pristup "niti" mogu omogućiti attackers da biste pristupili resurse. Centar za sigurnost će preporučujemo da uređujete te ulazna pravila da biste ograničili pristup izvorni IP adresama doista potreban pristup.

Za bilo koji nisu web priključak koja ima "" kao izvor generira se ove preporuke.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer. Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. **Preporuke plohu**odaberite **Ograniči pristup putem Interneta dostupnog krajnje točke**.
![Ograničiti pristup putem Interneta nasuprotne krajnje točke][1]

2. Otvorit će se plohu **Ograniči pristup putem Interneta dostupnog krajnje točke**. U ovom plohu popisi virtualnim strojevima (VMs) s ulazna pravila stvaranje potencijalne sigurnošću. Odaberite na VM.
![Odaberite na VM][2]

3. Plohu **NSG** prikazuje informacije, povezane ulazna pravila i pridruženi VM mreže sigurnosne grupe. Odaberite **Uređivanje ulazna pravila** da biste nastavili s uređivanjem ulazna pravila.
![Plohu mreže sigurnosne grupe][3]

4. Na **dolazni sigurnosna pravila** plohu odaberite ulazna pravila za uređivanje. U ovom primjeru odaberimo **AllowWeb**.
![Ulazna pravila sigurnosti][4]

  Imajte na umu, možete odabrati i **Zadana pravila** da biste vidjeli skup zadana pravila nalaze svi NSGs. Zadana pravila nije moguće izbrisati, ali jer se dodjeljuju niži prioritet, oni mogu nadjačati pravila koje ste stvorili. Saznajte više o [Zadana pravila](../virtual-network/virtual-networks-nsg.md#default-rules).
![Zadana pravila][5]

5. Na plohu **AllowWeb** uredite svojstva ulaznog pravilo tako da je **izvor** IP adresa ili blokiranje IP adresa. Dodatne informacije o svojstvima ulazna pravila potražite u članku [NSG pravila](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Uređivanje ulazna pravila][6]

## <a name="see-also"></a>Vidi također

U ovom se članku prikazivao kako implementirati preporuke centar za sigurnost "Ograniči pristup putem Interneta dostupnog krajnje točke". Dodatne informacije o omogućivanju NSGs i pravila potražite u sljedećim člancima:

- [Što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Kako upravljati NSGs pomoću portala za Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md)– upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md)– Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md)– upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md)– upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md)– traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/)– traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
