<properties 
    pageTitle="Integracija direktorija između Azure višestruke provjere autentičnosti i servisa Active Directory"
    description="To je stranica za provjeru autentičnosti Azure multi-factor koji opisuje kako integrirati provjeru autentičnosti poslužitelja za višestruku Azure Active Directory tako da biste mogli sinkronizirati direktorija."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Integracija direktorija između Azure MFA Server i servisa Active Directory

U odjeljku Integracija direktorija omogućuje vam da biste konfigurirali poslužitelj integraciji servisa Active Directory ili neki drugi LDAP direktorij.  Omogućuje konfiguriranje atribute odgovaraju sheme direktorija i postaviti automatsku sinkronizaciju korisnika.

## <a name="settings"></a>Postavke
Prema zadanim postavkama Azure višestruku provjeru autentičnosti poslužitelja je konfiguriran za uvoz i sinkronizacija korisnika iz servisa Active Directory.  Na kartici omogućuje vam da biste nadjačali zadano ponašanje i povezivanje na neki drugi direktorij LDAP, direktorija SLIKA ili određene kontroler domene servisa Active Directory.  Također nudi za korištenje LDAP provjeru autentičnosti proxyja LDAP ili LDAP vezati kao cilj POLUMJER, stara provjere autentičnosti za provjeru autentičnosti IIS ili primarni provjere autentičnosti korisnika portala.  U sljedećoj tablici opisane pojedinačne postavke.

![Postavke](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Značajka | Opis |
| ------- | ----------- |
| Korištenje servisa Active Directory | Odaberite mogućnost pomoću servisa Active Directory za korištenje servisa Active Directory za uvoz i sinkronizacija.  Ovo je zadana postavka. <br>Napomena: Računalo mora biti pridruženo domenu, a morate biti prijavljeni s računom domene za integraciju servisa Active Directory ispravno funkcionirao. |
| Uključite pouzdanih domena | Potvrdite potvrdni okvir Uključi pouzdanih domena biti agent pokušaj povezivanja s domenama pouzdana trenutnu domenu, drugi domeni ili domena uključeni u skup stabala pouzdanost.  Kada ne uvoza ili sinkroniziranje korisnika iz svih pouzdanih domena, poništite potvrdni okvir da biste poboljšali performanse.  Zadani je označeno. |
| Konfiguracija određene LDAP | Odaberite mogućnost LDAP koristite da biste koristili postavke LDAP navedena za uvoz i sinkronizacija. Napomena: Korištenje LDAP je odabrana korisničkog sučelja mijenja reference iz servisa Active Directory LDAP. |
| Slika gumba | Gumb za uređivanje omogućuje trenutne postavke konfiguracije LDAP izmijeniti. |
| Korištenje atributa opseg upita | Označava hoće li se koristiti atribut opseg upita.  Atribut opseg upita omogućuju učinkovito direktorija pretraživanja koje ispunjavaju uvjete zapisa na temelju stavki u drugi zapis atribut.  Poslužitelj za provjeru autentičnosti Azure multi-factor koristi atribut opseg upita učinkovito upita za korisnike koji su članovi sigurnosne grupe.   <br>Napomena: Postoje neke slučajevi u kojima atribut opseg upita podržane su, ali ne bi trebalo koristiti.  Ako, na primjer, servisa Active Directory može imati probleme s atribut opseg upita kada sigurnosne grupe sadrži članove s više od jedne domene.  U ovom slučaju potvrdni okvir mora biti poništen. |

U sljedećoj tablici opisane LDAP konfiguracijske postavke.

| Značajka | Opis |
| ------- | ----------- |
| Poslužitelj | Unesite naziv glavnog računala ili IP adresu poslužitelja na kojem je LDAP imenika.  Poslužitelj za sigurnosne kopije može biti naveden i odvojene točka-zarez. <br>Napomena: Kada je povezati vrsta SSL, potpuno kvalificirani naziv glavnog računala obično potreban je. |
| Base DN | Unesite Razlikovni naziv objekta osnovni direktorija iz kojeg će se sve upite imenika pokrenuti.  Na primjer, kontroler = abc, kontroler = com. |
| Povezivanje vrsta - upiti | Odaberite vrstu odgovarajuće vezanja za korištenje prilikom povezivanja pretraživanje imenika LDAP.  Koristi se za uvozi, sinkronizacija i razlučivost korisničko ime. <br><br>  Anonimni - provest će se anonimni vezanja.  DN vezanja i lozinku za povezivanje će se koristiti.  Će raditi samo ako LDAP imenik omogućuje povezivanje anonimni i dozvole omogućuju ispitivanje atributa i odgovarajuće zapise.  <br><br> Jednostavno - vezanja DN i lozinku za povezivanje će se proslijediti kao običan tekst za povezivanje s LDAP imenika.  To mogu koristiti samo za testiranje da biste potvrdili da je poslužitelj proslijediti te sadrži li račun vezanja odgovarajući pristup.  Preporučuje se da SSL se koristiti umjesto nakon instalacije odgovarajuće certifikata.  <br><br> SSL - vezanja DN i lozinku za povezivanje bit će šifrirana putem SSL-a za povezivanje s LDAP imenika.  To je certifikata mora instalirati lokalno da LDAP imenik smatra pouzdanima.  <br><br> Windows – vezanja korisničko ime i lozinku za povezivanje će se koristiti za sigurno povezivanje kontroler domene servisa Active Directory ili SLIKA direktorija.  Povezivanje korisničko ime prazna, prijavili na račun korisnika će se koristiti za povezivanje. |
| Povezivanje vrsta - Authentications | Odaberite vrstu odgovarajuće vezanja za korištenje prilikom izvršavanja provjere autentičnosti vezanja LDAP.  U odjeljku vezanja opisi u odjeljku Vrsta vezanja – upita.  Ako, na primjer, time je za anonimni Poveži sa moguće je koristiti za upite vezanja SSL koristi se za sigurnu LDAP vezanja authentications. |
| Vezanja DN ili vezanja korisničko ime | Unesite Razlikovni naziv zapis o korisniku za račun koji želite koristiti prilikom povezivanja LDAP imenika.<br><br>Razlikovni naziv vezanja koristi samo kada je vrsta povezati jednostavnih ili SSL.  <br><br>Unesite korisničko ime računa za Windows za korištenje prilikom povezivanja LDAP imenik kada je povezivanje vrste Windows.  Ako ostavite prazno, prijavili na račun korisnika će se koristiti za povezivanje. |
| Povezivanje lozinke | Unesite lozinku vezanja za povezivanje DN ili korisničko ime koji se koristi za povezivanje s LDAP imenika.  Da biste konfigurirali lozinku za servis za AdSync višestruku provjeru autentičnosti poslužitelja, mora biti omogućena Sinkronizacija i usluge mora biti pokrenut na lokalnom računalu.  Lozinka spremit će se u Windows pohranjena korisnička imena i lozinke u odjeljku višestruke provjere servis za provjeru autentičnosti poslužitelja AdSync se izvodi kao račun.  Lozinku i spremit će se u odjeljcima korisničko sučelje višestruku provjeru autentičnosti poslužitelja koja se izvodi kao račun i servis višestruku provjeru autentičnosti poslužitelja koja se izvodi kao račun.  <br><br> Napomena: Budući da lozinku samo je pohranjena na lokalnom poslužitelju Windows pohranjena korisnička imena i lozinke, ovaj korak morat ćete napraviti na svakom poslužitelju višestruku provjere autentičnosti koje treba pristup lozinku. |
| Ograničenje veličine upita | Navedite ograničenje veličine za maksimalni broj korisnika koji će vratiti pretraživanje direktorija.  To ograničenje moraju se podudarati konfiguraciji na LDAP imenika.  Za velike pretraživanja kojima se ne podržava broja stranica za uvoz i sinkronizacija će pokušati dohvatiti korisnika u grupama.  Ako je ograničenje veličine naveden Evo premašuje ograničenje konfigurirali LDAP imenik, promakli se nekim korisnicima. |
| Gumb za testiranje | Kliknite gumb za testiranje za testiranje povezivanja s poslužiteljem LDAP.  <br><br> Napomena: Korištenje LDAP mogućnost ne moraju biti odabrana da bi se testiranje povezivanja.  Time se omogućuje povezivanje testira prije korištenja LDAP konfiguracije. |

## <a name="filters"></a>Filtri
Filtri omogućuju da biste postavili kriterije da biste potvrdili zapisa pri izvođenju pretraživanja imenika.  Postavljanjem filtra možete ograničiti objekte koje želite sinkronizirati.  

![Filtri](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure višestruka provjera autentičnosti ima sljedeće mogućnosti 3.

- **Spremnik filtar** - navede kriterije filtriranja koristi da biste potvrdili spremnik zapisa pri izvođenju pretraživanja imenika.  Za Active Directory i SLIKA, (| () objectClass=organizationalUnit)(objectClass=container)) se obično koristi.  Za ostale direktorija LDAP kriterija filtra koji ispunjava uvjete za svaku vrstu objekt spremnika želite koristiti ovisno o sheme direktorija.  <br>Napomena: Ako ostavite prazno, ((objectClass=organizationalUnit)(objectClass=container)) koristit će prema zadanim postavkama.

- **Filtar sigurnosne grupe** - navede kriterije filtriranja koristi da biste potvrdili sigurnosne grupe zapisa pri izvođenju pretraživanja imenika.  Za Active Directory i SLIKA, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) se obično koristi.  Za ostale direktorija LDAP kriterija filtra koji ispunjava uvjete za svaku vrstu objekta sigurnosne grupe želite koristiti ovisno o sheme direktorija.  <br>Napomena: Ako ostavite prazno, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) koristit će se po zadanom.

- **Filtar korisnika** – navede kriterije filtriranja koristi da biste potvrdili korisničkih zapisa pri izvođenju pretraživanja imenika.  Za Active Directory i SLIKA, (& (objectClass=user)(objectCategory=person)) se obično koristi.  Za ostale direktorije LDAP (objectClass = inetOrgPerson) ili nešto slično treba koristiti ovisno o sheme direktorija. <br>Napomena: Ako ostavite prazno, (i (po zadanom će se koristiti objectCategory=person)(objectClass=user)).

## <a name="attributes"></a>Atributi
Atributi moguće je prilagoditi prema potrebi za određeni direktorij.  Time da biste dodali prilagođeni atributi i precizno podešavanje sinkronizacije atribute koje su vam potrebne.  Vrijednost za svako polje atributa mora biti naziv atributa kako je definirano u shemi direktorija.  U sljedećoj tablici ispod dodatne informacije.

![Atributi](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Značajka | Opis |
| ------- | ----------- |
| Jedinstveni identifikator | Unesite naziv atributa atributa koji služi kao jedinstveni identifikator spremnik, sigurnosne grupe i korisničkih zapisa.  U servisu Active Directory, to je obično objectGUID.  U drugi LDAP implementacije, možda ćete entryUUID ili nešto slično.  Zadano je objectGUID. |
| ... Gumbi (odaberite atribut) | Svako polje atributa ima "..." gumb pokraj tog prikaza koji će se prikazati dijaloški okvir Odabir atribut dopuštanja atribut odabir s popisa. <br><br>Odaberite dijaloški okvir atribut.<br><br>Napomena: Atributi mogu ručno unijeti i nije potreban za podudaranje atribut na popisu atributa. |
| Jedinstveni identifikator vrsta | Odaberite vrstu atribut Jedinstveni identifikator.  U servisu Active Directory atribut objectGUID je vrsta GUID.  U druge LDAP implementacije, možda će biti vrste ASCII bajt polja ili niz.  Zadano je GUID. <br><br>Napomena: Važno je da biste postavili to pravilno jer stavke za sinkronizaciju su referencirane prema svojim Jedinstveni identifikator i Jedinstveni identifikator vrsta koristi se za izravno pronaći objekt u direktoriju.  Postavljanje niz kada imenika zapravo pohranjuje vrijednost kao raspon bajtova ASCII znakova će onemogućuju sinkroniziranje s radi ispravno. |
| Razlikovni naziv | Unesite naziv atributa atributa koji sadrži Razlikovni naziv za svaki zapis.  U servisu Active Directory, to je obično distinguishedName.  U drugi LDAP implementacije, možda ćete entryDN ili nešto slično.  Zadano je distinguishedName. <br><br>Napomena: Ako atribut koji sadrži samo Razlikovni naziv ne postoji, atribut adspath može se koristiti.  Na "LDAP: / /<server>/" dio puta će automatski uklone isključivanje ostavite samo Razlikovni naziv objekta. |
| Naziv spremnika | Unesite naziv atributa atributa koji sadrži naziv u zapisu kontejner.  Vrijednost toga atributa prikazat će se u hijerarhiji spremnik prilikom uvoza iz servisa Active Directory i dodavanje stavke za sinkronizaciju.  Zadani je naziv. <br><br>Napomena: Ako različiti spremnici koristili različite atribute njihova imena, više spremnik naziv atributa može biti naveden u odvojenih točkom sa zarezom.  Da biste prikazali njegov naziv koristit će prvi atribut naziva spremnik na objekt spremnika. |
| Naziv sigurnosne grupe | Unesite naziv atributa atributa koji sadrži naziv u zapisu sigurnosne grupe.  Vrijednost toga atributa prikazat će se na popisu sigurnosne grupe prilikom uvoza iz servisa Active Directory i dodavanje stavke za sinkronizaciju.  Zadani je naziv. |
| Korisnici | Sljedećim atributima koriste se za traženje, prikaz, uvoz i sinkronizacija korisničkih podataka iz imenika. |
| Korisničko ime | Unesite naziv atributa atributa koji sadrži korisničko ime u zapisu korisnika.  Vrijednost toga atributa bit će upotrijebljen kao korisničko ime za višestruku provjeru autentičnosti poslužitelja.  Drugi atribut može biti navedena kao sigurnosnu kopiju prvom.  Atribut drugi će se koristiti samo ako atribut prvi sadrže vrijednost za korisnika.  Zadane su userPrincipalName i sAMAccountName. |
| Ime | Unesite naziv atributa atributa koji sadrži ime u zapisu korisnika.  Zadano je givenName. |
| Prezime | Unesite naziv atributa atributa koji sadrži ime i prezime u zapisu korisnika.  Zadano je SB. |
| Adresa e-pošte | Unesite naziv atributa atributa koja sadrži adresu e-pošte u zapisu korisnika.  Da biste poslali dobrodošlice i ažuriranje poruke e-pošte korisniku će se koristiti adresu e-pošte.  Zadano je pošta. |
| Grupa korisnika | Unesite naziv atributa atributa koji sadrži grupi korisnika u zapisu korisnika.  Grupa korisnika se može koristiti za filtriranje korisnika u agenta te na izvješća na portalu za upravljanje višestruku provjeru autentičnosti poslužitelja. |
| Opis | Unesite naziv atributa atributa koji sadrži opis u zapisu korisnika.  Opis koristi se samo za pretraživanje.  Zadani je opis. |
| Jezik govorne poziva | Unesite naziv atributa atributa koji sadrži kratki naziv jezik koji želite koristiti za govorne pozive za korisnika. |
| Jezik SMS teksta | Unesite naziv atributa atributa koji sadrži kratki naziv jezik koji želite koristiti za SMS poruke s tekstom za korisnika. |
| Jezik aplikacije telefona | Unesite naziv atributa atributa koji sadrži kratki naziv jezik koji želite koristiti za poruke s tekstom aplikacije telefona za korisnika. |
| OATH tokena jezika | Unesite naziv atributa atributa koji sadrži kratki naziv jezik koji želite koristiti za OATH tokena tekstne poruke za korisnika. |
| Telefoni | Sljedećim atributima koriste se za uvoz i sinkronizacija telefonske brojeve korisnika.  Ako atribut naziv nije naveden za vrste telefona, vrste telefona neće biti dostupne kada uvoz iz servisa Active Directory i dodavanje stavke za sinkronizaciju. |
| Tvrtke | Unesite naziv atributa atributa koji sadrži Poslovni telefonski broj u zapisu korisnika.  Zadano je telephoneNumber. |
| Početna stranica | Unesite naziv atributa atributa koji sadrži broj Kućni telefon u zapisu korisnika.  Zadano je DOM telefon. |
| Dojavljivač | Unesite naziv atributa atributa koji sadrži broj Dojavljivač u zapisu korisnika.  Zadano je Dojavljivač. |
| Mobile | Unesite naziv atributa atributa koji sadrži broj mobilnog telefona u zapisu korisnika.  Zadano je mobilni. |
| Faksa | Unesite naziv atributa atributa koji sadrži broj faksa u zapisu korisnika.  Zadano je facsimileTelephoneNumber. |
| IP telefon | Unesite naziv atributa atributa koji sadrži IP telefonski broj u zapisu korisnika.  Zadano je ipPhone. |
| Prilagođeno | Unesite naziv atributa atributa koji sadrži prilagođeni telefonskog broja u |
|  | zapis o korisniku.  Zadana je vrijednost prazna. |
| Proširenje | Unesite naziv atributa atributa koji sadrži kućni broj telefona u zapisu korisnika.  Vrijednosti polja proširenje bit će upotrijebljen kao nastavka u primarni telefonski broj samo.  Zadana je vrijednost prazna. <br><br>Napomena: Ako atribut proširenje nije naveden, proširenja može biti dio atribut telefona.  Proširenje mora biti ispred "x" tako da možete mogu raščlaniti.  Na primjer 555-123-4567 x890 rezultat 555-123-4567 kao telefonski broj i 890 kao datotečni nastavak. |
| Vraćanje zadanih postavki gumba | Kliknite gumb Vrati zadano da biste se vratili sve atribute njihove zadane vrijednosti.  Zadane postavke treba raditi s normalnom sheme servisa Active Directory ili SLIKA. |

Da biste uredili atribute, jednostavno kliknite gumb za uređivanje na kartici atribute.  To će se srušiti sustava windows koji vam omogućuje uređivanje atribute.

![Uređivanje atributa](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Sinkronizacija
Sinkronizacija zadržava Azure multi-factor baze podataka korisnika sinkronizirati s korisnicima u servisu Active Directory ili neki drugi direktorij Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol.  Postupak je slična ručno uvoz korisnika iz servisa Active Directory, ali povremeno ankete za korisnika servisa Active Directory i promjena sigurnosnih grupa za obradu.  Na raspolaganju su za onemogućivanje ili uklanjanje korisnika uklanja iz spremnik ili sigurnosne grupe i uklanjanje korisnika izbrisane iz servisa Active Directory.

Servis za višestruku provjeru autentičnosti ADSync je servis Windows koji izvršava periodičku ankete od servisa Active Directory.  Time se neće se zbuniti sa sinkronizacijom Azure AD ili Azure AD Connect.  Provjera autentičnosti ADSync multi-factor iako utemeljena na sličan baza kod, je na poslužitelj za Azure višestruke provjere autentičnosti.  Instalirana je u stanju zaustavljanja i pokreće servis višestruku provjeru autentičnosti poslužitelja kada je konfiguriran za pokretanje.  Ako imate konfiguraciju višestruku provjeru autentičnosti poslužitelja više poslužitelja, provjere autentičnosti ADSync multi-factor može izvesti samo na jednom poslužitelju.

Servis multi-factor provjere autentičnosti ADSync koristi nastavak poslužitelj DirSync LDAP pruža Microsoft učinkovito provjeriti ima li promjene.  Ovaj pozivatelja kontrola DirSync mora imati "imenika se promjene" desno i DS-replikacije-Get-promjene proširiti kontrolu pristupa desno.  Prema zadanim postavkama ta prava dodjeljuju Administrator i LocalSystem računa kontrolera domena.  Servis AdSync provjere autentičnosti za višestruku konfiguriran za pokretanje kao LocalSystem prema zadanim postavkama.  Stoga je najjednostavnije pokrenuti servis kontroler domene.  Servis možete pokrenuti kao račun s dozvolama za manje Ako konfigurirate uvijek izvršiti potpune sinkronizacije.  To je manje učinkovita, ali potrebno manje ovlasti za račun.

Ako je konfiguriran za korištenje LDAP i imenik LDAP podržava kontrolu DirSync, zatim provjere za korisnika i promjena sigurnosnih grupa će funkcionira na isti način kao i sa servisa Active Directory.  Ako LDAP imenik ne podržava kontrolu DirSync, zatim potpune sinkronizacije provest će se prilikom svakog ciklusa.

![Sinkronizacija](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Dodatne informacije o svim pojedinačne postavke na kartici Sinkronizacija pomoću tablice ispod.

| Značajka | Opis |
| ------- | ----------- |
| Omogućivanje sinkronizacije sa servisom Active Directory | Kad se potvrdi, servis za višestruku provjeru autentičnosti poslužitelja pokrenut će se povremeno provjeriti servisa Active Directory za promjene. <br><br>Napomena: najmanje jednu stavku sinkronizacije mora biti dodan i na Sinkroniziraj sada moraju izvršiti prije nego provjere autentičnosti višestruku servis pokrenut obrade promjene. |
| Sinkronizacija svakih | Navedite razdoblje servis višestruku provjeru autentičnosti poslužitelja čekati između provjere i obrada promjene. <br><br> Napomena: Interval naveden je vrijeme između na početak svakog ciklusa.  Ako vrijeme obrade promjene premašuje interval, servis će se odmah ponovno ankete. |
| Uklanjanje korisnika više ne u servisu Active Directory | Kad se potvrdi, servis za višestruku provjeru autentičnosti poslužitelja će obraditi tombstones korisnik servisa Active Directory izbrisane i uklanjanje povezanih višestruku provjeru autentičnosti poslužitelja korisnika. |
| Izvrši potpune sinkronizacije | Kad se potvrdi, servis za višestruku provjeru autentičnosti poslužitelja će izvrši potpune sinkronizacije.  Kada je isključen servis za višestruku provjeru autentičnosti poslužitelja izvođenje rastuće sinkronizacije samo upitom korisnicima koji su se promijenile.  Zadano je isključen. <br><br> Napomena: Kada je isključen rastuće sinkronizacije može izvršiti samo kada imenika podržava kontrolu DirSync, a zatim račun koji se koristi za povezivanje s imenika ima odgovarajuće dozvole za izvođenje DirSync rastuće upita.  Ako račun nema potrebne dozvole ili više domena koje su povezane s sinkronizacije, provedite potpuno preporučuje se sinkronizacije. |
| Potrebno odobrenje administratora kada više od X korisnici će biti onemogućena ili ukloniti | Stavke za sinkronizaciju moguće je konfigurirati da biste onemogućili ili uklanjanje korisnika koji više nisu član spremnik stavke ili sigurnosne grupe.  Kao zaštitu, odobrenje administratora mogu biti potrebna kada broj korisnika za onemogućivanje ili uklanjanje premašuje praga.  Kad se potvrdi, bit će potrebno za određeni prag odobrenje.  Zadana vrijednost je 5 i raspon je 1 do 999. <br><br> Odobrenje olakšano je prvi slanjem obavijest e-poštom administratorima. Obavijesti e-pošte daje upute za pregled i odobravanje onemogućavanje i uklanjanje korisnika.  Kada se pokrene korisničko sučelje višestruku provjeru autentičnosti poslužitelja, ponudit će na odobrenje. |

Gumb **Sinkroniziraj sada** omogućuje vam pokretanje potpune sinkronizacije za navedene stavke za sinkronizaciju.  Kad god stavke za sinkronizaciju se dodaju, mijenjati, uklanja ili ponovno naručiti, potreban je potpune sinkronizacije.  Je potrebno prije servis za višestruku provjeru autentičnosti AdSync bit će radu jer postavlja polazište iz kojeg će promjena u koracima ankete servis.  Ako su promjene na sinkronizacije stavke, a nije izvršena potpune sinkronizacije, zatražit će se da biste Sinkroniziraj sada kada se pomičete po drugom sekciju ili prilikom zatvaranja korisničkog sučelja.

Gumb **Ukloni** omogućuje administrator da biste izbrisali jednu ili više stavki za sinkronizaciju s popisa stavki sinkronizacije višestruku provjeru autentičnosti poslužitelja.

>[AZURE.WARNING]Kada se zapis stavku sinkronizacije uklonjen, nije moguće oporaviti. Morat ćete ponovno dodajte zapis za stavku sinkronizacije ako je greškom izbrisali.

Sinkronizacija stavke ili stavki sinkronizacije uklonjeni su iz višestruku provjeru autentičnosti poslužitelja.  Servis za višestruku provjeru autentičnosti poslužitelja obradit će se više ne stavke za sinkronizaciju.

Gumbi Premjesti gore i Premjesti dolje omogućuju administrator da biste promijenili redoslijed stavki sinkronizacije.  Budući da se isti korisnik može biti član više od jedne stavke sinkronizacije (npr. spremnik i sigurnosne grupe), važno je redoslijed.  Postavke primijeniti korisniku tijekom sinkronizacije će potjecati iz prve sinkronizacije stavke na popisu u koji je korisnik pridružen.  Stoga stavke za sinkronizaciju moraju biti poredani prioritet.

>[AZURE.TIP]Nakon uklanjanja stavke za sinkronizaciju treba izvršiti potpune sinkronizacije.  Potpune sinkronizacije treba izvršiti nakon redoslijed stavki sinkronizacije.  Kliknite gumb Sinkroniziraj sada da biste izvršili potpune sinkronizacije.

## <a name="multi-factor-auth-servers"></a>Provjera višestruke provjere autentičnosti poslužitelja
Dodatne višestruku provjeru autentičnosti poslužitelja mogu postaviti tako da bi služio kao sigurnosnu kopiju POLUMJER proxy poslužitelj, LDAP proxy ili za provjeru autentičnosti IIS. Konfiguracija sinkronizacije zajedničkog korištenja sve na agenata. Međutim, samo jednu od ovih agenata imati pokrenut servis za višestruku provjeru autentičnosti poslužitelja. Ta kartica omogućuje odaberite poslužitelj za provjeru autentičnosti višestruke provjere koje želite omogućiti za sinkronizaciju.

![Višestruka-Factor-provjeru autentičnosti poslužitelja](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
