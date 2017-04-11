<properties
    pageTitle="Zaštita identiteta Azure Active Directory | Microsoft Azure"
    description="Saznajte kako Azure AD identiteta zaštita omogućuje vam ograničiti mogućnost napadaču izrabljuje ugrožena identiteta ili uređaj i sigurne identitet ili uređaj koji je prethodno sumnjivu ili zna da biti ugrožena."
    services="active-directory"
    keywords="Zaštita identiteta Azure active directory, otkrivanje aplikacije oblaka, Upravljanje aplikacijama, sigurnost, rizika, razina rizika, slabe, sigurnosna pravila"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Zaštita identiteta Azure Active Directory 

Azure Active Directory identiteta zaštita je značajka servisa Azure AD Premium P2 edition koji omogućuje vam konsolidirani prikaz u događaje rizik i potencijalne slabe točke utjecaja identiteta vaše tvrtke ili ustanove. Microsoft je je zaštita identitete utemeljene na oblaku za putem u deset godina i sa Azure AD identiteta, Microsoft je dostupnost ti isti sustavi zaštita tvrtke. Zaštita identiteta upravlja postojeće Azure AD-značajkom prepoznavanja mogućnosti (dostupno samo putem Azure AD Anomalous aktivnosti izvješća), a predstavlja nove vrste rizika događaja koji se može prepoznati anomalies u stvarnom vremenu.



##<a name="getting-started"></a>Početak rada

Većina breaches sigurnost odvija attackers ostvariti pristup okruženju tako da onemogućuje krađu identiteta korisnika. Attackers prekorače sve važeće na korištenje breaches drugih proizvođača i koristite složene od krađe identiteta. Kada napadaču poboljšava pristup čak i niska povlaštene korisnički račun, je relativno jednostavne za njih da biste pristupili resursa važne tvrtke kroz lateral premještanja. Stoga je ključna da biste zaštitili sve identiteta i kada identitet ugrožena, doći sprječavate compromised identitet koji se abused. 

Otkrivanje ugrožena identiteta je bez jednostavno zadatak. Srećom, može pomoći identiteta zaštita: zaštita identitet koristi algoritama prilagodljivo strojnog učenja i heuristike za otkrivanje anomalies i rizikom događaja koji se može značiti da je identitet ugrožena.
 
Pomoću ove podatke, zaštitu identiteta generira izvješća i upozorenja koja omogućuje vam da biste istražili tih rizika događaja i poduzeti odgovarajuće olakšava ili ublažiti akcija.
 
Dok je Azure Active Directory identiteta zaštita više alat za nadzor i izvješćivanja. Na temelju rizika događaje identiteta zaštita izračunava razinu rizika korisnika za svakog korisnika, što vam omogućuje da konfiguriranje pravilnika o utemeljen na rizika automatski zaštitu identitet tvrtke ili ustanove.  Tih rizika sustavom pravila, uz druge uvjetno pristup kontrolama za Azure Active Directory i EMS, možete automatski blokirati ili nude prilagodljivo olakšava akcije koje sadrže lozinki i provođenje višestruke provjere autentičnosti.  

####<a name="explore-identity-protections-capabilities"></a>Istražite mogućnosti zaštite za identitet 

**Otkrivanje događaja rizik i opasan računa:**  

- Otkrivanje 6 rizik vrsta događaja pomoću strojnog učenja i heurističkih pravila 

- Izračunavanje razine rizika korisnika

- Pružanje prilagođene preporuke za poboljšanje cjelokupan posture sigurnost tako da ističe slabe točke



**Istražuje rizika događaja:**

- Slanje obavijesti o događajima rizika

- Istražuje rizika događaje korištenje relevantan i kontekstnih podataka

- Pružanje osnovni tijekova rada za praćenje istrage

- Omogućuje brz pristup olakšava akcije kao što je ponovno postavljanje lozinke



**Pristup utemeljen na rizika uvjetnog pravila:**

- Pravila da biste smanjili opasan znak dodataka tako da blokira dodatke za prijavu ili koje je obavezna izazove višestruke provjere autentičnosti.

- Pravilnik blok ili sigurne opasan korisničkih računa

- Pravila da biste korisnike Obvezali da se registrirati za višestruka provjera autentičnosti


## <a name="detection-and-risk"></a>Prepoznavanje i rizika

### <a name="risk-events"></a>Rizik događaja

Rizik događaja su događaje koje su označene kao sumnjiva identiteta zaštitu i označava da identitet možda ste je ugrožena. Popis svih rizika događaja potražite u članku [vrste događaja rizika otkrio Azure Active Directory identiteta zaštitu](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Razina rizika

Razina rizika za događaj rizika je naznaku (visoke, Srednje ili najniža) težinu rizika događaj. Razinu rizika olakšava zaštitu identitet korisnika prioritet akcije morate proizvela smanjenju rizika njihovoj tvrtki ili ustanovi. Težinu događaj rizika predstavlja jačinu signal kao predictor od identiteta narušena pouzdanost u kombinaciji s količinu suvišnih koji obično predstavlja. 

- **Visoke**: Povjerljivi i visoke težinu rizik događaj. Ti događaji su istaknuti pokazatelja ugrožena identitet korisnika, a sve korisničke račune utjecati treba remediated odmah.

- **Srednje**: visoke težinu, ali donjem događaj confidence rizika, ili obrnuto. Ti događaji su potencijalno opasan, a sve korisničke račune utjecati mora biti remediated.

- **Niska**: niska pouzdanosti i niska težinu rizik događaj. Taj događaj nije potrebna trenutne akcije, ali kada se kombinirati s drugim događajima rizika pružati istaknuti oznaka da identitet ugrožena. 


![Razina rizika] (./media/active-directory-identityprotection/01.png "Razina rizika")

 

Rizik događaje ili prepoznaju u **stvarnom vremenu**, ili u nakon obrade nakon događaja rizika je već preuzeli postavite (izvan mreže). Trenutno Većina rizika događaja u identiteta zaštita izvanmrežno izračunati i prikazuju se u identitet zaštite u roku od 2 do 4 sati. Dok se procjenjuje u u stvarnom vremenu, događaji u stvarnom vremenu rizika prikazat će se u konzole za zaštitu identiteta za 5 10 minuta.

Nekoliko starije verzije klijenata trenutno ne podržava otkrivanje događaja u stvarnom vremenu rizik i sprječavanje. Kao rezultat znak dodataka s tih klijentskih računala ne mogu se provjeravati sustavu ni spriječio u stvarnom vremenu.


## <a name="investigation"></a>Istraživanja
Vaš putovanja kroz identiteta zaštita pravilu počinje nadzorne ploče za zaštitu identitet. 

![Olakšava] (./media/active-directory-identityprotection/1000.png "Olakšava")

Na nadzornoj ploči omogućuje vam pristup:
 
- Izvješća kao što su **korisnici označene za rizika**, **događaji rizik** i **slabe točke**
- Postavke kao što je konfiguriranje **Sigurnosnih pravilnika**, **obavijesti** i **Registracija višestruka provjera autentičnosti**
 

Najčešće početnu točku za istraga, koji je postupak pregledavanje aktivnosti, zapisnika i ostale važne informacije vezane uz rizika događaj odlučiti hoće li se olakšava ili ublažiti korake potrebne i načinu na koji identitet je ugrožena i razumijevanje korištenja compromised identitet.

Možete sve svoje aktivnosti istrage [obavijesti](active-directory-identityprotection-notifications.md) Azure Active Directory zaštitu šalje po e-pošte.

Sljedeći odjeljci sadrže dodatne informacije i koraci koje su vezane uz istrazi.  



## <a name="what-is-a-user-risk-level"></a>Što je razinu rizika korisnika?

Na razini korisnika rizika je oznaka (visoke, Srednje ili najniža) vjerojatnost da je identitet korisnika ugrožena. To se izračunava na temelju događaje rizika korisnika koje su vezane uz identitet korisnika. 

Status rizika događaj nije **aktivna** ni **Zatvoreno**. Samo rizika događaji koje su **aktivne** pridonijeti rizika izračuna korisnika. 

Rizik razina korisničke izračunava se pomoću sljedeće unose:

- Aktivni rizika događaje koje utječu na korisnika
- Razina rizik od ovih događaja 
- Je li imati radnje olakšava izvršena 

![Korisnik rizika] (./media/active-directory-identityprotection/1001.png "Korisnik rizika")



Možete stvoriti pravila za uvjetno pristup da biste blokirali opasan korisnika iz prijave pomoću rizik razine korisnika ili tjerajte ih da biste sigurno promijenili lozinku. 


## <a name="closing-risk-events-manually"></a>Ručno zatvaranja rizika događaja

U većini slučajeva će trajati olakšava akcije kao što su sigurne za vraćanje izvorne lozinke da biste automatski zatvorili rizika događaja. No to možda nije uvijek moguće.  
To vrijedi, na primjer, kada:

- Korisnik s Active rizika događaja koji je izbrisan
- Istrazi otkriva da sadrži je korisnik provjerene izvršiti prijavljenim rizika događaja

Jer rizik događaje koje su **aktivne** doprinos rizik izračuna korisnika, možda ćete morati ručno smanjite razinu rizika zatvaranjem rizik događaje ručno.  
Tijekom trajanja istraga, možete odabrati da biste poduzeli bilo koju od ove akcije da biste promijenili status rizika događaja:

![Akcija] (./media/active-directory-identityprotection/34.png "Akcija")

- **Razrješavanje** – Ako nakon istražuje rizika događaj snimljene akciju odgovarajuće olakšava izvan identiteta zaštitu i mislite da će se događaj rizika razmatranje Zatvoreno, označiti događaj Riješeno. Riješiti događaje će Postavljanje statusa događaj rizika Zatvoreno te događaj rizika će više dopisivanje rizika korisnika.

- **Označi kao false pozitivno** - u nekim slučajevima može istražiti rizika događaja i otkrivanje da ga je neispravno označena kao na opasan. Mogu pomoći smanjiti označavanju događaj rizika kao False pozitivnog broja takve ponavljanja. To će vam pomoći strojnog učenja algoritama za poboljšanje klasifikacija slične događajima u budućnosti. Status false pozitivnog događaja je **zatvoren** i će više pridonose rizika korisnika.

- **Zanemari** – ako ne poduzmete ništa olakšava, ali rizika događaja da bi se ukloniti iz aktivnom popisu, možete označiti rizika događaj Zanemari i zatvorit će se status događaj. Zanemareni događaje povećava rizik korisnika. Tu mogućnost mogu koristiti samo neobično okolnostima. 

- **Ponovna aktivacija** - događaji rizika zatvorene ručno (tako da odaberete **riješili**, **pogrešno otkrivanje**ili **Zanemari**) možete ponovno aktivirati, postavljanja statusa događaj natrag na **aktivni**. Ponovno aktivirani rizika događaje dopisivanje razine izračuna rizika korisnika. Ne možete ponovno aktivirati rizika događaje zatvoren kroz olakšava (kao što su sigurne lozinke ponovno postavljanje). 




**Da biste otvorili dijaloški okvir za konfiguraciju povezane**:

1. Na plohu **Azure AD identiteta zaštita** u odjeljku **Investigate**kliknite **rizika događaja**.

    ![Ponovno postavljanje lozinke za ručno] (./media/active-directory-identityprotection/1002.png "Ponovno postavljanje lozinke za ručno")

2. Na popisu **rizika događaji** kliknite rizika.

    ![Ponovno postavljanje lozinke za ručno] (./media/active-directory-identityprotection/1003.png "Ponovno postavljanje lozinke za ručno")

2. Na plohu rizika desnom tipkom miša kliknite korisnika.

    ![Ponovno postavljanje lozinke za ručno] (./media/active-directory-identityprotection/1004.png "Ponovno postavljanje lozinke za ručno")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Ručno zatvaranja svih događaja odgovornost za korisnika

Umjesto da ručno pojedinačno zatvaranja događaje odgovornost za korisnika, Azure Active Directory identiteta zaštita također nudi način da biste zatvorili sve događaje za korisnika jednim klikom.


![Akcija] (./media/active-directory-identityprotection/2222.png "Akcija")

Kada kliknete **odbacili sve događaje**, zatvorite sve događaje i problematične korisnika nije više rizik.



## <a name="remediating-user-risk-events"></a>Remediating događaje rizika korisnika

Na olakšava je akcija za zaštitu identitet ili uređaj koji je prethodno sumnjivu ili zna da biti ugrožena. Olakšava akcija vraća identiteta ili uređaj sigurni stanje, a razrješava prethodne rizika događaje vezane uz identiteta ili uređaj.

Da biste remediate korisnik rizika događaje, možete učiniti sljedeće:

- Izvođenje sigurne za vraćanje izvorne lozinke da biste ručno remediate korisnik rizika događaja 

- Konfiguriranje korisničkih pravila zaštite rizika prevladavanje ili automatski remediate korisnik rizika događaja

- Ponovno slika zaražene uređaja  


### <a name="manual-secure-password-reset"></a>Ručno sigurne ponovno postavljanje lozinke

Sigurne lozinke je učinkovitih olakšava za mnoge rizika događaje, a kada izvršena, automatski zatvara tih rizika događaja i ponovno izračunava rizika razina korisničke. Na nadzornoj ploči identitet zaštitu možete koristiti da biste započeli za vraćanje izvorne lozinke za opasan korisnika. 

Dijaloški okvir povezani nudi dvije različite načine za vraćanje lozinke:

**Ponovno postavljanje lozinke** – odaberite **korisnik mora ponovno postaviti lozinku** da biste korisniku omogućili koja se sama oporaviti ako korisnik ima registriran za višestruke provjere autentičnosti. Tijekom korisnika sljedeće prijave, korisnik bit će potrebno da biste riješili višestruka provjera autentičnosti test uspješno i pa prisilno za promjenu lozinke. Ta mogućnost nije dostupna ako korisnički račun nije već registrirane višestruke provjere autentičnosti.

**Privremena lozinka** – odaberite **Generiraj privremenu lozinku** da biste odmah poništiti valjanost postojeću lozinku pa stvorite novu privremenu lozinku za korisnika. Novoj privremenoj lozinci poslati na adresu zamjenska e-pošte za korisnika ili upravitelja korisnika. Budući da je privremenu lozinku, korisnik će se zatražiti da biste promijenili lozinku prilikom prijave.


![Pravila] (./media/active-directory-identityprotection/1005.png "Pravila")


**Da biste otvorili dijaloški okvir za konfiguraciju povezane**:

1. Na plohu **Azure AD identiteta zaštita** kliknite **korisnici označene za rizika**.

    ![Ponovno postavljanje lozinke za ručno] (./media/active-directory-identityprotection/1006.png "Ponovno postavljanje lozinke za ručno")


2. Na popisu korisnici odaberite korisnika s najmanje jedan rizika događajima.

    ![Ponovno postavljanje lozinke za ručno] (./media/active-directory-identityprotection/1007.png "Ponovno postavljanje lozinke za ručno")


2. Na plohu korisnika kliknite **ponovno postavljanje lozinke**.

    ![Ponovno postavljanje lozinke za ručno] (./media/active-directory-identityprotection/1008.png "Ponovno postavljanje lozinke za ručno")





## <a name="user-risk-security-policy"></a>Pravila za sigurnosni rizik korisnika

Korisnička pravila sigurnosni rizik je uvjetno pristup pravila koja se vrednuje razinu odgovornost za određenog korisnika i primjenjuje olakšava i ublažiti akcije na temelju unaprijed definirane Uvjeti i pravila.


![Korisnička ridk pravila] (./media/active-directory-identityprotection/1009.png "Korisnička ridk pravila")


Zaštita Azure AD identiteta pomaže vam upravljanje ublažiti i potražite alat za korisnika označene za rizika omogućujući vam da:

- Postavite korisnike i grupe pravilo primjenjuje se na: 

    ![Korisnička ridk pravila] (./media/active-directory-identityprotection/1010.png "Korisnička ridk pravila")


- Postavljanje korisnika rizika razine praga (Nisko, Srednje ili visoko) koji se pokreće pravila: 

    ![Korisnička ridk pravila] (./media/active-directory-identityprotection/1011.png "Korisnička ridk pravila")


- Postavljanje kontrole za provesti kada pravila:

    ![Korisnička ridk pravila] (./media/active-directory-identityprotection/1012.png "Korisnička ridk pravila")


- Promijenite stanje pravilo:

    ![Korisnička ridk pravila] (./media/active-directory-identityprotection/403.png "Registracija za MFA")


- Pregledajte i procjenu posljedice promjena prije nego što aktivirate je:

    ![Korisnička ridk pravila] (./media/active-directory-identityprotection/1013.png "Korisnička ridk pravila")


Odabir **visoke** prag smanjuje broj puta pravilo se aktivira i u skladu s tim minimizira utjecaj na korisnike.
Međutim, isključuje **najniža** i **Srednja** korisnici označene za rizika od pravila, možda sigurne identiteta ili uređaje koje ste prethodno sumnjivu ili zna da biti ugrožena.

Prilikom postavljanja pravilnika,

- Isključivanje korisnika koji su vjerojatno da biste generirali mnogo false – positives (razvojnim inženjerima, sigurnost analitičar analitičar podataka)

- Isključivanje korisnika u regionalne sheme koju Omogućivanje pravilnik nije praktično (na primjer bez pristupa službom za korisnike)

- Korištenje **visoke** prag tijekom početne pravila uvođenju, ili ako morate minimizirati izazove vidjeti krajnjim korisnicima.

- Ako tvrtka ili ustanova zahtijeva veću sigurnost, koristite **najniža** praga. Odabir **najniža** prag predstavlja dodatne korisničke prijavu izazove, ali povećana sigurnost.

Preporučena zadano za većinu tvrtki ili ustanova je da biste konfigurirali pravilo za **Srednje** praga za pak postizanje ravnoteže između upotrebljivosti i sigurnost.

Pregled povezanih korisničkog sučelja potražite u članku:

- [Tijek za oporavak računa Compromised](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Račun Compromised blokirane tijek](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Da biste otvorili dijaloški okvir za konfiguraciju povezane**:

1. Na plohu **zaštitu Azure AD identiteta** , u odjeljku **Konfiguriranje** kliknite **pravila rizika korisnika**.

    ![Korisnička ridk pravila] (./media/active-directory-identityprotection/1009.png "Korisnička ridk pravila")






## <a name="mitigating-user-risk-events"></a>Mitigating korisnika rizika događaja
Administratori mogu postaviti pravila zaštite rizika korisnika da biste korisnicima onemogućite nakon prijave ovisno o razini rizika. 

Blokiranje na prijave:
 
- Sprječava generacije nove događaje rizika korisnika za problematične korisnika

- Ručno remediate događaje rizika utjecaja identitet korisnika i vraćanje sigurne stanje administratorima omogućuje



## <a name="what-is-a-sign-in-risk-level"></a>Što je razinu rizika za prijavu?

Razine odgovornost za prijavu je oznaka (visoke, Srednje ili najniža) vjerojatnost da za na određeni prijavu, netko drugi pokušava uspješnoj identitet korisnika. Razinu rizika za prijavu procjenjuje u trenutku u prijavu i smatra rizika događaje te pokazatelja otkriven u stvarnom vremenu za taj određeni prijavu. 

## <a name="mitigating-sign-in-risk-events"></a>Događaji mitigating odgovornost za prijavu 
Na ublažiti je akcija ograničiti mogućnost napadaču izrabljuje ugrožena identiteta ili uređaj bez vraćanja identiteta ili uređaj sigurni stanje. Na ublažiti riješiti prethodne odgovornost za prijavu događaje vezane uz identiteta ili uređaj.

Uvjetno access možete koristiti u Azure AD identiteta zaštite u automatski smanjenju rizika za prijavu događaja. Pomoću tih pravila, preporučujemo da rizika razinu korisnika ili prijavu da biste blokirali opasan znak dodataka ili od korisnika da biste izvršili višestruke provjere autentičnosti. Akcije može spriječiti exploiting ukradeno identiteta oštećenja napadaču te vam dati neko vrijeme da se sigurne identitet. 


## <a name="sign-in-risk-security-policy"></a>Pravilnik o sigurnosti odgovornost za prijavu

Pravilo odgovornost za prijavu je uvjetno pristup pravila koja se vrednuje rizika na određene prijaviti i primjenjuje mitigations utemeljenog na unaprijed definirane Uvjeti i pravila.

![Pravila odgovornost za prijavu] (./media/active-directory-identityprotection/1014.png "Pravila odgovornost za prijavu")


Zaštita Azure AD identiteta olakšava upravljanje ublažiti opasan znak dodataka omogućujući vam da:

- Postavite korisnike i grupe pravilo primjenjuje se na: 

    ![Pravila odgovornost za prijavu] (./media/active-directory-identityprotection/1015.png "Pravila odgovornost za prijavu")


- Postavite odgovornost za prijavu razine praga (najniža, Srednje ili visoko) koji se pokreće pravila: 

    ![Pravila odgovornost za prijavu] (./media/active-directory-identityprotection/1016.png "Pravila odgovornost za prijavu")


- Postavljanje kontrole za provesti kada pravila:  

    ![Pravila odgovornost za prijavu] (./media/active-directory-identityprotection/1017.png "Pravila odgovornost za prijavu")
    

- Promijenite stanje pravilo:

    ![Registracija za MFA] (./media/active-directory-identityprotection/403.png "Registracija za MFA")

- Pregledajte i procjenu posljedice promjena prije nego što aktivirate je: 

    ![Pravila odgovornost za prijavu] (./media/active-directory-identityprotection/1018.png "Pravila odgovornost za prijavu")


### <a name="what-you-need-to-know"></a>Što trebate znati

Možete konfigurirati pravila zaštite odgovornost za prijavu obavezne višestruke provjere autentičnosti:

![Pravila odgovornost za prijavu] (./media/active-directory-identityprotection/1017.png "Pravila odgovornost za prijavu")

Međutim, sigurnosnih vam razloga Ova postavka funkcionira samo za korisnike koji su već registrirane za višestruke provjere autentičnosti. Ako se uvjet obavezne višestruka provjera autentičnosti zadovoljeni za korisnike koji još nije registriran za multi-factor authentication, korisnik je blokirana. 

Kao preporučenim načinom rada, ako želite obavezan višestruke provjere autentičnosti za opasan znak dodatke, trebali biste:

1. Omogućiti [višestruke provjere autentičnosti Registracija pravila](#multi-factor-authentication-registration-policy) za zahvaćene korisnike.
2. Obavezan zahvaćene korisnike da biste se prijavili u sesiji koje nisu opasan za izvođenje MFA Registracija

Tih koraka osigurava višestruke provjere autentičnosti za je potreban je opasan prijavu. 


### <a name="best-practices"></a>Najbolje prakse

 
Odabir **visoke** prag smanjuje broj pojavljivanja pravilo se aktivira i minimizirati utjecaj na korisnike.  
 
Međutim, isključuje **najniža** i **Srednja** znak dodataka označene za rizika od pravila, možda blokira napadaču s exploiting compromised identitet. 

Prilikom postavljanja pravilnika, 

- Isključivanje korisnicima koji nemaju / ne smije sadržavati višestruka provjera autentičnosti

- Isključivanje korisnika u regionalne sheme koju Omogućivanje pravilnik nije praktično (na primjer bez pristupa službom za korisnike)

- Isključivanje korisnika koji su vjerojatno da biste generirali mnogo false – positives (razvojnim inženjerima, sigurnost analitičar analitičar podataka)

- Korištenje **visoke** prag tijekom početne pravila uvođenju, ili ako morate minimizirati izazove vidjeti krajnjim korisnicima.

- Ako tvrtka ili ustanova zahtijeva veću sigurnost, koristite **najniža** praga. Odabir **najniža** prag predstavlja dodatne korisničke prijavu izazove, ali povećana sigurnost.

Preporučena zadano za većinu tvrtki ili ustanova je da biste konfigurirali pravilo za **Srednje** praga za pak postizanje ravnoteže između upotrebljivosti i sigurnost.

 
Pravila odgovornost za prijavu je:

- Primjenjuje se na sve promet za preglednik i znak dodataka pomoću modernom provjere autentičnosti.
- Ne primjenjuje se na aplikacije koje koriste starije sigurnosne protokole onemogućivanjem krajnju točku WS pouzdanost pri pridruženim IDP, kao što su ADFS.

Stranici **Rizika događaja** na konzoli za zaštitu identiteta popis svih događaja:

- Ovo pravilo zatvorena
- Možete pregledati aktivnosti i odrediti je li akciju odgovarajuće ili ne 

Pregled povezanih korisničkog sučelja potražite u članku:

- [Opasan oporavak za prijavu](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Opasan prijavu blokiran](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Višestruka provjera autentičnosti Registracija tijekom na opasan prijavu](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Da biste otvorili dijaloški okvir za konfiguraciju povezane**:

1. Na plohu **zaštitu Azure AD identiteta** , u odjeljku **Konfiguriranje** kliknite **pravila odgovornost za prijavu**.

    ![Korisnička ridk pravila] (./media/active-directory-identityprotection/1014.png "Korisnička ridk pravila")





## <a name="multi-factor-authentication-registration-policy"></a>Višestruka provjera autentičnosti Registracija pravila

Azure višestruka provjera autentičnosti je način potvrđivanja tko ste koji zahtijeva korištenje više od samo korisničko ime i lozinku. Pruža drugu razinu zaštite znak dodaci za korisnika i transakcije.  
Preporučujemo da zahtijevate Azure višestruke provjere autentičnosti za prijavu dodaci korisnika jer je:

- Nudi Jaka provjera autentičnosti s raspon mogućnosti jednostavno provjere

- Reproducira ulogu u pripremi tvrtke ili ustanove da biste zaštitili i oporavak računa kompromisa

![Korisnička ridk pravila] (./media/active-directory-identityprotection/1019.png "Korisnička ridk pravila")



Dodatne informacije potražite u članku [što je Azure višestruka provjera autentičnosti?](../multi-factor-authentication/multi-factor-authentication.md)


Zaštita Azure AD identiteta olakšava upravljanje snimljene fotografije iz višestruka provjera autentičnosti Registracija konfiguriranjem pravila koja vam omogućuje: 



- Postavite korisnike i grupe pravilo primjenjuje se na: 

    ![Pravilnik za MFA] (./media/active-directory-identityprotection/1020.png "Pravilnik za MFA")



- Postavljanje kontrole za provesti kada pravila::  

    ![Pravilnik za MFA] (./media/active-directory-identityprotection/1021.png "Pravilnik za MFA")


- Promijenite stanje pravilo:

    ![Pravilnik za MFA] (./media/active-directory-identityprotection/403.png "Pravilnik za MFA")

- Pregled trenutnog statusa Registracija: 

    ![Pravilnik za MFA] (./media/active-directory-identityprotection/1022.png "Pravilnik za MFA")


Pregled povezanih korisničkog sučelja potražite u članku:

- [Višestruka provjera autentičnosti Registracija toka](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Višestruka provjera autentičnosti Registracija tijekom na opasan prijavu](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Da biste otvorili dijaloški okvir za konfiguraciju povezane**:

1. Na plohu **zaštitu Azure AD identiteta** , u odjeljku **Konfiguriranje** kliknite **Registracija višestruke provjere autentičnosti**.

    ![Pravilnik za MFA] (./media/active-directory-identityprotection/1019.png "Pravilnik za MFA")





## <a name="next-steps"></a>Daljnji koraci

 - [Kanal 9: Azure AD i prikaz identiteta: Pretpregled zaštitu identiteta](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Vrste događaja rizika otkrio Azure Active Directory identiteta zaštitu](active-directory-identityprotection-risk-events-types.md)
 - [Slabe točke otkrio Azure Active Directory identiteta zaštitu](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory identiteta zaštita obavijesti](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory identiteta zaštita playbook](active-directory-identityprotection-playbook.md)
 - [Pojmovnik Azure Active Directory identiteta zaštitu](active-directory-identityprotection-glossary.md)

 - [Prijava pomoću značajke zaštite Azure AD identiteta](active-directory-identityprotection-flows.md)

 - [Omogućivanje zaštitu identiteta Azure Active Directory](active-directory-identityprotection-enable.md)
 - [Azure Active Directory identiteta zaštita – upute za Omogućivanje korisnicima](active-directory-identityprotection-unblock-howto.md)

 - [Početak rada s Azure Active Directory identiteta zaštitu i Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


