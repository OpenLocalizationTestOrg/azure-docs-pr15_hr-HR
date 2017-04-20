# <a name="internet-of-things-security-architecture"></a>Internet stvari sigurnosna arhitektura

Kada dizajnirate sustav, važno je razumjeti potencijalnih prijetnji te sustav i sukladno tome, dodajte odgovarajući obranu kao sustav dizajniran i architected. Je osobito važno za dizajniranje proizvoda iz početka s sigurnosti na umu jer su Razumevanje pomaže kako napadaču možda moći ugroziti sustava provjerite je li odgovarajuće mitigations mjestu od početka. 

## <a name="security-starts-with-a-threat-model"></a>Započinje sigurnosna prijetnja modela
 
Microsoft je dugo koristi prijetnja modeli za svoje proizvode i izvršio poduzeća prijetnja modeliranja publically dostupan proces. Tvrtka doživljaja demonstrira na modeliranja ima li neočekivane pogodnosti izvan immediate razumjeti što prijetnji su najčešće vezi. Ako, na primjer, također stvara avenue za otvaranje raspravu s drugima izvan razvojni tim koji može dovesti do novih ideja i poboljšanja proizvoda.
  
Cilj prijetnja modeliranja je razumjeti kako napadaču možda moći ugroziti sustava i provjerite jesu li odgovarajuće mitigations mjestu. Modeliranje prijetnja prisiljava dizajn timu da razmislite o mitigations kao dizajniran sustav umjesto nakon sustav je uveden. Ovaj činjenica je kritično važno jer retrofitting obranu sigurnosti za myriad uređaji u polju je šifriraju, pogreška podložni i ostavlja kupci rizik.

Mnogi timovi razvoj učinite Odličan posao hvatanje funkcionalno preduvjeti za sustav prednost kupcima. Međutim, označavanje koji nisu očite načina netko možda misuse sustav je više izazov. Modeliranje prijetnja može pomoći razumjeti što mogu učiniti napadaču razvojni timovi i zašto. Modeliranje prijetnja je strukturirane proces koji stvara odluke dizajna rasprave o sigurnosti u sustavu, kao i promjene dizajna koje su napravili usput taj učinak sigurnosti. Dok je model prijetnja jednostavno dokumenta, ove dokumentacije predstavlja najbolji način osigurava continuity znanja zadržavanja lekcije naučili i novi pomoći timu se hitro. Konačno, ishod prijetnja modeliranja je omogućiti razmotrite druge aspekte zaštite, na primjer što sigurnost preuzete obveze koje želite dostaviti klijentima. Ove preuzete obveze zajedno s prijetnja modeliranja će obavijestiti i pogona testiranje rješenja Internet stvari (IoT).
 

### <a name="when-to-threat-model"></a>Kada prijetnja modela

[Modeliranje prijetnja](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) nudi Najveća vrijednost ako je umetnut u faze dizajniranja. Kada dizajnirate, imate najveću fleksibilnost za promjene da biste eliminirali prijetnji. Eliminiranja prijetnji po dizajnu je željeni ishod. Je mnogo jednostavnije od dodavanje mitigations, testiranje ih i osiguravanje ostaju trenutni i nadalje, takve eliminacije nije uvijek moguće. Postaje ga sve teže eliminirajte prijetnji kao proizvod postaje više Zrela i Zauzvrat će konačnici zahtijevati više rad i mnogo teže tradeoffs od prijetnja modeliranja rano na razvoja.

### <a name="what-to-threat-model"></a>Što prijetnja modela

Trebali biste niti model rješenja kao cjelinu i također fokus u sljedećim područjima:

- Značajke sigurnosti i privatnosti
- Neuspjesi čije su odgovarajuće sigurnosne značajke
- Značajke koje dodira granicu pouzdanost 

### <a name="who-threat-models"></a>Tko prijetnja modeli

Modeliranje prijetnja je postupak poput drugih.  Je dobro tretirao prijetnja model dokumenta kao bilo koju komponentu rješenja i provjeriti ga. Mnogi timovi razvoj učinite Odličan posao hvatanje funkcionalno preduvjeti za sustav prednost kupcima. Međutim, označavanje koji nisu očite načina netko možda misuse sustav je više izazov. Modeliranje prijetnja može pomoći razumjeti što mogu učiniti napadaču razvojni timovi i zašto.

### <a name="how-to-threat-model"></a>Kako prijetnja modela

Prijetnja modeliranja proces sastoji se od četiri koraka; Koraci su:

- Model aplikacije
- Nabrajanje prijetnji
- Umanjiti prijetnji
- Provjera valjanosti u mitigations

#### <a name="the-process-steps"></a>Korake postupka

Tri pravila opće imati na umu kada se gradi prijetnja modela:

1. Stvoriti dijagram iz reference arhitekture. 
2. Pokrenite breadth prvi. Dobili pregled i razumijevanje sustav kao cjelinu, prije dublje čitanje.  Ovo pomaže osigurati da ste dublje-dive u desni mjesta.
3. Pogon proces, nemojte dopustiti proces koji pogon. Ako u fazi modeliranja pronaći problem i želite ga istraživati, idite na za njega!  Nemoj napipati morati slavishly slijedite ove korake.  

#### <a name="threats"></a>Prijetnji

Četiri osnovni elementi prijetnja model su:

- Procesi (web-usluge, Win32 servise, * nix daemons, itd. Imajte na umu da se neki entiteti složene (npr. polje pristupnici i senzori) možete izvučene kao proces kada je tehničke produbljivanje tih područja nije moguće.
- Pohranjuje podatke (bilo gdje podaci spremljeni, primjerice konfiguracijske datoteke ili baze podataka)
- Toka podataka (gdje podataka premješta između druge elemente u aplikaciji)
- Vanjskim entitetima (ništa u interakciji s sustav, ali nije pod kontrolom aplikacije, Primjeri Uključi korisnike i satelitskog sažetaka sadržaja)

Svi elementi u arhitektonski dijagram su podložne različite prijetnji; koristimo mnemonic KORAK. Čitanje [Prijetnja modeliranja ponovno KORAKA](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) znati više o elementima KORAK.

Različite elemente dijagrama aplikacije utječe neke prijetnje KORAK:

- Procesi su podložne KORAK
- Tokova podataka su podložne TID
- Sprema podatke su podložne TID, a ponekad i R, ako su datoteke zapisnika spremišta podataka.
- Vanjski entiteti su dio SRD

## <a name="security-in-iot"></a>Sigurnosna IoT

Povezani uređaji posebne namjene imate značajnu broj potencijalnih područja plošnog interakcije i interakcije uzorci koje morate smatra da pružaju okvir za optimalnu digitalni access te uređaje. Termin "digitalni access" Ovdje se koristi razlikovati od operacije koje se izvršavaju putem izravne uređaj interakcije gdje odvija access sigurnost pomoću kontrole fizički pristup. Na primjer, stavljanje uređaj u sobi s zaključati na vrata. Dok fizički pristup ne može biti odbijen pomoću softvera i hardvera, mjere se može preuzeti da biste spriječili fizički pristup iz vodeće smetnje sustava. 

Kao možemo istražiti uzorci interakcije smo će pogledajte "uređaj kontrola" i "uređaj podatke" s istom razinom pozornost. "Uređaj control" može klasificirati kao informacije koje ste dobili s uređajem bilo strana s ciljem promjena ili određujući njegovo ponašanje prema njegovo stanje ili stanje okruženju. "Uređaj podataka" može klasificirati kao informacije koje uređaj emits drugih proizvođača o njegovo stanje i opaženih stanje okruženju.
   
Da biste optimizirali Najbolji postupci za sigurnost, preporučuje se da tipične IoT arhitekture biti podijeljen na nekoliko komponenta/zone kao dio prijetnja modeliranja vježbi. Ove zone opisane su u potpunosti cijeloj ovu sekciju i uključuju:

-   Uređaj,
-   Polje pristupnika
-   Oblak pristupnici, i
-   Usluge.

Zone su široki način da biste segmentirali rješenja; svaku zonu često ima vlastitu preduvjeti podataka i provjeru autentičnosti i ovlaštenja. Zone možete se koristiti izolacije oštećenja i ograničavanje utjecaja slaba pouzdanost zone na višu zone sigurnosti.

Svaku zonu odvojene granicu pouzdanost koji zabilježili kao točkasto crvena crta u dijagramu ispod. Predstavlja prijelaza podatke o iz jednog izvora u drugu. Tijekom prijelaza, podataka/informacije mogu biti dio Spoofing, Tampering, repudijacijskih, otkrivanja informacija, uskraćivanjem usluge i povećanje od privilegija (KORAK).

![IoT sigurnosne zone](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Komponente izmišljeni unutar svakog granicu također trpi li KORAK omogućavanja puni 360 prijetnja modeliranja prikaz rješenja. Ispod sekcije razradili na svakom komponente i određenu sigurnosnu nedoumice i rješenja koja treba staviti u mjestu.

Sekcija koja slijedi će raspravljati standardne komponente obično se nalazi na te zone.

### <a name="the-device-zone"></a>Zona uređaja

Okruženje uređaj je odmah fizički prostor oko uređaj gdje je fizički pristup i/ili "lokalne mreže" ravnopravnim digitalni pristup za uređaj nije izvedivo. "Lokalne mreže" pretpostavlja da je mreža koja je distinct i insulated iz – ali potencijalno bridged do – javni Internet i uključuje sve short-range bežičnog primopredajnika tehnologija koja dopušta komunikacije ravnopravnim uređaja. Ona ne *ne* Uključi sve mrežne tehnologije Virtualizacija stvaranje dao privid takve lokalne mreže i ne također uključuje mrežama javne operator koji zahtijevaju dva uređaja komunikaciju preko javna mreža prostora ako su bile unesite komunikacije ravnopravnim odnos.

### <a name="the-field-gateway-zone"></a>Polje zona pristupnika

Polje pristupnik je uređaj/uređaj ili softver računala neke općenite namjene poslužitelja koji djeluje kao enabler komunikacije i, potencijalno, kao uređaj kontrole sustava i koncentrator za obradu podataka uređaja. Pristupnik zonu polje uključuje polje pristupnik samu i sve uređaje koje su na nju priključene. Kao što mu podrazumijeva i naziv, pristupnici polje djeluju pogoni izvan namjenske obrade podataka, obično su mjesto vezana, potencijalno utječe fizičke upada i će ograničena operativne zalihosti. Sve za reći je li polje pristupnik najčešće na stvar jedan možete dodira i sabotage znajući što je svoju funkciju. 

Polje pristupnik razlikuje od jednostavnog promet usmjerivača u tom je prošao aktivna uloga u Upravljanje pristupom i Protok informacija, što znači da je aplikacija adresirana entiteta i mrežne veze ili sesije terminalskog. NAT uređaj ili vatrozid, za razliku od toga, nije klasificirana kao polje pristupnici jer nisu eksplicitno veze ili WBT sesije, ali umjesto na smjera (ili bloka) veze ili sesija napravili kroz njih. Pristupnik polje ima dva različita područja plošnog. Jedan prema uređaje koje su joj priložene i predstavlja unutar zone i druge prema svim stranama vanjski i je rub zone.   

### <a name="the-cloud-gateway-zone"></a>Pristupnik zonu oblaka

Pristupnik oblak je sustav koji omogućuje daljinskoj komunikaciji iz i za uređaje ili pristupnici polje iz nekoliko različitih mjesta preko prostora javna mreža, obično prema oblak temelji kontrolu i sustavu analizu podataka, Federacija takvi sustavi. U nekim slučajevima oblak pristupnik možda odmah olakšali pristup za posebne namjene uređaje iz WBT što tableta ili telefoni. U kontekstu raspravljati ovdje, "oblak" je značilo za upućivanje na namjenske obrade podataka sustava koji je povezan s istog mjesta kao priključene uređaje ili pristupnici polje. Također u zoni oblak operativne mjerama spriječili ciljano fizički pristup i nije nužno izložen Infrastruktura "javne oblak".  

Oblak pristupnik možda potencijalno mapirani u prekriveno Virtualizacija mreže za insulate pristupnik oblaka i sve njegove priključene uređaje ili pristupnici polje iz mrežnog prometa. Pristupnik oblak samu je ni sustava za kontrolu uređaja niti obrade ili objekat spremišta za uređaj podataka; te funkcije sučelja s pristupnika oblaka. Pristupnik zonu oblak uključuje pristupnik oblak samu uz sve pristupnici polje i uređaje koji su izravno ili neizravno pridružen. Rub zonu je distinct područje površine gdje sve vanjske strane komunicirati kroz.

### <a name="the-services-zone"></a>Zona services

"Usluga" definiran za kontekst kao softverska komponenta ili modul koji je interfacing s uređajima putem polje ili oblak pristupnika za zbirku podataka i analize, kao i za naredba i kontrola.  Usluge su mediators. Oni djeluju pod njihov identitet prema pristupnici i druge podsustava, spremanje i analizirati podatke, autonomously problem naredbe na uređaje na temelju podataka uvide ili rasporede i izložiti informacije i kontrolirati sposobnosti ovlaštenog krajnjim korisnicima.

### <a name="information-devices-vs-special-purpose-devices"></a>Informacije o uređajima nasuprot posebne namjene uređaji

PCs, telefoni i tableta su prvenstveno uređaji interaktivne podatke. Telefoni i tableta izričito optimizirana oko maksimiziranja vijek trajanja baterije. Oni Četverostruko veći isključiti djelomično kada nije odmah interakciji osoba ili kada ne pruža usluge poput reprodukcije glazbe ili navođenjem njihovih vlasnika na određeno mjesto. Iz perspektive sustavima, ti uređaji tehnologija informacije su uglavnom ulozi proxyji prema ljudi. Oni su "ljudi actuators" predlaganja akcije i "ljudi senzori" prikupljanje unos. 

Uređaji za posebne namjene, iz senzora jednostavne temperatura retke složene tvornice radnog s tisućama komponente unutar ih, razlikuju. Ti uređaji mnogo više opsegu u svrhu i čak i ako pružaju neki korisničko sučelje, oni velikoj mjeri djelokrugom na interfacing s ili integrirani u imovine u fizičkom svijetu. Oni mjerenje i izvješća okoline okolnostima, uključivanje valves, kontrolu servos, zvuk alarms, prebacite svjetla i mnoge druge zadatke. One pomoći za rad za koji uređaj informacije je previše općenit, preskup, prevelika ili previše brittle. Konkretni svrhu odmah određuje njihov dizajn tehničke kao dobro dostupne novčani proračuna za njihove proizvodnje i operacija zakazano vrijeme trajanja. Kombinacija tih dviju ključni faktori constrains na dostupne operativne proračuna energiju, fizičke footprint i stoga dostupan prostor za pohranu za izračun i sigurnosne mogućnosti.  

Ako nešto "stoji pogrešan" s automatizirani ili daljinski upravljani uređaja, možete, na primjer, fizička oštećenja ili logike kontrole neispravnostima willful neovlaštenog upada i rukovanje. Možda uništava mnogo proizvodnje, zgrade možda looted ili snimiti i osobe može biti injured ili čak die. Ovo je, Naravno, cijela različite klase štetu od nekoga maxing out ukradeno kreditne kartice 's ograničenje. Traka sigurnosti za uređaja provjerite premještanje stvari, kao i za podaci senzora koji naposljetku rezultira naredbi stvari za premještanje, mora biti veće od e-trgovine ili bankarstvo scenarij. 


### <a name="device-control-and-device-data-interactions"></a>Kontrola uređaja i interakcija podataka uređaja

Povezani uređaji posebne namjene imate značajnu broj potencijalnih područja plošnog interakcije i interakcije uzorci koje morate smatra da pružaju okvir za optimalnu digitalni access te uređaje. Termin "digitalni access" Ovdje se koristi razlikovati od operacije koje se izvršavaju putem izravne uređaj interakcije gdje odvija access sigurnost pomoću kontrole fizički pristup. Na primjer, stavljanje uređaj u sobi s zaključati na vrata. Dok fizički pristup ne može biti odbijen pomoću softvera i hardvera, mjere se može preuzeti da biste spriječili fizički pristup iz vodeće smetnje sustava. 
 
Kao možemo istražiti uzorci interakcije smo će pogledajte "uređaj kontrola" i "uređaj podatke" s istom razinom pozornost tijekom modeliranja prijetnja. "Uređaj control" može klasificirati kao informacije koje ste dobili s uređajem bilo strana s ciljem promjena ili određujući njegovo ponašanje prema njegovo stanje ili stanje okruženju. "Uređaj podataka" može klasificirati kao informacije koje uređaj emits drugih proizvođača o njegovo stanje i opaženih stanje okruženju. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Prijetnja modeliranja referenca arhitektura Azure IoT

Microsoft koristi framework strukturiranih iznad prijetnja modeliranja Azure IoT. U sekciji ispod stoga koristimo konkretni primjer Azure IoT referenca arhitekture za demonstraciju kako razmislite o prijetnja modeliranja IoT i kako adresa prepoznaju prijetnje. U slučaju našim smo identificirani četiri glavna područja žarište:

-   Uređaji i izvore podataka
-   Prijenos podataka
-   Uređaj i obrade događaja i
-   Prezentacije

![Prijetnja modeliranja Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Donji dijagram pruža pojednostavljeni prikaz tvrtke Microsoft IoT arhitekture pomoću modela dijagram toka podataka koji koristi Microsoft prijetnja modeliranja alat:

![Prijetnja modeliranja Azure IoT pomoću alata modeliranja prijetnja MS](media/iot-security-architecture/iot-security-architecture-fig3.png)

Važno je da Zapamtite da s arhitekturom razdvaja sposobnosti uređaja i pristupnik. To omogućuje korisniku da uravnotežavanje pristupnik uređaje koji su sigurnije: su sposobnost komunikacije s korištenjem sigurne protokole pristupnika oblak koji obično zahtijeva veći obrada Indirektni troškovi koji nije native uređaj - poput thermostat - pružaju na vlastitu. U zoni Azure services smo pretpostavljaju pristupnik oblak je predstavljen servis koncentrator Azure IoT.

### <a name="device-and-data-sourcesdata-transport"></a>Uređaj i podataka prijevoza izvori podataka

Ova sekcija istražuje arhitektura strukturiranih iznad kroz leće prijetnja Modeliranje i daje pregled kako možemo su addressing neke od postojećih nedoumice. Možemo će usredotočiti na osnovni elementi prijetnja modela:

- Procesi (one pod naše kontrolu i vanjske stavke)
- Komunikacije (naziva se i tokova podataka)
- Pohrana (naziva se i pohranjuje podatke)

#### <a name="processes"></a>Procesi

U svakoj od kategorija strukturiranih arhitektura Azure IoT ćemo pokušati umanjiti broj različitih prijetnji kroz različite faze podatke o postoji u: proces, komunikacije i pohranu. Ispod smo daju pregled najčešćih za kategoriju "postupak" nakon čega slijedi pregled kako ih može biti najbolje mitigated: 

**Lažno predstavljanje (S)**: napadaču može izdvojiti materijal ključa šifriranja s uređaja ili na razini softver ili hardver i naknadno je izvršena pristup sustavu s drugi fizički ili virtualni uređaj pod identitet uređaj materijal ključa iz. Ilustracija dobar je daljinskog kontrole koje možete uključiti bilo TV i koji su alati popularne prankster.

**Uskraćivanje od servisa (D)**: uređaja moguće renderirati prestari radi ili komunikacija Ometanjem radijske frekvencije ili izrezivanje žice. Na primjer, surveillance kamere imala njegov napajanja ili mrežne veze namjerno knocked out će nije izvještaja podataka, uopće.

**Neovlašteno mijenjanje (T)**: napadaču mogu djelomično ili u celini zamijeniti softver koji se izvodi na uređaju, potencijalno dopuštajući zamijenjen softver za uravnotežavanje genuine identitet uređaj ako materijal ključa ili držanjem materijala ključa šifriranja funkcije su dostupne Nedopuštena program. Na primjer, napadaču možda uravnotežavanje izdvojene materijal ključa presretne i izostavi podataka s uređaja na put komunikacije i zamijeniti false podataka ovjerena ukradeno materijal ključa.

**Otkrivanja informacija (kojeg)**: Ako uređaj je pokrenut podešavati softvera, takav softver manipulated nije potencijalno osipanje podataka neovlašteno stranama. Na primjer, napadaču možda uravnotežavanje izdvojene materijal ubaciti sam u put komunikaciju između uređaja i kontroler ili polje pristupnika ili oblak pristupnik za siphon isključivanje informacije.

**Povećavanjem od privilegija (E)**: uređaj koji ne određenu funkciju možete prisilno učiniti nešto drugo. Ako, na primjer, ventil koji programirani da biste otvorili pola način možete tricked otvorite sve način.

| **Komponenta** | **Prijetnja** | **Ublažiti**                                                                                                                                                | **Rizik**                                                                                                                                                                                                    | **Implementacija**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uređaj        | S          | Dodjeljivanje identiteta na uređaj i provjeravanje autentičnosti uređaja                                                                                                | Zamjena uređaj ili dio uređaj s neki drugi uređaj. Kako bismo znali smo razgovor desno uređaj?                                                                                           | Provjeravanje autentičnosti uređaja pomoću prijevoza sloj sigurnosti (TLS) ili IPSec. Infrastruktura treba podržava korištenje zajednički ključ (PSK) na tim uređajima ne može rukovati puno asimetrična kriptografija. Podelite Azure AD, [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt)                             |
|               | TRID       | Primijeniti mehanizme tamperproof uređaj na primjer radeći ga vrlo teško nemoguće izdvojiti ključeva i druge materijale kriptografske iz uređaja. | Rizik je ako netko je neovlašteno mijenjanje uređaj (fizičke smetnje). Kako ćemo su sigurni, taj uređaj nije neovlašteno.                                                                                 | Najučinkovitiji ublažiti je mogućnost pouzdanih platform module (TPM) koji omogućuje pohranjivanje ključeva u posebne na čip strujni krugovi iz kojeg tipke ne može čitati, ali može se koristiti samo za kriptografske postupke koji koristite ključ, ali nikada razotkriti ključ. Šifriranje memorije uređaja. Upravljanje ključevima za uređaj. Potpisivanje šifru. |
|               | E          | Imate kontrolu pristupa uređaja. Shema autorizacije.                                                                                                    | Ako uređaj dopušta za pojedinačne akcija koje se provode na temelju naredbe iz izvan izvor ili čak ugrožena senzori, on će dopustiti napada za izvođenje operacija nije drugačije pristupačniji. | Shema autorizacije uređaja za potrebe                                                                                                                                                                                                                                                                                                             |
| Polje pristupnika | S          | Provjeravanje autentičnosti polje pristupnik za pristupnik oblaka (cert temelji, PSK, zahtjeva temelji,...)                                                                           | Ako netko možete lažiraju pristupnik polje, zatim ga može prikazati kao bilo koji uređaj.                                                                                                                               | RSA/PSK, IPSe, TLS [RFC 4279](https://tools.ietf.org/html/rfc4279). Sva ključa nedoumice pohrana i attestation uređaja u Općenito – slučaju najbolje je koristiti TPM. 6LowPAN nastavak za IPSec podržava mrežama bežična senzor (WSN).                                                                                                              |
|               | TRID       | Zaštiti pristupnik polje protiv neovlašteno mijenjanje (TPM)?                                                                                                            | Lažno predstavljanje napada od misle pristupnik oblak razgovor pristupnik polje može rezultirati otkrivanja informacija i neovlašteno mijenjanje podataka                                                             | Memorije šifriranje, TPM's, provjeru autentičnosti.                                                                                                                                                                                                                                                                                                              |
|               | E          | Access control mehanizam za polje pristupnika                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Ovdje su neki primjeri prijetnje u ovoj kategoriji:

Lažno predstavljanje: Napadaču može izdvojiti materijal ključa šifriranja s uređaja, bilo na softver ili hardver razinu i naknadno pristup sustavu s drugi fizički ili virtualni uređaj pod identitet uređaj materijal ključa je izvršena iz.

**Uskraćivanje usluga**: uređaja moguće renderirati prestari radi ili komunikacija Ometanjem radijske frekvencije ili izrezivanje žice. Na primjer, surveillance kamere imala njegov napajanja ili mrežne veze namjerno knocked out će nije izvještaja podataka, uopće.

**Tampering**: napadaču mogu djelomično ili u celini zamijeniti softver koji se izvodi na uređaju, potencijalno dopuštajući zamijenjen softver za uravnotežavanje genuine identitet uređaj ako materijal ključa ili držanjem materijala ključa šifriranja funkcije su dostupne Nedopuštena program.

**Tampering**: surveillance kameru koja prikazuje vidljive spektra slika prazan usputnih nije namjenjenih fotografiju takve usputnih. Dim ili požara senzor nije moguće izvješćivanje netko držanjem svjetlije ispod njega. U svakom slučaju, uređaj možda Tehnički potpuno pouzdan prema sustavu, ali će izvješće podešavati informacije.

**Tampering**: napadaču možda uravnotežavanje izdvojene materijal ključa presretne i izostavi podataka s uređaja na put komunikacije i zamijeniti false podataka ovjerena ukradeno materijal ključa.

**Tampering**: napadaču mogu djelomično ili potpuno zamijeniti softver koji se izvodi na uređaju, potencijalno dopuštajući zamijenjen softver za uravnotežavanje genuine identitet uređaj ako materijal ključa ili držanjem materijala ključa šifriranja funkcije su dostupne Nedopuštena program.
   
**Informacije otkrivanja**: Ako uređaj je pokrenut podešavati softvera, takav softver manipulated nije potencijalno osipanje podataka neovlašteno stranama.

**Informacije otkrivanja**: napadaču možda uravnotežavanje izdvojene materijal ubaciti sam u put komunikaciju između uređaja i kontroler ili polje pristupnika ili oblak pristupnik za siphon isključivanje informacije.

**Uskraćivanje usluga**: uređaj biti isključeno ili uključen u način rada u kojem komunikacija nije moguća (koji je namjerno u mnogim industrijske strojeva).

**Tampering**: uređaj treba rekonfigurirati raditi u stanju Nepoznata kontrole sustava (izvan poznate kalibraciju parametre) i stoga pružanje podataka možete misinterpreted

**Visinu privilegij**: uređaj koji ne određenu funkciju možete prisilno učiniti nešto drugo. Ako, na primjer, ventil koji programirani da biste otvorili pola način možete tricked otvorite sve način.

**Uskraćivanje usluga**: uređaj biti uključen u stanje gdje komunikacija nije moguća.

**Tampering**: uređaj treba rekonfigurirati raditi u stanju Nepoznata kontrole sustava (izvan poznate kalibraciju parametre) i stoga pružanje podataka možete misinterpreted.
 
**Tampering/spoofing/priznavanje**: Ako nije osiguran (koji je rijetko kada slučaj s kontrolama daljinskog potrošača) napadaču možete rukovati stanje uređaja anonimno. Ilustracija dobar je daljinskog kontrole koje možete uključiti bilo TV i koji su alati popularne prankster.

#### <a name="communication"></a>Komunikacije

Prijetnji oko put komunikaciju između uređaja, uređaji i polje pristupnici i pristupni uređaj i oblaka. U tablici dolje ima nekih smjernica oko otvorenih utičnica na uređaje ili VPN:

| **Komponenta**               | **Prijetnja** | **Ublažiti**                                      | **Rizik**                                                                                                      | **Implementacija**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Koncentrator IoT uređaj              | TID        | (D) TLS (PSK/RSA) šifriranje promet             | Eavesdropping ili utječu komunikaciju između uređaja i pristupnika                             | Sigurnost na razinu protokola. Prilagođeni protokole smo potrebno otkriti kako ih zaštititi. U većini slučajeva komunikacije desi iz uređaja s koncentratorom IoT (uređaj inicira vezu).                                                                                                                                                                 |
| Uređaj za uređaj               | TID        | (D) TLS (PSK/RSA) šifriranje promet.            | Čitanje podataka u tranzitu između uređaja. Neovlašteno mijenjanje podataka. Preopterećenje uređaj s nove veze | Sigurnost na razinu protokola (MQTT/AMQP/HTTP/CoAP. Prilagođeni protokole smo potrebno otkriti kako ih zaštititi. Ublažiti prijetnja DoS je ravnopravni uređajima putem pristupnika oblak ili polje i ih imaju samo act kao klijenti prema tom mrežom. U peering može rezultirati direktne veze između ravnopravnih osoba nakon potrebe pristupnika posreduju |
| Entitet vanjski uređaj      | TID        | Jaka uparivanje vanjski entiteta na uređaj | Eavesdropping veze na uređaj. Komunikacija ometaju uređaj                     | Sigurno uparivanje vanjski entiteta s NFC Bluetooth uređaj. Kontroliranje operativne ploči uređaj (fizičke)                                                                                                                                                                                                                                                  |
| Polje pristupnik oblak pristupnika | TID        | TLS (PSK/RSA) šifriranje promet.               | Eavesdropping ili utječu komunikaciju između uređaja i pristupnika                             | Sigurnost na razinu protokola (MQTT/AMQP/HTTP/CoAP). Prilagođeni protokole smo potrebno otkriti kako ih zaštititi.                                                                                                                                                                                                                                                       |
| Pristupni uređaj oblaka        | TID        | TLS (PSK/RSA) šifriranje promet.               | Eavesdropping ili utječu komunikaciju između uređaja i pristupnika                             | Sigurnost na razinu protokola (MQTT/AMQP/HTTP/CoAP). Prilagođeni protokole smo potrebno otkriti kako ih zaštititi.                                                                                                                                                                                                                                                       |

Ovdje su neki primjeri prijetnje u ovoj kategoriji:

**Uskraćivanje usluga**: ograničenog uređaji su uglavnom pod DoS prijetnja kada aktivno slušanje za unutarnje veze ili neželjene datagrama na mreži, jer možete otvoriti napadaču mnogo veze paralelno i neće servisa ih ili ih za servis vrlo sporo ili uređaj može biti flooded s neželjene promet. U oba slučaja uređaja možete učinkovito postati neupotrebljivo na mreži.

**Spoofing, informacije otkrivanja**: ograničenog uređajima i uređajima za posebne namjene često imaju sigurnosne jedan za sve funkcije poput lozinku ili PIN zaštite ili celini oslanjaju na vjerovanje mreže, što znači da će oni odobriti pristup informacije kada je uređaj na istoj mreži i tu mrežu često samo zaštićen je zajednički ključ. To znači da kad dijeljena tajna uređaj ili mreži je podaci se otkrivaju, moguće je kontrolirati uređaj ili se pridržavajte podataka čuje iz uređaja.  

**Spoofing**: napadaču možda presretne ili djelomično nadjačati emitiranje i lažiraju pošiljaoc (čovjek u sredini)

**Tampering**: napadaču možda presretne ili djelomično nadjačati emitiranje i poslati informacije false 

**Otkrivanja informacija:** napadaču možda eavesdrop emitiranje i dobiti informacije bez autorizacije **uskraćivanje usluga:** napadaču možda jam emitiranja signala i uskraćivanje raspodjelu informacija

#### <a name="storage"></a>Pohrana

Svaki uređaj i polje pristupnik ima neke obrasca pohranu (Privremene za stavljanje podataka, pohrana slika os).

| **Komponenta**                            | **Prijetnja** | **Ublažiti**                       | **Rizik**                                                                                                                                                                                                                                                                                                                | **Implementacija**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uređaj za pohranu                           | TRID       | Šifriranje pohranu potpisivanja zapisnike | Čitanje podataka iz spremišta (PII podataka) neovlašteno telemetry podataka. Neovlašteno u redu čekanja ili predmemorirani naredbe kontrole podataka. Neovlašteno paketi ažuriranja konfiguracija ili firmver dok predmemorirani ili lokalno u redu čekanja može dovesti do sistemska ili OS komponente se ugrožena                                         | Šifriranje, kod provjere autentičnosti poruke (MAC) ili digitalni potpis. Gdje kontrole pristupa moguće, strong kroz resursa access kontroliraju popise (ACL) ili dozvole. |
| OS uređaja slike                          | TRID       |                                      | Neovlašteno OS / zamjena komponente OS                                                                                                                                                                                                                                                                         | OS sliku, šifriranje potpisane OS particiju samo za čitanje                                                                                                                    |
| Polje pristupnik pohranu (stavljanje podataka) | TRID       | Šifriranje pohranu potpisivanja zapisnike | Čitanje podataka iz spremišta (PII podataka) neovlašteno telemetry podataka neovlašteno u redu čekanja ili predmemorirani naredbe kontrole podataka. Neovlašteno konfiguraciju ili firmver paketi ažuriranja (namijenjene prikazivanju za uređaje ili polje pristupnik) dok predmemorirani ili lokalno u redu čekanja može dovesti do sistemska ili OS komponente se ugrožena | BitLocker                                                                                                                                                              |
| OS pristupnik polje slike                   | TRID       |                                      | Neovlašteno OS / zamjena komponente OS                                                                                                                                                                                                                                                                          | OS sliku, šifriranje potpisane OS particiju samo za čitanje                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Uređaj i događaj zonu pristupnik obrada oblaka

Pristupnik oblak je sustav koji omogućuje daljinskoj komunikaciji iz i za uređaje ili pristupnici polje iz nekoliko različitih mjesta preko prostora javna mreža, obično prema oblak temelji kontrolu i sustavu analizu podataka, Federacija takvi sustavi. U nekim slučajevima oblak pristupnik možda odmah olakšali pristup za posebne namjene uređaje iz WBT što tableta ili telefoni. U kontekstu raspravljati ovdje, "oblak" namijenjena za upućivanje na namjenske obrade podataka sustava koji je povezan s istog mjesta kao priključene uređaje ili polje pristupnici i gdje se spriječiti operativne mjerama ciljane fizički pristup, ali nije nužno za infrastrukturu "javne oblak".  Oblak pristupnik možda potencijalno mapirani u prekriveno Virtualizacija mreže za insulate pristupnik oblaka i sve njegove priključene uređaje ili pristupnici polje iz mrežnog prometa. Pristupnik oblak samu je ni sustava za kontrolu uređaja niti obrade ili objekat spremišta za uređaj podataka; te funkcije sučelja s pristupnika oblaka. Pristupnik zonu oblak uključuje pristupnik oblak samu uz sve pristupnici polje i uređaje koji su izravno ili neizravno pridružen.

Pristupnik oblak je uglavnom prilagođene ugrađeni komad softver koji se izvodi kao servis s izloženi krajnje točke koje polje pristupnik i uređajima povezivanje. Kao takve mora biti dizajnirana uz obraćanje pažnje na sigurnost. Slijedite postupak [SDL](http://www.microsoft.com/sdl) za dizajniranje i sastavljanje ovaj servis. 

#### <a name="services-zone"></a>Zona Services

Kontrola sustava (ili kontroler) je softverskog rješenja sučelja s uređajem, ili polje pristupnika ili oblak pristupnik radi Kontrolisanje jedan ili više uređaja i/ili za prikupljanje i/ili pohraniti i/ili analizu podataka uređaj za prezentaciju ili naknadne kontrolu svrhe. Sustavi kontrole su samo entitete u dosegu ovog rasprave odmah olakšavaju interakciju s ljudima. Izuzetak su posrednu fizičke kontrole površine na uređajima, poput skretnica koji omogućuje osobi da biste isključili uređaj ili promjena drugih svojstava i za koje postoji nema funkcionalnu ekvivalent digitalno može pristupiti. 

Neposredni fizičke kontrole površine su one gdje bilo sortiranje logiku upravljanja constrains funkcija fizičke kontrole plošni takva da ekvivalentnu funkciju možete inicirao daljinski ili izbjegavati unos u sukobu s udaljenom unos – takve intermediated površine kontrole su pojmovno priložene kontrolu lokalni sustav koji leverages podlozi ista funkcija kao drugi sustav daljinskog upravljača koje uređaj može priložiti paralelno. Gornji ugroza oblak računalstvo može pročitati stranicu [Oblak sigurnosne Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) .


## <a name="additional-resources"></a>Dodatni resursi

Pogledajte sljedeće članke dodatne informacije:

- [SDL prijetnja modeliranja alat](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [Arhitektura referenca Microsoft Azure IoT](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
