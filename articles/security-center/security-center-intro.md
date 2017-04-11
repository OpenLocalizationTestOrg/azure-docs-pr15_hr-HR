<properties
   pageTitle="Uvod u Centar za sigurnost Azure | Microsoft Azure"
   description="Saznajte više o centru za sigurnost Azure, njegovim ključa mogućnostima i kako funkcionira."
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
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Uvod u Centar za sigurnost Azure

Saznajte više o centru za sigurnost Azure, njegovim ključa mogućnostima i kako funkcionira.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.

## <a name="what-is-azure-security-center"></a>Što je centar za sigurnost Azure?
 Centar za sigurnost pomaže vam sprječavanje, otkrivanje i odgovoriti prijetnji s bolje vidio u i kontrolu nad sigurnost Azure resurse. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

##  <a name="key-capabilities"></a>Ključne mogućnosti
 Centar za sigurnost isporučuje jednostavno koristi i učinkovitih prijetnju mogućnosti sprječavanje, otkrivanje i odgovor koji su ugrađeni u Azure. Mogućnosti za ključne su:

| | |
|----- |-----|
| Sprječavanje | Nadzire stanja sigurnosti Azure resursa |
| | Definira pravila za Azure pretplate i grupa resursa, ovisno o vašoj tvrtki sigurnosne preduvjete vrste aplikacije koju koristite i osjetljivost podataka |
| | Preporuke o sigurnosti koristi pravila utemeljenih na vodič za servis vlasnici kroz postupak implementacije potrebne kontrole |
| | Brzo uvodi usluge zaštite i aparata tvrtke Microsoft i partneri |
| Otkrivanje |Automatski prikuplja i analizira sigurnost podataka iz Azure resursa, mreže i partnerskih rješenja kao što su programi antimalware i vatrozida |
| | Globalni leverages prijetnju obavještavanje iz Microsoftove proizvode i usluge, Microsoft jedinica digitalni Crimes (DCU), Microsoft centar za sigurnost odgovor (MSRC) i vanjske sažetaka sadržaja |
| | Odnosi se naprednom analitikom, uključujući strojnog učenja i behavioral analizu |
| Odgovori | Omogućuje kupljenih upozorenja prioritet sigurnosti |
| | Nudi uvida u izvor napadi i utjecati resursi |
| | Predlaže načina za zaustavljanje trenutnog napadi i spriječili buduće napada |

## <a name="introductory-walkthrough"></a>Uvodni vodič
 Centar za sigurnost pristupiti s [portala za Azure](https://azure.microsoft.com/features/azure-portal/). [Prijavite se na portal sustava](https://portal.azure.com), odaberite **Pregledaj**, pomaknite se do mogućnosti **Centar za sigurnost** i odaberite pločicu **Centar za sigurnost** koji koje su prethodno prikvačene portala nadzorne ploče.

![Sigurnost servisu Azure portal][1]

Iz centra za sigurnost, postavljanje sigurnosnih pravilnika, praćenje konfiguracije sigurnost, a prikaz sigurnosnih upozorenja.

### <a name="security-policies"></a>Sigurnosna pravila

Možete definirati pravila za Azure pretplate i grupa resursa prema sigurnosne preduvjete koje je vaša tvrtka. Možete prilagodite i ih vrste programe koje koristite ili osjetljivosti podatke u svaku pretplatu. Na primjer, resursa koji se koriste za razvoj i testiranje možda različite sigurnosne preduvjete koje je od onih koje koristi za proizvodnju aplikacije. Isto tako, aplikacija pomoću regulated podataka kao što su PII može zahtijevati više razine sigurnosti.

> [AZURE.NOTE] Da biste izmijenili pravila sigurnosti na razini pretplate ili razina grupe resursa, morate biti vlasnik pretplate ili suradnika na njega.

Na plohu **Centar za sigurnost** odaberite pločicu **pravila** popis pretplata i grupa resursa.   

![Centar za sigurnost plohu][2]

**Sigurnosna pravila** za plohu odaberite pretplatu da biste vidjeli detalje pravila.

![Sigurnosna pravila plohu pretplate][3]

**Prikupljanje podataka** (vidi iznad) omogućuje prikupljanje podataka za sigurnosna pravila. Omogućivanje omogućuje:

- Dnevni pregled svih podržana virtualnog računala za nadzor sigurnosti i preporuke.
- Zbirka sigurnosnih događaja za analizu i prijetnju otkrivanje.

**Odaberite račun za pohranu po regijama** (vidi iznad) omogućuje vam da odaberete, za svaku regiju koja vam omogućuje virtualnim strojevima pokrenut, računa za pohranu pohranjuju podaci prikupljeni s tim virtualnim računalima. Ako se odlučite na račun za pohranu za svaku regiju, stvorit će se za vas. Podaci koji se prikupljaju se logički Izolirani iz podataka ostali korisnici sigurnosnih vam razloga.

> [AZURE.NOTE] Prikupljanje podataka, a zatim odaberete račun za pohranu po regijama je konfiguriran na razini pretplate.

Odaberite **pravilnik o sprječavanju** (vidi iznad) da biste otvorili plohu **pravilnik o sprječavanju** . **Prikaži preporuke za** omogućuje vam da odaberete sigurnosne kontrole koje želite nadzirati, a preporučujemo da na temelju potreba sigurnost resursa u pretplatu.

Zatim odaberite grupu resursa da biste vidjeli detalje pravila.

![Grupa resursa plohu pravila za sigurnost][4]

**Nasljeđivanje** (vidi iznad) omogućuje definiranje grupa resursa kao:

- Naslijeđeno i (zadano) što znači svih sigurnosnih pravilnika za ovu grupu resursa nasljeđuju od razinu pretplate.
- Jedinstveni što znači da se u grupu resursa će imati pravila za prilagođene sigurnosti. Morat ćete promjene u odjeljku **Prikaži preporuke za**.

> [AZURE.NOTE] Ako vam se prikazuje sukob između razina pravila pretplate i razine pravila grupe za resursa, razine pravila grupe resursa prednost.

### <a name="security-recommendations"></a>Preporuke o sigurnosti

 Centar za sigurnost analizira stanja sigurnosti Azure resurse za prepoznavanje potencijalne slabe. Popis preporuke će vas voditi kroz postupak konfiguriranja potrebnih kontrola. Primjeri:

- Dodjeljivanje antimalware da biste lakše identificirati i ukloniti zlonamjernog softvera
- Konfiguriranje mreže sigurnosne grupe i pravila za kontrolu promet na virtualnim računalima
- Dodjela resursa sustava web aplikacije vatrozidima radi obrane protiv napada taj cilj web-aplikacije
- Implementacija nedostaju ažuriranja sustava
- Adresiranje OS konfiguracije koje odgovaraju preporučene referente vrijednosti

Kliknite pločicu **preporuke** popis preporuke. Kliknite svaki preporuke za prikaz dodatnih informacija ili poduzeti da biste riješili taj problem.

![Preporuke o sigurnosti u centru za sigurnost Azure][5]

### <a name="resource-health"></a>Stanje resursa

Pločica **stanja sigurnosti resursa** prikazuje cjelokupni posture sigurnost okruženja po vrsti resursa, uključujući virtualnih računala, web-aplikacije i ostale resurse.   

Odaberite vrstu resursa na pločici **resursa sigurnost stanja** da biste pogledali dodatne informacije, uključujući popis sve potencijalne slabe koji je otkrili. (**Virtualnim strojevima** uključen u primjeru u nastavku)

![Pločica stanja resursi][6]

### <a name="security-alerts"></a>Sigurnosna upozorenja

 Centar za sigurnost automatski prikuplja, analizira i integrira podaci iz zapisnika iz Azure resursa, mreže i partnerskih rješenja kao što su programi antimalware i vatrozida. Kada se otkriju prijetnji, stvara se sigurnosno upozorenje. Primjeri otkrivanje:

- Ugrožena virtualnim strojevima komunikaciju s poznatim zlonamjerni IP adrese
- Napredni zlonamjernog softvera otkriven pomoću izvješćivanje o pogreškama u sustavu Windows
- Brute prisilno napada protiv virtualnim strojevima
- Sigurnosnih upozorenja iz programa integrirani antimalware i vatrozidi

Kliknite pločicu **sigurnosnih upozorenja** i prikazat će se popis prioritet upozorenja.

![Sigurnosna upozorenja][7]

Dodatne informacije o napadi i kako ga remediate odaberete upozorenje prikazuje.

![Upozorenja pojedinosti o sigurnosti][8]

### <a name="partner-solutions"></a>Partnerskih rješenja

Pločica **partnerskih rješenja** omogućuje monitora na prvi pogled status stanja partnerskih rješenja integriran s pretplatom Azure. Centar za sigurnost prikazuje upozorenja koji dolaze iz rješenja.

Odaberite pločicu **partnerskih rješenja** . Otvorit će se na plohu prikazuje popis svih povezanih partnerskih rješenja.

![Partnerskih rješenja][9]

## <a name="get-started"></a>Početak rada
Da biste započeli centar za sigurnost, potrebno vam je pretplate na Microsoft Azure. Centar za sigurnost je omogućeno za vašu pretplatu na Azure. Ako nemate pretplatu, možete se prijaviti za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

 Centar za sigurnost pristupiti s [portala za Azure](https://azure.microsoft.com/features/azure-portal/). Potražite u [dokumentaciji portala](https://azure.microsoft.com/documentation/services/azure-portal/) da biste saznali više.

[Uvod u Centar za sigurnost Azure](security-center-get-started.md) brzo vodi vas kroz sigurnost – nadzor i upravljanje pravilima komponenti centar za sigurnost.

## <a name="see-also"></a>Vidi također
U ovom dokumentu su predstavljena centar za sigurnost, njegovim ključa mogućnostima i upute za početak rada. Dodatne informacije potražite u sljedećim člancima:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa.
- [Sigurnost Azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
