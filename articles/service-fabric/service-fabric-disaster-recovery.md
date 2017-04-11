<properties
   pageTitle="Azure oporavak Izrada servisa tkanina | Microsoft Azure"
   description="Azure tkanina servis nudi i mogućnosti koje su potrebne radnju za sve vrste disasters. U ovom se članku opisuju vrste disasters koji se mogu pojaviti i kako ih baviti."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Izrada oporavak u tkanina servisa Azure

Ključnih dio izlaganja visoku dostupnost oblaka aplikacija je jamči da ga možete preživi sve različite vrste pogrešaka, uključujući one koje su izvan kontrole. U ovom se članku opisuje fizički izgled programa klaster tkanina servisa Azure u kontekstu potencijalne disasters i sadrži smjernice o tome kako se baviti takve disasters da biste ograničili ili uklonili rizik od gubitka nedostupnost ili podatke.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Fizički izgled servisa tkanina klaster servisu Azure

Da biste shvatili rizika posed po različite vrste pogrešaka, je korisno je znati kako klastere su fizički podsjeća Azure.

Kada stvorite servisa tkanina klaster u Azure, su potrebne da biste odabrali područje u kojem će se nalaziti. Infrastruktura za Azure zatim dodjeljuje resurse za taj klaster unutar područja, većina čest broj virtualnim strojevima (VMs) zatražili. Pogledajmo pobliže gdje i kako se te VMs dodjeli.

### <a name="fault-domains"></a>Kvara domene

Prema zadanim postavkama VMs u klasteru ravnomjerno širi preko logičkim grupama naziva domene kvara (FDs), koji se fazi strojeva na temelju potencijalne pogreške u hardveru glavnog računala. Konkretno, ako se dva VMs nalaze u dva različita FDs, možete biti sigurni da ne koriste zajednički isti power izvor ili mreže parametar. Kao rezultat lokalnoj mreži ili struje utjecaja jedan VM neće utjecati na drugi dopuštanja tkanina servisa da biste poduzme opterećenje ne reagira računala unutar klaster.

Izgled svoj klaster možete vizualizirati svim domenama kvara pomoću karte klaster navedene u [Programu Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md):

![Proširite čvorove svim domenama kvara u programu Explorer tkanina servisa][sfx-cluster-map]

>[AZURE.NOTE] Na osi na karti klaster prikazuje nadogradnje domene koje logično grupirali čvorove na temelju planiranog održavanja aktivnosti. Servis tkanina klastere u Azure uvijek podsjeća svim pet nadogradnje domenama.

### <a name="geographic-distribution"></a>Geografske distribuciju.

Trenutno postoje [26 Azure područja cijelom svijetu][azure-regions], s nekoliko više objaviti. Pojedinačne područja mogu sadržavati centrima fizičkim podacima ovisno o zahtjev i dostupnost odgovarajuću mjesta, među Ostali čimbenici. Međutim, imajte na umu da čak i u regije koje sadrže više centre za fizičke podataka, nema jamstva da svoj klaster VMs će se jednoliko dodijeliti većem te mjestima. Uistinu, trenutno sve VMs za dani klaster su dodjeli unutar jednog fizičke web-mjesta.

## <a name="dealing-with-failures"></a>Postupanje s neuspjeha

Postoji nekoliko vrsta neuspjeha koji mogu utjecati na svoj klaster, svaka s vlastitom ublažiti. Ne možemo izgledat će ih u redoslijedu vjerojatnost da se pokreće.

### <a name="individual-machine-failures"></a>Pogreške na pojedinačna računala

Kao što je rečeno, pojedinačne strojno pogrešaka, unutar na VM ili u hardver ili softver koji se nalaze unutar domene kvara, ugroziti bez sami. Servis tkanina obično se otkriti neuspjeh sekundi te odgovaranje sukladno tome ovisno o stanju klaster. Ako, primjerice, ako čvor je hosting primarni replike za particije, novi primarni odabrali se iz sekundarnog replike na particije. Kada Azure poziva nije uspjelo strojno sigurnosnu kopiju, ponovno uključivanje klaster automatski će se i ponovno iskoristite na njegov zajednički povećavaju.

### <a name="multiple-concurrent-machine-failures"></a>Više Istodobni strojno neuspjeha

Dok kvara domene znatno smanjenju rizika od neuspjeha Istodobni računalo, uvijek je riječ o potencijal za više slučajni pogreške da bi se prikazala dolje nekoliko strojeva klasteru istodobno.

Općenito, pod uvjetom da većina čvorove ostaju dostupni, klaster će i dalje funkcionirati, koriste Premda je na donjem kapaciteta s praćenjem stanja replike dobiti zapakirane u manji skup strojeva i manje bez praćenja stanja instanci su dostupne za širenje Učitaj.

#### <a name="quorum-loss"></a>Kvorum gubitka

Ako se većina replike particije s praćenjem stanja servisa prema dolje, ta particija unosi stanje naziva "kvorum gubitka". Sada servisa tkanina zaustavlja dopustite upisivanje ta particija da biste bili sigurni stanje ostaje dosljedan i pouzdane. Na snazi su smo odabir da biste prihvatili razdoblje nedostupnosti da biste bili sigurni da je klijenti ne rečeno svoje podatke spremljena kada zaista nije. Imajte na umu da ste odlučili u dopuštanja čitanja iz sekundarnog replike za tu uslugu s praćenjem stanja, možete i dalje da biste izvršili one čitati operacije u tom stanju. Particije ostaje gubitak kvorum dok dovoljno broj replike vratite ili dok administrator klaster navodi sustava da biste premjestili na korištenje [Popravak ServiceFabricPartition API][repair-partition-ps].

>[AZURE.WARNING] Izvođenje akcije za popravak dok je primarni replike dolje uzrokovat će gubitak podataka.

Servisa sustava mogu se gubitka kvorum s utjecaj koji se odnose na servis u pitanju. Ako, primjerice, gubitka kvorum imenovanja servisu će utjecati razlučivanje naziva dok kvorum gubitka u servis upravitelja prebacivanje blokira stvaranja novog servisa i failovers. Imajte na umu da za razliku od za vlastitu servise pokušava popraviti servisa sustava *ne* preporučuje se. Umjesto toga, preporučuje se da jednostavno Pričekajte dok se ne vratite dolje replike.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Minimiziranje rizika od gubitka kvorum

Rizik od gubitka kvorum rješenje smanjite povećavanjem Odredišna veličina skupa replike servisa. Ovo je razmisliti o broju replike morate pomoću broj dostupna čvorove možete tolerate pri kada tijekom preostalo dostupne za zapisivanje zadržavanjem smeta tu aplikaciju ili nadogradnje klaster izvršiti čvorove privremeno nedostupan, osim hardverske pogreške.

Razmislite o sljedećim primjerima pod pretpostavkom da ste konfigurirali servisa da bi MinReplicaSetSize tri najmanji broj koji se preporučuje za servise radnog. S TargetReplicaSetSize tri (jedan primarni i dva secondaries), kvara hardvera tijekom nadogradnje (dvije replike prema dolje), rezultirat će kvorum gubitka, a na servisu neće biti samo za čitanje. Osim toga, ako imate pet replike, će moći withstand dva pogreške tijekom nadogradnje (tri replike prema dolje) kao što je preostale dvije replike možete i dalje obrasca kvorum unutar skupa minimalne replike.

### <a name="data-center-outages-or-destruction"></a>Kvarove centar podataka ili istrazi

U koje se rijetko pojavljuju se slučajevima centre za fizičke podataka može postati privremeno nedostupan zbog gubitka power ili mrežne veze. U tim slučajevima klastere tkanina servisa i aplikacije isto tako bit će dostupan, ali će se sačuvati podataka. Za klastere koji se izvodi u Azure, možete vidjeti ažuriranja kvarove na [stranici stanje Azure][azure-status-dashboard].

U događaj vrlo je vjerojatno se uništava centar za cijelu fizičkim podacima, bilo koji servis tkanina klastere hostira postoji izgubit će se, uz njihovo stanje.

Da biste zaštitili tu mogućnost, je kritično važno je da povremeno zemlj suvišnih spremište [sigurnosno kopiranje stanje](service-fabric-reliable-services-backup-restore.md) i provjerite imaju li provjeriti mogućnost da biste ih vratili. Koliko se često načinite sigurnosnu kopiju bit će ovisi o vaš cilj oporavak zareza (RPO). Čak i ako se potpuno nije implementirana sigurnosnog kopiranja i vraćanja još, trebali biste provesti upravljački program za na `OnDataLoss` događaj tako da se možete prijaviti kada se pojavi kao slijedi:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Do neuspjele softvera i drugih izvora od gubitka podataka

Kao uzrok gubitka podataka ubuduće, kod pogreške u services, Ljudski radu pogrešaka i sigurnost breaches su uobičajene od neuspjeha centar Primamo podataka. No u svakom slučaju, strategije oporavak je ista: preuzimanje redovito sigurnosno kopiranje svih s praćenjem stanja servisa i Nekorištenje mogućnost da biste vratili to stanje.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako u programu Publisher razne pogreške pomoću [testability framework](service-fabric-testability-overview.md)
- Ostali resursi Izrada oporavak i visoku dostupnost za čitanje. Microsoft je objavljen veliku količinu upute na ove teme. Dok neke od tih dokumenata se odnose na određene tehnike za korištenje u druge proizvode, koje sadrže mnogo Općenito najbolje prakse možete primijeniti u kontekstu tkanina servisa:
 - [Kontrolni popis za dostupnosti](../best-practices-availability-checklist.md)
 - [Detaljnu analizu oporavak Izrada](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Izrada oporavak i visoke dostupnosti za Azure aplikacije][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
