# <a name="compute"></a>Izračun

Azure omogućuje vam i praćenje vaše aplikacije kod koji se izvodi u Microsoftovu podatkovnom centru. Kada stvorite aplikaciju i pokrenuti na Azure, kod i konfiguracija zajedno naziva programa Azure hostira servisa. Glavnom računalu usluge su jednostavno upravljanje skaliranje gore i dolje, konfigurirajte i ažurirati novim verzijama kod vašeg računala. U ovom članku fokus je na Azure hostira modelu servisa.<a id="compare" name="compare"></a>

## Tablice sadržaja<a id="_GoBack" name="_GoBack"></a>

-   [Prednosti modela Azure aplikacije][]
-   [Servis za osnovni koncepti][]
-   [Zahtjevi za servis dizajna][]
-   [Dizajniranje aplikacije skaliranja][]
-   [Servis za definiciju i konfiguracija][]
-   [Datoteka za definiciju servisa][]
-   [Konfiguracijska datoteka servisa][]
-   [Stvaranje i implementacija servis][]
-   [Reference][]

## <a id="benefits"> </a>Pogodnosti modela azure aplikacije

Ako pokrenete program kao servis, Azure stvara jednu ili više virtualnim strojevima (VMs) koji sadrže kod za vaše aplikacije, a pokretati na VMs na fizičke residing u jednom od središta Azure podataka. Zahtjevi za klijenta u glavnom računalu aplikaciju unosu podatkovnog centra, raspoređivača opterećenja distribuira te zahtjeve jednako da biste na VMs. Dok je aplikacija nalazi se u Azure, korisniku će se tri ključne prednosti:

-   **Visoke dostupnosti.** Visoke dostupnosti znači Azure osigurava aplikacija je pokrenut najveću moguću, a može odgovoriti na zahtjev za klijenta. Ako aplikacije prekida (zbog neobrađenu iznimku, na primjer), a zatim Azure će otkriti to i će automatski ponovno pokrenuti aplikacije. Ako na računalu aplikaciju radi sučelja neke vrste kvara hardvera, zatim Azure će otkriti to i automatski stvoriti novi VM na nekom drugom računalu fizičke rad i pokretanje koda iz nje. Napomena: Redoslijedom aplikacije da biste dobili Microsoftovim razinu ugovor o usluzi % 99.95 dostupna, morate imati barem dva VMs izvodi kodu aplikacije. Time se omogućuje jedan VM da obradi zahtjeve klijenta dok Azure pomiče kod neuspjelih VM novi, dobro VM.

-   **Skalabilnost.** Azure omogućuje jednostavno i dinamički promijenite broj VMs izvodi kodu aplikacije za rukovanje stvarni opterećenje smještaju na aplikaciju. Omogućuje prilagodbu aplikacije da biste radno opterećenje koje korisnici stavljanje na njemu prilikom plaćanja samo za VMs koje trebate kada su vam potrebne. Kada želite promijeniti broj VMs Azure odgovori nekoliko minuta uspostavljanja moguće dinamički promijeniti broj VMs koji se izvodi kao često po želji.

-   **Mogućnost upravljanja.** Budući da Azure platforma kao Service (PaaS) koja nudi, upravlja (hardver sam, električne energije, i povezivanje s mrežom) potrebne infrastrukture da biste zadržali te računala izvode. Azure upravlja i platforme osiguravanje ažuran operacijski sustav sa svih na odgovarajuće zakrpa i sigurnosna ažuriranja, kao i ažuriranja komponente kao što su .NET Framework i Internet Information Server. Budući da sve VMs radite u sustavu Windows Server 2008, Azure nudi dodatne značajke kao što su dijagnostičkih nadzor, podrška za udaljenu radnu površinu, vatrozida i konfiguracija spremište potvrde. Te značajke navedeni su na bez dodatnog troška. Zapravo, prilikom pokretanja aplikacije u Azure, Windows Server 2008 licencu operacijskim sustavima (OS) uključen. Budući da sve na VMs radite u sustavu Windows Server 2008, kod koji se izvodi Windows Server 2008 samo dobro funkcionira kada se pokrene u Azure.

## <a id="concepts"> </a>Hostira servisa osnovni koncepti

Kada aplikacija je uveden kao servis servisu Azure, pokreće jedan ili više *uloge.* *Uloga* jednostavno odnosi se na web-mjesto datoteka aplikacije i u okvir za konfiguraciju. Možete definirati jednu ili više uloga aplikacije, svaka s vlastiti skup datoteka aplikacije i konfiguraciji. Za svaku ulogu u aplikaciji možete navesti broj VMs ili *uloga instance*, da biste pokrenuli. Na slici u nastavku prikazuje dva jednostavnog primjera aplikaciju u katalog modeliran kao servis pomoću uloge i uloge instance.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Slika 1: Jedan uloga s tri instance (VMs) izvodi u centru za Azure podataka

![Slika][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Slika 2: Dva uloge, svaka s dvije instance (VMs) izvodi u centru za Azure podataka

![Slika][1]

Uloga instance obično postupka internetski klijent zahtjevi unos podatkovnog centra kroz pod nazivom *unos krajnjoj točki*. Jedan uloga može imati krajnje točke 0 ili više unosa. Svaki krajnjoj točki upućuje na to web-mjesto protocol (HTTP, HTTPS ili TCP) i u okvir za priključak. Da biste konfigurirali ulogu želite imati dvije krajnje točke unosa uobičajeno je: HTTP slušanje priključak 80 i HTTPS slušanje na priključak 443. Na slici u nastavku prikazuje primjer dvije različite uloge s različitim unos krajnje točke koja usmjeruje zahtjevi klijenta na njih.

![Slika][2]

Kada stvorite servis servisu Azure, što vam je dodijeljen javno moguće adresirati IP adresu koja klijenti mogu koristiti za pristup. Nakon stvaranja nalazi servisa morate odabrati i URL prefiksa koja je mapirana te IP adresa. Ovaj prefiks mora biti jedinstvena kao što je zapravo su rezervirati *prefiks*. cloudapp.net URL tako da je nitko drugi mogu imati. Klijenti za komunikaciju vaša uloga instance pomoću URL-a. Obično ćete ne distribucija ili objavljivanje Azure *prefiks*. cloudapp.net URL-a. Umjesto toga će DNS naziv možete kupiti od registrara DNS odabranih i konfiguriranje DNS naziv da biste preusmjerili zahtjevi klijenta na URL Azure. Dodatne informacije potražite u članku [Konfiguriranje prilagođeni naziv domene u Azure][].

## <a id="considerations"> </a>Hostira zahtjevi dizajna servisa

Prilikom dizajniranja aplikacije da biste pokrenuli u okruženje oblaka, postoji nekoliko pitanja vezana uz da razmislite o kao što su Latencija, visoku dostupnost i skalabilnost.

Odlučujete kamo da biste pronašli kodu aplikacije je važna činjenica dok je pokrenut servis za u Azure. Zajednička implementacija aplikacije da biste centre za podatke koji su najbliži klijentima smanjiti Latencija i dobiti najbolje performanse moguće je.
Međutim, možete podatkovnog centra bliže vaše tvrtke ili bliže podataka ako imate neki jurisdictional ili pravni opasnosti o vašim podacima i gdje se nalazi. Postoje instaliranog hosting kodu aplikacije šest centrima podataka oko globusa. U sljedećoj tablici prikazane dostupnih mjesta:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Država/regija**

</td>
<td style="width: 200px;">
**Dodatni regije**

</td>
</tr>
<tr>
<td>
Sjedinjene Države

</td>
<td>
Jug središnje & Pazi središnje

</td>
</tr>
<tr>
<td>
Europa

</td>
<td>
Sjeverni & Zapad

</td>
</tr>
<tr>
<td>
Azija

</td>
<td>
Jugoistok & Istok

</td>
</tr>
</tbody>
</table>
Prilikom stvaranja servis, odaberite područje podmjesta koja upućuje na željeno mjesto kod za izvršavanje.

Da biste postigli visoke dostupnosti i skalabilnost, je kritično važnih podataka vaše aplikacije ostane u središnjem spremniku dostupno više instanci ulogu. Da biste lakše uz to, Azure nudi nekoliko mogućnosti za pohranu kao što su blob-ova, tablice i SQL baze podataka. Dodatne informacije o ovim tehnologijama prostora za pohranu u članku [Ponuda prostora za pohranu podataka u Azure][] . Na slici u nastavku prikazuje kako raspoređivača opterećenja u centru za Azure podataka distribuira klijentskog zahtjevi za drugu ulogu instanci koje imaju pristup isti prostor za pohranu podataka.

![Slika][3]

Obično na koji želite pronaći kodu aplikacije i podataka u centru za iste podatke kao što je to dopušta niske latencije (bolje performanse) kada kodu aplikacije pristupa podacima. Osim toga, vam se neće naplatiti za propusnost kada podataka premještati unutar iste podatkovnog centra.

## <a id="scale"> </a>Dizajniranja aplikacije skaliranja

Ponekad, preporučujemo vam da biste snimili aplikacija za jedinstvenu (kao što je jednostavno web-mjesta) i koristiti smješten u Azure. No često aplikacije mogu se sastojati od nekoliko uloge da sve Suradnja. Ako, na primjer, na slici u nastavku, postoje dvije instance uloge web-mjesta, tri instance uloge obrada narudžbe i jednu instancu uloge Generator izvješća. Te uloge su sve komponente za zajedničko funkcioniranje i šifru za sve ih možete pakirat zajedno i implementirati kao jedna jedinica najviše Azure.

![Slika][4]

Glavna razloga za podjelu aplikacije u različite uloge svaki izvode na vlastiti skup uloga instance (to jest, VMs) je neovisno skaliranje uloge. Na primjer, tijekom praznika, broju korisnika može biti kupnju proizvoda iz vaše tvrtke pa možda ćete morati povećati broj instanci uloge koje su pokrenute vaša uloga web-mjesto, kao i broj instanci uloge koje su pokrenute vaša uloga obrada narudžbe. Nakon blagdane, možda ćete dobiti mnogo vraćeni, proizvodi tako da se može biti potrebno još mnogo instance web-mjesta, ali manje Obrada naloga instance. Tijekom ostatak godinu samo trebali nekoliko web-mjesta i Obrada naloga instance. Kroz sve to, morat ćete samo jedna instanca Generator izvješća. Fleksibilnost na temelju uloga implementacije u Azure omogućuje vam da jednostavno prilagoditi aplikaciju poslovne potrebe.

Zajednička uloga međusobno komunicirati instance unutar vaše servis je. Na primjer, uloga web-mjesto prihvaća je narudžba, ali zatim offloads redoslijed obrade uloga instance obrada narudžbe. Najbolji način za prosljeđivanje rad obrazac jedan skup instanci uloga na drugi skup instanci je pomoću tehnologije za stavljanja nudi Azure, usluga reda čekanja ili redova Bus servisa. Korištenje značajke reda nije od ključne važnosti dio priče ovdje. Red čekanja omogućuje servis za promjenu veličine njegov uloge neovisno dopustite saldo radno opterećenje protiv trošak. Ako se povećava se broj poruka u redu čekanja tijekom vremena, možete proširenja broj Obrada naloga instanci uloge. Ako se broj poruka u redu čekanja smanjuje tijekom vremena, koje možete skalirali broj Obrada naloga uloga instance. Na taj način vam samo platili se instanci potrebne za rukovanje stvarni radno opterećenje.

Red čekanja nudi pouzdanosti. Kada skaliranja dolje broj uloga instanci Obrada naloga Azure odlučuje koje će se instance prekinuti. Možda odlučite da biste prekinuli instance koji se nalazi na sredini obradu reda čekanja poruke. Međutim, jer obrade poruke uspješno ne dovrši, poruku postaje vidljivo ponovno na drugi Obrada naloga uloga instancu koju je odabire i obrađuje ga. Zbog reda čekanja poruka vidljivost poruke su zajamčiti na koncu se obrađuju. Red čekanja i djeluje kao raspoređivača opterećenja učinkovito raspodijeliti njezine poruke na instance sve uloge kojima se traže poruke iz nje.

Uloga instanci web-mjesto možete nadzirati promet ulaze ih te odlučite da biste skalirali broj ih gore ili dolje kao i. Red čekanja omogućuje skalirali broj instanci ulogu web-mjesta neovisno o ulogu instance obrada narudžbe. Ovo je vrlo Napredna te vam nudi mnoštvo. Naravno, ako aplikacija sastoji se od dodatnih uloge, možete dodati dodatne redove kao conduit komunikaciju između njih da bi se pod utjecajem isti skaliranje i troškova prednosti.

## <a id="defandcfg"> </a>Hostira definicija servisa i konfiguracija

Servis za implementaciju u Azure zahtijeva imate datoteka za definiciju servisa i datoteke za konfiguraciju servisa. Obje se datoteke su XML datoteke, a oni omogućuju deklarativno određivanje implementacije mogućnosti za vaš servis. Datoteka za definiciju servisa opisuju sve uloge koje čine na glavnom računalu servisu i kako ih komunikaciju. Konfiguracijska datoteka servisa opisuje broj instanci za svaku ulogu i postavke koje se koriste za konfiguriranje svaku instancu uloge.

## <a id="def"> </a>Datoteku definicije servisa

Kao što je li spomenute u starijim verzijama definiciju servisa (CSDEF) datoteka je XML datoteku koja opisuje različite uloge koje čine dovršeno aplikacije. Potpunu shemu XML datoteke Ovdje možete pronaći: [[http://msdn.microsoft.com/library/windowsazure/ee758711.aspx]].
Datoteka CSDEF sadrži WebRole ili WorkerRole element za svaku ulogu koje želite u aplikaciji. Implementacija uloge kao uloge web (pomoću WebRole element) označava hoće li kod funkcionirati na instancu komponente uloga koja sadrži Windows Server 2008 i Internet Information Server (IIS).
Implementacija uloge kao uloge suradnika (pomoću WorkerRole element) znači instancu uloga će Windows Server 2008 na njemu (IIS neće se instalirati).

Certainly možete stvoriti i implementirati ulogu suradnika koji koristi neki drugi mehanizam da biste preslušali za zahtjevi za web (na primjer, kod nije moguće stvoriti i koristiti .NET HttpListener). Budući da instance uloga sve sustav Windows Server 2008, kod možete izvršiti sve operacije koje su obično dostupne aplikacije koje se izvode u sustavu Windows Server
2008.

Za svaku ulogu ukazujete željene veličine VM koje treba koristiti instance te uloge. U sljedećoj tablici prikazane raznih veličina VM danas dostupne i atribute svakog:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**Veličina VM**

</td>
<td>
**PROCESOR**

</td>
<td>
**RAM-A**

</td>
<td>
**Na disku**

</td>
<td>
**Vršna mrežom/i**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Dodatni male**

</td>
<td>
1 x 1.0 GHz

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Small**

</td>
<td>
1 x 1.6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Srednje**

</td>
<td>
2 x 1.6 GHz

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Veliki**

</td>
<td>
4 x 1.6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Vrlo veliki**

</td>
<td>
8 x 1.6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 MB/s

</td>
</tr>
</tbody>
</table>
Vam se naplatiti zaračunava za svaki VM koristite kao instanca ulogu i vam se i naplatiti za sve podatke da se vaša uloga instance slanje izvan podatkovnog centra. Ne naplaćuje za podatke unos podatkovnog centra. Dodatne informacije potražite u članku [Azure cijena] []. Općenito govoreći, preporučuje se da koristite mnogo instanci small uloga umjesto nekoliko velikih instance da bi aplikacija je više prebacuju kvara. Imate manje instance ulogu, više disastrous a neuspješna instalacija u jedan od njih je ukupna aplikacije. Osim toga, kao što je rečeno prije, morate implementirati barem dvije instance za svaku ulogu da biste dobili 99.95% razine usluzi Microsoft pruža.

Datoteku definicije (CSDEF) servis je i gdje želite navesti mnogo atributi o ulogama u aplikaciji. Evo još korisnijim stavki dostupne:

-   **Certifikati**. Pomoću certifikata za šifriranje podataka ili podržava li web-servisa SSL. Bilo koji certifikati morati prenijeti Azure. Dodatne informacije potražite u članku [Upravljanje potvrde u Azure][]. Ta postavka XML instalira potvrde koje ste prethodno prenijeli u spremište certifikata instancu uloga tako da su može koristiti kodu aplikacije.

-   **Konfiguriranje postavki imena**. Za vrijednosti koje želite da vaše aplikacije da biste pročitali pokrenutim uloga instance. Stvarna vrijednost konfiguracijske postavke postavlja se u datoteku servisa konfiguracije (CSCFG) koja se može ažurirati u bilo kojem trenutku ne zahtijeva ponovno implementirate kod. Zapravo, možete kod aplikacija tako da biste otkrili vrijednosti promijenjene konfiguracije bez povećavanja sve isključiti.

-   **Krajnje točke za unos**. Ovdje navedite sve HTTP, HTTPS ili TCP krajnje točke (s priključci) koje želite da biste otkrili vanjske svijeta putem *prefiks*. cloadapp.net URL-a. Kada Azure uvodi vaša uloga, ga konfigurirati vatrozid na instancu uloga automatski.

-   **Interna krajnje točke**. Ovdje navedite sve HTTP-a ili TCP krajnje točke koje želite izložen druge instance uloge koje su uvedene kao dio aplikacije. Interna krajnje točke Dopusti sve instance ulogu aplikacije za razgovor međusobno, ali nisu dostupne sve instance uloge koje su izvan vaše aplikacije.

-   **Uvoz module**. Te po želji instalirajte korisne komponente na vaša uloga instance. Komponente postoje za dijagnostičkih nadzor, udaljene radne površine i Azure povezati (koji omogućuje vašoj instanci ulogu pristup lokalnim izvorima putem sigurnog kanala).

-   **Lokalno spremište**. Time se dodjeljuje poddirektorij na instancu uloge za svoju aplikaciju za korištenje. To je podrobnije opisan u u članku [Ponuda prostora za pohranu podataka u Azure][] .

-   **Pokretanje zadataka**. Pokretanje zadaci dati način da biste instalirali pripremni komponente uloga instanci kao pokrene prema gore. Zadaci koje možete pokrenuti povećane kao administrator potrebi.

## <a id="cfg"> </a>Konfiguracijska datoteka servisa

Konfiguracija (CSCFG) datoteke servisa je XML datoteku koja opisuje postavke koje se mogu mijenjati bez redeploying aplikacije. Potpunu shemu XML datoteke nalazi se ovdje: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
Datoteka CSCFG sadrži element uloge za svaku ulogu u aplikaciji. Evo nekoliko stavki možete odrediti u datoteci CSCFG:

-   **Verzija OS-a**. Taj atribut omogućuje vam da biste odabrali operacijskim sustavima (OS) verziju koju želite koristiti za sve instance uloga radi kodu aplikacije. Ovaj OS poznato je kao *Gost OS*, a svaku novu verziju sadrži najnovija sigurnosna zakrpa i ažuriranja dostupna u trenutku kad je objavio goste OS. Ako je vrijednost atributa osVersion postavljeno na "\*", zatim Azure automatski ažurira gost OS na svakoj instanci uloga kao postaju dostupne nove verzije s operacijskim Sustavom gost. Međutim, možete odabrati iz Automatska ažuriranja tako da odaberete određenu goste verzija OS-a. Na primjer, postavljanje atribut osVersion vrijednosti "WA-GOSTE-OS-2,8\_201109 01" uzrokuje sve svoje uloge instance da biste dobili što je opisano na web-stranica: [http://msdn.microsoft.com/library/hh560567.aspx][]. Dodatne informacije o verzijama OS goste potražite u članku [Upravljanje nadogradnje za goste OS Azure].

-   **Instance**. Taj element vrijednost upućuje na to koliko je instanci ulogu želite dodijeljenu pokrenuti kod za odgovarajućom ulogom. Budući da se na Azure možete prenijeti nove datoteke CSCFG (bez redeploying aplikaciju), je trivially možete jednostavno promijeniti vrijednost za taj element, a zatim Prenesite novu datoteku CSCFG dinamički povećati ili smanjiti broj instanci uloge koje su pokrenute kodu aplikacije. Omogućuje jednostavno skaliranje aplikacija prema gore ili dolje da biste susret stvarni zahtjevima tijekom i kontrola koliko vam se naplatiti za pokrenute instance uloge.

-   **Konfiguriranje postavki vrijednosti**. Taj element označava vrijednosti postavki (kako je definirano u datoteci CSDEF). Vaša uloga mogu čitati te vrijednosti dok je pokrenut. Ove vrijednosti postavki konfiguriranje obično se koriste za nizu za povezivanje s bazom podataka SQL ili Azure prostora za pohranu, ali se može koristiti za bilo koju svrhu po volji.

## <a id="hostedservices"> </a>Stvaranja i implementacija servis

Stvaranje servis zahtijeva da najprije otvorite [Portal za upravljanje Azure] Dodjela servis navođenjem prefiks DNS i podatkovnog centra želite kod koji se izvodi. Zatim u razvojno okruženje stvorite datoteku definicije (CSDEF) servisa, sastavljanje kod aplikacije i sve to datoteka paketa (zip) u datoteku paketa (CSPKG) za servis. Morate pripremiti i (CSCFG) konfiguracijskoj datoteci servisa. Da biste implementirali vaša uloga, prenesite datoteke CSPKG i CSCFG s upravljanja API-JA servisa Azure. Kada implementiran Azure, će dodjele uloga instance u podatkovnom centru (na temelju podataka konfiguracije), izdvojite kodu aplikacije iz paketa, kopirajte instance uloga i pokretanje instance. Sada kod je s radom.

Na slici u nastavku prikazuje CSPKG i CSCFG datoteke koje ste stvorili na računalu razvoj. CSPKG datoteka sadrži datoteku CSDEF i kod za dva uloge. Nakon prijenosa datoteke CSPKG i CSCFG s upravljanja API-JA servisa Azure Azure stvara instance uloga u podatkovnom centru. U ovom primjeru CSCFG datoteke označene Azure potrebno stvoriti tri instance uloge \#1 i dvije instance uloge \#2.

![Slika][5]

Dodatne informacije o implementaciji, Nadogradnja i ponovno konfiguriranje uloge, potražite u članku [Deploying i ažuriranje aplikacije Azure][] .<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Reference

-   [Stvaranje servis za Azure][]

-   [Upravljanje uslugama smještene u Azure][]

-   [Prijenos aplikacije za Azure][]

-   [Konfiguriranje programa Azure][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Napisao neke Richter (Wintellect)</p>

</div>

  [Prednosti modela Azure aplikacije]: #benefits
  [Servis za osnovni koncepti]: #concepts
  [Zahtjevi za servis dizajna]: #considerations
  [Dizajniranje aplikacije skaliranja]: #scale
  [Servis za definiciju i konfiguracija]: #defandcfg
  [Datoteka za definiciju servisa]: #def
  [Konfiguracijska datoteka servisa]: #cfg
  [Stvaranje i implementacija servis]: #hostedservices
  [Reference]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Konfiguriranje prilagođenog naziva domene u Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Ponuda prostora za pohranu podataka u Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Upravljanje certifikatima servisu Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Upravljanje nadogradnje Azure Gosti OS]: http://msdn.microsoft.com/library/ee924680.aspx
  [Portal za upravljanje Azure]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Implementacija i ažuriranje Azure aplikacije]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Stvaranje servis za Azure]: http://msdn.microsoft.com/library/gg432967.aspx
  [Upravljanje uslugama smještene u Azure]: http://msdn.microsoft.com/library/gg433038.aspx
  [Prijenos aplikacije za Azure]: http://msdn.microsoft.com/library/gg186051.aspx
  [Konfiguriranje programa Azure]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
