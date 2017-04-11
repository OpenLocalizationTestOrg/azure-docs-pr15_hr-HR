Neke pakete mogu instalirati pomoću točaka kada se izvoditi na Azure.  Jednostavno možda je paket nije dostupan na indeksu Python paketa.  Razlog može biti nužan je compiler (u prevoditelj (Compiler) nije dostupan na računalu s instaliranim programom web-aplikaciju u aplikacije servisa za Azure).

U ovom ćete odjeljku smo ćete pregled načina da biste riješili taj problem.

### <a name="request-wheels"></a>Zahtjev za kotače

Ako instalacijski paket potrebno je compiler, pokušajte obratite vlasniku paketa da biste zatražili da kotače biti dostupne paketa.

S nedavne dostupnost [Microsoft Visual C++ Compiler za Python 2.7][]sada je lakše sastavljanje pakete koje imaju izvorni kod za Python 2.7.

### <a name="build-wheels-requires-windows"></a>Sastavljanje kotače (potreban je Windows)

Napomena: Kada koristite tu mogućnost, provjerite je li prikupiti paketa pomoću okruženju Python koja odgovara platformu/arhitektura/verzije koji se koristi na web-aplikaciju u aplikacije servisa za Azure (Windows/32-bit/2.7 ili 3.4).

Ako je paket nije moguće instalirati jer je potrebno je compiler, možete instalirati na compiler na lokalno računalo i sastavljanje kotačić za paket, tada će se uključiti u vašem spremište.

Korisnici Mac i Linux: Ako još nemate pristup stroj Windows potražite u članku [Stvaranje virtualnog računala izvodi Windows][] za stvaranje na VM na Azure.  Možete je koristiti za sastavljanje kotače, njihovo dodavanje u spremištu i ako želite odbaciti na VM. 

Za Python 2.7 instalirate [Microsoft Visual C++ Compiler za Python 2.7][].

Za Python 3.4, možete instalirati [Microsoft Visual C++ 2010 Express][].

Da biste sastavili kotača, potreban vam je paket kotačić:

    env\scripts\pip install wheel

Koristit ćete `pip wheel` prikupiti ovisnosti:

    env\scripts\pip wheel azure==0.8.4

Time ste stvorili .whl datoteke u mapi \wheelhouse.  Dodajte \wheelhouse mapu i kotač datoteke na spremište.

Uređivanje vaše requirements.txt da biste dodali u `--find-links` mogućnost pri vrhu. Ovo govori točaka da biste potražili podudaranja u lokalnoj mapi prije prelaska u indeks python paketa.

    --find-links wheelhouse
    azure==0.8.4

Ako želite uvrstiti sve ovisnosti u mapi \wheelhouse i uopće koristi index paketa python, možete nametnuti točaka da biste zanemarili indeks paketa dodavanjem `--no-index` na vrh vaše requirements.txt.

    --no-index

### <a name="customize-installation"></a>Prilagodba instalacija

Moguće je prilagoditi implementacijsku skriptu da biste instalirali paket virtualne okruženje pomoću zamjenski instalacijskog programa, kao što su jednostavno\_instalirati.  Pogledajte deploy.cmd za primjer u kojem se komentar.  Provjerite nalazi li nisu takvih paketa u requirements.txt da biste spriječili instalacije točaka.

Dodajte to implementacijsku skriptu:

    env\scripts\easy_install somepackage

I moći koristiti jednostavno\_Instaliraj da biste instalaciju exe instalacijski program (neke su poštanski kompatibilnih i tako lako\_Instaliraj podržava ih).  Dodavanje instalacijskog programa sustava spremište i jednostavno pozvati\_instalacija prosljeđivanjem put do datoteke.

Dodajte to implementacijsku skriptu:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Okruženje virtualne obuhvatiti spremište (potreban je Windows)

Napomena: Kada koristite tu mogućnost, provjerite jeste li koristiti okruženje virtualne koja odgovara platformu/arhitekture/verzije koji se koristi na web-aplikaciju u aplikacije servisa za Azure (Windows/32-bit/2.7 ili 3.4).

Uključite li okruženje virtualne u spremištu, možete spriječiti implementacijsku skriptu način upravljanja virtualne okruženje za Azure tako da stvorite praznu datoteku:

    .skipPythonDeployment

Preporučujemo da izbrišete postojeće okruženje virtualne u aplikaciju da biste spriječili ubacivanje datoteke iz kada je okruženje virtualne upravlja automatski.


[Stvaranje virtualnog računala sa sustavom Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler za Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
