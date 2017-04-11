<properties
   pageTitle="Upravljanje preporuke o sigurnosti u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument vodit će vas kroz kako preporuke u centru za sigurnost Azure olakšavaju zaštita Azure resurse i zadržavanje razgovora u skladu s sigurnosna pravila."
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

# <a name="managing-security-recommendations-in-azure-security-center"></a>Upravljanje preporuke o sigurnosti u centru za sigurnost Azure

Ovaj dokument vodit će vas kroz korištenje preporuke u Centar za sigurnost Azure da biste zaštitili Azure resurse.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="what-are-security-recommendations"></a>Što su preporuke o sigurnosti?
Centar za sigurnost povremeno analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, stvara preporuke. Preporuke će vas voditi kroz postupak konfiguriranja potrebnih kontrola.

## <a name="implementing-security-recommendations"></a>Implementacijom preporuke o sigurnosti

### <a name="set-recommendations"></a>Preporuke za postavljanje

[Pravila sigurnosne postavke u centru za sigurnost Azure](security-center-policies.md), saznat ćete na:

- Konfiguriranje sigurnosnih pravila.
- Uključite prikupljanje podataka.
- Odaberite koje preporuke da biste vidjeli kao dio sigurnosna pravila.

Trenutni pravila preporuke centar oko ažuriranja sustava, osnovne pravila, a zatim programa antimalware [sigurnosnim grupama s mrežom](../virtual-network/virtual-networks-nsg.md) na podmreže i sučelje mreže, nadzor baze podataka SQL, prozirne podataka šifriranje baze podataka za SQL i vatrozidi aplikaciju za web.  [Postavljanje sigurnosnih pravilnika](security-center-policies.md) sadrži opis svake mogućnosti preporuke.

### <a name="monitor-recommendations"></a>Preporuke za praćenje
Nakon postavljanja sigurnosnog pravilnika, centar za sigurnost analizira stanja sigurnosti resurse da biste odredili potencijalne slabe točke. Pločice **preporuke** plohu **Centar za sigurnost** obavještava ukupan broj preporuke otkrije centar za sigurnost.

![Preporuke pločica][1]

Da biste vidjeli detalje o svakom preporuka:

1. Kliknite na **preporuke pločica** na plohu **Centar za sigurnost** . Otvorit će se plohu **preporuke** .

Preporuke prikazuju se u obliku tablice pri čemu svaki redak predstavlja jednu određenu preporuke. Stupci u ovoj su tablici su sljedeći:

- **Opis**: objašnjava se preporuke i što je potrebno učiniti da biste ga riješili.
- **RESURSA**: popis resursa na koji se primjenjuje ovaj preporuke.
- **STANJE**: u članku se opisuje trenutno stanje u preporuka:
    - **Otvaranje**: U preporuke nije još adresirana.
    - **U tijeku**: U preporuke trenutno primijenjena resursima i vam je potreban ne poduzmete.
    - **Riješeno**: U preporuke već obavljen (u tom slučaju redak će biti zasivljen je).
- **TEŽINU**: u članku se opisuje težinu taj određeni preporuka:
    - **Visoke**: na slabe postoji smisleni resursa (primjerice aplikaciju, u VM ili sigurnosne grupe mreže) i potrebna.
    - **Srednje**: postoji u slabe i se rješavaju ili dodatne korake potrebne da biste ga uklonili ili da biste dovršili postupak.
    - **Niska**: na slabe postoji koji moraju biti adresirane, ali ne zahtijeva hitnoj temi. (Po zadanom niskog preporuke ne prikazuju, ali možete filtrirati prema niskoj preporuke ako želite da biste ih vidjeli.)

Pomoću tablice ispod kao referenca pomoću kojih se objašnjava dostupna preporuke i što svaki od njih će učiniti ako ga primijeniti.

> [AZURE.NOTE] Želite razumjeti [Klasični i resursima implementacije modela](../azure-classic-rm.md) za Azure resurse.

|Preporuka|Opis|
|-----|-----|
|[Omogućivanje prikupljanja podataka za pretplate](security-center-enable-data-collection.md)|Preporučuje uključivanje prikupljanje podataka u sigurnosna pravila za svaki od pretplate i virtualnim strojevima (VMs) u svoje pretplate.|
|[Remediate OS slabe točke](security-center-remediate-os-vulnerabilities.md)|Preporučuje se da poravnanje vaše OS konfiguracije s pravilima preporučena konfiguracija – primjerice ne dopuštaju lozinke spremiti.|
|[Primjena ažuriranja sustava](security-center-apply-system-updates.md)|Preporučuje se da implementacije nedostaju sigurnost sustava i kritičnih ažuriranja VMs.|
|[Ponovno nakon ažuriranja sustava](security-center-apply-system-updates.md#reboot-after-system-updates)|Preporučuje se da ponovno pokrenete VM da biste dovršili postupak primjene ažuriranja sustava.|
|[Dodavanje aplikacije Vatrozid za web](security-center-add-web-application-firewall.md)|Preporučuje implementacije Vatrozid za aplikaciju web (WAF) za krajnje točke web. Dodavanjem te aplikacije na vaše postojeće WAF implementacijama možete zaštititi više web-aplikacija u centru za sigurnost. WAF aparata (stvorena pomoću modela implementaciju upravljanja resursima) moraju biti implementirano u zasebnom virtualne mreže. WAF aparata (stvorena pomoću modela uvođenje klasičnog) su ograničeni na korištenje mreže sigurnosne grupe. Podrška za ovaj će se proširiti da biste potpuno Prilagođeno implementacije potražite WAF (klasični) u budućnosti. Centar za sigurnost će vam Dodjela resursa WAF da biste lakše obrane protiv napada ciljnog web-aplikacije na VMs i na aplikaciju servisa okruženje (elika i mala slova). Da biste saznali više o elika i mala slova, potražite u [Dokumentaciji o servisu okruženje za aplikacije](../app-service/app-service-app-service-environments-readme.md). |
|[Dovršavanje zaštite računala](security-center-add-web-application-firewall.md#finalize-application-protection)|Da biste dovršili konfiguraciju u WAF, promet mora biti preusmjereni potražite WAF. Slijedite ove preporuke će dovršiti instalaciju potrebne promjene.|
|[Dodajte sljedeće Vatrozid za generiranje](security-center-add-next-generation-firewall.md)|Preporučuje se da dodate sljedeće generacije vatrozid (NGFW) iz programa Microsoft partner da biste povećali vaše zaštiti sigurnosti.|
|[Usmjeravanje prometa NGFW samo](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Preporučuje konfigurirate mreže sigurnosnih grupa (NSG) pravila koja prisilno dolazni promet na vaše VM putem vaše NGFW.|
|[Instalacija Zaštita na krajnjoj točki](security-center-install-endpoint-protection.md)|Preporučuje se Dodjela resursa antimalware programa VMs (samo za Windows VMs).|
|[Razrješavanje Zaštita na krajnjoj točki stanja upozorenja](security-center-resolve-endpoint-protection-health-alerts.md)|Preporučuje se da riješiti pogreške Zaštita na krajnjoj točki.|
|[Omogućivanje mreže sigurnosne grupe na podmreže ili virtualnim strojevima](security-center-enable-network-security-groups.md)|Preporučuje se Omogućivanje NSGs podmreže ili VMs.|
|[Ograničiti pristup putem Interneta nasuprotne krajnje točke](security-center-restrict-access-through-internet-facing-endpoints.md)|Preporučuje se konfigurirati pravila ulazne promet za NSGs.|
|[Omogućivanje poslužitelja SQL nadzora](security-center-enable-auditing-on-sql-servers.md)|Preporučuje uključivanje nadzora za Azure SQL poslužitelja (servisa Azure SQL samo ne sadrži SQL koji se izvode na virtualnim strojevima).|
|[Omogućivanje baze podataka SQL nadzora](security-center-enable-auditing-on-sql-databases.md)|Preporučuje uključivanje nadzora za baze podataka Azure SQL (servisa Azure SQL samo ne sadrži SQL koji se izvode na virtualnim strojevima).|
|[Omogućivanje prozirne šifriranje podataka na baze podataka SQL](security-center-enable-transparent-data-encryption.md)|Preporučuje se da omogućite šifriranje za baze podataka SQL (samo za Azure SQL servis).|
|[Omogućivanje VM Agent](security-center-enable-vm-agent.md)|Omogućuje vam da biste vidjeli koje zahtijevaju VMs VM Agent. Agent za VM na VMs mora biti instaliran da bi se Dodjela zakrpu skeniranje osnovne skeniranje i antimalware programe. Agent za VM se instalira prema zadanim postavkama za VMs koji su raspoređeni iz trgovine Azure Marketplace. U članku [VM Agent i proširenja – dio 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) pruža informacije o instalaciji VM Agent.|
| [Primjena šifriranje](security-center-apply-disk-encryption.md) |Preporučuje šifriranja vaše diskova VM šifriranjem Azure Disk (Windows i Linux VMs). Šifriranje preporučuje količine OS i podatke na vašem VM.|
|[Navedite sigurnost podataka o kontaktima](security-center-provide-security-contact-details.md) | Preporučuje unesete sigurnost podaci za kontakt za svaku pretplate. Podaci za kontakt je e-pošte adresu i telefonski broj. Podaci će se koristiti s vama u kontakt ako naš tim za sigurnost pronađe su oštećene resurse. |
| [Ažuriranje verzija OS-a](security-center-update-os-version.md) | Preporučuje se da ažurirate verzija operacijskog sustava (OS) za vaš servis u Oblaku na najnoviju verziju za vaše obitelji OS.  Da biste saznali više o servise u Oblaku, potražite u članku [Pregled servise u Oblaku](../cloud-services/cloud-services-choose-me.md). |
| [Procjena slabe nije instaliran](security-center-vulnerability-assessment-recommendations.md) | Preporučuje se instalacija rješenja za procjenu slabe na vašem VM. |
| [Remediate slabe točke](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Omogućuje vam da biste vidjeli sustav i aplikacije slabe točke otkrio rješenja za procjenu slabe instaliran na vašem VM. |

Možete filtrirati i Odbaci preporuke.

1. Kliknite **Filtar** na plohu **preporuke** . Otvorit će se plohu **Filtar** i odaberite težinu i stanje vrijednosti koje želite vidjeti.

    ![Preporuke za filtriranje][2]

2. Ako zaključite da preporuku nije primjenjivo, možete odbaciti na preporuke i filtrirati ih iz prikaza. Da biste otkazali preporuku na dva načina. Jedan tako da desnom tipkom miša kliknite stavku, a zatim odaberite **Odbaci**. Drugi je pokazivač miša postavite iznad stavke, kliknite tri točke koje se pojavljuju na desnoj strani, a zatim odaberite **Odbaci**. Preporuke za nehotice možete pogledati tako da kliknete **Filtar**, a zatim odaberete **Dismissed**.

    ![Poništavanje preporuka][3]

### <a name="apply-recommendations"></a>Primjena preporuke
Kada pregledate sve preporuke odlučite koje možete primjenjivati prvi put. Preporučujemo da koristite ocjena težinu kao glavni parametar za procjenu koje preporuke se primjenjuje prvo.

U tablici preporuke iznad odaberite preporuku i voditi kroz kao primjer kako primijeniti i preporuke.

## <a name="see-also"></a>Vidi također
U ovom dokumentu su predstavljena preporuke o sigurnosti u centru za sigurnost. Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa.
- [Sigurnost Azure blog](http://blogs.msdn.com/b/azuresecurity/) – pronaći bloga o Azure sigurnost i usklađenost.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
