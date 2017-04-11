<properties
  pageTitle="Učitavanje prilagođene probes opterećenja i praćenje stanja status | Microsoft Azure"
  description="Naučite koristiti prilagođene probes za Azure opterećenja praćenje instance iza opterećenja"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Razumijevanje probes raspoređivača učitavanja

Azure opterećenja nudi mogućnost praćenje stanja server instancama pomoću probes. Kada je probni ne reagira, opterećenja zaustavlja slanje nove veze dobro instance. Postojeće veze ne utječe na, a nove veze koju se šalju dobar instance.

Oblak uloge servisa (tempiranja uloge i uloge web) za nadzor probni koristite agent za goste. TCP i HTTP prilagođene probes mora biti konfigurirana kada koristite virtualnim strojevima iza raspoređivača opterećenja.

## <a name="understand-probe-count-and-timeout"></a>Razumijevanje probni count i vremenskog ograničenja

Ovisi o probni ponašanje:

- Broj uspješno probes koje omogućuju instancu da biste biti označene kao prema gore.
- Broj neuspjelih probes koji uzrokuju instancu da biste biti označene kao prema dolje.

Vremensko ograničenje i učestalost vrijednost u SuccessFailCount odrediti je li instance potvrđena pokrenut ili nije pokrenut. Na portalu Azure vremensko ograničenje je postavljeno na dva puta vrijednost učestalosti.

Konfiguriranje probni sve instance uravnoteženja krajnje točke (to jest, uravnoteženja skupu) mora biti isti. To znači da ne možete imati različite probni konfiguracije za svaku instancu uloga ili virtualnog računala iste servis za kombinaciju određene krajnje točke. Na primjer, svaku instancu morate imati identične lokalnim priključcima i vremenska ograničenja.

>[AZURE.IMPORTANT] Opterećenja probni koristi se s IP adresom 168.63.129.16. Javnu IP adresu olakšava komunikacije Interna platformu resursi za na Premjesti-vaše – vlasnik – IP scenarij Azure virtualne mreže. Virtualna javnu IP adresu 168.63.129.16 se koristi u svim regijama i neće se promijeniti. Preporučujemo da dopusti ovu IP adresu u sva pravila vatrozida za lokalni. To ne treba smatrati sigurnosni rizik jer samo Interna platforme Azure možete izvora poruke s te adrese. Ako to ne učinite, bit će neočekivano ponašanje u različitim scenarijima kao što je konfiguriranje istom rasponu IP adresa 168.63.129.16 i pritom duplicirati IP adrese.

## <a name="learn-about-the-types-of-probes"></a>Dodatne informacije o vrstama probes

### <a name="guest-agent-probe"></a>Probni agent za goste

U ovom probni dostupna je za servise u Oblaku Azure samo. Opterećenja koristi agent za goste unutar virtualnog računala, a zatim očekuje podatke i Odgovori s je u redu 200 HTTP odgovor samo kada instanci je u stanju spremno (to jest, ne i u drugu stanja kao što je zauzeto, recikliranje ili prekida).

Dodatne informacije potražite u članku [Konfiguriranje datoteku definicije servisa (csdef) za stanje probes](https://msdn.microsoft.com/library/azure/ee758710.aspx) ili [Početak rada prilikom stvaranja mjesto na Internetu opterećenja za servise u oblaku](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Što čini probni agent za goste označavanje instance kao dobro?

Ako agent za goste ne reagira u redu 200 HTTP, raspoređivača opterećenja označava instancu kao reagirati i slati promet na tu instancu. Pomoću naredbe ping instanci raspoređivača opterećenja i dalje. Ako agent za goste Odgovori s do 200 HTTP, raspoređivača opterećenja šalje promet na tu instancu ponovno.

Kada koristite ulogu web, web-mjesta kod obično pokreće se u w3wp.exe koji ne nadzire na Azure tkanina ili gost agent. To znači da problemi s w3wp.exe (na primjer, odgovore HTTP 500) ne se prijavljivati agent za goste, a raspoređivača opterećenja neće moći koristiti tu instancu iz rotacije.

### <a name="http-custom-probe"></a>Prilagođeni probni HTTP

Prilagođeni probni HTTP opterećenja nadjačava goste zadani agent probni što znači da možete stvoriti vlastiti prilagođeni logiku da biste odredili stanje instance uloge. Raspoređivača opterećenja probes na krajnjoj točki svakih 15 sekundi prema zadanim postavkama. Instancu smatra se u zakretanje raspoređivača opterećenja ako on Odgovori s do 200 HTTP unutar isteklo (31 sekunde po zadanom).

To može biti korisno ako želite implementirati vlastite logike da biste uklonili instance s zakretanje raspoređivača opterećenja. Ako, na primjer, nije odlučite ukloniti instancu ako je iznad 90% procesora i vraća-200 status. Ako imate web uloge koje koriste w3wp.exe, također znači da se automatski nadzor web-mjesta, jer-200 stanja pogreške u web-mjesto kod će se vratiti u probni raspoređivača opterećenja.

>[AZURE.NOTE] Probni prilagođene HTTP podržava relativni putovi i HTTP protokola samo. HTTPS nije podržana.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Što čini se probni HTTP prilagođene označavanje instance kao dobro?

- Aplikacija HTTP vraća HTTP odgovor kod osim 200 (na primjer, 403, 404 ili 500). Ovo je pozitivan potvrde koje instanci aplikacije treba poduzeti iz servisa odmah.
- HTTP poslužitelj ne reagira uopće nakon isteklo razdoblje. Ovisno o isteku vremena vrijednost koja je postavljena to može značiti da taj više probni zahtjeve Idi neodgovorena prije na probni dobiva označena da nije pokrenut (to jest, prije SuccessFailCount probes šalju).
- Poslužitelj zatvara vezu putem TCP Vrati izvorne postavke.

### <a name="tcp-custom-probe"></a>TCP prilagođene probni

TCP probes pokretanje veze izvođenjem tri način rukovanja s definirani priključak.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Što čini TCP prilagođene probni označavanje instance kao dobro?

- TCP poslužitelj ne reagira uopće nakon isteklo razdoblje. Kada se na probni označen kao nije pokrenut ovisi o broju zahtjeva nije uspjelo probni podešena za slanje neodgovorena prije označavanju probni kao nije pokrenut.
- Na probni prima TCP Vrati iz uloge instance.

Dodatne informacije o konfiguriranju programa probni stanja HTTP ili TCP probni potražite u članku [Početak rada prilikom stvaranja mjesto na Internetu raspoređivača opterećenja u Voditelj resursa pomoću komponente PowerShell](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Dodavanje dobar instance zaključali zakretanje raspoređivača učitavanja

TCP i HTTP probes smatraju dobar i označavanje instancu uloga kao dobar kada:

- Raspoređivača opterećenja dohvaća pozitivan probni prvi put u VM pokretati.
- Broj SuccessFailCount (opisan u prethodnom) određuje vrijednost uspješno probes koje su potrebne da biste označili instancu uloga kao dobar. Ako uloga instance uklonjena, broj uspjela, uzastopna probes morate jednako ili premašuju vrijednost SuccessFailCount da biste označili instancu uloga kao tekući.

>[AZURE.NOTE] Ako je stanje instance uloga je koje fluktuiraju, raspoređivača opterećenja čeka više prije stavljanja instancu uloga natrag u dobar stanju. To možete učiniti pravilnikom zaštiti korisnika i Infrastruktura.

## <a name="use-log-analytics-for-load-balancer"></a>Korištenje zapisnika analize opterećenja

[Zapisnik analize opterećenja](load-balancer-monitor-log.md) možete koristiti da biste provjerili na status probni stanja i probni count. Zapisivanje mogu se s web-mjesto servisa Power BI ili u okvir za Azure radu uvida dati statistiku o statusu raspoređivača opterećenja stanja.
