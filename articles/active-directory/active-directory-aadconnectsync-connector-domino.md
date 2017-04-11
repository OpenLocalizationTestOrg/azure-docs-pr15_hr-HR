<properties
   pageTitle="Poveznik programa Lotus Domino | Microsoft Azure"
   description="U ovom se članku objašnjava kako konfigurirati Microsoftov Lotus Domino poveznik."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Tehnička referenca za Lotus Domino poveznika
U ovom se članku opisuju poveznik za Lotus Domino. U članku odnosi se na sljedeće proizvode:

- Microsoftov Upravitelj identiteta 2016 (MIM2016)
- Upravitelj identiteta Forefront 2010 R2 (FIM2010R2)
    -   Morate koristiti hitni popravak 4.1.3671.0 ili noviji [KB3092178](https://support.microsoft.com/kb/3092178).

Poveznik za MIM2016 i FIM2010R2, nije dostupan kao preuzeti iz [Microsoftova centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Pregled Domino poveznik programa Lotus
Poveznik za Lotus Domino omogućuje integraciju servisa sinkronizacije s poslužiteljem IBM's Lotus Domino.

Iz perspektive više razine, sljedeće značajke su podržane u trenutnom izdanju sustava poveznik:

Značajka | Podrška
--- | ---
Izvora povezanih podataka | Poslužitelj: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Klijent:<li>Lotus Notes 9.x</li>
Scenariji | <li>Upravljanje životni ciklus objekta</li><li>Upravljanje grupe</li><li>Upravljanje lozinkama</li>
Operacije | <li>Potpuna i uvoz Delta</li><li>Izvoz</li><li>Postavljanje i promjena lozinke na HTTP lozinke</li>
Shema | <li>Osobe (Roaming korisniku kontakata (osobe s nema potvrde))</li><li>Grupa</li><li>Resurs (resursa, sobu za mrežni sastanak)</li><li>Pošta u bazi podataka</li><li>Dinamični otkrivanje atributi podržane objekte</li>

Poveznik za Lotus Domino koristi klijent programa Lotus Notes za komunikaciju s poslužitelja Lotus Domino. Kao posljedica ovaj ovisnost podržani klijent bilješke Lotus mora biti instaliran na poslužitelju za sinkronizaciju. Komunikacije između klijenta i poslužitelja je implementirana putem sučelja programa Lotus Notes .NET Interop (Interop.domino.dll). Ovo sučelje olakšava komunikaciju između Microsoft.NET platforme i klijent programa Lotus Notes i podržava pristup Lotus Domino dokumente i prikaze. Za uvoz delta moguće je i sučelje nativni C++ koristi (ovisno o tome koji je način uvoza odabrane delta).

### <a name="prerequisites"></a>Preduvjeti
Prije korištenja poveznik, pripazite da imate sljedeće na poslužitelju sinkronizacije:

- 4.5.2 za Microsoft .NET Framework ili novije verzije
- Klijent programa Lotus Notes mora biti instaliran na vašem poslužitelju sinkronizacije
- Poveznik za Lotus Domino zahtijeva zadane Lotus Domino LDAP shemu baze podataka (schema.nsf) da biste postojati na poslužitelju sustava Domino direktorija. Ako ga nema, možete ga instalirati tako da radi ili ponovno pokrenuti servis LDAP na poslužitelju sustava Domino.

### <a name="connected-data-source-permissions"></a>Povezani dozvole za izvor podataka
Da biste izvršili bilo koji od podržanih zadataka u Lotus Domino poveznika, morate biti član grupe sljedeće:

- Puni pristup administratorima
- Administratori
- Administratori baze podataka

U sljedećoj su tablici navedeni dozvole koje su potrebne za svaku operaciju:

Postupak | Prava pristupa
--- | ---
Uvoz | <li>Čitanje javno dokumenata</li><li> Puni pristup Administrator (kada ste član grupe administratora puni pristup, automatski imate pristup učinkovitih u ACL.)</li>
Izvoz i postavljanje lozinke | Učinkovita pristup: <li>Stvaranje dokumenata</li><li>Brisanje dokumenata</li><li>Čitanje javno dokumenata</li><li>Pisanje Javni dokumenti</li><li>Replicirati ili kopiranje dokumenata</li>Za operacije izvoza i potrebne su vam sljedeće uloge: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Izravni operacije i AdminP
Operacije ili idite izravno u direktoriju Domino ili kroz postupak AdminP. Sljedeće tablice popis svih podržani objekti, operacije i, ako je potrebno, metodu povezane implementacije:

**Primarni adresar**

Objekt | Stvaranje | Ažuriranje | Brisanje
--- | --- | --- | ---
Osobe | AdminP | Izravni | AdminP
Grupa | AdminP | Izravni | AdminP
MailInDB | Izravni | Izravni | Izravni
Resurs | AdminP | Izravni | AdminP

**Sekundarni adresar**

Objekt | Stvaranje | Ažuriranje | Brisanje
--- | --- | --- | ---
Osobe | N/D | Izravni | Izravni
Grupa | Izravni | Izravni | Izravni
MailInDB | Izravni | Izravni | Izravni
Resurs | N/D | N/D | N/D

Pri stvaranju resursa dokumenta bilješke se stvara. Isto tako, nakon brisanja resursa dokument bilješke se briše.

### <a name="ports-and-protocols"></a>Priključci i protokoli
Klijent programa IBM Lotus Notes i poslužitelji Domino komunikaciju putem bilješke udaljene Procedure poziva (NRPC) kojemu NRPC trebali biste koristiti TCP/IP. Zadani broj priključka je 1352, ali možete promijeniti administrator sustava Domino.

### <a name="not-supported"></a>Nije podržano
Trenutnom izdanju sustava poveznik za Lotus Domino ne podržava sljedeće postupke:

- Premještanje poštanskog sandučića između poslužitelja.

## <a name="create-a-new-connector"></a>Stvaranje nove poveznika

### <a name="client-software-installation-and-configuration"></a>Klijentski softver instalacije i konfiguracije
Lotus Notes mora biti instaliran na na poslužitelju **prije nego što** je instaliran poveznik.

Kada instalirate, provjerite je li radite na **Jednom korisniku instalirati**. Zadani **Instalirati više korisnika** ne funkcionira.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Na stranici značajke instalirati samo potrebne značajke programa Lotus Notes i **Jednom prijava klijenta**. Za poveznik da biste se mogli prijaviti na poslužitelj sustava Domino potreban je jedan prijave.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Bilješke:** Započnite programa Lotus Notes jednom korisnika koji se nalazi na istom poslužitelju kao i račun koji koristite kao račun servisa na poveznik. I provjerite je li da biste zatvorili klijent programa Lotus Notes na poslužitelju. Ne može se izvoditi u isto vrijeme poveznik pokušava se povezati s poslužiteljem sustava Domino.

### <a name="create-connector"></a>Stvaranje poveznika
Da biste stvorili poveznik za Lotus Domino, u **Servis sinkronizacije** odaberite **Management Agent** i **Stvaranje**. Odaberite poveznik za **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Ako vaša verzija servisa sinkronizacije nudi mogućnost da biste konfigurirali **arhitektura**, provjerite je li poveznik postavljeno na zadanu vrijednost rada u **tijeku**.

### <a name="connectivity"></a>Povezivanje
Na stranici povezivanje Navedite naziv poslužitelja Lotus Domino i unesite vjerodajnice za prijavu.  
![Povezivanje](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Svojstva poslužitelja Domino podržava dva oblika za naziv poslužitelja:

- Naziv poslužitelja
- Naziv poslužitelja/direktorija

**Naziv poslužitelja/direktorija** oblik je Preferirani format za taj atribut jer pruža bržeg odgovora kada poveznik obrati poslužitelja Domino.

Navedeni korisnički ID datoteke se pohranjuju u bazi podataka za konfiguraciju sinkronizacije servisa.

Za **Uvoz Delta** imaju sljedeće mogućnosti:

- **Ništa**. Poveznik učiniti sve delta uvozi.
- **Dodavanje i ažuriranje**. Uvoz delta Connector ne dodaju i ažuriraju operacije. Za brisanje, potreban je postupak za **Potpuni uvoz** . Ovaj postupak koristi .net interop.
- **Dodavanje i ažuriranje i brisanje**. Delta uvoza ne poveznik za dodavanje, ažuriranje i brisanje operacije. Ovaj postupak koristi nativni C++ sučelja.

U **Mogućnosti sheme** su vam sljedeće mogućnosti:

- **Zadani shemu**. Poveznik otkriva sheme s poslužitelja Domino. Ovo je zadana mogućnost.
- **DSML shemu**. Koristiti samo ako poslužitelja Domino izlažite u shemi. Zatim možete stvoriti DSML datoteke sa shemom i uvezite ga umjesto toga. Dodatne informacije o DSML potražite u članku [ORGANIZACIJA](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Kada kliknete Dalje, konfiguracijske parametre korisnički ID i lozinka potvrde.

### <a name="global-parameters"></a>Globalni parametara
Na stranici globalni parametara konfiguriranje vremensku zonu i uvoz i izvoz mogućnost operacija.  
![Globalni parametara](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**Vremenska zona poslužitelja Domino** parametar određuje mjesto poslužitelja Domino.

Potreban je ta mogućnost konfiguracija za podršku operacije **uvoza delta** jer omogućuje sinkronizacije servisa određivanje razlike između zadnje dvije uvozi.

#### <a name="import-settings-method"></a>Uvoz postavki, način
Za **Izvođenje puni uvoz tako da** sadrži ove mogućnosti:

- Pretraživanje
- Prikaz (preporučeno)

**Pretraživanje** pomoću indeksiranja u Domino, ali je uobičajenih indeksi se ažuriraju u stvarnom vremenu te podatke koji se vraćaju s poslužitelja nije uvijek točan. Za sustav s mnogo promjene, tu mogućnost obično ne funkcionira dobro i njihovi false briše u nekim slučajevima. Međutim, **pretraživanje** je brže nego **Prikaz**.

**Prikaz** je preporučena mogućnost jer pruža odgovarajućem stanju podataka. To je malo manja od pretraživanja.

#### <a name="creation-of-virtual-contact-objects"></a>Stvaranje virtualne kontakta objekata
Na **omogućiti stvaranje \_objekta kontakta** sadrži ove mogućnosti:

- Ništa
- Vrijednosti koje nisu Reference
- Pregled i -referenca vrijednosti

U Domino, referenca atribute mogu sadržavati mnogo različitih oblika referentni drugim objektima. Da biste mogli predstavljaju različite varijacije, primjenjuje poveznik \_obratite objekte, poznata i kao **Virtualne kontakata** (VC). Ti objekti je stvorila da bi ih mogle pridružiti postojeće MV objekte ili predviđena kao novih objekata. Na taj način mogu se očuvati atribut reference.

Omogućivanjem tu postavku i ako je sadržaj atribut referenca nije obliku DN na \_stvara se objekta kontakta. Na primjer, atribut člana grupe može sadržavati SMTP adresa. Ujedno shortName i druge atribute koje su prisutne u atribute referencu. U ovom scenariju, odaberite **Vrijednosti koje nisu Reference**. Tu konfiguraciju je najčešće postavka za implementacije sustava Domino.

Kada je Lotus Domino postavljen na zasebnom adresari imaju različite nazive razlikovni koji predstavlja na isti objekt, moguće je stvoriti \_objekata zatražite sve reference vrijednosti koje se nalaze u adresar. U ovom scenariju, odaberite mogućnost **Reference i vrijednosti koje nisu referencu** .

Ako imate više vrijednosti iz atributa **ime i prezime** u Domino, pa želite omogućiti stvaranje virtualne kontakata tako da se može riješiti reference. Na primjer, taj atribut može imati više vrijednosti nakon braka ili divorce. Potvrdite okvir Omogući **... Ime i prezime sadrži više vrijednosti** za taj scenarij.

Spajanjem ispravne atribute u \_objekata kontakt će se spajaju MV objekt.

Te se objekte odnose VC =\_kontaktu dodaje njihove DN.

#### <a name="import-settings-conflict-object"></a>Uvoz postavki, objekta u sukobu
**Isključivanje sukoba objekta**

U velikim implementaciji sustava Domino, moguće je da više objekata imaju isti DN zbog problema s replikacijom. U tim slučajevima poveznik vidjeti dva objekta s različitim UniversalIDs, ali isti DN. Ovaj sukob može prouzročiti tranzitne objekt stvoren u prostoru poveznik. Poveznik možete zanemariti objekata koji ste odabrali u Domino kao žrtve replikacije. Na preporuka je da biste zadržali ovaj potvrdni okvir odabran.

#### <a name="export-settings"></a>Izvoz postavki
Ako nije odabrana mogućnost **Koristi AdminP za ažuriranje referenci** , zatim izvoz atributa referenca, kao što su člana, Izravni pozive i Upotreba postupka AdminP. Tu mogućnost koristite samo kada AdminP nije konfiguriran za održavanje referencijalni integritet.

#### <a name="routing-information"></a>Usmjeravanje
U Domino, moguće je ima li referenca atribut usmjeravanje ugrađen kao sufiks da bi se DN. Na primjer, može sadržavati atribut člana u grupu **CN=example/organization@ABC**. Sufiks @ABC usmjeravanje informacije. Podaci o usmjeravanju koristi Domino za slanje poruke e-pošte točan sustava Domino, koja može biti sustava u drugi tvrtki ili ustanovi. U polju usmjeravanje informacije možete odrediti usmjeravanje nastavke koriste unutar tvrtke ili ustanove u opsegu poveznika. Ako jednu od ovih vrijednosti nalazi se kao sufiks u atributu referenca, podaci o usmjeravanju je uklonjena iz referencu. Ako usmjeravanje sufiks na vrijednost referenca ne podudaraju se na jednu od tih vrijednosti navedene, na \_stvara se objekta kontakta. Te \_kontakt objekti su stvorene pomoću ** RO=@ ** umetnut u DN. O tim \_kontakt objekata sljedećim atributima dodaju Dopusti uključivanje na stvarni objekt, ako je potrebno: \_routingName, \_ime kontakta, \_riješiti problem, i UniversalID.

#### <a name="additional-address-books"></a>Dodatni adresari
Ako nemate **imeniku** instaliran, koji pruža Naziv sekundarne adresari pa možete ručno unijeti te adresara.

#### <a name="multivalued-transformation"></a>Pretvorba s više vrijednosti
Mnogo atributi u Lotus Domino su s više vrijednosti. Odgovarajući atribute metaverse su obično jedan vrijednosti. Konfiguriranjem uvoza i mogućnost za operaciju izvoza omogućiti poveznik će vam pomoći u potrebna prijevod zahvaćene atribute.

**Izvoz**  
Mogućnost za operaciju izvoza podržava dva načina:

- Dodavanje stavke
- Zamjena stavke

**Zamjena stavke** – kada odaberete tu mogućnost, uvijek poveznik uklanjanje trenutne vrijednosti atributa u Domino i zamijenite navedene vrijednosti. U navedeni vrijednosti može biti jednom vrijednosti ili više vrijednosti.

Primjer: Atribut pomoćnika objekta osoba koja ima sljedeće vrijednosti:

- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Marko Smith/OU=Contoso/O=Americas,NAB=names.nsf

Ako je taj objekt osoba koja je dodijeljen novog pomoćnika pod nazivom **Nevena Alexander** , rezultat je:

- CN = Nevena Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Dodavanje stavke** – kad odaberete tu mogućnost, poveznik zadržava postojećih vrijednosti na atributa u Domino i Umetanje novih vrijednosti pri vrhu popisa podataka.

Primjer: Atribut pomoćnika objekta osoba koja ima sljedeće vrijednosti:

- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Marko Smith/OU=Contoso/O=Americas,NAB=names.nsf

Ako je taj objekt osoba koja je dodijeljen novog pomoćnika pod nazivom **Nevena Alexander** , rezultat je:

- CN = Nevena Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Marko Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Uvoz**  
Mogućnost operacije uvoza podržava dva načina:

- Zadani
- Polja s više jednu vrijednost

**Zadani** – prilikom odabira zadana mogućnost, sve vrijednosti svih atributa se uvoze.

**Multivalued jednu vrijednost** – kada odaberete tu mogućnost, atribut s više vrijednosti pretvara se u atribut s jednom vrijednosti. Ako postoji više od jedne vrijednosti, koristi se vrijednost na vrhu (tu vrijednost je obično i najnoviji).

Primjer: Atribut pomoćnika objekta osoba koja ima sljedeće vrijednosti:

- CN = Nevena Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Marko Smith/OU=Contoso/O=Americas,NAB=names.nsf

Najnovija ažuriranja za taj atribut je **Neven Alexander**. Jer je mogućnost operacije uvoza postavljen na Multivalued jednu vrijednost, poveznika samo uvozi **Nevena Alexander** u prostor poveznik.

Logika da biste pretvorili atribute s više vrijednosti u pojedinačnim atributa ne primjenjuju se atribut člana grupe i atribut ime i prezime osobe.

Ga i moguće konfigurirati uvoz i izvoz pravila transformacije za polja s više atribute po atribut, kao iznimku globalni pravilo. Da biste konfigurirali tu mogućnost, unesite [vrstu objekta]. [attributename] u tekstnim okvirima **popisa atributa izuzetaka za uvoz** i **Izvoz popisa atributa izuzetaka** . Ako, na primjer, ako unesete Person.Assistant i globalne zastavice postavljen da biste uvezli sve vrijednosti, samo prva vrijednost uvoze se u Pomoćniku za.

#### <a name="certifiers"></a>Certifiers
Sve jedinice tvrtke ili ustanove/tvrtke ili ustanove navedene su po poveznik. Da biste mogli izvesti osoba objekata u primarni adresar, potreban je certifier s lozinku.

Ako sve certifiers imaju istu lozinku, **lozinke za sve Certifers** može se koristiti. Zatim unesite lozinku u nastavku i navedite samo certifier datoteku.

Ako samo uvozite, zatim nemate da biste naveli bilo koje certifiers.

### <a name="configure-provisioning-hierarchy"></a>Konfiguriranje dodjele resursa hijerarhije
Kada konfigurirate poveznik za Lotus Domino, preskočite ove stranice dijaloški okvir. Poveznik za Lotus Domino ne podržava hijerarhije dodjele resursa.  
![Dodjeljivanje hijerarhije](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguriranje particije i hijerarhije
Kada konfigurirate particije i hijerarhije, morate odabrati primarni adresar pod nazivom NAB=names.nsf. Osim primarnog address book odaberite sekundarni adresari ako postoje.  
![Particije](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Odaberite atributa
Kada konfigurirate svoje atribute, morate odabrati svih atributa koji su mjestu s ** \_MMS\_**. Ti atributi su potrebne prilikom Dodjela nove objekte za Lotus Domino

![Atributi](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Upravljanje životni ciklus objekta
Ovo poglavlje sadrži pregled različitih objekata u Domino.

### <a name="person-objects"></a>Objekti osoba
Objekt osoba koja predstavlja korisnika u tvrtki ili ustanovi i jedinice tvrtke ili ustanove. Osim zadanih atributa administrator sustava Domino možete dodati prilagođene atribute osoba objekta. Barem osoba objekt mora sadržavati sve obavezno atribute. Popis svih obaveznih atribute potražite u članku [Programa Lotus Notes svojstva](#lotus-notes-properties). Da biste registrirali osoba objekta, mora biti zadovoljen sljedeće preduvjete:

- Adresar (names.nsf) morate definirani i mora biti primarni adresar.
- Morate imati certifier O/OU Id i lozinku da biste registrirali određenom korisniku u tvrtki ili ustanovi / Organizacijska jedinica.
- Morate postaviti određene programa Lotus Notes svojstva za objekte osoba. Ova svojstva se koriste za dodjeljivanje objekta osoba. Dodatne informacije potražite u odjeljku naziva [Programa Lotus Notes svojstva](#lotus-notes-properties) kasnije u ovom dokumentu.
- Početna HTTP lozinku za osobu je atribut i postavljanje prilikom dodjele resursa.
- Objekt osoba mora biti jedna od sljedeće tri podržane vrste:
    1. Normalno korisnika koji ima datoteku e-pošte i datoteke korisničkog ID-a
    2. Prebacivanje korisnika (obično korisnika koji obuhvaća sve roaming datoteke baze podataka)
    3. Kontakti (korisnik bez datoteke ID-a)

Osobe (osim Kontakti) možete dodatno grupirati u NAM i korisnika međunarodni kako je definirano vrijednost u \_MMS\_IDRegType svojstvo. Ove osoba koristi klijent bilješke da biste pristupili poslužitelje Lotus Domino imati Id za bilješke te osobe dokumenta. Ako koriste pošte bilješke, zatim i imaju datoteku e-pošte. Korisnik mora biti registriran postaje aktivno. Dodatne informacije potražite u članku:

- [Postavljanje korisnika bilješke](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Registracija korisnika](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Upravljanje korisnicima](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [Preimenovanje korisnika](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Te operacije su pokrenuto u Lotus Domino, a zatim uvezeni u servisu sinkronizacije.

### <a name="resources-and-rooms"></a>Resurse i soba
Resurs je neku drugu vrstu baze podataka u Lotus Domino. Resursi mogu biti sobe za sastanke s različitim vrstama opreme, kao što su projektora. Postoje podvrste resursa koje podržava Lotus Domino poveznika, a koji su definirani atribut vrsta resursa:

Vrste resursa | Vrsta atributa resursa
--- | ---
Sobe | 1
Resurs (ostalo) | 2
Mrežni sastanak | 3

Za vrstu objekta resursa za rad u nastavku je potrebno:

- Baza podataka rezervacija resursa mora već postoje u povezanog poslužitelja Domino
- Web-mjesto je već definiran za resurs

Rezervacija resursa baza podataka sadrži tri vrste dokumenata:

- Profil na web-mjesta
- Resurs
- Rezervacija

Dodatne informacije o postavljanju rezervacija resursa baze podataka potražite u članku [Postavljanje rezervacija resursa baze podataka](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Stvaranje, ažuriranje i brisanje resursi**  
Stvaranje, ažuriranje i brisanje operacije izvršava poveznik za Lotus Domino u bazi podataka rezervacija resursa. Resursi stvaraju se kao dokumenata u Names.nsf (to jest, primarni address book). Dodatne informacije o uređivanju i brisanju resursima potražite u članku [Uređivanje i brisanje dokumenata resursa](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Uvoz i izvoz postupak za resurse**  
Resursi možete uvesti i izvezeni sa servisu sinkronizacije, kao što je bilo koju drugu vrstu objekta. Odaberite vrstu objekta kao resurs tijekom konfiguracije. Za uspješan izvoza, imat ćete detalje o vrsta resursa, konferencijski baze podataka i naziv web-mjesta.

### <a name="mail-in-databases"></a>Baze podataka ulazne pošte
Pošta u bazi podataka je baza podataka namijenjen primati poštu. Je li za Lotus Domino poštanskom sandučiću koji nije povezan s bilo kojeg određene Lotus Domino korisnički račun (to jest, nema vlastitu datoteku ID i lozinku). Pošta u bazi podataka ima jedinstveni korisnički ("kratki naziv") pridružene i vlastitu adresu e-pošte.

Ako je potrebno za zasebnom poštanski sandučić s svoju adresu e-pošte koji se mogu zajednički koristiti različite korisnike (na primjer, group@contoso.com), pošte se baza podataka. Pristup na ovaj poštanski sandučić kontrolira kroz njegov pristup kontrola popisa (ACL), koji sadrži imena korisnika bilješke dopušteni da biste otvorili poštanski sandučić.

Popis potrebne atribute, potražite u odjeljku naziva [Obaveznim atributa](#mandatory-attributes) u nastavku ovog članka.

Kada bazi podataka namijenjen prima e-poštu, dokument pošta u bazi podataka se stvara u Lotus Domino. Ovaj dokument mora postojati u direktoriju Domino svaki poslužitelju na kojem se sprema kopiju baze podataka. Detaljan opis o stvaranju dokumenta pošta u bazi podataka potražite u članku [Stvaranje dokumenta pošta u bazi podataka](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Prije stvaranja pošte u bazu podataka, trebali biste baza podataka već postoji (potrebno je stvorio administrator Lotus) na poslužitelju sustava Domino.

### <a name="group-management"></a>Upravljanje grupe
Detaljne pregled upravljanje grupe Lotus Domino možete dobiti od u sljedećim resursima:

- [Korištenje grupa](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Stvaranje grupe](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Stvaranje i izmjena grupe](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Upravljanje grupama](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [Preimenovanje grupe](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Upravljanje lozinkama
Za Lotus Domino korisnikom registrirani postoje dvije vrste lozinki:

1. Korisničke lozinke (spremljene u datoteci User.id)
2. Internet / HTTP lozinke

Poveznik za Lotus Domino podržava samo operacije HTTP lozinkom.

Da biste izvršili upravljanje lozinkama, trebate omogućiti upravljanje lozinkama poveznik u dizajneru Agent za upravljanje. Da biste omogućili upravljanje lozinkom, odaberite **Omogući upravljanje lozinkama** na stranici **Konfiguracija proširenja** dijaloški okvir.  
![Konfiguriranje proširenja](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Podrška za Lotus Domino poveznik pratiti postupci za internetsku lozinku:

- Postavljanje lozinke: Postavljanje lozinke postavlja novu lozinku HTTP/Internet na korisnike u Domino. Prema zadanim postavkama računa ujedno otključane. Zastavica otključaj predstavljeni sučelje WMI modula za sinkroniziranje programa.
- Promjena lozinke: U ovom primjeru korisnik možda želite promijeniti lozinku ili je upit da biste promijenili lozinku nakon određenog vremena. Ovaj postupak da biste preuzeli postavite, obavezno (stari i novu lozinku). Kada se promijene, Lotus Domino će se ažurirati s novom lozinkom.

Dodatne informacije potražite u članku:

- [Korištenje značajke lockout Internet](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Upravljanje lozinkama Internet](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Informacije o referenci
U ovom se odjeljku navedene kao što su atribut opise i atributa preduvjeti za poveznik za Lotus Domino.

### <a name="lotus-notes-properties"></a>Lotus Notes svojstva
Prilikom dodjele resursa objekata osoba u imeniku Lotus Domino objekte mora imati određeni skup svojstava s određenim vrijednostima popunjeno. Ove vrijednosti samo su potrebne za stvaranje operacije.

U sljedećoj su tablici navedeni tih svojstava i sadrži opis od njih.

Svojstvo | Opis
--- | ---
\_MMS_AltFullName | Zamjenski puno ime korisnika.
\_MMS_AltFullNameLanguage | Jezik koji želite koristiti za određivanje zamjenski puno ime korisnika.
\_MMS_CertDaysToExpire | Broj dana od trenutni datum prije certifikat istekne. Ako nije naveden, zadanog oblika datuma je dvije godine od trenutnog datuma.
\_MMS_Certifier | Svojstvo koje sadrži naziv organizacijskoj hijerarhiji na certifier. Ako, na primjer: OU = OrganizationUnit, O = tvrtke ili ustanove, C = država.
\_MMS_IDPath | Ako je svojstvo prazno, ne korisnika identifikacijski stvara se datoteka lokalno na poslužitelju za sinkronizaciju. Ako je svojstvo sadrži naziv datoteke, datoteke korisničkog ID-a je stvoriti u mapi madata. Svojstvo može sadržavati cijeli put.
\_MMS_IDRegType | Osobe mogu se klasificirati kao kontakte, NAM korisnika i međunarodne korisnika. U sljedećoj su tablici navedeni moguće vrijednosti: <li>0 - kontakta</li><li>1 – SAD-a korisnika</li><li>2 – međunarodni korisnika</li>
\_MMS_IDStoreType | Svojstvo obavezno za SAD-a i međunarodne korisnika. Svojstvo sadrži cjelobrojnu vrijednost koja određuje hoće li se identifikacija korisnika pohranjen kao privitak u adresaru bilješke ili u datoteku e-pošte te osobe. Ako je datoteka korisnički ID privitka u adresaru, ga po želji moguće stvoriti kao datoteku s \_MMS_IDPath. <li>Prazno – ID pohrane datoteke u sigurnog ID, nijedna datoteka identifikacijski (koristi se za kontakte).</li><li> 1 – privitak u adresaru bilješke. U \_MMS_Password svojstvo mora biti postavljena za korisničkih identifikacijski datoteka privitaka</li><li>2 – spremanje ID-a u datoteku e-pošte te osobe. U \_MMS_UseAdminP mora biti postavljeno na false da biste omogućili datoteka pošte moguće stvoriti tijekom registracije osoba. U \_MMS_Password svojstvo mora biti postavljena za korisničke identifikacijski datoteke.</li>
\_MMS_MailQuotaSizeLimit | Broj megabajta koji je dopušteno datoteka baze podataka e-pošte.
\_MMS_MailQuotaWarningThreshold | Broj megabajta je dopušteno datoteka baze podataka e-pošte prije izdavanja upozorenje.
\_MMS_MailTemplateName | Datoteka predloška e-pošte koja se koristi za stvaranje datoteke e-pošte za korisnika. Ako predložak nije naveden, datoteka pošte se stvara pomoću predloška za navedeni. Ako nijedan predložak nije naveden, zadanu datoteku predloška se koristi za stvaranje datoteke.
\_MMS_OU | Neobavezni svojstvo koje je naziv parametra OU u odjeljku s certifier. Ovo svojstvo mora biti prazna za kontakte.
\_MMS_Password | Svojstvo obavezno za korisnike. Svojstvo sadrži lozinke za identifikaciju datoteku objekta.
\_MMS_UseAdminP | Svojstvo mora biti postavljeno TRUE ako je datoteka pošte treba kreirati proces AdminP na poslužitelju sustava Domino (asinkronog postupku izvoza). Ako je svojstvo postavljeno na false, datoteka pošte stvara se s korisnikom sustava Domino (sinkrono u postupku izvoza).

Za korisnike s datotekom povezani identifikacijski u \_MMS_Password svojstvo mora sadržavati vrijednost. Pristup e-pošte putem klijentskog programa Lotus Notes, MailServer i MailFile svojstva korisnik mora sadržavati vrijednost.

Da biste pristupili e-pošte putem web-preglednika, sljedeća svojstva mora sadržavati vrijednosti:

- MailFile - svojstvo obavezno koja sadrži put na poslužitelju Lotus Domino gdje je pohranjena datoteka pošte.
- MailServer - svojstvo obavezno koja sadrži naziv poslužitelja Lotus Domino. Ova vrijednost je naziv koji će se koristiti prilikom stvaranja cirkularnih datoteka za Lotus na poslužitelju sustava Domino.
- HTTPPassword - neobavezno svojstva koje sadrži lozinku za pristup Web objekta.

Da biste pristupili poslužitelja Domino bez pošte mogućnost, svojstvo HTTPPassword mora sadržavati vrijednost. Svojstvo MailFile i svojstvo MailServer može biti prazna.

S \_MMS_ IDStoreType = 2 (id pohrane u datoteku e-pošte), svojstvo MailSystem NotesRegistrationclass postavljeno na REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Obavezno atributa
Poveznik za Lotus Domino uglavnom podržava sljedeće vrste objekata (vrste dokumenta):

- Grupa
- Pošta u bazi podataka
- Osobe
- Kontakt (osoba koja ima bez certifier)
- Resurs

U ovom se odjeljku navedeni atributa koji su obavezna za svaki podržani objekt da biste izvezli poslužitelja Domino.

Vrsta objekta | Obavezno atributa
--- | ---
Grupa | <li>ListName</li>
U glavnom bazom podataka | <li>Ime i prezime</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>
Osobe | <li>Prezime</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Kontakt (osoba koja ima bez certifier) | <li>\_MMS_IDRegType</li>
Resurs | <li>Ime i prezime</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Web-mjesta</li><li>Riješiti problem</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Uobičajeni problemi i pitanja

### <a name="schema-detection-does-not-work"></a>Otkrivanje sheme ne funkcionira
Da biste mogli otkriti shemu, nije potrebno da je datoteka schema.nsf prezentacija na poslužitelju sustava Domino. Datoteka prikazuje se samo ako je instaliran LDAP na poslužitelju. Ako je u shemi nalaziti neprepoznatljive, provjerite sljedeće:

- Postoji schema.nsf datoteka u korijensku mapu Domino poslužitelja
- Korisnik ima dozvole za prikaz datoteke schema.nsf.
- Obavezno ponovno pokretanje LDAP poslužitelj. Otvorite **Lotus Domino konzole** i pomoću naredbe **Reći ReloadSchema LDAP** da biste ponovno učitali u shemi.

### <a name="not-all-secondary-address-books-are-visible"></a>Vidljivi su sve sekundarne adresari
Poveznik za Domino ovisi o značajku **Direktorija pomoć** da biste mogli pronaći sekundarne adresari. Ako nema sekundarne adresara, provjerite ako [Imeniku](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) omogućen i konfiguriran na poslužitelju sustava Domino.

### <a name="custom-attributes-in-domino"></a>Prilagođene atribute u Domino
Postoji nekoliko načina u Domino da biste proširili shemu tako da se prikaže kao prilagođeni atribut potrošni po poveznik.

**Pristup 1: Proširivanje Lotus Domino sheme**

1. Stvaranje kopija predloška Domino direktorija {PUBNAMES. NTF} po sljedeće [korake](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (ne treba Prilagodi IBM Lotus Domino imenika zadani predložak):
2. Otvorite kopiju Domino direktorija predložak {CONTOSO. NTF} predložak koji je stvoren u programu Designer Domino i slijedite ove korake:
    - Kliknite zajedničko korištenje elemenata, a zatim proširite podobrazaca
    - Dvokliknite {ObjectName} $InheritableSchema podobrazac (pri čemu {ObjectName} je klase strukturnih objekt zadane, na primjer: osobe).
    - Naziv atributa koji želite dodati u shemi {MyPersonAtrribute} i odgovarajući atribut. Stvaranje polja, odaberite **Stvori** izbornik, a zatim odaberite **polje** s izbornika.
    - U polju dodane postaviti svojstva tako da odaberete vrstu, stil, veličinu, fonta i ostale povezane parametara u prozoru svojstva polja.
    - Zadrži atribut zadanu vrijednost ista kao naziv dan za taj atribut (na primjer, ako je naziv atributa MyPersonAttribute, ostati zadanu vrijednost s istim nazivom).
    - Spremite InheritableSchema podobrazac ${ObjectName} ažurirane vrijednostima.
3. Zamjena imenika predložak Domino {PUBNAMES. NTF} s novi prilagođeni predložak {CONTOSO. NTF} po sljedeće [korake](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zatvorite Domino administrator i otvorite Domino konzolu da biste ponovno pokrenite servis za LDAP, a da biste ponovno učitali LDAP sheme:
    - Naredba u odjeljku tekst **Naredbe Domino** spremljen ponovno pokrenuti servis LDAP - [Ponovno pokrenite LDAP zadatka]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)Umetni na konzoli sustava Domino.
    - Da biste ponovno učitali LDAP sheme naredbom reći LDAP - reći LDAP ReloadSchema
5. Otvaranje Domino administrator i odaberite karticu osobe i grupe da biste vidjeli dodane atribut prikazuje se domino dodajte osobu.
6. Otvorite Schema.nsf, na kartici **datoteke** i potražite u članku dodane atribut prikazuje se u dominoPerson LDAP objekt predmete.

**Pristup 2: Stvaranje programa auxClass s prilagođeni atribut i pridružiti klase objekta**

1. Stvaranje kopija predloška Domino direktorija {PUBNAMES. NTF} po sljedeće [korake](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nikad se prilagodi IBM Lotus Domino imenika zadani predložak):
2. Otvorite kopiju Domino direktorija predložak {CONTOSO. NTF} predložak koji je stvoren, u dizajneru Domino.
3. U lijevom oknu odaberite zajedničko korištenje kod, a zatim podobrazaca.
4. Kliknite novi podobrazac
5. Učinite sljedeće da biste odredili svojstva za novi podobrazac:
    - Novi podobrazac otvoren, odaberite dizajn - svojstva podobrasca
    - Uz stavku svojstvo Naziv unesite naziv klase pomoćnih objekta – na primjer, TestSubform.
    - Zadrži svojstvo mogućnosti "Odabrana mogućnost Obuhvati Umetanje podobrazac... dijaloški okvir"
    - Poništite odabir u mogućnosti svojstva "renderiranje prolazni HTML u bilješkama."
    - Ostavite na drugim svojstvima isti i zatvorite okvir svojstva podobrasca.
    - Spremite i zatvorite novi podobrazac.
6. Učinite sljedeće da biste dodali polje da biste definirali klase pomoćnih objekta:
    - Otvorite podobrazac koju ste stvorili.
    - Odaberite stvaranje - polja.
    - Uz naziv na kartici osnove u dijaloškom okviru polje, navedite naziv, na primjer: {MyPersonTestAttribute}.
    - U polju dodane postaviti svojstva tako da odaberete vrsta, stil, veličinu, fonta i povezane svojstva.
    - Zadrži atribut zadanu vrijednost ista kao naziv dan za taj atribut (na primjer, ako je naziv atributa MyPersonTestAttribute, ostati zadanu vrijednost s istim nazivom).
    - Spremite podobrazac ažurirane vrijednosti i učinite sljedeće:
        - U lijevom oknu odaberite zajedničko korištenje kod, a zatim podobrasce
        - Odaberite novi podobrazac, a zatim odaberite dizajn - svojstva dizajna.
        - Kliknite karticu treći slijeva pa odaberite **Promjena Propagate ovaj prohibition dizajna**.
7. Otvorite podobrazac ${ObjectName} ExtensibleSchema (pri čemu {ObjectName} je zadani klase strukturnih objekta, na primjer – osoba).
8. Umetanje resursa i odaberite podobrazac (koju ste stvorili, na primjer – TestSubform) i spremite podobrazac ExtensibleSchema ${ObjectName}.
9. Zamjena imenika predložak Domino {PUBNAMES. NTF} s novi prilagođeni predložak {CONTOSO. NTF} po sljedeće [korake](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zatvorite Domino administrator i otvorite Domino konzolu da biste ponovno pokrenite servis za LDAP, a da biste ponovno učitali LDAP sheme:
    - Naredba u odjeljku tekst **Naredbe Domino** spremljen ponovno pokrenuti servis LDAP - [Ponovno pokrenite LDAP zadatka](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)Umetni na konzoli sustava Domino.
    - Da biste ponovno učitali LDAP sheme pomoću naredbe reći LDAP **Reći ReloadSchema LDAP**.
11. Otvorite administrator sustava Domino, a zatim odaberite karticu osobe i grupe da biste vidjeli dodane atribut prikazuje se domino dodavanje osoba (u odjeljku drugima kartica).
12. Otvorite Schema.nsf, na kartici **datoteke** i potražite u članku dodane atribut prikazuje se u odjeljku TestSubform LDAP pomoćna klasa objekta.

**Pristup 3: Dodajte prilagođeni atribut klase ExtensibleObject**

1. Otvaranje {Schema.nsf} datoteke smještene na korijenskom direktoriju
2. Odaberite LDAP objekt klase na lijevom izborniku u odjeljku **Svi dokumenti sheme** , a zatim kliknite gumb **Dodaj klase** :
3. Navedite naziv LDAP u obliku {zzzExtensibleSchema} (pri čemu zzz je klase strukturnih objekt zadane, primjerice osobe). Na primjer, da biste proširili shemu za osobu klase objekta, navedite naziv LDAP {PersonExtensibleSchema}.
4. Navedite naziv klase nadređeni objekt, za koji želite da biste proširili shemu. Na primjer, da biste proširili sheme za predmete objekt osoba, navedite naziv klase nadređeni objekt {dominoPerson}:
5. Navedite valjani OID odgovara klase objekta.
6. Odaberite atribute prošireno/Prilagođeno u odjeljku obavezna ili neobavezno vrste atributa polja po potrebama:
7. Nakon dodavanja potrebne atribute u ExtensibleObjectClass, kliknite **Spremi i Zatvori**.
8. Odgovarajući zadanu klasu objekt s dodatne atribute stvara sustava ExtensibleObjectClass.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

-   Informacije o omogućivanju zapisivanja poveznik za otklanjanje poteškoća s potražite u članku [kako omogućiti praćenje ETW za poveznika](http://go.microsoft.com/fwlink/?LinkId=335731).
