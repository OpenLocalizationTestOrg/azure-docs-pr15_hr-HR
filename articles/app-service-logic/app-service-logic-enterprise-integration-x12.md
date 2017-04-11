<properties 
    pageTitle="Pregled X12 i integracija paket Enterprise | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Saznajte kako koristiti X12 ugovore da biste stvorili logike aplikacije" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-x12"></a>Enterprise Integracija s X12 

>[AZURE.NOTE]U ovom značajke stranice naslovnice u X12 logike aplikacija. Dodatne informacije o EDIFACT kliknite [ovdje](app-service-logic-enterprise-integration-edifact.md).

## <a name="create-an-x12-agreement"></a>Stvaranje programa X12 ugovor 
Prije nego što možete razmjenjivati X12 poruke, morate stvoriti programa X12 ugovor i spremite ga na vašem računu integracije. Sljedeći koraci će vas voditi kroz postupak stvaranja programa X12 ugovor.

### <a name="heres-what-you-need-before-you-get-started"></a>Evo što vam je potrebno prije nego što počnete
- [Račun za integraciju](./app-service-logic-enterprise-integration-accounts.md) definirano u pretplatu za Azure  
- Najmanje dva [partnere](./app-service-logic-enterprise-integration-partners.md) već definiran na vašem računu Integracija  

>[AZURE.NOTE]Kada stvorite ugovor, sadržaj u datoteci ugovor mora podudarati s vrstom ugovor.    


Kada ste [stvorili račun integracije](./app-service-logic-enterprise-integration-accounts.md) i [dodati partnera](./app-service-logic-enterprise-integration-partners.md), možete stvoriti programa X12 ugovor slijedeći ove korake:  

### <a name="from-the-azure-portal-home-page"></a>Iz Azure početnoj stranici portala

Nakon prijave na [portal za Azure](http://portal.azure.com "Azure portal"):  
1. Na izborniku na lijevoj strani odaberite **Pregledaj** .  

>[AZURE.TIP]Ako ne vidite vezu **Pregledavanje** , morate najprije proširite izbornik. To tako da odaberete vezu **Prikaz izbornika** koji se nalazi u gornjem lijevom kutu sažeti izbornika.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integracija* upišite u okvir za pretraživanje filtar, a zatim odaberite **Računi za integraciju** s popisa rezultata.       
![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)    
3. U **Račune za integraciju** plohu koji se otvara odaberite račun za integraciju u kojem ćete stvoriti ugovor. Ako ne vidite računa za integraciju popisa, [stvorili prvu](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)  
4.  Odaberite pločicu **ugovore** . Ako ne vidite ugovore pločica, najprije ga dodati.   
![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)     
5. Odaberite gumb **Dodaj** u plohu ugovore koji će se otvoriti.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Unesite **naziv** za svoje ugovor, a zatim odaberite **vrstu ugovor**, **Partnera glavno računalo**, **Identiteta glavno računalo**, **Partnera za goste**, **Goste identiteta**u plohu ugovore koji će se otvoriti.  
![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)  
7. Nakon što postavite na primanje postavke svojstva, odaberite gumb **u redu**  
Prijeđite:  
8. Odaberite **Primanje postavke** da biste konfigurirali kako se poruke koje su stigle putem ovog ugovora treba obraditi.  
9. Kontrola postavki primanja podijeljen u sljedećim se odjeljcima, uključujući identifikatora, potvrdu, sheme, omotnice, kontrola brojeve, provjere valjanosti i interna postavke. Konfiguriranje tih svojstava na temelju ugovor s partnerom će razmjena poruka s. Ovdje je prikaz te kontrole, konfiguriranje ih na temelju kako želite da se ovaj ugovor za otkrivanje i obrađuje dolazne poruke:  
![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)  
10. Odaberite gumb **u redu** da biste spremili postavke.  

### <a name="identifiers"></a>Identifikatori

|Svojstvo|Opis |
|---|---|
|ISA1 (autorizacija razdjelnik)|Odaberite razdjelnik vrijednost autorizacije s padajućeg popisa.|
|ISA2|Neobavezno. Unesite vrijednost za autorizaciju informacije. Ako je vrijednost koju ste unijeli za ISA1 osim 00, unesite najmanje jedan alfanumerički znak i maksimalno 10.|
|ISA3 (razdjelnik sigurnost)|Odaberite razdjelnik vrijednost sigurnost s padajućeg popisa.|
|ISA4|Neobavezno. Unesite vrijednost za sigurnost podataka. Ako je vrijednost koju ste unijeli za ISA3 osim 00, unesite najmanje jedan alfanumerički znak i maksimalno 10.|

### <a name="acknowledgments"></a>Potvrda 

|Svojstvo|Opis |
|----|----|
|TA1 Očekivan|Potvrdite ovaj okvir da biste se vratili tehničke potvrdu (TA1) za razmjenu pošiljatelja. Ove potvrda šalju pošiljatelju razmjenu ovisno o postavkama slanje za ugovor.|
|DI Očekivan|Potvrdite ovaj okvir da biste se vratili funkcionalni potvrdu (DI) razmjenu pošiljatelja. Zatim odaberite li acknowledgements 997 ili 999, ovisno o verzijama sheme radite. Ove potvrda šalju pošiljatelju razmjenu ovisno o postavkama slanje za ugovor.|
|Uključiti AK2/IK2 petlje|Potvrdite ovaj okvir da biste omogućili generacije AK2 petlje u funkcionalno potvrda za skupove prihvaćenom transakcije. Napomena: Ovaj okvir omogućena je samo ako ste odabrali potvrdni okvir DI očekivanjima.|

### <a name="schemas"></a>Shema

Odaberite shemu za svaku vrstu transakcije (ST1) te pošiljatelj aplikacije (GS2). Primanje kanal rastavlja dolazne poruke uparivanjem vrijednosti za ST1 i GS2 u dolazne poruke s vrijednostima koje ovdje postavite i shema dolazne poruke sa shemom Ovdje postavite.

|Svojstvo|Opis |
|----|----|
|Verzija|Odaberite na X12 verzija|
|Vrsta transakcije (ST01)|Odaberite vrstu transakcije|
|Pošiljatelj aplikacije (GS02)|Odaberite aplikaciju za pošiljatelja|
|Shema|Odaberite želite li nam datoteke sheme. Datoteke shema nalaze se na vašem računu integracije.|

### <a name="envelopes"></a>Omotnice

|Svojstvo|Opis |
|----|----|
|Korištenje ISA11|Ovo polje koristite da biste odredili razdjelnik u skupu transakcije:</br></br>Odaberite standardni identifikator da biste koristili decimalni notaciju od "." umjesto decimalni notacija dolazni dokument u na uređivanje primiti kanal.</br></br>Odaberite razdjelnik ponavljanja da biste odredili razdjelnika koji se ponavljaju pojavljivanja element jednostavne podatke i strukturu podataka koji se ponavljaju. Na primjer, (^) obično koristi kao razdjelnik ponavljanja. Za HIPAA sheme, možete koristiti samo (^).|

### <a name="control-numbers"></a>Kontrola brojeva

|Svojstvo|Opis |
|----|----|
|Onemogućite razmjenu broj kontrole duplikate|Odaberite ovu mogućnost da biste blokirali duplicirane interchanges. Ako je odabrana ta mogućnost, na portalu servisa BizTalk provjerava broj kontrole za razmjenu (ISA13) za razmjenu primljene odgovaraju broj kontrole za razmjenu. Ako je otkriven podudaranje, kanal primanje obraditi razmjenu.<br/>Ako ste odlučili da bi se onemogućilo duplicirane razmjenu kontrola brojeva, možete odrediti broj dana koji se na kojoj se provjera provodi dodjeljivanjem odgovarajuću vrijednost za traženje duplikata ISA13 svakih x dana.|
|Onemogućivanje grupa kontrolu broja duplikate|Odaberite ovu mogućnost da biste blokirali interchanges s brojevima dvostruke grupe kontrola.|
|Onemogućite transakcije skup kontrola broj duplikate|Odaberite ovu mogućnost da biste blokirali interchanges duplicirane transakcije skup kontrola brojevima.|

### <a name="validations"></a>Provjera valjanosti

|Svojstvo|Opis |
|----|----|
|Vrsta poruke|Upišite poruku za uređivanje, kao što su 850 narudžbenice ili potvrđivanje 999 implementacije.|
|Uređivanje provjere valjanosti|Izvodi provjere valjanosti za uređivanje na vrste podataka, kao što je definirao uređivanje svojstava shemu, Duljina ograničenja, prazan podatkovnih elemenata i završne razdjelnika.|
|Prošireni provjere valjanosti|Ako vrsta podataka nije uređivanje, provjera valjanosti na element obavezne podatke i dopuštene ponavljanja, Enumeracije i element duljine primajte (min na max).|
|Dopusti/na kraju početne nule|Dodatni prostor i nula znakove koji se početni ili završni se zadržavaju. One se uklanjaju.|
|Na kraju razdjelnik pravila|Stvara završne razdjelnika na razmjenu primili. Mogućnosti obuhvaćaju NotAllowed, neobavezno i obavezno.|

### <a name="internal-settings"></a>Interna postavke

|Svojstvo|Opis |
|----|----|
|Pretvaranje izričitu decimalnom obliku Nn temeljiti 10 numerička vrijednost.|Pretvara broj uređivanje koji je naveden u obliku Nn u bazu 10 numeričku vrijednost u Srednja XML na portalu servisa BizTalk.|
|Ako završne razdjelnici dopušteno, stvorite prazan XML oznake|Potvrdite ovaj okvir da bi razmjenu pošiljatelj sadržavati prazan XML oznake za završne razdjelnika.|
|Dolazni grupnog slanja promjena obrada|Podijeli razmjenu kao skupove transakcije – privremeno obustavljanje transakcije skupove pogreške: raščlanjuje pojedine transakcije postavljanje u programa za razmjenu u zasebnim dokumentom XML primjenom odgovarajuće omotnice za postavljanje transakcije. Uz tu se mogućnost ako transakcije jedan ili više skupova u razmjenu ne prođu provjeru, zatim BizTalk Services obustavlja samo one skupove transakcije. </br></br>Podjela razmjenu kao skupove transakcije - odgoditi za razmjenu pogreške: raščlanjuje pojedine transakcije postavljanje u programa za razmjenu u zasebnim dokumentom XML primjenom odgovarajuće omotnice. Uz tu se mogućnost ako transakcije jedan ili više skupova u razmjenu ne prođu provjeru, zatim BizTalk Services obustavlja cijelu razmjenu.</br></br>Očuvanje razmjenu – privremeno obustavljanje transakcije skupove pogreške: razmjenu ostaje netaknuta Stvaranje XML dokument za cijelu odbacivanja razmjenu. Uz tu se mogućnost ako onAe ili više transakcije postavlja razmjenu ne prođu provjeru, zatim BizTalk Services obustavlja samo one transakcije skupova tijekom nastavka za obradu sve druge skupove transakcije.</br></br>Očuvanje razmjenu – privremeno obustavljanje razmjenu pogreške: razmjenu ostaje netaknuta Stvaranje XML dokument za cijelu odbacivanja razmjenu. Uz tu se mogućnost ako transakcije jedan ili više skupova u razmjenu ne prođu provjeru, zatim BizTalk Services obustavlja cijelu razmjenu.</br></br>|

Da se slažete bude spremna za obrađuje dolazne poruke u skladu sa shemom koji ste odabrali.

Konfiguriranje postavki koje rukovati porukama koje šaljete na partnere:  
11. Odaberite **Pošalji postavke** da biste konfigurirali kako se poruke koje se šalju putem ovog ugovora treba obraditi.  

Kontrolu slanje postavke je podijeljen u sljedećim odjeljcima, uključujući identifikatora, potvrdu, sheme, omotnice, kontrola brojeve, skupovi znakova i razdjelnici i provjere valjanosti. 

Ovdje je prikaz te kontrole. Odaberite kako želite rukovati poruke koje šaljete partnerima putem ovog ugovora na temelju:   
![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)  

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)  
12. Odaberite gumb **u redu** da biste spremili postavke.  

### <a name="identifiers"></a>Identifikatori
|Svojstvo|Opis |
|----|----|
|Autorizacija razdjelnik (ISA1)|Odaberite razdjelnik vrijednost autorizacije s padajućeg popisa.|
|ISA2|Unesite vrijednost za autorizaciju informacije. Ako je vrijednost osim 00, unesite najmanje jedan alfanumerički znak i maksimalno 10.|
|Sigurnost razdjelnik (ISA3)|Odaberite razdjelnik vrijednost sigurnost s padajućeg popisa.|
|ISA4|Unesite vrijednost za sigurnost podataka. Ako je vrijednost osim 00 za tekstni okvir vrijednost (ISA4) unesite najmanje jednu vrijednost alfanumerički i najviše 10.|

### <a name="acknowledgment"></a>Potvrda
|Svojstvo|Opis |
|----|----|
|TA1 Očekivan|Potvrdite ovaj okvir da biste se vratili tehničke potvrdu (TA1) za razmjenu pošiljatelja. Tom se postavkom određuje partnera glavnog računala koja se šalje poruku zahtjeva za potvrdom od partnera za goste ugovora. Ove potvrda se očekuje od partnera glavno računalo ovisno o postavkama primanje ugovora.|
|DI Očekivan|Potvrdite ovaj okvir da biste vratili funkcionalni (DI) potvrdu pošiljatelju razmjenu, a zatim odaberite želite acknowledgements 997 ili 999, ovisno o verzijama sheme radite li. Ove potvrda se očekuje od partnera glavno računalo ovisno o postavkama primanje ugovora.|
|DI verzija|Odaberite verziju podaci o dokumentu|

### <a name="schemas"></a>Shema
|Svojstvo|Opis |
|----|----|
|Verzija|Odaberite na X12 verzija|
|Vrsta transakcije (ST01)|Odaberite vrstu transakcije|
|SHEMA|Odaberite shemu da biste koristili. Sheme nalaze se na vašem računu integracije. Da biste pristupili vaše sheme, najprije povezati s računom integraciju u aplikaciju za logiku.|

### <a name="envelopes"></a>Omotnice
|Svojstvo|Opis |
|----|----|
|Korištenje ISA11|Ovo polje koristite da biste odredili razdjelnik u skupu transakcije:</br></br>Odaberite standardni identifikator da biste koristili decimalni notaciju od "." umjesto decimalni notacija dolazni dokument u na uređivanje primiti kanal.</br></br>Odaberite razdjelnik ponavljanja da biste odredili razdjelnika koji se ponavljaju pojavljivanja element jednostavne podatke i strukturu podataka koji se ponavljaju. Na primjer, (^) obično koristi kao razdjelnik ponavljanja. Za HIPAA sheme, možete koristiti samo (^).</br>|
|Razdjelnik ponavljanja|Unesite razdjelnik ponavljanja|
|Broj verzije kontrole (ISA12)|Odaberite verziju standardni X12 koji koriste BizTalk Portal za servise za generiranje odlazne razmjenu.|
|Pokazatelj upotrebe (ISA15)|Unesite li kontekstu sustava za razmjenu podataka (kojeg), radni podataka (P) ili Provjera podataka (T). Primanje za uređivanje kanala promiče ovo svojstvo na kontekst.|
|Shema|Možete unijeti kako na portalu servisa BizTalk generira segmenata Oznaka i ST za razmjenu za X12 kodirani koja se šalje kanal za slanje.</br></br>Možete povezati vrijednosti GS1, GS2, GS3, GS4, GS5, GS7 i GS8 podatkovnih elemenata s vrijednostima vrste transakcije i verzija/izdanje podatkovnih elemenata. Kada na portalu servisa BizTalk određuje koje XML poruke sadrži vrijednosti postavili za vrstu transakcije i verzija/izdanje elemenata u retku rešetke, a zatim ga popunjava GS1, GS2, GS3, GS4, GS5, GS7 i GS8 podatkovnih elemenata u omotnicu odlazne razmjenu s vrijednostima iz isti redak u rešetku. Vrijednosti transakcije vrsta i elemenata verzija/izdanje mora biti jedinstvena.</br></br>Neobavezno. Za GS1, s padajućeg popisa odaberite vrijednost za šifru funkcionalni.</br></br>Obavezan. Za GS2, unesite alfanumerički vrijednosti za aplikaciju pošiljatelja s najmanje dva znaka i najviše 15 znakova.</br></br>Obavezan. Za GS3, unesite alfanumerički vrijednost tekstnog okvira aplikacije s najmanje dva znaka i najviše 15 znakova.</br></br>Neobavezno. Za GS4, odaberite CCYYMMDD ili YYMMDD.</br></br>Neobavezno. Za GS5, odaberite put Spremljen, HHMMSS ili HHMMSSdd.</br></br>Neobavezno. Za GS7, s padajućeg popisa odaberite vrijednost za odgovorni agencija.</br></br>Neobavezno. Za GS8, unesite alfanumerički vrijednost za dokument koji se povezuje s najmanje jedan znak, a najviše 12 znakova.</br></br>**Napomena**: to su vrijednosti koje se na portalu servisa BizTalk unosi u poljima Oznaka razmjenu je stvaranje upišete transakcije i verzija/izdanje elemenata u istom retku su podudaranje onih pridruženih razmjenu.|

### <a name="control-numbers"></a>Kontrola brojeva
|Svojstvo|Opis |
|----|----|
|Broj kontrole (ISA13) za razmjenu|Obavezan. Unesite raspon vrijednosti za broj kontrole za razmjenu koji koristi BizTalk Portal servisa u generiranje odlazne razmjenu. Unesite numeričku vrijednost s najmanje 1 i maksimalno 999999999.|
|Broj grupe kontrola (GS06)|Obavezan. Unesite raspon brojeva koji se na portalu servisa BizTalk trebali biste koristiti broj kontrolu grupe. Unesite numeričku vrijednost s najmanje jedan znak i maksimalno devet znakova.|
|Broj kontrola transakcije skup (ST02)|Za transakcije postavite kontrolu broj (ST02), unesite raspon numeričke vrijednosti za obavezna polja u srednjem i alfanumeričkih vrijednosti neobavezno prefiks i sufiks. Maksimalna duljina sva četiri polja je devet znakova.|
|Prefiks|Da biste odredili raspon transakcije skup kontrola brojeva koji se koriste u potvrdu, unesite vrijednosti u polja za kontrolu ACK broj (ST02). Unesite brojčanu vrijednost Srednja dva polja i alfanumerički vrijednost (po želji) za prefiks i sufiks polja. Srednji polja su potrebne i sadrže minimalne i maksimalne vrijednosti za broj kontrola Prefiks i sufiks nije obavezno. Maksimalna duljina za sva tri polja je devet znakova.|
|Nastavak|Da biste odredili raspon transakcije skup kontrola brojeva koji se koriste u potvrdu, unesite vrijednosti u polja za kontrolu ACK broj (ST02). Unesite brojčanu vrijednost Srednja dva polja i alfanumerički vrijednost (po želji) za prefiks i sufiks polja. Srednji polja su potrebne i sadrže minimalne i maksimalne vrijednosti za broj kontrola Prefiks i sufiks nije obavezno. Maksimalna duljina za sva tri polja je devet znakova.|

### <a name="character-sets-and-separators"></a>Znak skupova i razdjelnicima
Osim znak skupu, možete unijeti drukčiji skup graničnike koja će se koristiti za svaku vrstu poruka. Ako za određenu poruku shemu nije navedena skup znakova, koristi se zadani skup znakova.

|Svojstvo|Opis |
|----|----|
|Koja će se koristiti skup znakova|Odaberite na X12 znakovni skup za provjeru valjanosti svojstva koje ste unijeli za ugovor.</br></br>**Napomena**: servisi portalu BizTalk samo koristi tu postavku da biste provjerili valjanost vrijednosti za svojstva povezane ugovor unijeli. Primanje kanalu ili Pošalji kanal zanemaruje ovo svojstvo skup znakova prilikom izvršavanja izvođenju obrada.|
|Shema|Simbol odaberite (+) i odaberite shemu iz padajućeg popisa. Za odabranu shemu, odaberite razdjelnici postavljena za uporabu:</br></br>Komponenta element razdjelnik – Enter pojedinačni znak za razdvajanje elemenata složenih podataka.</br></br>Podaci Element razdjelnik – Enter pojedinačni znak za razdvajanje elemenata jednostavne podatke unutar elemenata složenih podataka.</br></br></br></br>Zamjenski znak – potvrdite ovaj okvir ako opseg podataka sadrži znakove koji se koriste kao podatke, segment ili komponente razdjelnika. Zatim možete unijeti zamjenski znak. Pri stvaranju izlaznom X12 poruka, sve instance znakove razdjelnika u opseg podataka zamjenjuju navedenim znakom.</br></br>Fazi kraj – unesite znak da biste označili kraj za uređivanje segmenta.</br></br>Nastavak – odaberite znak koji se koristi s identifikator segmenta. Ako odredite sufiks element segmenta kraj podataka može biti prazna. Ako kraj segmenta ostavite prazno, morate odrediti sufiks.|

### <a name="validation"></a>Provjera valjanosti
|Svojstvo|Opis |
|----|----|
|Vrsta poruke|Tom se mogućnošću omogućuje Provjera valjanosti na razmjenu primatelja. Ova provjera valjanosti izvodi provjere valjanosti za uređivanje na transakcije skupu podataka elemenata, provjera valjanosti vrste podataka, Duljina ograničenja i prazan podatkovnih elemenata i na kraju razdjelnika.|
|Uređivanje provjere valjanosti||
|Prošireni provjere valjanosti|Tom se mogućnošću omogućuje prošireni provjeru interchanges primili pošiljatelja razmjenu. To uključuje provjere valjanosti polja Duljina, optionality i broj ponavljanja osim XSD provjeru valjanosti vrsta podataka. Možete omogućiti provjere valjanosti za nastavak bez omogućivanja provjere valjanosti za uređivanje, ili obrnuto.|
|Dopusti početne / završne nule|Tom se mogućnošću određuje da je uređivanje razmjenu poslao strana ne ne prođu provjeru ako element podataka u programa za uređivanje razmjenu nije usklađen sa njegov duljine zahtjeva jer ili završne razmake, ali u skladu sa njegov duljine zahtjev kad se oni se uklanjaju.|
|Na kraju razdjelnika|Tom se mogućnošću određuje programa za razmjenu uređivanje poslao strana ne ne prođu provjeru ako element podataka u programa za razmjenu uređivanje nije usklađen sa njegov duljine zahtjev zbog nule uvodnih (ili krajnjih) ili završne razmake, ali u skladu sa njegov duljine zahtjev kad se oni se uklanjaju.</br></br>Ako ne želite dopustiti završne graničnike i razdjelnike u programa za razmjenu primili pošiljatelja razmjenu, odaberite nije dopušteno. Ako razmjenu sadrži završne graničnike i razdjelnika, je prijavljen koji nisu valjani.</br></br>Odaberite dodatno da biste prihvatili interchanges sa ili bez završne graničnike i razdjelnici.</br></br>Obavezno odaberite ako primljene razmjenu mora sadržavati završne graničnike i razdjelnike.|

Nakon što instalirate na otvorene blades odaberite **u redu** :  
13. Odaberite pločicu **ugovore** na plohu Integracija računa i vidjet ćete novododani ugovor na popisu.  
![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>uči više
- [Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija")  
