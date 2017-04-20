# <a name="internet-of-things-security-from-the-ground-up"></a>Internet stvari sigurnost od dna prema gore

Internet od stvari (IoT) predstavlja jedinstveni sigurnosti, privatnosti i usklađenosti izazove u tvrtkama širom svijeta. Za razliku od tehnologija tradicionalni cyber gdje ti problemi Vrtnja oko oko softvera i kako je implementirana, IoT odnosi što se događa u cyber i worlds fizičke konvergira. Zaštita IoT rješenja zahtijeva osiguravanje sigurne dodjeljivanje uređaji sigurne veze između tih uređaja i oblaka i zaštita sigurne podataka u oblak tijekom obrade i pohranu. Natječete takve funkcionalnost, međutim, su resursa ograničeno uređajima, zemljopisno raspodjele uvođenja i velik broj uređaja unutar rješenja.

Ovaj članak istražuje kako programski paket IoT Azure Microsoft nudi rješenje oblak Internet stvari sigurne i privatne. IoT programski Azure isporučuje dovršeno end-to-end rješenje s sigurnosti ugrađen u svakoj fazi od dna prema gore. U tvrtki Microsoft, razvijanje sigurne softver je dio vježbe engineering softver s korijenom u našu desetljeća dugo iskustvo od razvijanju sigurne softvera. Da biste osigurali, sigurnost razvoj životni ciklus (SDL) je methodology foundational razvoj coupled domaćin sigurnost infrastrukture razina usluge operativne sigurnost kontrolu (OSA) i Microsoft digitalni Crimes jedinica, Microsoft Security Response Center i Microsoft Malware Protection Center. 

IoT programski Azure nudi jedinstveni značajke koje Napravi dodjele resursa, povezivanje i pohranjivanje podataka iz IoT uređaja prozirnim i lako i većinu sve, sigurne. U ovom papira smo pregledajte sigurnosne značajke paketa Azure IoT i adresirana Strategije implementacije za provjeru sigurnosti, privatnosti i usklađenosti izazove. 

## <a name="introduction"></a>Uvod

Internet od stvari (IoT) je Val budućnosti nudi preduzeća immediate i prilike stvarnom svijetu smanjiti troškove, povećanje prihoda i pretvorbu svoje poslovne. Mnoge se tvrtke su, međutim, nesigurnom uvođenje IoT njihove organizacije zbog nedoumice o sigurnosti, privatnosti i usklađenosti. Glavna točka zabrinjavajuće dolazi iz jedinstvenosti Infrastruktura IoT kojima spajanja cyber i fizičke worlds zajedno, ukamaćivanja pojedinačne rizike ugrađeno u ove produktivnijima. Sigurnosna IoT odnosi Osiguravanje integriteta kod koji se izvodi na uređajima, davanja uređaj i korisničke provjere autentičnosti, definiranje Očisti vlasništvo nad uređaji (kao i podaci generira te uređaje) i koji se resilient cyber i fizičke napada. 

Zatim, postoji problem privatnosti. Poduzeća želite prozirnost vezane uz prikupljanje podataka, kao što se prikupljaju i zašto, tko vidi ga, koji kontrolira pristup i tako dalje. Konačno, postoje problemi Općenito sigurnost opreme s ljudima operacijski ih i problemi održavanja industrijske standardima usklađenosti.

Dani sigurnosti, privatnosti, prozirnost i usklađenosti nedoumice odabirom desno davatelju rješenja IoT ostaje zahtjevno. Uvodi praznine u sigurnost, privatnosti, prozirnost i usklađenosti koji može biti teško otkriti, let alone popravak Šivanje zajedno pojedinačni dijelovi IoT softvera i usluga pruža različite dobavljače. Izbor desno IoT softvera i servisa davatelja temelji se na pronalaženje davatelji koji imaju opsežne doživljaj izvodi services koji se protežu preko verticals i geographies, ali su moći skalirati sigurne i prozirna način. Slično tome, olakšava za odabranog davatelja imaju desetljeća doživljaj razvijanju sigurne softver koji se izvodi na milijardama strojeva diljem svijeta i imaju mogućnost Hvala pejzaž prijetnja posed ovaj novi svijet Internet stvari.

## <a name="secure-infrastructure-from-the-ground-up"></a>Sigurna Infrastruktura od dna prema gore 

Infrastrukture [Microsoft oblak](https://www.microsoft.com/enterprise/microsoftcloud/default.aspx#fbid=WzBsRQi6aGk) podržava više od jednog milijarde Kupci u državama 127. Crtež na našu desetljeća dugo doživljaj izgradnji enterprise softver i neke od najvećeg online services pokrenut u svijetu, možemo ponuditi više razine poboljšane sigurnosti, privatnosti, usklađenosti i prijetnja ublažiti prakse nego Većina kupaca može postići na svoje vlastite.

Naše [Sigurnost razvoj životni ciklus (SDL)](https://www.microsoft.com/sdl/) pruža obavezna tvrtke wide razvoj proces koji ugrađuje sigurnosnim zahtjevima u životnom ciklusu cijelu softver. Kako biste osigurali operativne aktivnosti slijedite je istu razinu sigurnosti postupci, koristimo stroge sigurnosne smjernice raspoređenog u našem proces operativne sigurnost kontrolu (OSA). Radimo i s rada nadzora neovisnih proizvođača za ovjeru trajnih smo zadovoljavaju naše usklađenosti obveze i možemo sudjelovanje u opširnim raspravama opsežnog sigurnost napora kroz stvaranje centre organizaciji, uključujući Microsoft digitalni Crimes jedinica, Microsoft Security Response Center i Microsoft Malware Protection Center.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure - sigurne IoT infrastruktura za poslovanja

Microsoft Azure nudi rješenje dovršeno oblak jedan koji kombinira neprestano sve veći zbirke oblak integrated services — analytics, stroj učenje, pohranu, sigurnost, umrežavanja i web — s industrijske vodeće obvezujuć zaštitu i privatnost vaših podataka. Naše strategije [pretpostavljaju kršenje](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) koristi namjenski "crveno tima" od Stručnjaci za sigurnost softvera koji simulira napada, testiranje mogućnost od Azure otkriti zaštiti protiv novih prijetnji i oporaviti iz breaches. Našem timu [globalne incident odgovor](https://www.microsoft.com/TrustCenter/Security/DesignOpSecurity) oko clock radi u smanjenju efekti napada i zlonamjerne aktivnosti. Tim slijedi uobičajene postupke za upravljanje incident, komunikacije i oporavak i koristi vidljivim i predvidljive sučelja s partnerima unutarnji i vanjski.

Naš sustav pružaju neprekinuti upada otkrivanje i sprječavanje, sprječavanje napada servisa, testiranje običnog penetration i forensic Alati za pomoć u identificiranju i umanjiti prijetnji. Omogućuje [višestruku provjeru autentičnosti](../articles/multi-factor-authentication/multi-factor-authentication.md) dodatni sloj sigurnost krajnjim korisnicima pristup mreži. I za aplikacije i davatelja glavnog računala nudimo kontrole pristupa, nadzor, zlonamjernih, slabe skeniranje, zakrpe i konfiguraciju upravljanja.

IoT programski paket Microsoft Azure iskorištava sigurnosti i privatnosti ugrađen u Azure platforme uz našu SDL i OSA procesa za sigurnu razvoj i operacija sve Microsoftov softver. Ove postupke pružaju zaštitu Infrastruktura, zaštita mrežnog i identitet i upravljanje značajkama osnova za sigurnost rješenjem. 

[Koncentrator Azure IoT](../articles/iot-hub/iot-hub-what-is-iot-hub.md) unutar [Paketa IoT](../articles/iot-suite/iot-suite-what-is-azure-iot.md) nudi potpuno Upravljani servis koji omogućuje pouzdan i sigurna komunikacija dvosmjerni između IoT uređaja i Azure services poput [Učenje Azure stroj](../articles/machine-learning/machine-learning-what-is-machine-learning.md) i [Azure strujanje Analytics](../articles/stream-analytics/stream-analytics-introduction.md) korištenjem-uređaj sigurnosne vjerodajnice i kontrole pristupa.  

Najbolje komunikaciju značajke sigurnosti i privatnosti ugrađen u Azure IoT Suite ćemo ste podijeljene suite u tri područja primarni sigurnosni. 

![Programski Azure IoT](media/iot-security-ground-up/securing-iot-ground-up-fig2.png)

### <a name="secure-device-provisioning-and-authentication"></a>Dodjeljivanje siguran uređaj i provjera autentičnosti

IoT programski Azure secures uređaja dok su van u polju pružanjem identitet jedinstveni ključ za svaki uređaj koji možete IoT Infrastruktura koristi za komunikaciju s uređajem dok je operacija. Postupak je brz i jednostavan program za postavljanje. Generirani ključ s ID-om korisnika odabrani uređaj čini osnovu token koristi sva komunikacija između uređaja i koncentrator Azure IoT.

ID uređaja mogu biti povezane s uređajem tijekom proizvodnje (tj flashed u modulu pouzdanost hardvera) ili koristite postojeću dugotrajnu identitet kao proxy (npr. CPU serijski brojevi). Budući da promjena ovaj identifikacijski podaci uređaj nije jednostavan, važno je za upoznavanje logičke uređaju ID u slučaju podlozi promjene hardverski uređaj, ali logičkog uređaja ostaje isti. U nekim slučajevima pridruživanja identitet uređaj može dogoditi vrijeme uvođenja uređaj (tj provjerene polje inženjeru fizički konfigurira novi uređaj prilikom komuniciranja s pozadinskom rješenje IoT). [Koncentrator Azure IoT identitet registra](../articles/iot-hub/iot-hub-devguide.md) pruža sigurnu pohranu identitete uređaj i sigurnosnih ključeva za rješenje. Pojedinac ili grupe uređaj identiteti mogu se dodati Dopusti popis ili popis blokova omogućavanja potpunu kontrolu pristupa uređaj.
 
Azure koncentrator IoT pristupa kontrole pravila u oblak omogućuju aktivacije i onemogućivanje bilo koji uređaj identiteta pruža način za razdruživanje uređaj iz IoT uvođenja po potrebi. Pridruživanje i odvajanjem uređaji temelji se na svaki uređaj identitet.

Dodatni uređaj sigurnosne značajke uključuju sljedeće:

- Uređaji prihvatiti neželjene mrežne veze. Oni izlaznog samo način uspostaviti sve veze i smjerova. Za uređaj za primanje naredbu iz pozadinski uređaj mora započeti vezu za provjeru za sve neriješene naredbe za obradu. Uspostavljanja veze između uređaja i koncentrator IoT sigurno messaging iz oblak uređaj i uređaj oblak mogu se slati proziran.  
- Uređaji povezati ili samo uspostaviti smjerova dobro poznati services kojem oni su peered, kao što je koncentrator Azure IoT.
- Autorizacija razinu sustava i provjera autentičnosti koristiti po uređaj identiteta, čime access vjerodajnice i dozvole blizu-odmah revocable.

### <a name="secure-connectivity"></a>Osigurala povezivost 

Rok trajanja messaging je važno značajka IoT rješenja. Potrebe za durably isporuči naredbe i/ili primanja podataka s uređaja podcrtano po činjenica da IoT uređaji povezani putem Interneta ili drugim sličnim mrežama koje mogu biti nepouzdan. Koncentrator Azure IoT nudi rok trajanja messaging između oblaka i uređaji kroz sustav potvrda odgovora porukama. Predmemoriranje poruke u koncentrator IoT za do sedam dana telemetry i dva dana za naredbe postiže se dodatni rok trajanja za razmjenu poruka.
 
Učinkovitost je važno osigurati razgovor resursa i operacija u okruženju resursa ograničeno. HTTPS (HTTP sigurne), standardne industrijske sigurne verziju protokola http popularne podržava Azure IoT koncentrator, omogućavanjem učinkovitu komunikaciju. Napredno protokol red čekanja poruka (AMQP) i poruku čekanja Telemetry prijevoza (MQTT), podržava Azure IoT koncentrator dizajnirane ne samo za učinkovitost pomoću korištenja resursa, ali i isporuka poruka pouzdan. 

Skalabilnost zahtijeva sposobnost sigurno biti uskladiva s širok raspon uređaja. Koncentrator Azure IoT omogućuje sigurne veze s uređajima i IP omogućen i koji nisu IP-omogućen. IP omogućeno uređaji su moći izravno povezati i komunikaciju s koncentratorom IoT putem sigurne veze. Uređaji koji nisu IP-omogućeni su resursa ograničeno i povezati samo putem protokola za komunikaciju kratki udaljenost, Zwave, ZigBee i Bluetooth. Pristupnik polje se koristi za zbrajanje ti uređaji i izvodi protokol prijevod omogućivanje sigurne komunikacije dvosmjerni s oblaka.

Dodatne veze sigurnosne značajke uključuju sljedeće:

- Put komunikaciju između uređaja i Azure IoT koncentrator ili između pristupnici i Azure IoT koncentrator je osigurana korištenjem standardne industrijske prijevoza sloj sigurnosti (TLS) s autentičnost pomoću protokola X.509 Azure IoT koncentrator.
- Uređaji za zaštitu od neželjene unutarnje veze, koncentrator Azure IoT otvorite povezivanje s uređajem. Uređaj inicira sve veze. 
- Koncentrator Azure IoT durably sprema poruke za uređaje i čeka povezivanje uređaja. Ove naredbe pohranjuju za dva dana omogućavanja uređajima povezivanje povremeno, zbog uštede energije ili connectivity nedoumice primanje tih naredbi. Koncentrator Azure IoT održava reda čekanja po uređaj za svaki uređaj.

### <a name="secure-processing-and-storage-in-the-cloud"></a>Sigurna obradu i pohranu u oblaka 

Iz šifrirana komunikacija za obradu podataka u oblak Azure IoT Suite štiti podatke. Pruža fleksibilnost implementirati dodatne šifriranje i upravljanje sigurnosnim ključevima. Pomoću Azure Active Directory (AAD) za provjeru autentičnosti korisnika i autorizacije Azure IoT programski možete pružiti model temelji pravila autorizacije za podatke u oblaka, omogućivanja upravljanja jednostavan pristup koji možete nadzirati i pregledala. Ovaj model omogućuje blizu izravnih opoziva pristup podacima oblaka i uređajima povezanima programskog Azure IoT.

Kada se podaci u oblaka, možete se obrađuju i spremljene u svaki tijek rada korisnički definirana. Pristup svaki dio podataka Kontrolirana s Azure Active Directory, ovisno o servisima za pohranu koristi.
   
Sve tipke koriste Infrastruktura IoT spremljene su u oblak sigurne pohrane s mogućnošću roll u slučaju da ključevi moraju biti ponovno Dodjela resursa. Podatke možete spremiti u [DocumentDB](../articles/documentdb/documentdb-introduction.md) ili u [bazama podataka SQL](../articles/sql-database/sql-database-faq.md), omogućavanjem definicija razina sigurnosti koje želite. Uz to, Azure pruža način za nadzor i nadzora sve pristup podacima za upozorenje o bilo upada ili neovlaštenog pristupa.

## <a name="conclusion"></a>Zaključak

Internet stvari započinje s vaše stvari — stvari koje najčešće važno za tvrtkama. IoT mogu isporučiti Nevjerojatno vrijednost poslovnom prema smanjenju troškova, povećanje prihoda i transformisanje poslovne. Uspjeh ovu transformaciju velikoj mjeri ovisi o odabiru prave IoT softvera i servisa davatelja. To znači da pronalaženje davatelja koji ne samo catalyzes transformacije Razumevanje poslovnim potrebama i preduvjetima, ali također pruža usluge i softver izgrađena s sigurnosti, privatnosti, prozirnost i usklađenosti kao razmatranja glavna dizajna. Microsoft je opsežne doživljaj razvoju i implementaciji sigurne softver i usluge i nastavlja biti vodeći znak u ovu novu dob Internet stvari. 

IoT programski paket Microsoft Azure izgrađuje u sigurnosne mjere po dizajnu omogućivanje sigurne nadzor imovine poboljšanja učinkovitosti pogona operativne performanse da biste omogućili jedna inovacija i uključivanja dodatnih podataka analytics Pretvorba tvrtkama. S njegov slojevitog pristup prema sigurnosti, više sigurnosnih značajki i uzorci dizajna Azure IoT Suite pomaže uvođenje Infrastruktura koje mogu biti pouzdani pretvorbu poslovanja. 

## <a name="additional-information"></a>Dodatne informacije

Svakog Azure IoT paketa rješenja prethodno konfiguriranih stvara instance Azure servisa sljedeću:

- [**Koncentrator Azure IoT**](https://azure.microsoft.com/services/iot-hub/): vaš pristupnik povezuje oblak "stvari". S podrškom za provjeru autentičnosti-uređaj doprinos sigurne vaše rješenje možete skalirati milijunima veze po koncentratora i proces ogromne jedinice podataka.
- [**Azure DocumentDB**](https://azure.microsoft.com/services/documentdb/): usluga skalabilni, potpuno indeksirati baze podataka za polu-strukturirane podatke koji upravlja metapodataka za uređaje koje Dodjela, atribute, konfiguracije i sigurnosna svojstva. DocumentDB nudi visoke performanse i visokom propusnošću obrade, sheme agnostic indeksiranje podataka i obogaćenog sučelje SQL upita.
- [**Azure strujanje Analytics**](https://azure.microsoft.com/services/stream-analytics/): u stvarnom vremenu strujanje obrada oblak omogućuje hitro razviti i uvesti rešenje niska trošak analytics otkriti uvida u stvarnom vremenu s uređaja, senzori, infrastrukturu i aplikacije. Podatke iz potpuno Upravljani servis možete skalirati na bilo koje jedinice dok i dalje postizanje visoke propusnost, niske latencije i resiliency.
- [**Azure App Services**](https://azure.microsoft.com/services/app-service/): oblak platforma za izgradnju snažne web i mobilne apps povezivanje podataka bilo gdje; u oblak ili lokalno. Sastavi zanimljivih apps mobile za iOS, Android i Windows. Integrirati s softver kao usluge (SaaS) i enterprise aplikacije s povezivanjem out-of-the-okvir deseci oblak temelji services i enterprise aplikacije. Kod u omiljene jezik i IDE — .NET, NodeJS, i, Python ili Java — za izgradnju web apps API-ji brže nego ikad.
- [**Logika Apps**](https://azure.microsoft.com/services/app-service/logic/): U logiku Apps značajka usluge App Azure pomaže integrirati IoT rješenje za postojeći redak poslovnih sustava i automatizovali procese tijeka rada. Logika Apps omogućuje programerima dizajna tijekove rada koji se pokreće iz okidač i izvršavanje niz koraka — pravila i akcije koje koristite snažne poveznici za integraciju poslovnih procesa. Logika Apps nudi povezivost out-of-the-okvir za Golema ekosustav SaaS oblak temelji, i lokalno aplikacije.
- [**Azure bloba pohranu**](https://azure.microsoft.com/services/storage/): pouzdan, najekonomičniji oblak spremišta za podatke koji uređajima poslati oblaka.

