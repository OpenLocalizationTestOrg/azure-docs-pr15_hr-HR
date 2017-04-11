<properties
   pageTitle="Omogućivanje mreže sigurnosne grupe u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **Omogućili mrežni sigurnosne grupe**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-network-security-groups-in-azure-security-center"></a>Omogućivanje mreže sigurnosne grupe u centru za sigurnost Azure

Centar za sigurnost Azure će vam omogućiti mreže sigurnosne grupe (NSG) ako nešto već nije omogućen. NSGs sadrži popis pravila za pristup kontrola popisa (ACL) koji dopustiti ili zabraniti mrežni promet na vaše VM instance u virtualne mreže. NSGs može biti pridruženi podmreže ili pojedinačne instance VM unutar tog podmreže. Kada je NSG pridružena podmreži, ACL pravila primjenjuju se na sve instance VM u tom podmreže. Osim toga, promet za pojedinačne VM može biti ograničena daljnje povezivanjem NSG izravno u tom VM. Da biste saznali dodatne potražite u člancima [što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)

Ako nemate NSGs omogućena, centar za sigurnost prikazati dvije preporuke vam: Omogućivanje mreže sigurnosne grupe na podmreže i omogućivanje mreže sigurnosne grupe na virtualnim računalima. Možete odabrati koji razine, podmreže ili VM da biste primijenili NSGs.


> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **Omogući mreže sigurnosne grupe** na podmreže ili na virtualnim računalima.
![Omogućivanje mreže sigurnosne grupe][1]

2. Otvorit će se plohu **Konfiguriranje nedostaje mreže sigurnosne grupe** za podmreže ili virtualnim strojevima, ovisno o preporuke koji ste odabrali. Odaberite podmreži ili virtualnog računala da biste konfigurirali na NSG.

  ![Konfiguriranje NSG za podmreže][2]

  ![Konfiguriranje NSG za VM][3]
3. Na plohu **Odabir mreže sigurnosne grupe** odaberite postojeći NSG ili da biste stvorili novi NSG.

  ![Odaberite mrežni sigurnosne grupe][4]

Ako stvorite novi NSG, slijedite korake u [upravljanju NSGs pomoću portala za Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) ćete stvoriti na NSG i postavljanje pravila za sigurnost.

## <a name="see-also"></a>Vidi također

U ovom se članku prikazivao kako implementirati preporuke za centar za sigurnost "Omogući mreže sigurnosne grupe" za podmreže ili virtualnim računalima. Dodatne informacije o omogućivanju NSGs potražite u sljedećim člancima:

- [Što je mreža grupe sigurnosti (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Kako upravljati NSGs pomoću portala za Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
