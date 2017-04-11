<properties
   pageTitle="Vodič za brzi početak centar za sigurnost Azure | Microsoft Azure"
   description="Ovaj članak sadrži Uvod brzo centar za sigurnost Azure navođenjem koju kroz sigurnosne nadzor i pravila upravljanja komponente i povezivanjem vam sljedeće korake."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Vodič za brzi početak centar za sigurnost Azure

U ovom se članku pomaže vam brzo Uvod u Centar za sigurnost Azure tako da vam navođenjem kroz sigurnosne nadzor i pravila upravljanja komponente centar za sigurnost.

> [AZURE.NOTE] U ovom se članku predstavlja servis pomoću implementacije sustava primjer. U ovom se članku nije Postupni vodič.

## <a name="prerequisites"></a>Preduvjeti

Za početak rada s centar za sigurnost, morate imati pretplatu na Microsoft Azure. Ako nemate pretplatu, možete se prijaviti [pomoću računa](https://azure.microsoft.com/pricing/free-trial/).

Besplatni sloj centar za sigurnost je automatski omogućena za vašu pretplatu i daje uvid u stanja sigurnosti Azure resurse. Azure partnera pruža osnovne sigurnosne Upravljanje pravilima, preporuke o sigurnosti i Integracija sa sigurnosnim proizvodima i uslugama.

Centar za sigurnost pristupiti s [portala za Azure](https://azure.microsoft.com/features/azure-portal/). Da biste saznali više o portalu Azure, potražite u [dokumentaciji portala](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Prikupljanje podataka

Centar za sigurnost prikuplja podatke iz virtualnim strojevima (VMs) procijenite njihovo stanje sigurnost, navedite preporuke o sigurnosti i upozorava vas na prijetnji. Kada pristupite centru za sigurnost, prikupljanje podataka je omogućena na sve VMs u svoju pretplatu. Preporučuje se prikupljanje podataka, ali možete odustati tako da isključite prikupljanje podataka u centru za sigurnost pravila.

Sljedeći koraci opisuju kako pristupa i korištenja komponente centar za sigurnost. U sljedećim koracima, Pokazat ćemo vam kako isključiti prikupljanje podataka, odlučite li odustati.

## <a name="access-security-center"></a>Centar za sigurnost programa Access

Na portalu, slijedite ove korake da biste pristupili centru za sigurnost:

1. Na izborniku **Microsoft Azure** odaberite **Centar za sigurnost**.
![Izbornik za Azure][1]

2. Ako pristupate centar za sigurnost, otvorit će se **zaslon dobrodošlice** plohu. Odaberite **da! Želim centar za sigurnost pokretanje Azure** da biste otvorili **Centar za sigurnost** plohu i omogućuje prikupljanje podataka.
![Zaslon dobrodošlice][10]

3. Nakon što pokretanje centar za sigurnost na dobrodošlice plohu ili odabrati centar za sigurnost na izborniku Microsoft Azure, otvorit će se plohu **Centar za sigurnost** . Radi lakšeg pristupa plohu **Centar za sigurnost** u budućnosti, odaberite mogućnost **Pin plohu nadzorne ploče** (gornji desni kut).
![PIN plohu mogućnost nadzorne ploče][2]

## <a name="use-security-center"></a>Korištenje centra za sigurnost

Možete konfigurirati sigurnosne pravilnike za Azure pretplate i grupa resursa. Pogledajmo konfigurirati sigurnosna pravila za vašu pretplatu:

1. Na plohu **Centar za sigurnost** odaberite pločicu **pravila** .
![Sigurnosna pravila][3]

2. Na plohu **Sigurnosna pravila - definirati pravilo po pretplatu ili resursa grupe** odaberite pretplatu.
3. **Prikupljanje podataka** na plohu **Sigurnosna pravila** omogućena je automatski prikupljanje zapisnika. Nadzor proširenje dodjeli se na sve trenutne i nove VMs pretplate. (Možete odabrati iz zbirke podataka postavljanjem **Prikupljanje podataka** da biste **isključili**, ali to sprječava centar za sigurnost dodjeljivanja sigurnosnih upozorenja i preporuke.)
4. Na plohu **Sigurnosna pravila** odaberite **Odaberite račun za pohranu po regijama**. Za svaku regiju u kojoj ćete imati VMs radi odaberite račun za pohranu koji se pohranjuju podaci prikupljeni s tim VMs. Ako se odlučite na račun za pohranu za svaku regiju, stvaranja umjesto vas. Podaci koji se prikupljaju se logički Izolirani iz podataka ostali korisnici sigurnosnih vam razloga.

     > [AZURE.NOTE] Preporučujemo da Omogući prikupljanje podataka i najprije odaberite račun za pohranu na razini pretplate. Sigurnosna pravila možete postaviti na razini Azure pretplate i razina grupe resursa, ali konfiguracije prikupljanje podataka i račun za pohranu koji se pojavljuje na razini pretplate samo.

5. Na plohu **Sigurnosna pravila** odaberite **pravilnik o sprječavanju**. Time se otvara **u pravilnik o sprječavanju** plohu.
![Pravilnik o sprječavanju][4]

6. Uključite preporuke za koje želite vidjeti kao dio sigurnosna pravila plohu **pravilnik o sprječavanju** . Primjeri:

 - Postavljanje **ažuriranja sustava** **na** pregledava sve podržane virtualnim strojevima za OS ažuriranja koja nedostaju.
 - Postavljanje **OS slabe točke** **na** pregledava sve podržane virtualnih računala da biste odredili sve OS konfiguracija koji bi se mogla virtualnog računala više ranjivo.

### <a name="view-recommendations"></a>Preporuke za prikaz

1. Vratite se u **Centar za sigurnost** plohu i odaberite pločicu **preporuke** . Centar za sigurnost povremeno analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, prikazuje preporuke na plohu **preporuke** .
![Preporuke u centru za sigurnost Azure][5]

2.  Odaberite preporuku na plohu **preporuke** da biste pogledali dodatne informacije i/ili da biste poduzeti da biste riješili taj problem.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Prikaz stanja i sigurnost stanje resursa

1.  Vratite se u **Centar za sigurnost** plohu. Pločicu **Resursi sigurnost stanja** sadrži pokazatelje stanja sigurnosti za virtualnim strojevima, povezivanje s mrežom, podataka i aplikacije.
2.  Odaberite **virtualnih računala** da biste pogledali dodatne informacije. Plohu **virtualnim strojevima** otvara i prikazuje sažetak status antimalware programe, ažuriranja sustava, ponovnog pokretanja i slabe točke OS od vašeg VMs.
![Pločicu resursi stanja u centru za sigurnost Azure][6]

3.  Odaberite preporuke u odjeljku **PREPORUKE VIRTUALNOG računala** da biste pogledali dodatne informacije i/ili poduzeti da biste konfigurirali potrebne kontrole.
4.  Odaberite VM u odjeljku **virtualnih računala** da biste vidjeli dodatne detalje.

### <a name="view-security-alerts"></a>Prikaz sigurnosnih upozorenja

1.  Vratite se u **Centar za sigurnost** plohu i odaberite pločicu **sigurnosnih upozorenja** . **Sigurnosna upozorenja** plohu otvara i prikazuje popis upozorenja. Centar za sigurnost analizu zapisnika sigurnost i aktivnosti mreže stvara te upozorenja. Uključene su upozorenja s Integriranom partnerskih rješenja.
![Sigurnosna upozorenja u centru za sigurnost Azure][7]

    > [AZURE.NOTE] Sigurnosna upozorenja su dostupne samo ako je omogućeno standardne sloj centar za sigurnost. Dostupna je 90 dana za besplatnu probnu verziju sustava standardne sloju. Informacije o tome kako se standardni sloju potražite u članku [sljedeće korake](#next-steps) .

2.  Odaberite upozorenja da biste pogledali dodatne informacije. U ovom primjeru odaberimo **otkrivanja sustava Izmijenjeno binarni**. Otvorit će se blades koji sadrže dodatne pojedinosti o upozorenje.
![Detalje sigurnosnih upozorenja u centru za sigurnost Azure][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Prikaz stanja partnerskih rješenja

1. Vratite se u **Centar za sigurnost** plohu. Pločica **partnerskih rješenja** omogućuje praćenje na prvi pogled, status stanje vašeg partnerskih rješenja integriran s pretplatom Azure.
2. Odaberite pločicu **partnerskih rješenja** . Na plohu otvara i prikazuje popis vaše partnerskih rješenja povezan s centar za sigurnost.
![Partnerskih rješenja][9]

3. Odabir rješenja partnera. U ovom primjeru odaberimo **F5 WAF** rješenja.  Na plohu otvara i prikazuje status rješenja partnera i rješenja pridružene resurse. Odaberite **konzolu rješenja** da biste otvorili sučelje za upravljanje partnera za rješenja.

## <a name="next-steps"></a>Daljnji koraci
U ovom se članku uvedena sigurnosne nadzor i pravila upravljanja komponente centar za sigurnost. Sad kad ste upoznati s centar za sigurnost, isprobajte sljedeće korake:

- Konfiguriranje sigurnosna pravila za pretplatu Azure. Dodatne informacije potražite u članku [pravila sigurnosne postavke u centru za sigurnost Azure](security-center-policies.md).
- Koristite preporuke u centru za sigurnost da biste zaštitili Azure resurse. Dodatne informacije potražite u članku [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md).
- Pregledajte i upravljanje trenutni sigurnosnih upozorenja. Dodatne informacije potražite u članku [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md).
- Saznajte više o na [Napredne značajke otkrivanja prijetnju](security-center-detection-capabilities.md) koje se isporučuju sa [standardnom sloju](security-center-pricing.md) centar za sigurnost. Dostupna je 90 dana za besplatnu probnu verziju sustava standardne sloju.
- Ako imate pitanja o korištenju centar za sigurnost, potražite u članku [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
