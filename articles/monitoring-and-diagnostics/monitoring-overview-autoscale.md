<properties
    pageTitle="Pregled automatsko skaliranje u virtualnim računalima sustava Microsoft Azure, servise u Oblaku i web-aplikacije | Microsoft Azure"
    description="Pregled automatsko skaliranje u Microsoft Azure. Odnosi se na virtualnim računalima, servise u Oblaku i web-aplikacije."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Pregled automatsko skaliranje u virtualnim računalima sustava Microsoft Azure, servise u Oblaku i web-aplikacije

U ovom se članku opisuje što automatsko skaliranje Microsoft Azure, njegov prednosti te kako se da biste počeli koristiti ga.  

Azure automatsko skaliranje Monitor odnosi samo na [Skupove skaliranje virtualnog računala](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Servise u Oblaku](https://azure.microsoft.com/services/cloud-services/)i [Aplikacije servisa – web-aplikacije](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure ima dvije metode automatsko skaliranje. Starija verzija programa automatsko skaliranje primjenjuje se na virtualnim strojevima (dostupnost skupovima). Značajka je ograničena podrška i preporučujemo migracije skupovima VM mjerilo za podršku za automatsko skaliranje brže i više pouzdane. Veza na upute za korištenje starije tehnologija uvrštava u ovom članku.  


## <a name="what-is-autoscale"></a>Što je automatsko skaliranje?

Automatsko skaliranje omogućuje vam desnom količinu resursa koji se izvodi učiniti opterećenje aplikacije. Omogućuje da biste dodali resurse za rukovanje povećanja u opterećenja i spremiti i novac uklanjanjem resursi koji su koji se nalaze neaktivan. Odredite najmanji i najveći broj instanci pokretanja i dodavanje ili uklanjanje VMs automatski na temelju skupa pravila. Imate minimalne čini li program uvijek izvodi još nema opterećenju. Imate najviše ograničenjima vaše ukupni moguće zaračunava trošak. Automatski Skaliraj između tih dvaju extremes pomoću pravila koja ste stvorili.

 ![Automatsko skaliranje objašnjenje. Dodavanje i uklanjanje VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Kada se zadovolje Uvjeti pravila, jednu ili više akcija automatsko skaliranje se pokreće. Možete dodati i ukloniti VMs ili druge radnje. Sljedeće konceptualni dijagram prikazuje postupak.  

 ![Konceptualni automatsko skaliranje dijagrama toka](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Objašnjenje automatsko skaliranje postupak
Sljedeća objašnjenja odnose se na dijelove prethodni dijagrama.   

### <a name="resource-metrics"></a>Metriku resursa
Resursi šalji metrike koji se kasnije obradili pravila. Metriku potjecati putem različite načine.
Skupove skaliranje VM koristi telemetrijskih podataka iz Azure Dijagnostika agente dok telemetrijskih za web-aplikacije i servise u Oblaku potječe izravno infrastruktura za Azure. Neke najčešće korištenih Statistika obuhvaća procesora, memorije, niti broji, Duljina reda čekanja i korištenje diska. Popis telemetrijskih podataka koje možete koristiti potražite u članku [Automatsko skaliranje uobičajenih metriku](insights-autoscale-common-metrics.md).

### <a name="time"></a>Vrijeme
Pravila temeljena na raspored temelje se na UTC-a. Vremenska zona mora pravilno postavljen pri postavljanju pravila.  

### <a name="rules"></a>Pravila
Dijagram prikazuje samo jedno pravilo za automatsko skaliranje, ali imate više od njih. Možete stvoriti složene preklapajućih pravila kao što je potrebno za vašu situaciju.  Vrste pravila  

 - **Utemeljen na metriku** – na primjer, postupak kada je procesora veću od 50%.
 - **Temeljene** -, na primjer, okidača webhook svakih 8 am u subotu u određenom vremensku zonu.

Pravila temeljena na metriku Izmjerite učitavanja aplikacije i dodavanje ili uklanjanje VMs koji se temelji na toj Učitaj. Pravila temeljena na raspored omogućuju skaliranje kada uvidjeti vrijeme u vašem učitavanja i želite da biste skalirali prije moguće učitavanja povećanje ili smanjenje dogodit će se.  


### <a name="actions-and-automation"></a>Akcije i automatizacije

Pravila možete pokrenuti jednu ili više vrsta akcije.

- **Skaliranje** - skaliranje VMs ili smanjivanje
- **E-pošta** – Pošalji e-pošte za administratore pretplatu, suadministratora i/ili adresu dodatne e-pošte koju navedete
- **Automate putem webhooks** - webhooks poziva koje možete pokrenuti više složenih akcija unutar ili izvan Azure. Unutar Azure, možete pokrenuti Automatizacija Azure runbook, funkcija Azure ili Azure logike aplikacije. Primjer 3 URL proizvođača izvan Azure sadržavati servisa kao što je prazan hod i Twilio.


## <a name="autoscale-settings"></a>Automatsko skaliranje postavke
Automatsko skaliranje pomoću sljedećih terminologija i strukturu.

- **Automatsko skaliranje postavka** je pročitati modul automatsko skaliranje da donesete odluku o skaliranje prema gore ili dolje. Sadrži jedan ili više profila, informacije o cilj resursa i postavki obavijesti.
    - Kombinacija kapaciteta postavka skupa pravila koje se tiču okidača i skaliranje akcije za profil i ponavljajuće je **Automatsko skaliranje profila** . Možete imati više profila koje omogućuju vam voditi brigu o različitim preklapajućih preduvjeti.
        - **Postavka kapaciteta** znači postavke minimum, maksimum i zadane vrijednosti za broj instanci. [odgovarajućem mjestu za korištenje fig 1]
        - **Pravilo** sadrži okidač – metričkim okidača ili vrijeme pokretanje – i skaliranje akciju, koji označava hoće li automatsko skaliranje skaliranje prema gore ili dolje kada se to pravilo zadovoljeni.
        - **Ponavljanje** označava kada automatsko skaliranje stavite ovaj profil vrijediti. Možete postaviti automatsko drugu skaliranje profila za različita doba dana ili dane u tjednu, primjerice.
- **Postavka obavijesti** definira koje obavijesti se događa kada se pojavi automatsko skaliranje događaj ovisno o koje zadovoljavaju kriterije jednog automatsko skaliranje postavke profila. Automatsko skaliranje možete obavijestiti jednu ili više adresa e-pošte ili upućivati pozive na jednu ili više webhooks.

![Postavka Azure automatsko skaliranje, profila, i pravila strukture](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

Cijeli popis gornjih polja i opisa dostupna je u [Automatsko skaliranje REST API -JA](https://msdn.microsoft.com/library/dn931928.aspx).

Primjeri kod potražite u članku

* [Napredna automatsko skaliranje konfiguracija pomoću predložaka resursima za skupove VM skala](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Automatsko skaliranje REST API-JA](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Dodavanje veze za vodoravno vanjskih okomito skaliranje

Automatsko skaliranje vodoravno povećava resursa u samo ljestvice koji povećava ("više") ili smanjite broj instanci VM ("u").  Vodoravno skaliranje, što je fleksibilnije u oblaku situaciji kao što je omogućuje vam pokretanje potencijalno tisuće VMs učiniti Učitaj. Okomito skaliranje razlikuje. Zadržava isti broj VMs, ali čini u VM više ("gore") ili manje ("dolje") Napredna. Power mjeri se u memorije brzine procesora, prostora na disku, itd.  Okomito skaliranje ima više ograničenja. Je ovisna o dostupnosti veće hardveru i može se razlikovati po regijama i brzo dodirne i gornju granicu. Okomito skaliranje i obično zahtijeva VM tabulatora i start. Dodatne informacije potražite u članku [okomito skaliranje Azure virtualnog računala s Azure automatizaciju](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## <a name="methods-of-access"></a>Načini pristupa
Možete postaviti automatsko skaliranje putem

- [Portal za Azure](insights-how-to-scale.md)
- [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Različite platforme naredbeni redak sučelja (EŽA)](insights-cli-samples.md#autoscale )
- [Azure Monitor REST API-JA](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Podržane usluge za automatsko skaliranje


| Servis                              | Sheme i dokumenti                                       |
|--------------------------------------|-----------------------------------------------------|
| Web-aplikacije                             | [Promjena veličine web-aplikacije](insights-how-to-scale.md)              |
| Servisi u oblaku                       | [Automatsko skaliranje servis u Oblaku](../cloud-services/cloud-services-how-to-scale.md) |
| Virtualnim strojevima: klasični           | [Promjena veličine skupove dostupnost klasični virtualnog računala](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtualnim strojevima: Windows skaliranje skupova| [Skaliranje VM skaliranje postavlja u sustavu Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Virtualnim strojevima: Postavlja Linux skala  | [Skaliranje VM skaliranje postavlja u Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtualnim strojevima: Primjer Windows   | [Napredna automatsko skaliranje konfiguracija pomoću predložaka resursima za skupove VM skala](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o automatsko skaliranje koristi automatsko skaliranje vodiči navedene ili potražite u sljedećim resursima:

- [Azure Monitor automatsko skaliranje uobičajenih mjerenja](insights-autoscale-common-metrics.md)
- [Najbolje prakse za automatsko skaliranje Azure monitora](insights-autoscale-best-practices.md)
- [Korištenje automatsko skaliranje akcija za slanje e-pošte i webhook upozorenja](insights-autoscale-to-webhook-email.md)
- [Automatsko skaliranje REST API-JA](https://msdn.microsoft.com/library/dn931953.aspx)
- [Automatsko skaliranje za skupove mjerilo za otklanjanje poteškoća virtualnog računala](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
