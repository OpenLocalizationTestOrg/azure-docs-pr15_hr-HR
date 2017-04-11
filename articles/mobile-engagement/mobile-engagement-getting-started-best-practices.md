<properties 
    pageTitle="Vodič za Azure mobilne radnje početak rada s najbolje prakse"
    description="Početak vodič za početak rada za Azure Mobile radnje i najbolje prakse za uhodavanje" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure mobilne radnje - priručnik za početak rada s najbolje postupke

## <a name="overview"></a>Pregled

**Zaslonu mobilnih uređaja je prostor za vrlo prenatrpana:** U 2013 na studije pronaći average mobilnog uređaja imali 27 instalirane aplikacije. Korisnici obično potrošeno 30 sati mjesečno na svojih aplikacija. Većina ovaj put je utrošeno na društvenu mrežu i igranje (oko 20 sati). Po 2014., Android market imao oko 1,5 milijuna aplikacije za korisnike možete birati. Iz trgovine Apple sadržavao oko 1.2 milijuna aplikacije. Korištenje mobilne aplikacije i dalje povećava kao što je razvojni inženjeri se natječu u ovoj rast tržištu. 

Prosječna mobilni korisnik će instalacija i Deinstalacija aplikacije s velika učestalost ovisno o promjeni interesa i sučelja u aplikaciju. Da biste odredili uspjeh aplikacije postaje ključni Saznajte više o broju korisnika instalirati aplikaciju. Važno je znati kako korisno je aplikacija i ako se mijenja koji teže korištenje. Važno postaju na sljedeća pitanja:

- Korisnici su početka da biste pronašli aplikacije uninteresting ili zastarjeli? 
- Koliko korisnika da su zaustavljena uopće korištenja aplikacije? 
- U aplikaciji Nabava popularne prema gore ili prema dolje?
- Su korisnici ne uspijeva da biste dovršili tijekove rada zbog problemi vezani uz aplikacije i nedostatak koja vas zanima? 
- Nije moguće zadržite aplikacije koristan i odgovarajući pritiskom najnoviji sadržaj na bazu korisnika? 
- Bi ovaj najnoviji sadržaj biti ista za sve korisnike ili filtriran na segmenata korisnika koji se temelji na ponašanje u aplikaciju? 
 
Odgovori na pitanja slično te mogu pomoći pri širenje živost i prihoda iz aplikacije. Oni i omogućuju definiranje i zadržati svoje baze korisnika. 

Medijskih sadržaja povezane su aplikacije da bi se neke od najveće zadržavanja među korisnicima. Jedan od razloga za taj se oni stalno su omogućuje korisnicima najnoviji sadržaj. Prijevremeni prihvaćanja korisne automatske obavijesti usmjereni na određeni korisnički segment ima imati visoke utjecaj na zadržavanja aplikacije. 

Program Azure Mobile radnje namijenjen je da biste lakše proširivanje živost i zadržavanja aplikacije unosom način da biste prikupili i analizirati detaljne informacije o korištenju aplikacije. To će vam pomoći klasifikaciju korisnički osnovni prema ponašanje i stvaranje kojoj je žarište kampanja za slanje obavijesti i poruka u aplikaciji odsječaka identificirani korisnika. Ključni pokazatelji uspješnosti (KPI) Izmjerite kako aktivni korisnici su uz različite aspekte aplikacije. Azure radnje Mobile omogućuje metode morate odrediti sljedeće KPI-ja. Omogućuje povećavanje povrat na Uloženo (pi) unosom infrastrukture ćete morati dodatno angažirali pomoću aplikacije za mobilne uređaje. 

Da bi se korištenje svih pogodnosti Azure Mobile radnje, morate započeti s plan dobro dizajniranih radnje. Plan će vam olakšati prepoznavanje zrnastog podataka morat ćete moći fazi vaša osnovna korisnika. To se temelji na ponašanje i sučelja u aplikaciju. Na umu da namjeravate biti uspješno je najbolje jasnu definiciju KPI koji će mjere ciljevi aplikacije. S Očisti pokazatelji definirane, jednostavno možete ugraditi potrebne logike u aplikaciji za prikupljanje precizno grained podataka koji će se koristiti za analizu i procjenu vaše KPI-ja. U ovoj se temi je najbolja praksa vodič za definiranje KPI-jeve koje ćete koristiti s plan radnje. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Korak 1: Definiranje KPI-ja tako da stane u model PRIJEDLOGA


Pravilno definiranje KPI-ja može biti teško zadatak da biste dovršili. Aplikacija namijenjene različite delatnosti imaju vlastite specifičnosti i ciljeva. To su zbuniti vaš pristup. Da biste izbjegli ovo, ciljevi i KPI-ja treba podijeliti u tri glavna kategorije: **tvrtke**, **radnje**i **tehničke**. Ovo nazivamo **PRIJEDLOGA modela**.

Dobar plan imat će obično ciljevi s KPI-koji mjere uspjeha u svakom od sljedećih kategorija modela PRIJEDLOG.


#### <a name="business-kpis"></a>Ključni pokazatelji uspješnosti tvrtke

Ključni pokazatelji uspješnosti tvrtke mora biti najlakši dio da biste sastavili. Ste vjerojatno je već definirali ih u neki oblik kada planirano aplikacije za mobilne uređaje. Ove ključne pokazatelje uspješnosti obično pomoći mjera prihoda i povrat ulaganja za koju aplikaciju. Sljedeći popis sadrži neke ključne pokazatelje uspješnosti tvrtke koji vam mogu pomoći da će vas voditi prilikom definiranja vaše pokazatelji primjer:

- Medijskih sadržaja tvrtke KPI-ja
    - Broj reklame kliknuli
    - Broj stranice posjeta po korisniku
    - Broj trenutne pretplate
- Igraće tvrtke KPI-ja
    - Broj Kupovina u aplikaciji
    - Prosječni prihod po korisniku (ARPU)
    - Vrijeme utrošeno po sesiji
    - Dani reproduciranih i trenutni igraće razine
- Ključni pokazatelji uspješnosti tvrtke E-trgovine
    - Dana aplikacije koji se koristi
    - Prosječni prihod po korisniku (ARPU)
    - Prosječni iznos u košaricu pri završetku kupnje
    - Kategorija proizvoda za većinu prikaza i nabavu
- Banke i osiguranje tvrtke KPI-ja
    - Broj računa
    - Značajke koje su aktivirane
    - Stranica za ponudu posjetili
    - Upozorenja kliknuli ili aktiviran      



#### <a name="engagement-kpis"></a>Ključni pokazatelji uspješnosti radnje

KPI u radnje je pokazatelj uspješnosti za mjerenje radnje korisnika. Trendova u tom području utvrditi zadržavanja aplikacije. Evo nekoliko primjera pokazatelji za tu vrstu KPI-ja:

- Aktivni korisnici u zadnjih sedam dana
- Neaktivni broja za zadnjih sedam dana
- Broj korisnika koji ste koristili aplikaciju u 30 dana  

Neke očite vanjski čimbenici može utjecati pokazatelja u ovom području. Na primjer, razmotrite mobilni uređaj da biste se s korisnikom cijelo vrijeme. Možda ili možda neće biti true. Igraće aplikacije često može imati veći korištenje oko praznike kada je gamer može reproducirati više tijekom izvan ureda i smanjivanje obrazovne ustanove.   

Dobro definiran KPI-ja u ovoj kategoriji treba vam mjerenje odnos između aplikacije i klijentima.



#### <a name="technical-kpis"></a>Tehnički KPI-ja

Pokazatelji iz te kategorije pomoći pri određivanju ako aplikacije se ponaša pravilno, viseće ili ruši. Te pokazatelje možete mjerenje stanja aplikacije i upotrebljivosti probleme koji mogu korisnicima onemogućiti korištenje aplikacije. Podaci prikupljeni za kategoriju može sadržavati i informacije o performansama koji će biti relevantan za marketing timovima. Podaci i mogu biti korisni za otklanjanje poteškoća prema IT i podrška timovima da vam pomogne identificirati neprijavljenih programskih pogrešaka. 
 
Evo nekih primjera tehničke KPI-ja:

- Podaci o iznimci neobrađenu ili obrađene i count 
- Vremenska oznaka zadnjeg rušenje
- Netko klikne gumb posljednji ili posjetili posljednjoj stranici
- Korištenje memorije aplikacije
- Broj prikazanih aplikacije
- Verzija OS-a aplikacije na kojoj se izvršava
- Verzije aplikacije

Definiranje te KPI-ja da biste poboljšali performanse aplikacije mjere i pinpoint potencijalne programskih pogrešaka. U ovom pokazatelja trebala bi smanjiti vrijeme potrebno za isporuku popravak za klijente. Nije moguće i pomažu određivanje korisnički segment koji je naišao određeni problemi. Taj korisnik segmente možete koristiti da biste stvorili kampanja za isporuku obavijesti vezanih uz raspoloživih popravaka i potencijalne promocije za oporavak zadovoljstva korisnika. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook vježbu 1: Stvaranje nadzorne ploče KPI-JA

Pri definiranju strategije marketinške vaše KPI-ja trebaju prezentirati prikaza za sve svoje glavni ciljeve. Trebali bi jasno definiranih točaka omogućit će vam prikupljanje ključnih informacija praćenje aplikacije i ponašanja krajnji korisnik.

Stvaranje KPI-JA nadzorne ploče koje sadrži u nastavku informacije

1.  Što su KPI-ja za aplikaciju?
2.  Točke koje podataka će se koristi se za predstavljanje svaki KPI-JA?
3.  Gdje se nalazi te podatke za Moje aplikacije (odnosno zaslona, postavke, sustav...)?
4.  Moguće reproducirati u nizu radnje za KPI-JA?

Radni list **Builder KPI-JA** možete koristiti u našem [Media Playbook predložak] [ Media Playbook link] Primjeri i smjernice.



## <a name="step-2-your-engagement-program"></a>Korak 2: Korištenje programa


Sjajan mobilne radnje program razmatranje ključa dio aplikacije. To apsolutno mora obuhvaćati sjajno dobrodošlice programa koji izvršava za korisnike tijekom prvog dana upotrebe aplikacije. To ima vrlo pozitivno učinak na radnje i zadržavanja aplikacije. Studije ste pokazali da većina korisnika prestali koristiti aplikaciju prvih nekoliko dana nakon instalacije. Želite li pokušajte zadovoljavaju ili biti dulji od kupca očekivanja vožnju kamata ranijeg dok korisnik i dalje filtriran na aplikaciju. Provjerite je li izlaganje vrijednost ključa i pogodnosti paketa aplikacije klijentima. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Automatske obavijesti su najbolje pristup za Prijevremeni obaveze s korisnicima na mobilnom uređaju. Sjajan Oprez se pak treba uzeti segmentiranjem korisnika za slanje obavijesti. Jer kada korisnik izgleda kao što su primanja neželjene e-pošte ili uninteresting obavijesti može imati ozbiljne utječu na. U nekoliko klikova korisnik može izbrisati aplikacije nikad ne da biste se vratili. Korisnik mora prima iznimno prilagođene vrijednosti u aplikaciji umjesto generički neželjene e-pošte.

Kada korisnici su aktivno aktivan, zatim programa radnje može pridonijeti pogon ostalih aspekata aplikaciju.

Ako, primjerice, nije moguće postavljanje kampanje koji zahtijeva aktivni korisnici ocjenjivanje aplikacije. Budući da ovaj korisnički segment je aktivna najčešće i sadrži aplikacije u većini okruženje s vama, što biste očekivali ih da bi se dobilo najčešće točne ocjena. Nakon što dodate ocjenu visoke aplikacije, može pomoći pogon gore prirodno preuzimanje aplikacije kao i smanjivanje nove troškove acquisition klijenta.



#### <a name="engagement-sequence"></a>Slijed radnje


Globalni radnje Program obuhvaća nizove različite radnje. Svaki niz trebao dosegne nekoliko ciljeva.


###### <a name="life-push-sequence"></a>Vijek automatske niza


Ciljevi za slijed automatske vijek razlikuju se ovisno o životnom ciklusu radnje korisnika pomoću aplikacije. Određeni korisnik može biti novi, Neaktivni ili vrlo active. Na različite faze za životni ciklus radnje, korisnici možda pogodnost vaše najnoviji sadržaj u obliku savjeti ili veze u dokumentaciji. 

Primjer novog korisnika možda potrebna pomoć početak snalaženje u aplikaciju programa sustavu ili im je novi korisnik incentive sličnu ovoj prvo ih pokrenite aplikaciju...

*"Drago da bi vam se! Imajte na umu prijava da biste dobili 1 mjesec besplatno!"*


###### <a name="behavioral-push-sequence"></a>Slijed behavioral pribadače

Slijed behavioral automatske trebao da biste povećali korištenje koji se temelji na ponašanje korisnika koji se prikupljaju za aplikaciju.  

Ako, na primjer, vrlo aktivnog korisnika u aplikaciji nogometu likova mogu koristiti se aktivan uz sljedeće automatske obavijesti...

*"Nevena su ozbiljne nogometu Lepeza! Prijavite se u našem NFL sekcije i osvajaju besplatne pristup na SuperBowl!"*


###### <a name="alerting-push-sequence"></a>Upozorenjem automatske niza

Korisnici će Hvala relevantne novosti filtriran na njihovim interesima. U nizu upozorenja automatske poboljšava radnje slanjem upozorenja na temelju interesima korisnik ima jasno prikazano. To može biti eksplicitnih kada korisnik odabere vlastite interese u aplikaciji. To nije moguće i određivanja implicitno na temelju podataka koji se prikupljaju tijekom interakcije s korisnikom pomoću aplikacije.

Ako, na primjer, korisnik aplikacije za E-trgovine redovito kupiti određene marku primjer koji ste zabilježene sa business KPI-JA. Sljedeće upozorenje možete poboljšati radnje za tog korisnika pomoću aplikacije.
 
*"Najviša Wes, jednu od omiljene vrste primjer će se prikazivati na Prodaja 25% prvi tjedan u rujnu 2015. Hvala vam kao klijenta i željeli da biste bili sigurni ste svjesni."*

###### <a name="rentention-push-sequence"></a>Slijed automatske Rentention

U ovom nizu trebao da biste zadržali korisnika putem kampanja za obavijesti koji se ponavljaju automatske radi pogon običnog habit od zanimljivih pomoću aplikacije. To može pomoći povećati zadržavanja aplikacije ako korisnik enjoys interakcija. 

Ako, na primjer, korisnika aplikacije povezane Sportovi primiti sljedeću obavijest automatske tjednih na temelju omiljene timovima korisnika:

*"Za mogućnost osvajaju 200 bodova, otvorite glasovanja li Yankees New York kod ovaj tjedan protiv Toronto plava Jays osvajaju njihove utakmica!"*


#### <a name="the-3w-approach"></a>Pristup 3W

Mastering nizove različite automatske pomoći će vam sudjelovati s krajnjim korisnicima. Međutim, i dalje trebate koristiti 3W pristup za stvaranje osobnog dojma obavijesti. Pristup 3W treba tko adresa što i kada za svaki obavijesti. Ako ste jasno ispunjavaju ta tri pitanja vam obavijesti moraju biti ispravno odnosila za radnje.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Tko: Korisnik fazi koje će primati poruke

Odaberite obavijesti korisnicima razmatranje vrlo osjetljive komunikacije kanala. Provjerite je li obavijesti usmjerite da biste poslali korisnički segment i iz djelokruga uz interese taj korisnički segment. Obavijest o neispravno usmjerena vrlo je vjerojatno imati negativan utječu na na korisniku. Oni razmotrite ga neželjene pošte oni mogu dovesti do traje Deinstaliranje aplikacije. 

Pri definiranju segmenata korisnika koji će primati obavijesti, poslužite se kombinacijom određene tehničke i behavioral kriterija. Definiranje korisnički segment jednostavnog primjera može biti sličan sljedeću naredbu:

"Svi korisnici koji su pokrenuti na mobilnu aplikaciju za prvu vrijeme 3 dana i ste posjetili na stranicu za prijavu dvaput bez zapravo dovršetka prijave".
 
Ta izjava olakšava prepoznavanje podataka, morat ćete prikupljanje za podršku određene scenarij.


###### <a name="what-the-message-that-you-will-send"></a>Što je: Poruku koju šaljete

**Tona**

U vaše obaveze pomoću nijansu koji odgovara vašem za segmentiranog korisnike. To je sigurno dobar način možete povezivati s vašeg krajnjim korisnicima i Promicanje kamata korisnika u svojoj aplikaciji. 

**Preusmjeravanje**

Automatske obavijesti može se koristiti više od otvaranja gore aplikacije. Ako poruke s obavijesti omogućuje kontekstu kao što je emitiranje vijesti ili promotivne ponude proizvoda, ta se obavijest možda precizno izravnu vezu pravi sadržaj u aplikaciji. Da biste to podržava, morate stvoriti shemu URL-a da biste omogućili aplikacije upravljanje preusmjeravanje. Kada radite na vašem nizove radnje, to je važan korak koji morate zaboravili.

Preusmjeravanje je moguće upravljati i za druge sustave. Ako, primjerice, s URL akcije moguće je preusmjeravanje krajnjim korisnicima na mnogim drugim sustavima uključujući sljedeće:

- Web-mjesto
- Poštanski sandučić s već postavljanje e-pošte
- Okvir za SMS PORUKE
- Servis za biranje
- Izravno u aplikaciju za pohranu za ocjena aplikacije. 

To pruža mnoštvo mogućnosti za sudjelovati krajnjim korisnicima i stvaranje pravila automatskih da biste poboljšali izvedbe.


**Oblikovanje i sadržaj**

Različite vrste i oblici automatske obavijesti:

1. **Objava** : omogućuje vam slanje poruka oglašavanje korisnika na drugu sekundi dok (iz aplikacije u aplikaciji ili bilo kojem trenutku).
2. **Ankete** : omogućuju Prikupite informacije s krajnjim korisnicima tako da biste zatražili pitanja. Te odgovore dostupne zatim prilikom stvaranja kriterija krajnjim korisnicima cilj.
3. **Ih gura podataka** : omogućuje vam slanje binarni ili base64 podatkovnu datoteku da biste ažurirali aplikaciju. Informacije koje se nalaze u podataka automatske se šalje aplikacije da biste prilagodili zadovoljstva korisnika u svojoj aplikaciji. Aplikacija mora biti dizajnirane za podršku podatke u podataka pribadače.
4. **Pločice (samo za Windows Phone)** : omogućeno korištenje sustava Microsoft automatske obavijesti servisa (MPNS) da biste poslali izvorni automatske obavijesti koja sadrži XML podataka (podržani od SDK verzija 0.9.0. Konačni opseg za pločice ne može biti dulji od 32 kilobajta.). Ova se poruka pojavljuje se izravno na panel za pločice.
5. **Prikaza** : web-prikaz je skočni koji sadrže web-sadržaja. U ovom se skočni prozor se pojavljuje kada krajnji korisnik ima nakon klika na automatske obavijesti. Web-prikaz omogućuje vam više interakciju s krajnji korisnik.
 
>[AZURE.NOTE] Pripazite da sadržaj šaljete kao automatske obavijesti uspostavu odgovarajući platformu (iOS, Android, Windows) smjernice za razvoj aplikacija i slanje automatskih obavijesti.

 


###### <a name="when-the-timing-of-your-campaign"></a>Kada: Tempiranja kampanje

Kada je to najbolje vrijeme za aktiviranje kampanje pokretanje automatske obavijesti? Treba li biti ručnu ili automatsku? Trebali biste ga se ponavlja? Određivanje pravo vrijeme ili učestalost je ključna sudjelovati korisnici s najbolje rezultate. Za svaki niz radnje i scenarij, potrebno je odrediti kada će se najbolje vrijeme za slanje automatskih obavijesti. Evo nekoliko mogućih primjera:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Ako šaljete mnogo obavijesti svakodnevno, morate preuzeti ozbiljne razmotriti vaši korisnici mogu to komunikacije kao neželjena pošta. 

Azure radnje Mobile omogućuje dva načina da biste izbjegli komunikacije koji se izgledati kao neželjene e-pošte. Najprije pomoću precizno Žitne segmente da biste bili sigurni da neće usmjeriti istim korisnicima. Uz to, Azure Mobile radnje sadrži značajku "kvote". Tu značajku možete ograničiti obavijesti koje se šalju kampanje. Ako, na primjer, postavka zadanog kvote za 5 po tjednu će Pobrinite se da korisnik kao dio korisnički segment kampanje prima najviše 5 obavijesti za taj dan.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook vježbu 2: Stvaranje programa radnje

Potrošite neko vrijeme sažimanja svoje ciljeve i definiranje kampanje očekujete da biste reproducirali pomoću određenih nizove. Provjerite je li primijeniti pristup 3W obavijesti u kampanja. 

Koristite radni list **Programa radnje** u [Predložak Playbook Media] [ Media Playbook link] Primjeri i smjernice.


## <a name="step-3-app-integration"></a>Korak 3: Integracija aplikacije


#### <a name="create-a-tag-plan"></a>Stvaranje plana oznaka

Integracija Azure Mobile radnje u aplikaciju morat ćete stvoriti plan oznaka. Planiranje oznaka je temelja projekta. Definira odnos između marketinške specifikacije, tijek rada aplikacije i realni oznake podataka prikupljenih u aplikaciji za mjerenje KPI-ja. Označava koje analize moći da biste vidjeli na portalu. Također olakšava vam definiranje segmenata korisnika i slanje kojoj je žarište automatske obavijesti da biste sudjelovati na krajnjim korisnicima. Jednom imate plan oznaka definirali, dodavanje koda za integraciju u aplikaciju je jednostavno pomoću SDK radnje za Azure Mobile.

Plan oznaka treba Označi sve u aplikaciji. Trebali biste samo sadrže oznake podataka koja je dio strategije mobilne radnje. To vjerojatno će biti različiti između aplikacija. [Predložak Playbook Media] [ Media Playbook link] pod uvjetom da po radnje Azure Mobile omogućuje sastavljanje plan oznaku pomoću navedene metode. Koristite radni list **Plan oznaka** kao vodič za stvaranje tarifa oznaka.

Pri definiranju sekcije oznake na radnom listu, biti vrlo konkretnih. To je važno da biste izbjegli zbrku. Detalji o svakom očekuje scenarij u kojem će biti poslana svakoj oznaci. Navedite naziv aktivnosti koje će biti uložen svakoj oznaci. To želite sve uvrstiti u **Informative** dio radnog lista. Na radnom listu plan oznaka mora biti glavni referenca za potvrdu test. 

U odjeljku **Podaci za prikupljanje** razvojnom timu treba pronaći vrste, imena, vrijednosti i parove ključa vrijednosti extra informacije potrebne za svaku oznaku koja će biti uložen u aplikaciji.

Preporučujemo da pregled plan oznaku sa svim timovima pridružene projekta. Izvršite potrebne promjene i potvrdite je za marketing i razvoj timovima Očisti sve.

Na radnom listu **Izjava rada** može se koristiti da biste su svi uključeni u projektu.


#### <a name="data-types"></a>Vrste podataka

To su uobičajene vrste podataka podršku tako da Azure Mobile radnje.

###### <a name="devices-and-users"></a>Uređaji i korisnika

Azure Mobile radnje označava korisnicima tako da generira Jedinstveni identifikator za svaki uređaj. Ovaj identifikator zove identifikator uređaja (ili deviceid). Generira se na način sve aplikacije na tom uređaju isti zajednički koristiti iste identifikator uređaja.

###### <a name="sessions-and-activities"></a>Sesije i aktivnosti

Sesija je jedna pojava aplikaciju pokrenut korisnik. Sesije proteže se od vremena korisnik pokrene aplikaciju, dok se ne zaustavi.

Aktivnost je logički grupiranja skupa značajki aplikaciju možete učiniti tijekom sesije. To je obično određeni zaslona u aplikaciji, ali može biti ništa definira logičku vrijednost aplikacije. Najmanje trebali biste označili svaki zaslona ili aktivnosti za aplikacije. To će vam omogućiti da biste shvatili put do korisničkog.


###### <a name="events"></a>Događaji

Događaji koje koriste se za izvješća interakcije s korisnikom pomoću aplikacije. Zna biti izravne Akcije, kao što je zajedničko korištenje sadržaja ili pokretanja videozapisa. Označavanje događaje će vam pružiti zbirke podataka koji se način na koji korisnici rade s aplikacijom. 

###### <a name="jobs"></a>Zadaci

Zadaci se koriste za izvješća akcije koje su trajanje. Sadrži nekoliko primjera:

- Izvršavanje API poziva
- Prikaz vrijeme reklame
- Trajanje pozadinske zadatke 
- Trajanje postupak za kupnju
- Gledanje videozapisa


###### <a name="errors"></a>Pogreške

Pogreške koriste se za prijave problema otkrio aplikaciju. Na primjer, akcije korisnika nisu ispravne ili API poziva pogreške.

###### <a name="application-information"></a>Informacije o aplikaciji

Informacije o aplikaciji (aplikacija informacije) koristi se za označavanje podaci koji se odnose na korisnički doživljaj pomoću nekog od programa. Generira ga korisničke interakcije s aplikacijom. 

Za ključ za dani aplikacije informacije Azure Mobile radnje samo evidentira najkasnija vrijednost (bez povijest). Aplikacija informacije otkriva stanja aplikacije ili vaše krajnjim korisnicima. Na primjer statusa u zapisniku ili grupu favorita proizvoda za korisnika.

###### <a name="crash-data"></a>Pasti podataka

Neočekivano zatvoriti podatke automatski prikupljene putem pogrešaka aplikacije izvješća SDK radnje Mobile ne obrađuje aplikacije. Na primjer neobrađenu iznimku koji se pojavljuje.


###### <a name="extra-data"></a>Dodatni podaci

S parametrima mogu biti nadograđeni događaje, pogreške, aktivnosti i zadacima. Ovo je extra informacije razvojni inženjer može ponuditi kao konkretne podatke iz aplikacije. To je važno za definiranje preciznije segmente. 

Na primjer, vrijednost oznaku "članak" će vam omogućiti krajnjim korisnicima segment koji se temelji na koji je prikazati određeni članak. Međutim, koja možda neće biti dovoljno. Možda je bolji Ako tu istu oznaku "članak" uključeni extra informacije kao što su "news_category" unutar aktivnosti. To će biti korisno da biste odredili dinamički omiljene kategorija za korisnika. 

Extra informacije prijavljuje u paru ključa vrijednosti. U ovom primjeru za aplikaciju za medijske sadržaje extra informacije za "news_category" bi vrijednost za tu kategoriju. Na primjer, "Sportovi", "potrošnje" ili "politika".





#### <a name="tag-and-sdk-integration"></a>Integracija oznake i SDK 

Korak po korak upute za integriranje radnje SDK za Azure Mobile u aplikacije, slijedite u dokumentaciji za [Integraciju SDK radnje](mobile-engagement-windows-store-integrate-engagement.md) na Azure web-mjesta. Odaberite svoju platformu cilj veze pri vrhu stranice.

Preporučujemo stvaranje projekata za dva aplikacije on nudi Azure Mobile radnje. Jedan za razvoj i testiranje pripremna i drugi za proizvodnju pripremna. Vaš tim za IT možete zatim Promicanje iz pripremna test za radni kada testiranje prihvaćanje korisnik ne uspije.



#### <a name="user-acceptance-testing-uat"></a>Prihvaćanje korisnika testiranjem (UAT)

Korisnik prihvaćanje testiranje (UAT) uključuje provjerite sve funkcionira kako je dizajniran. Tijekove rada možete izvršiti i prikupite sve potrebne podatke ovisno o tarifi oznaka:
 
- Informacije o označavanju mora biti na mjestu prema dokumentirane AZME koncepti
- Sve informacije prikupljaju (uključujući Extra informacije, a vrijednost aplikacije informacije)
- Nomenclature rezultata prema izgleda planiranje oznaka
- Postoji nema duplicirane oznaka šalje

Temeljito testirajte sve vrste ponašanje imaju ugrađen u aplikaciju

- Objava, ankete, podataka ih gura iz aplikacije i u aplikaciji
- Tekst/webom prikazi
- Ažuriranje iskaznice, kategorija



#### <a name="setup"></a>Postavljanje

Postavljanje Azure Mobile radnje je vrlo jednostavne. Sve u dokumentaciji koji se odnosi na korisničko sučelje dostupna Azure Mobile radnje web-mjesta, [kako se kretati po korisničkog sučelja](mobile-engagement-user-interface-home.md).

Preporučuje se započnite postavljanjem desnom uloge i uloge članstva za korisnike projekta. To omogućuje upravljanje pristupom odgovarajuću platformu za sve korisnike. Može obuhvaćati i svoje uloge:

- Administratori
- Razvojni inženjeri
- Preglednici 

Naknadno:
- Registrirajte se vaš deviceID da biste testirali na vlastitu uređaju.
- Idite na postavke vašeg računa i postavljanje vremenske zone grafikone i vrijeme isporuke obavijesti Postavljanje vremenske zone.
- Idite na postavke vašeg računala i registrirati "Aplikacije-informacije" morate cilj pri ruci krajnjeg korisnika.

Dodatne informacije o tome kako pokrenuti prva kampanja automatske obavijesti, pročitajte članak [Kako započeti korištenje i upravljanje njima ih gura da biste stupili krajnjim korisnicima](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Zaključak


Korištenje programa su izračun s iteracijama, a trebali biste kontinuirano uvezete vaš kao koji isprobati što vam najbolje odgovara aplikacije. 

Prethodno, prilikom razvoja doživljaj radnje strategije nemojte da biste sastavili stvaranje strategije cijelu globalni radnje. Iskoristite korak po korak pristup Identificirajte vaše KPI-ja i kako ih odražava. Strategije radnje će biti jedinstvena za svaku aplikaciju.

Nakon što ste razvili iskustva razmislite o dodavanju sljedeće radnje programima:

- Praćenje: Nabavljate korisnika i vjerojatno Definirajte izvore za prikupljanje podataka. Azure mobilne radnje može povezati s izvorima podataka zbirke. Omogućuje praćenje izvedbe svaki izvor. Ove informacije bit će zanimljive Maksimiziranje na nabavu ulaganja. 

- A / B testiranje: to je ključni dio programa radnje. Svaku aplikaciju ima vlastitu specifičnosti. A / B testiranja, možete poboljšati programa radnje.

- Mjesto zemlj.: To je prevelik prilika za marke. Zahvaljujući tu značajku možete dobiti na pravo mjesto i vrijeme. Preporučujemo da potvrditi da ste prikupili podaci o ponašanju dovoljno krajnjeg korisnika prije početka da biste koristili zemlj mjesto.

- Slanje podataka: podaci automatske je nevidljivi automatske. Slanje podataka omogućuje prilagodbu aplikacije koji se temelji na ponašanje krajnjeg korisnika. Ako, na primjer, korisnički segment često consults visoke Tehnički proizvoda, vlasnik aplikacije možete poslati automatske podataka koji će personalizacija svoj Početna stranica sa sadržajem visoke Tehnički.



## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje računa Azure Mobile radnje](mobile-engagement-create.md).
- Posjetite [Određivanje strategije Mobile radnje](mobile-engagement-define-your-mobile-engagement-strategy.md) dodatne informacije o definiranju strategije Mobile radnje.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
