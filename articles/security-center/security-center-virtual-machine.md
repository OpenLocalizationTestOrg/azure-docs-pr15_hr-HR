<properties
   pageTitle="Centar za sigurnost Azure i Azure virtualnim strojevima | Microsoft Azure"
   description="Ovaj dokument pomaže vam da biste razumjeli kako centar za sigurnost Azure možete zaštitili vam virtualnim računalima sustava Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Centar za sigurnost Azure i Azure virtualnim strojevima

[Centar za sigurnost Azure](https://azure.microsoft.com/services/security-center/) pomaže vam sprječavanje, otkrivanje i odgovaranje na prijetnji. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

U ovom se članku prikazuje kako centar za sigurnost olakšavaju sigurne na virtualnim strojevima Azure (VM).

## <a name="why-use-security-center"></a>Zašto koristiti centar za sigurnost?

Centar za sigurnost pomaže vam zaštitili virtualnog računala podataka u Azure unosom uvid u postavke sigurnosti virtualnog računala. Kada centar za sigurnost safeguards vaše VMs, neće biti dostupne sljedeće mogućnosti:

- Postavke sigurnosti operacijski sustav (OS) s pravilima preporučena konfiguracija
- Sigurnost sustava i kritičnih ažuriranja koja nedostaju
- Preporuke za zaštitu krajnje točke
- Provjera valjanosti šifriranje diska
- Procjena slabe i potražite alat za
- Otkrivanje prijetnju

Osim Zaštitite svoje Azure VMs centar za sigurnost omogućuje sigurnost nadzor i upravljanje za servise u Oblaku, aplikacije servisa, virtualne mreže i drugo. 

>[AZURE.NOTE] Potražite u članku [Uvod u Centar za sigurnost Azure](security-center-intro.md) da biste saznali više o centru za sigurnost Azure.

## <a name="prerequisites"></a>Preduvjeti

Da biste započeli Azure centar za sigurnost, morat ćete znati i Imajte na umu sljedeće:

- Morate imati pretplatu na Microsoft Azure. Dodatne informacije o centar za sigurnost besplatne i standardne razine potražite [Cijene za centar za sigurnost](https://azure.microsoft.com/pricing/details/security-center/) .
- Planirajte svoje prihvaćanja centar za sigurnost, pročitajte članak [Vodič za planiranje Azure centar za sigurnost i operacije](security-center-planning-and-operations-guide.md) da biste saznali više o planiranju i operacije.
- Informacije o pružanje podrške za operacijski sustav potražite u članku [Centar za sigurnost Azure najčešća pitanja](security-center-faq.md). 

## <a name="set-security-policy"></a>Postavljanje sigurnosnih pravilnika

Prikupljanje podataka mora biti omogućena da bi se taj centar za sigurnost Azure možete prikupiti podatke potrebne za preporuke i upozorenja koje generiraju na temelju sigurnosnog pravilnika, konfigurirati. Na slici u nastavku, vidjet ćete **Prikupljanje podataka** je **uključen**.

Sigurnosna pravila definira skup kontrola koje su preporučena resursi unutar navedena pretplata ili grupu resursa. Prije omogućivanja sigurnosnog pravilnika, morate imati prikupljanje podataka omogućeno, centar za sigurnost prikuplja podatke iz virtualnim strojevima da bi se procijenite njihovo stanje sigurnost, navedite preporuke o sigurnosti i upozorava vas na prijetnji. U centru za sigurnost, definirajte pravila za Azure pretplate ili grupe resursa prema potrebama sigurnosti vaše tvrtke i vrstu aplikacije ili osjetljivosti podatke iz pretplate. 

![Sigurnosna pravila](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Da biste saznali više o svakom **pravilnik o sprječavanju** dostupna, potražite u članku [Postavljanje sigurnosnih pravilnika](security-center-policies.md) članka.

## <a name="manage-security-recommendations"></a>Upravljanje preporuke o sigurnosti

Centar za sigurnost analizira stanja sigurnosti Azure resurse. Kada centar za sigurnost označava potencijalne slabe, stvara preporuke. Preporuke će vas voditi kroz postupak konfiguriranja potrebnih kontrola.

Nakon postavljanja sigurnosnog pravilnika, centar za sigurnost analizira stanja sigurnosti resurse da biste odredili potencijalne slabe točke. Preporuke prikazuju se u obliku tablice pri čemu svaki redak predstavlja jednu određenu preporuke. U tablici u nastavku nudi nekoliko primjera preporuke za Azure VMs i što svaki od njih će učiniti ako ga primijeniti. Kad odaberete preporuku, će dobiti informacije koje se prikazuje kako implementirati na preporuke u centru za sigurnost.

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
| [Procjena slabe nije instaliran](security-center-vulnerability-assessment-recommendations.md) | Preporučuje se instalacija rješenja za procjenu slabe na vašem VM. |
| [Remediate slabe točke](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Omogućuje vam da biste vidjeli sustav i aplikacije slabe točke otkrio rješenja za procjenu slabe instaliran na vašem VM. |

>[AZURE.NOTE] Dodatne informacije o preporuke, potražite u članku [Upravljanje sigurnost preporuke](security-center-recommendations.md) članka.

## <a name="monitor-security-health"></a>Nadzor stanja sigurnosti

Kada omogućite [sigurnosne pravilnike](security-center-policies.md) za resurse za pretplatu na centar za sigurnost će analiza sigurnosti resurse da biste odredili potencijalne slabe točke.  Možete pogledati stanja sigurnosti resursa, uz probleme u plohu **stanja sigurnosti resursa** . Kada kliknete **virtualnim strojevima** na pločici stanja **sigurnosti resursa** , plohu **virtualnim strojevima** će se otvoriti s preporuke za vaše VMs. 

![Stanje sigurnosti](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Upravljanje i odgovaranje na sigurnosnih upozorenja

Centar za sigurnost automatski prikuplja, analizira i integrira podaci iz zapisnika iz Azure resursa, mreže i povezani partnerskih rješenja (kao što je vatrozid i krajnje točke zaštitu rješenja), možete otkriti realni prijetnji i smanjiti false positives. Po korištenje raznih zbrajanja mogućnosti za [otkrivanje](security-center-detection-capabilities.md), centar za sigurnost je moći generirati prioritet sigurnosnih upozorenja da biste lakše brzo Istraživanje problema i navedite preporuke za remediate mogućih napada.

![Sigurnosna upozorenja](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Odaberite sigurnosno upozorenje dodatne informacije o event(s) koja ga je pokrenula na upozorenja i što, ako postoje, koraci koje morate poduzeti da biste remediate u slučaju napada. Sigurnosna upozorenja grupiraju se prema [vrsti](security-center-alerts-type.md) i datum.


## <a name="see-also"></a>Vidi također

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
