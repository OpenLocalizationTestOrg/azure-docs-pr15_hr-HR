<properties
   pageTitle="Zaštita mreže u centru za sigurnost Azure | Microsoft Azure"
   description="Dokument adrese preporuke u centru za sigurnost Azure koje olakšavaju zaštititi Azure mrežu i ostati razgovora u skladu s sigurnosna pravila."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-network-in-azure-security-center"></a>Zaštita mreže u centru za sigurnost Azure

Centar za sigurnost Azure analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, stvara preporuke za koje će vas voditi kroz postupak konfiguriranja potrebnih kontrola.  Preporuke primijeniti na vrste Azure resursa: virtualnim strojevima (VMs), povezivanje s mrežom, SQL i aplikacije.

U ovom se članku adrese preporuke koji se odnose na vašoj mreži.  Mrežni centar preporuke oko sljedećem vatrozidima generacije, mreže sigurnosne grupe, konfiguriranje pravila dolaznog prometa i više.  Pomoću tablice ispod kao referenca pomoću kojih se objašnjava dostupnih mrežnih preporuke i što svaki od njih će učiniti ako ga primijeniti.

## <a name="available-network-recommendations"></a>Mrežni preporuke

|Preporuka|Opis|
|-----|-----|
|[Dodajte sljedeće Vatrozid za generiranje](security-center-add-next-generation-firewall.md)|Preporučuje se da dodate sljedeće generacije vatrozid (NGFW) iz programa Microsoft partner da biste povećali vaše zaštiti sigurnosti.|
|[Usmjeravanje prometa NGFW samo](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Preporučuje konfigurirate mreže sigurnosnih grupa (NSG) pravila koja prisilno dolazni promet na vaše VM putem vaše NGFW.|
|[Omogućivanje mreže sigurnosne grupe na podmreže ili virtualnim strojevima](security-center-enable-network-security-groups.md)|Preporučuje se Omogućivanje NSGs podmreže ili VMs.|
|[Ograničiti pristup putem Interneta nasuprotne krajnje točke](security-center-restrict-access-through-internet-facing-endpoints.md)|Preporučuje se konfigurirati pravila ulazne promet za NSGs.|

## <a name="see-also"></a>Vidi također

Da biste saznali više o preporuke koji se odnose na druge vrste Azure resursa, pogledajte sljedeće:

- [Zaštita virtualnim računalima sustava u centru za sigurnost Azure](security-center-virtual-machine-recommendations.md)
- [Zaštita vaše aplikacije u Centar za sigurnost Azure](security-center-application-recommendations.md)
- [Zaštita na servisu Azure SQL u centru za sigurnost Azure](security-center-sql-service-recommendations.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
