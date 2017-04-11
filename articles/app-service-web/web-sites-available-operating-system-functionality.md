<properties 
    pageTitle="Funkcija operacijski sustav na aplikacije servisa za Azure" 
    description="Informacije o funkcijama OS dostupne korisnicima web-aplikacije, pozadinski sustav mobilne aplikacije i aplikacije API-JA na aplikacije servisa za Azure" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/01/2016" 
    ms.author="cephalin"/>

# <a name="operating-system-functionality-on-azure-app-service"></a>Funkcija operacijski sustav na aplikacije servisa za Azure #

U ovom se članku opisuje uobičajene funkcionalnost osnovne operacijski sustav koji je dostupan za sve aplikacije koji se izvode na [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714). Ta je funkcija obuhvaća datoteke, mreža i pristup registra i dijagnostičkog zapisnika i događaje. 

<a id="tiers"></a>
## <a name="app-service-plan-tiers"></a>Aplikacije servisa za planiranje razine

Aplikacije servisa za pokreće kupca aplikacije u okruženju domaćinu više klijenta. Aplikacije u uveden u razine **slobodno** i **zajednički se koristi** se izvoditi u radnih procesa na zajedničke virtualnim strojevima kada aplikacija u uveden u **standardni** i razine **Premium** pokrenut na virtual machine(s) namjenski posebno za aplikacije koji je povezan s jednog klijenta.

Budući da aplikacije servisa za podržava objedinjenog skaliranja sučelje između različite razine, konfiguracije sigurnosti provode aplikacije servisa za aplikacije ostaje. Time se osigurava da aplikacija ne iznenada se drugačije ponašaju, ne uspijeva neočekivane načine kada aplikacije servisa za planiranje prebacuje s jednog sloju u drugu.

<a id="developmentframeworks"></a>
## <a name="development-frameworks"></a>Okviri za razvoj

Cijene razine aplikacije servisa za kontrolu količine računalnim (CPU-a, za pohranu na disku, memorije i izlazne mreže) dostupnih resursa za aplikacije. Međutim, breadth framework funkcionalnosti dostupne aplikacije ostaje na isti način bez obzira na to skaliranja razine.

Aplikacije servisa za podržava brojne razvoj okviri, uključujući ASP.NET, klasični ASP, node.js, PHP i Python - koja pokreće kao proširenja unutar IIS. Da biste pojednostavnili i normalizirati konfiguracija sigurnosti, aplikacije servisa za aplikacije obično koristiti različite razvoj okviri s zadane postavke. Jedan pristup za konfiguriranje aplikacije nije moguće je da biste prilagodili API površina i funkcije za svaki framework pojedinačne razvoj. Aplikacije servisa za umjesto vodi više generički pristup omogućivanjem uobičajenih osnovne funkcionalnosti operacijski sustav bez obzira na to framework razvoj aplikacije programa.

U sljedećim se odjeljcima sažetak Općenito vrste operacijski sustav funkcionalnosti dostupne aplikacije servisa za aplikacije.

<a id="FileAccess"></a>
##<a name="file-access"></a>Pristup datotekama

Postoje različite pogona aplikacije servisa, uključujući lokalnog pogona i mrežnih jedinica.

<a id="LocalDrives"></a>
### <a name="local-drives"></a>Lokalni pogoni

Na njegov core aplikacija je usluga izvodi pri vrhu infrastruktura za Azure PaaS (platforme kao service). Zbog toga lokalnog pogona priložene "" virtualnog računala su iste vrste pogon dostupne sve ulogu suradnika izvodi u Azure. To obuhvaća pogon operacijskog sustava (D:\ pogon), na pogonu aplikacije koja sadrži paketa Azure cspkg datoteke koristi isključivo aplikacije servisa (i ne može pristupiti klijentima) i "korisnik" pogon (C:\ pogon), čiju veličinu razlikuje se ovisno o veličini u VM.

<a id="NetworkDrives"></a>
### <a name="network-drives-aka-unc-shares"></a>Mrežni pogoni (ili UNC dijeli)

Jedna od jedinstveni aspekte aplikacije servisa za koji vam se čini implementaciju aplikacije i održavanje jednostavne je sav sadržaj korisnika pohranjen na skupu zajedničkih UNC. Ovaj model mapira vrlo dobro izgleda uobičajena uzorak sadržaja prostora za pohranu koriste web-lokalnog hosting okruženja koji imaju više uravnoteženja poslužitelja. 

Unutar aplikacije servisa postoje broj zajedničkih UNC stvorene u svakom podatkovnog centra. Dodijeljeno postotak sadržaja korisnika za sve korisnike u svakom podatkovnog centra za svaki UNC zajedničko korištenje. Osim toga, sve datoteke sadržaja za pretplatu jednog klijenta uvijek smještaju na istom UNC zajedničko korištenje. 

Imajte na umu da zbog kako oblaka services rad, određene virtualnog računala odgovoran za hostiranje zajednički UNC promijenit će se tijekom vremena. To je zajamčiti da zajedničkih UNC biti postavljena tako da drugi virtualnim strojevima tijekom unošenja gore i dolje tijekom operacije oblaka normalno. Zbog toga aplikacije nikad ne morate biti programiranih pretpostavke da računalo informacije u UNC puta datoteke će ostati stabilan tijekom vremena. Umjesto toga treba koristiti s praktičan *faux* apsolutni put **D:\home\site** koja omogućuje aplikacije servisa. Faux apsolutni put sadrži prijenosni uređaj, aplikacija – i – korisnika – agnostic način za koje upućuju na nešto na vlastitu aplikacije. Pomoću **D:\home\site**nešto možete prenijeti zajedničke datoteke iz aplikacije za aplikacije bez potrebe za konfiguriranje nove apsolutni put za svaki prijenos.

<a id="TypesOfFileAccess"></a>
### <a name="types-of-file-access-granted-to-an-app"></a>Vrste pristup datoteci dodijeljene aplikacije

Pretplate za svakoga od njih ima strukturu rezervirane direktorija na određene UNC zajedničko korištenje unutar podatkovnog centra. Klijent može imati više aplikacije stvorene u Centar za određene podatke da bi se sve direktorija pripadaju pretplatu jednog klijenta se stvaraju na isti UNC zajedničko korištenje. Zajedničko korištenje može obuhvaćati direktorija kao što su oni za sadržaj, pogreške i dijagnostičke zapisnike i starijim verzijama aplikacije stvorio izvor kontrole. Kako treba, direktorija klijenta aplikacija dostupni su za čitanje i pristup za pisanje prilikom izvođenja šifrom aplikaciju u aplikacije.

S lokalnog pogona priložiti virtualnog računala koja se pokreće aplikaciju, aplikacije servisa za rezervira bloka prostora na disku C:\ za specifične za aplikaciju privremene lokalno spremište. Iako aplikacije ima pristup cijelog čitanje/pisanje svoju privremenu lokalno spremište, taj prostor za pohranu zaista nije namijenjen koriste izravno kod aplikacije. Umjesto toga namjera je omogućuje pohranu privremenih datoteka za IIS i web-aplikacije okviri. Aplikacije servisa za ograničenjima i privremene lokalne prostora za pohranu za svaku aplikaciju da biste spriječili troše viškom količine prostora za pohranu lokalne datoteke pojedinačne aplikacije.

Dva primjera kako koristi aplikacije servisa za privremene lokalno spremište su direktorija za privremene datoteke ASP.NET i traži IIS u direktoriju komprimirane datoteke. Sastavljanje sustava ASP.NET koristi imenika "Privremenih datoteka platforme ASP.NET" kao predmemoriju za privremene sastavljanje. IIS koristi imenika "IIS privremene sažete datoteke" za pohranu izlaz Komprimirana odgovor. Od ove vrste datoteka korištenje (kao i drugi) su neispravno u aplikacije servisa u samoj aplikaciji privremene lokalno spremište. U ovom remapping osigurava da funkcija nastavlja na očekivani način.

Svaku aplikaciju u aplikacije servisa za izvodi kao identitet procesa slučajni jedinstvene najniža povlaštene tempiranja, pod nazivom "identiteta grupe aplikacija", dodatno ovdje opisuje: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Kod aplikacija koristi ovaj identitet za osnovni pristup samo za čitanje na pogon operacijski sustav (D:\ pogon). To znači da kod aplikaciju možete popis uobičajenih direktorija strukture i čitati zajedničke datoteke na pogon operacijskog sustava. Iako to može prikazati se donekle široku razinu pristupa, iste mape i datoteke su dostupne kada ste Dodjela uloga suradnika u programa Azure hostira servisa i pročitati pogon sadržaj. 

<a name="multipleinstances"></a>
### <a name="file-access-across-multiple-instances"></a>Pristup datotekama preko više instanci

Kućni directory sadrži sadržaj aplikacije programa, a kod aplikaciju možete pisati u nju. Ako aplikaciju izvodi na više instanci, kućni direktorija razdijeliti se sve instance tako da se sve instance potražite u članku isti direktorija. Stoga, na primjer, ako aplikaciju sprema prenesenih datoteka osnovne mape, te datoteke dostupnih odmah za sve instance. 

<a id="NetworkAccess"></a>
## <a name="network-access"></a>Pristup mreži
Aplikacija kod možete koristiti TCP/IP i UDP temelji protokoli da bi izlazni mrežne veze na Internet pristupačnih krajnje točke koji izlažu vanjske servise. Aplikacije možete koristiti te iste protokoli za povezuju sa servisima unutar Azure i #151, na primjer uspostavljanjem HTTP veza s bazom podataka SQL.

Postoji ograničeni mogućnost za aplikacije da biste ostvarili jedna lokalne povratna veza i imaju aplikacije osluškuju te socket lokalne povratna. Ta značajka postoji prvenstveno da biste omogućili aplikacije koje osluškuju lokalne povratna sockets kao dio njihove funkcije. Imajte na umu da svaku aplikaciju vidi "Privatno" povratna veza; aplikacija "A" nije moguće preslušavanje lokalne povratna socket postavio aplikacije "B".

Imenovanih kanala podržani su i kao mehanizam za inter-procesa komunikacije (IPC) između različitih procesa skupno pokrenite aplikaciju. Ako, na primjer, modul IIS FastCGI ovisi o imenovanih kanala koordiniranje pojedinačnih procesa koji se izvode i stranica.


<a id="Code"></a>
## <a name="code-execution-processes-and-memory"></a>Izvršavanje koda, procesa i memorije
Kao što je naznačeno ranije, aplikacija će se pokrenuti unutar najniža povlaštene radnih procesa koji se pomoću programa slučajni identiteta grupe aplikacija. Kod aplikacije ima pristup memorijski prostor na pridružene radnog procesa, kao i sve podređene procese koji mogu spawned CGI procesa ili drugih aplikacija. Međutim, jedan aplikacija ne može pristupiti memorije ili podatke o nekoj drugoj aplikaciji čak i ako se nalazi na istom virtualnog računala.

Aplikacije mogu se izvoditi skripte ili stranice namijenjene podržanih web razvoj okviri. Aplikacije servisa ne konfigurirati postavke framework web da biste ograničeniji načina. Ako, na primjer, aplikacije ASP.NET sustavom aplikacije servisa za pokrenite "cjelovit" upravljani umjesto ograničeniji pouzdanost način. Okviri za web, uključujući i klasični ASP i ASP.NET, da biste uputili poziv u tijeku COM komponente (ali ne za vrijeme odsutnosti postupak COM komponente) kao što je ADO (ActiveX objekte podataka) registrirane po zadanom u operacijskom sustavu Windows.

Aplikacije možete pokrenuti i pokrenuti proizvoljne kod. Nije dopuštena za radnje kao što su varijacije naredbe ljuske ili pokrenuti skriptu PowerShell aplikacije. Međutim, čak i ako se proizvoljne kod i procesa možete spawned iz aplikacije, izvršne datoteke programa i skripti su i dalje ograničeni ovlasti dodijeljene nadređenog aplikacija. Ako, na primjer, aplikaciju možete rezultiraju izvršne datoteke koji vam se čini izlaznog HTTP poziva, ali da isti izvršna ne može pokušati prekidanje veze IP adresu virtualnog računala iz njegova NIC. Uputite poziv je izlazni mrežni je dopušteno najniža povlaštene kod, ali pokušaja konfigurirajte postavke mreže na virtualnog računala zahtijeva administratorske ovlasti.


<a id="Diagnostics"></a>
## <a name="diagnostics-logs-and-events"></a>Dijagnostički Zapisnici i događaje
Informacije u zapisniku je drugi skup podataka koji se neke se aplikacije pokušaju pristupiti. Vrsta zapisnika informacije dostupne na kod koji se izvodi u aplikacije servisa za obuhvaća dijagnostičkih i prijavite se podaci nastali aplikaciju koja je pristupačne aplikaciju. 

Na primjer, W3C HTTP zapisnika generiranih aktivne aplikacije su dostupne na direktorij zapisnika mrežno mjesto zajedničko korištenje za aplikaciju, ili stvorili dostupne u spremište blobova platforme ako klijenta postavljen W3C zapisivanje za pohranu. Mogućnost potonjem omogućuje velike količine zapisnike da biste se prikupili bez rizik od premašuju ograničenja prostora za pohranu datoteka povezana sa zajedničkog mrežnog resursa.

U slične vein, podatke u stvarnom vremenu Dijagnostika iz aplikacija .NET možete također biti prijavljeni pomoću .NET praćenje i infrastruktura za dijagnostiku s mogućnostima pisati praćenje na mreži ili aplikacije, ili na mjesto spremišta blobova platforme.

Područja Dijagnostika bilježenje i praćenje koji nisu dostupni aplikacijama su za sustav Windows ETW događaja i uobičajenih zapisnici sustava Windows događaj (primjerice sustav, aplikacije i sigurnost zapisnika događaja). Budući da ETW prati informacije potencijalno mogu biti vidljivi strojno razini (s pravom ACL-a), čitanje i pisanje pristup ETW događaje blokiraju. Razvojni inženjeri mogli biste primijetiti da se API poziva za čitanje i pisanje ETW događaja i uobičajenih zapisnika događaja sustava Windows čini rad, ali to je zato što aplikacije servisa za je "faking" pozivi da bi se prikazale uspjeti. Stvari, kod aplikacije ima pristup podaci o događaju.

<a id="RegistryAccess"></a>
## <a name="registry-access"></a>Pristup registra
Aplikacija pristupiti samo za čitanje mogući (ali ne i svih) registra virtualnog računala na kojima rade. U praksi, to znači da ključevima registra koji omogućuju pristup samo za čitanje lokalne grupe korisnika mogu pristupiti samo aplikacije. Jedno područje registra koji trenutno nije podržano za čitanje ili pristup za pisanje je na HKEY\_trenutni\_grozd korisnika.

Pisanje pristup registar blokiran, uključujući pristup nijedan ključ registra po korisniku. Iz perspektive aplikacije, pristup zapisivanju registar treba nikad se relied nakon u okruženje za Azure jer aplikacija možete (i učinite) se prenijeti na različitim virtualnim računalima. Samo stalnih snimanje prostora za pohranu koji možete namijenjen na po aplikacije je struktura imenik sa sadržajem samoj aplikaciji pohranjene na zajedničko korištenje aplikacije servisa UNC 

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 
