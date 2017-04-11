<properties 
    pageTitle="Azure Service Bus | Microsoft Azure" 
    description="Uvod u korištenje Bus servisa za povezivanje Azure aplikacije za drugi softver." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Azure Service Bus

Hoće li aplikacija ili pokreće servis u oblaku ili lokalno, često potrebne za interakciju s drugim aplikacijama i servisima. Omogućuje njih svim korisnicima Dopusti korisno da biste to učinili, Microsoft Azure nudi Bus servisa. U ovom se članku vodi susret tehnologiju, koji opisuje što je to i zašto možda želite koristiti.

## <a name="service-bus-fundamentals"></a>Osnove Bus servisa

Slučaja poziva za različite stilove komunikacije. Ponekad aplikacije slanje i primanje poruka putem jednostavne reda čekanja da je najbolje rješenje. U drugim situacijama svakodnevne reda čekanja nije dovoljno; red čekanja s mehanizam za objavljivanje – i – pretplata je bolje. U nekim slučajevima sve što je zaista potrebna je veza između aplikacija; Redovi nisu potrebne. Servis Bus nudi sve tri mogućnosti Omogućivanje aplikacije za interakciju na nekoliko različitih načina.

Servis Bus je s više klijentu servis u oblaku, što znači da servis je zajednički koristiti tako da više korisnika. Svakom korisniku, kao što je razvojni inženjer, stvara *prostora za naziv*, a zatim definira načini komunikacije Zasad mora unutar taj prostor naziva. Slika 1 prikazuje kako to izgleda.

![][1]
 
**Slika 1: Bus servisa sadrži više klijenta servisa za povezivanje aplikacije putem oblaka.**

U polje naziva, možete koristiti jednu ili više instanci četiri različite komunikacije Mehanizmi, od kojih svaka povezuje aplikacije na drugačiji način. Na raspolaganju su:

- *Redovi*, koje omogućuju jedan dvosmjerna komunikacije. Svaki red ponaša se kao posrednik (ponekad se zove i *broker*) koji se pohranjuju poslane poruke dok primitku. Svaka poruka je primio jednog primatelja.
- *Teme*, čime se omogućuje komunikaciju jedan dvosmjerna pomoću *pretplate*- jednu temu može imati više pretplata. Kao što je red čekanja temu ponaša se kao na broker, ali svake pretplate po želji možete koristiti filtar da biste primali samo poruke koje odgovaraju određenom kriteriju.
- *Preusmjeravanje*, čime se omogućuje komunikaciju dvosmjerne. Za razliku od redova i teme na prijenos ne sprema in-flight poruke nije u broker. Umjesto toga ga samo prosljeđuje ih na odredišnu aplikaciju.

Kada stvorite red, temu ili preusmjeravanja, koje joj dodijeliti naziv. U kombinaciji s bilo koje pod nazivom vaše prostora za naziv tog naziva stvara jedinstveni identifikator objekta. Aplikacije možete unijeti taj naziv tržišta servisa za, a zatim pomoću tog reda čekanja, temu ili preusmjeravanja možete komunicirati s međusobno. 

Da biste koristili neki od tih objekata u scenariju preusmjeravanja, aplikacija za Windows možete koristiti Windows Communication Foundation (WCF). Redovima i teme, aplikacija za Windows možete koristiti servis Bus definirane razmjenu API-ji. Da biste olakšali tih objekata za korištenje iz aplikacije koje nisu u sustavu Windows, Microsoft pruža SDK-ovi za Java, Node.js i druge jezike. Možete pristupiti i redova i teme REST API-ji putem HTTP(s). 

Je važno je znati koje čak i ako servis sabirnice sama pokreće u oblaku (to jest, u Microsoftovu Azure podatkovnim centrima), aplikacije koje koriste ga možete pokrenuti s bilo kojeg mjesta. Možete koristiti Bus servisa za povezivanje aplikacije koje rade na Azure, na primjer, ili aplikacija unutar vlastite podatkovnog centra. Možete ga koristiti i da biste se povezali aplikacije koje se izvode na Azure ili neke druge platforme oblaka s lokalnim aplikacije ili tablete i mobitele. Moguće je čak i za povezivanje kućne aparata, senzori i drugih uređaja s aplikacija za središnju ili na neki drugi. Servis Bus je mehanizam komunikacije u oblaku koja se može pristupiti iz vrlo koliko bilo kojeg mjesta. Kako koristite ovisi što je aplikacija potrebno učiniti.

## <a name="queues"></a>Reda čekanja

Ako odlučite za povezivanje dva aplikacije koje se koriste reda Bus servisa. Slika 2 ilustrira tom slučaju.

![][2]
 
**Slika 2: Redova Bus servisa omogućuju jednosmjerna asinkronog čekanja.**

Postupak je jednostavan: pošiljatelja šalje poruku servisa Bus red i u odabire tu poruku neke kasnije. Red čekanja može imati samo jedan primatelj, prikazuje slika 2. Ili više aplikacija može čitati iz iste reda. U potonjem slučaju svake poruke je čitati samo jednog tekstnog okvira. Za više cast servis, trebali biste umjesto toga koristite temu.

Svaka poruka sadrži dva dijela: skup svojstava, svaki par ključa vrijednosti i binarni poruke. Kako se koristi ovisi o što aplikacija pokušava učinite. Na primjer, aplikacija slanja poruke o nedavne Prodaja mogu sadržavati svojstva *Prodavač = "Ava"* i *Iznos = 10000*. Tijelo poruke možda sadrži skeniranu sliku s prodajom traje ugovora ili, ako ne postoji, samo ostati prazan.

U može čitati poruke iz reda Bus servisa na dva načina. Prva mogućnost pod nazivom *ReceiveAndDelete*uklanja poruke iz reda čekanja i odmah briše. To je jednostavno, ali ako primatelja ruši se prije nego što završi obradu poruke, poruka bit će izgubljeni. Budući da je uklonjen je s reda, bez primatelj možete mu pristupiti. 

Druga mogućnost *PeekLock*je namijenjena će vam pomoći u taj problem. Kao što je **ReceiveAndDelete** **PeekLock** čitanje uklanja poruke iz reda. No ne briše poruke. Umjesto toga ga zaključati poruka, upućivanje nevidljivi da biste druge primatelje, a zatim čeka na jedan od tri događaja:

- Ako primatelja uspješno obrađuje poruku, poziva **Dovršeno**i reda čekanja briše poruku. 
- Ako primatelja odluči da ga ne može obraditi poruku uspješno, poziva **ćete**. Red uklanja zaključavanje iz poruke pa ga čini dostupnim za druge primatelje.
- Ako primatelja poziva nijedno od ovih unutar konfigurirati vremenskom razdoblju (prema zadanim postavkama, 60 sekundi), reda čekanja pretpostavlja da nije uspjela primatelja. U ovom slučaju ponaša kao da primatelja imali naziva **ćete**, dostupnost poruku da biste druge primatelje.

Obratite pozornost na to što se događa ovdje: ista poruka možda isporuku dvaput, možda za dvije različite primatelje. Aplikacije koje koriste redova servisa knjižna se potrebno pripremiti za to. Da biste olakšali duplikata, svaka poruka sadrži jedinstveni **MessageID** svojstvo koje se po zadanom ostaje na isti način bez obzira na to kako više puta poruka je za čitanje iz reda. 

Redovi korisne su u prilično nekoliko situacija. Omogućuju aplikacije za komunikaciju čak i kada obje ne izvode u isto vrijeme nešto što je posebno korisno s grupe i mobilne aplikacije. Red čekanja s više primatelja omogućuje automatsko opterećenja, budući da su poslane poruke proširite preko tih primatelje.

## <a name="topics"></a>Teme

Korisni kao što su, redove nisu uvijek pravo rješenje. Ponekad Bus servisa teme sadrže bolje. Slika 3 ilustrira ovu ideju.

![][3]
 
**Slika 3: Na temelju filtar određuje subscribing aplikacije, to možete primati neke ili sve poruke poslane na temu Bus servisa.**

*Tema* je slična na razne načine za reda. Pošiljatelji slanja poruke na temu na isti način slanja poruke u red i te poruke izgledaju isto kao i kod reda čekanja Velike razlike je omogućiti da teme svaku primanju aplikaciju da biste stvorili vlastiti *pretplate* definiranjem *Filtar*. Pretplatnik pa će vidjeti samo poruke koje odgovaraju filtar. Slika 3, na primjer, prikazuje pošiljatelja i tema s tri pretplatnike, svaka s vlastitom filtra:

- Pretplatnički 1 prima samo poruke koje sadrže svojstvo *Prodavač = "Ava"*.
- Pretplatnički 2 Prima poruke koje sadrže svojstvo *Prodavač = "Ruby"* i/ili sadrže *Iznos* svojstva u programu čija je vrijednost veća od 100,000. Ruby možda je Voditelj prodaje pa želi da biste vidjeli svoju prodaje i sva velika Prodaja bez obzira na to tko ih čini.
- Pretplatnički 3 sadrži postavite njegov filtar na *True*, što znači da primi sve poruke. Na primjer, ove aplikacije može biti odgovoran za održavanje popratnu i stoga je potrebno da biste vidjeli sve poruke.

Kao i kod redove, pretplatnike na temu poruke možete čitati pomoću **ReceiveAndDelete** ili **PeekLock**. Za razliku od redove, no jedne poruke poslane na temu mogu primati višestruke pretplate. Taj se način, pod nazivom *Objavljivanje i pretplata* (ili *pub/sub*), je korisna kad god žele više aplikacija na isti poruke. Definiranjem desnom filtar svaki pretplatnika pristupite možete samo dio strujanje poruke koje je potrebno da biste vidjeli.

## <a name="relays"></a>Preusmjeravanje

Redova i teme sadrže jednosmjerna Asinkrona komunikacija pomoću programa broker. Promet teče u samo jednom smjeru, a ne postoji Izravni veza između pošiljatelja i primatelja. No ako to ne želite? Pretpostavimo da aplikacija morate i slanje i primanje poruka, ili možda želite izravnu vezu između njih, a ne morate broker za spremanje poruka. Da biste scenariji adresu, kao što su ova, Bus servis nudi *Preusmjeri*, kao što je slika 4 prikazuje.

![][4]
 
**Slika 4: Bus usluge preusmjeravanja omogućuje sinkrono, dvosmjernu komunikaciju između aplikacija.**

Očite pitanje na pitanja o preusmjeravanje je li ovo: Zašto mi je koristiti? Čak i ako se ne trebate redovi, provjerite Zašto aplikacije komunikaciju putem servis u oblaku, a ne samo u interakciju izravno? Odgovor jest to što radi izravno teže nego što mislite.

Pretpostavimo da želite povezati dva lokalnog aplikacije, oba se izvodi unutar tvrtke podatkovnim centrima. Svaki od tih aplikacija nalazi iza vatrozida i svaki podatkovnog centra vjerojatno koristi prevođenja mrežnih adresa (NAT). Vatrozid blokira ulazne podatke na sve osim nekoliko priključaka i NAT podrazumijeva da svaku aplikaciju radi na računalu nema fiksnim IP adresu koja možete pristupiti izravno iz izvan s podatkovnim centrom. Bez nekim dodatnu pomoć s Interneta javno povezivanje te aplikacije je problem.

Bus usluge preusmjeravanja može pomoći. Komunikaciju bi-directionally kroz na prijenos, svaku aplikaciju uspostavlja izlazni TCP veza s Bus servisa, a zatim ostaje otvorena. Svu komunikaciju između dvije aplikacije putuju ove veze. Budući da svaka veza uspostavljena iz unutar s podatkovnim centrom, vatrozida omogućuje promet za svaku aplikaciju bez otvaranja nove priključci. Taj se način dohvaća i oko NAT problem jer svaku aplikaciju dosljedan krajnje točke u oblaku cijeloj komunikacije. Aplikacije možete izbjeći probleme koje bi inače komunikacije teško razmjena podataka putem prijenos. 

Da biste koristili preusmjeravanje Bus servisa, aplikacija je na sustava Windows Communication Foundation (WCF). Servis Bus nudi WCF povezivanja koje ga čine jednostavne za interakciju putem preusmjeravanje aplikacijama za Windows. Aplikacije koje se već koristi WCF možete obično samo navesti jednu od ovih povezivanja, a potom objasniti međusobno putem na prijenos. Za razliku od redova i teme pomoću preusmjeravanje iz aplikacije koje nisu u sustavu Windows, tijekom moguće, zauzima neki programski trud; Navedeni su bez standardne biblioteke.

Za razliku od redova i teme aplikacije izričito ne stvorite preusmjeravanje. Umjesto toga, kada aplikacija koja želi primati poruke uspostavi TCP veze sa servisa Bus, na preusmjeravanja automatski se stvara. Kada se prekine veza, briše se prijenos. Da biste omogućili aplikacije da biste pronašli preusmjeravanja stvorio određene ga slušatelj, Bus servis nudi registra koja omogućuje aplikacije da biste pronašli određenu preusmjeravanja po nazivu.

Preusmjeravanje su pravo rješenje kada je potrebno direktnu komunikaciju između aplikacija. Na primjer, razmotrite sustav rezervacije Zrakoplovna tvrtka koja se izvodi u lokalni podatkovnom centru koji morate pristupiti iz kiosks za prijavu, mobilnih uređaja i računala. Aplikacije koji se izvodi na svim tih sustava nije moguće je za preusmjeravanje Bus servis u oblaku za komunikaciju, gdje god može biti pokrenut.

## <a name="summary"></a>Sažetak

Povezivanje aplikacije nekad je bio dio izrade dovršeno rješenja i raspon scenarija koji zahtijevaju aplikacija i servisa za komunikaciju s drugom postavljen da biste povećali kao dodatne aplikacije i uređaji povezani s Internetom. Unosom oblaku tehnologije za postizanje to kroz redova, teme i preusmjeravanje Bus servisa bi trebao bi ključna funkcija lakše implementirati i dostupnijim.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste naučili osnove Bus servisa Azure, slijedite ove veze da biste saznali više.

- Upute za korištenje [servisa Bus redovi](service-bus-dotnet-get-started-with-queues.md)
- Upute za korištenje [servisa Bus teme](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Kako koristiti [Bus usluge preusmjeravanja](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Uzorci Bus servisa](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
