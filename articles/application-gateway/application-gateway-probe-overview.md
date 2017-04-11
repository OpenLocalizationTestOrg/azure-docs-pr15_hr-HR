

<properties
   pageTitle="Stanje nadzor pregled za pristupnik za aplikaciju Azure | Microsoft Azure"
   description="Dodatne informacije o praćenju mogućnosti u Azure aplikacije pristupnika"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="application-gateway-health-monitoring-overview"></a>Nadzor pregled stanje pristupnika aplikacije

Pristupnik za Azure aplikacije po zadanom nadzire funkcioniranje svih resursa u njegov pozadinske i automatski uklanja sve resursa koje se smatraju dobro iz na resurse. Pristupnik za aplikaciju nastavlja praćenje dobro instance te ih dodaje natrag na dobar skup pozadinske kada one postaju dostupne i odgovaranje na probes stanje. Pristupnik za aplikaciju šalje stanja probes s isti priključak koji je definiran u postavkama HTTP pozadinske. Taj se način u probni testira isti priključak koji korisnici želite upotrijebiti za povezivanje s pozadinski.

![primjer probni pristupnik za aplikaciju][1]

Osim pomoću zadanog stanja probni nadzor, možete prilagoditi i probni stanja kako bi odgovarao preduvjeti za aplikaciju sustava. U ovom se članku zadane i prilagođene stanja probes možete pronaći.

## <a name="default-health-probe"></a>Zadano stanje probni

Pristupnik za aplikaciju automatski konfigurira u zadano stanje probni kada je ne postavite sve prilagođene probni konfiguraciju. Nadzor ponašanje radi tako da HTTP zahtjev za IP adrese konfigurirana za skup pozadinske. Za zadani probes ako konfigurirane postavke http pozadinskog za HTTPS, na probni će koristiti https kao i za provjeru stanja u pozadinski sustav.

Na primjer: Konfiguriranje pristupnikom aplikacije da biste pozadinskih poslužitelja A, B i C primali HTTP mrežnog prometa na priključak 80. Nadzor zadanog stanja testira tri poslužitelji svakih 30 sekundi dobar HTTP odgovor. Dobar HTTP odgovor ima [Šifra stanja](https://msdn.microsoft.com/library/aa287675.aspx) između 200 i 399.

Ne uspijete probni potvrdite zadano za poslužitelj odgovora, pristupnika aplikacije uklanja iz njegova skup pozadinske i mrežni promet zaustavlja slijedi ovaj poslužitelj. Zadani probni nastavljaju se i dalje da biste provjerili poslužitelja svakih 30 sekundi. Kada poslužitelj A odgovori uspješno jedan zahtjev iz probni za stanje zadane, ona se dodaje natrag kao dobar skup pozadinske i promet započinje ponovno slijedi na poslužitelj.

### <a name="default-health-probe-settings"></a>Zadane postavke probni stanja

|Isprobati svojstvo | Vrijednost | Opis|
|---|---|---|
| Isprobati URL-a| http://127.0.0.1:\<priključak\>/ | Put URL-a |
| Interval | 30 | Isprobati interval u sekundama |
| Prekoračenje vremena  | 30 | Isprobati Prekoračenje vremena u sekundama |
| Dobro praga. | 3 | Isprobati count pokušajte ponovno. Pozadinskom poslužitelju je označen nakon zbroj neuspjeha uzastopnih probni dosegne dobro praga. |

> [AZURE.NOTE] Priključak uvijek će biti isti priključak kao pozadinske HTTP postavke.

Zadani probni razmatra samo http://127.0.0.1:\<priključak\> radi određivanja stanja statusa. Ako morate konfigurirati probni stanja da biste prešli prilagođenog URL-a ili izmjenu drugih postavki, morate koristiti prilagođene probes kao što je opisano u sljedećim koracima.

## <a name="custom-health-probe"></a>Probni prilagođene stanja

Prilagođeni probes omogućuju precizniji kontrolu nad nadzor stanja. Kada koristite prilagođene probes, možete konfigurirati interval probni, URL-a i put da biste testirali i koliko nije uspjelo odgovore da biste prihvatili prije označavanju instancu pozadinske skup kao dobro.

### <a name="custom-health-probe-settings"></a>Postavke probni prilagođene stanja

Sljedeća tablica sadrži definicije za svojstva prilagođene stanja probni.

|Isprobati svojstvo| Opis|
|---|---|
| Ime | Naziv na probni. Taj naziv se koristi za upućivanje na probni u pozadinskoj HTTP postavke. |
| Protokol | Protokol koji se koristi za slanje u probni. Na probni će koristiti protokol definiranim u postavkama pozadinske HTTP |
| Glavno računalo |  Naziv glavnog računala da biste poslali na probni. Primjenjivo samo kada više web-mjesta je konfiguriran na pristupnik za aplikaciju, u suprotnom koristiti "127.0.0.1". Time se razlikuje od VM naziv glavnog računala. |
| Put | Relativni put na probni. Valjani put započinje s '/'. |
| Interval | Interval isprobati u sekundama. Ovo je vremenski interval između dva uzastopna probes.|
| Prekoračenje vremena | Isprobati Prekoračenje vremena u sekundama. Na probni je označen kao nije uspjela ako valjani odgovor nije primili unutar tog isteka razdoblja. |
| Dobro praga. | Isprobati count pokušajte ponovno. Pozadinskom poslužitelju je označen nakon zbroj neuspjeha uzastopnih probni dosegne dobro praga. |

> [AZURE.IMPORTANT] Ako pristupnik za aplikaciju je konfiguriran za jednog web-mjesta, po zadanome domaćin potrebno navesti naziv kao "127.0.0.1", osim ako inače konfiguriran u prilagođene probni.
Za referencu prilagođene probni šalje \<protokol\>://\<glavno računalo\>:\<priključak\>\<put\>. Priključak koji se koristi bit će isti priključak kako je definirano u postavkama HTTP pozadinske.

## <a name="next-steps"></a>Daljnji koraci

Nakon učenje o nadzoru stanja računala pristupnika možete konfigurirati [probni prilagođene stanja](application-gateway-create-probe-portal.md) web-mjesto portala za Azure ili u okvir za [prilagođene stanja probni](application-gateway-create-probe-ps.md) pomoću komponente PowerShell i model implementacije Azure Voditelj resursa.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png