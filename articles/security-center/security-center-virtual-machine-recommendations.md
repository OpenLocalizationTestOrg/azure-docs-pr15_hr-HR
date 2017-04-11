<properties
   pageTitle="Zaštita virtualnim računalima sustava u centru za sigurnost Azure | Microsoft Azure"
   description="U ovom dokumentu adrese preporuke u centru za sigurnost Azure koje olakšavaju zaštita virtualnim strojevima i ostati razgovora u skladu s sigurnosna pravila."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Zaštita virtualnim računalima sustava u centru za sigurnost Azure

Centar za sigurnost Azure analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, stvara preporuke za koje će vas voditi kroz postupak konfiguriranja potrebnih kontrola.  Preporuke primijeniti na vrste Azure resursa: virtualnim strojevima (VMs), povezivanje s mrežom, SQL i aplikacije.

U ovom se članku adrese preporuke koji se odnose na VMs.  Centar za preporuke VM oko prikupljanje podataka, Primjena ažuriranja sustava dodjele resursa antimalware, šifriranje vaše VM diskova i drugo.  Pomoću tablice ispod kao referenca pomoću kojih se objašnjava dostupna VM preporuke i što svaki od njih će učiniti ako ga primijeniti.

## <a name="available-vm-recommendations"></a>Dostupne VM preporuke

|Preporuka|Opis|
|-----|-----|
|[Omogućivanje prikupljanja podataka za pretplate](security-center-enable-data-collection.md)|Preporučuje uključivanje prikupljanje podataka u sigurnosna pravila za svaki od pretplate i virtualnim strojevima (VMs) u svoje pretplate.|
|[Remediate OS slabe točke](security-center-remediate-os-vulnerabilities.md)|Preporučuje se da poravnanje vaše OS konfiguracije s pravilima preporučena konfiguracija – primjerice ne dopuštaju lozinke spremiti.|
|[Primjena ažuriranja sustava](security-center-apply-system-updates.md)|Preporučuje se da implementacije nedostaju sigurnost sustava i kritičnih ažuriranja VMs.|
|[Ponovno nakon ažuriranja sustava](security-center-apply-system-updates.md#reboot-after-system-updates)|Preporučuje se da ponovno pokrenete VM da biste dovršili postupak primjene ažuriranja sustava.|
|[Instalacija Zaštita na krajnjoj točki](security-center-install-endpoint-protection.md)|Preporučuje se Dodjela resursa antimalware programa VMs (samo za Windows VMs).|
|[Razrješavanje Zaštita na krajnjoj točki stanja upozorenja](security-center-resolve-endpoint-protection-health-alerts.md)|Preporučuje se da riješiti pogreške Zaštita na krajnjoj točki.|
|[Omogućivanje VM Agent](security-center-enable-vm-agent.md)|Omogućuje vam da biste vidjeli koje zahtijevaju VMs VM Agent. Agent za VM na VMs mora biti instaliran da bi se Dodjela zakrpu skeniranje osnovne skeniranje i antimalware programe. Agent za VM se instalira prema zadanim postavkama za VMs koji su raspoređeni iz trgovine Azure Marketplace. U članku [VM Agent i proširenja – dio 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) pruža informacije o instalaciji VM Agent.|
| [Primjena šifriranje](security-center-apply-disk-encryption.md) |Preporučuje šifriranja vaše diskova VM šifriranjem Azure Disk (Windows i Linux VMs). Šifriranje preporučuje količine OS i podatke na vašem VM.|
| [Ažuriranje verzija OS-a](security-center-update-os-version.md) | Preporučuje se da ažurirate verzija operacijskog sustava (OS) za vaš servis u Oblaku na najnoviju verziju za vaše obitelji OS.  Da biste saznali više o servise u Oblaku, potražite u članku [Pregled servise u Oblaku](../cloud-services/cloud-services-choose-me.md). |
| [Procjena slabe nije instaliran](security-center-vulnerability-assessment-recommendations.md) | Preporučuje se instalacija rješenja za procjenu slabe na vašem VM. |
| [Remediate slabe točke](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Omogućuje vam da biste vidjeli sustav i aplikacije slabe točke otkrio rješenja za procjenu slabe instaliran na vašem VM. |

## <a name="see-also"></a>Vidi također

Da biste saznali više o preporuke koji se odnose na druge vrste Azure resursa, pogledajte sljedeće:

- [Zaštita vaše aplikacije u Centar za sigurnost Azure](security-center-application-recommendations.md)
- [Zaštita mreže u centru za sigurnost Azure](security-center-network-recommendations.md)
- [Zaštita na servisu Azure SQL u centru za sigurnost Azure](security-center-sql-service-recommendations.md)

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
