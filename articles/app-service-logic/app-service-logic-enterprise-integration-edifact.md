<properties 
    pageTitle="Enterprise Integracija s EDIFACT | Microsoft Azure" 
    description="Saznajte kako koristiti EDIFACT ugovore da biste stvorili logike aplikacije" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="jonfan"/>

# <a name="enterprise-integration-with-edifact"></a>Enterprise Integracija s EDIFACT 

> [AZURE.NOTE] Ova stranica pokriva značajke EDIFACT logike aplikacija. Dodatne informacije o X12 kliknite [ovdje](app-service-logic-enterprise-integration-x12.md).

## <a name="create-an-edifact-agreement"></a>Stvorite ugovor EDIFACT 
Prije nego što možete razmjenjivati EDIFACT poruke, morate stvoriti EDIFACT ugovor i pohraniti na vašem računu integracije. Sljedeći koraci će vas voditi kroz postupak stvaranja EDIFACT ugovor.

### <a name="heres-what-you-need-before-you-get-started"></a>Evo što vam je potrebno prije nego što počnete
- [Račun za integraciju](./app-service-logic-enterprise-integration-accounts.md) definirano u pretplatu za Azure  
- Najmanje dva [partnere](./app-service-logic-enterprise-integration-partners.md) već definiran na vašem računu Integracija  

>[AZURE.NOTE]Kada stvorite ugovor, sadržaj u porukama koje će primati/slanje i iz partnera mora podudarati s vrstom ugovor.    


Nakon što ste [stvorili račun integracije](./app-service-logic-enterprise-integration-accounts.md) i [dodati partnera](./app-service-logic-enterprise-integration-partners.md), možete stvoriti ugovor EDIFACT slijedeći ove korake:  

### <a name="from-the-azure-portal-home-page"></a>Iz Azure početnoj stranici portala

Nakon prijave na [portal za Azure](http://portal.azure.com "Azure portal"):  
1. Na izborniku na lijevoj strani odaberite **Pregledaj** .  

>[AZURE.TIP]Ako ne vidite vezu **Pregledavanje** , morate najprije proširite izbornik. To tako da odaberete vezu **Prikaz izbornika** koji se nalazi u gornjem lijevom kutu sažeti izbornika.  

![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-0.png)    
2. *Integracija* upišite u okvir za pretraživanje filtar, a zatim odaberite **Računi za integraciju** s popisa rezultata.       
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-3.png)    
3. U **Račune za integraciju** plohu koji se otvara odaberite račun za integraciju u kojem ćete stvoriti ugovor. Ako ne vidite računa za integraciju popisa, [stvorili prvu](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-4.png)  
4.  Odaberite pločicu **ugovore** . Ako ne vidite ugovore pločica, najprije ga dodati.   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1-5.png)     
5. Odaberite gumb **Dodaj** u plohu ugovore koji će se otvoriti.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-agreement-2.png)  
6. Unesite **naziv** za svoje ugovor, a zatim odaberite **vrstu ugovor** za EDIFACT, **Partnera glavno računalo**, **Identitet glavno računalo**, **Partnera za goste**, **Goste identiteta**u plohu ugovore koji će se otvoriti.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-1.png)  
7. Nakon što postavite svojstva ugovor, odaberite **Primanje postavke** da biste konfigurirali kako se poruke koje su stigle putem ovog ugovora treba obraditi.  
8. Kontrola postavki primanja podijeljen u sljedećim se odjeljcima, uključujući identifikatora, potvrdu, sheme, kontrola brojeve, provjere valjanosti, internog postavke i obrade. Konfiguriranje tih svojstava na temelju ugovor s partnerom će razmjena poruka s. Ovdje je prikaz te kontrole, konfiguriranje ih na temelju kako želite da se ovaj ugovor za otkrivanje i obrađuje dolazne poruke:  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-2.png)  
9. Odaberite gumb **u redu** da biste spremili postavke.  

### <a name="identifiers"></a>Identifikatori

|Svojstvo|Opis |
|---|---|
|UNB6.1 (primatelja referenca lozinka)|Unesite vrijednost alfanumerički u rasponu između 1 i 14 znakova.|
|UNB6.2 (razdjelnik primatelja referenca)|Unesite alfanumerički vrijednost s najmanje jedan znak i maksimalno dva znaka.|

### <a name="acknowledgments"></a>Potvrda 

|Svojstvo|Opis |
|----|----|
|Potvrda poruke (CONTRL)|Potvrdite ovaj okvir da biste se vratili tehničke potvrdu (CONTRL) za razmjenu pošiljatelja. Za potvrdu se šalje razmjenu pošiljatelj koji se temelji na stranici Postavke slanje za ugovor.|
|Potvrđivanje (CONTRL)|Potvrdite ovaj okvir da biste se vratili funkcionalni potvrdu (CONTRL) pošiljatelj razmjenu na potvrdu šalje razmjenu pošiljatelj koji se temelji na stranici Postavke slanje za ugovor.|

### <a name="schemas"></a>Shema

|Svojstvo|Opis |
|----|----|
|UNH2.1 (VRSTA)|Odaberite vrstu skup transakcije.|
|UNH2.2 (VERZIJA)|Unesite broj verzije poruke. (Minimum, jedan znak; maksimum, tri znaka).|
|UNH2.3 (IZDANJE)|Unesite broj izdanje poruke. (Minimum, jedan znak; maksimum, tri znaka).|
|UNH2.5 (KOD PRIDRUŽENOG DODIJELJENI)|Unesite šifru dodijeljeni. (Najviše šest znakova. Mora biti alfanumerički).|
|UNG2.1 (APLIKACIJA POŠILJATELJA ID-JA)|Unesite alfanumerički vrijednost s najmanje jedan znak i najviše 35 znakova.|
|UNG2.2 (RAZDJELNIK KOD POŠILJATELJA APLIKACIJA)|Unesite alfanumerički vrijednost, s najviše četiri znaka.|
|SHEMA|Odaberite prethodno prenesene shemu koji želite koristiti s računa pridruženog integracije.|

### <a name="control-numbers"></a>Kontrola brojeva

|Svojstvo|Opis |
|----|----|
|Onemogućite razmjenu broj kontrole duplikate|Potvrdite ovaj okvir da biste blokirali duplicirane interchanges. Ako je odabrana ta mogućnost, akcije za dekodiranje EDIFACT provjerava broj kontrole za razmjenu (UNB5) za razmjenu primljene odgovaraju prethodno obrađeni razmjenu broj kontrola. Ako je otkriven podudaranje, a zatim na razmjenu nije obrađen.
|Traženje duplikata UNB5 svakih (dana)|Odlučili da bi se onemogućilo duplicirane razmjenu kontrola brojeva, možete navesti broj dana koji se na kojoj se provjera provodi dodjeljivanjem odgovarajuću vrijednost za **Traženje duplikata UNB5 svakih (dana)** mogućnost.|
|Onemogućivanje grupa kontrolu broja duplikate|Potvrdite ovaj okvir da biste blokirali interchanges s brojevima dvostruke grupu kontrola (UNG5).|
|Onemogućite transakcije skup kontrola broj duplikate|Potvrdite ovaj okvir da biste blokirali interchanges duplicirane transakcije skup kontrola brojeva (UNH1).|
|Broj kontrole potvrđivanje EDIFACT|Da biste odredili transakcije skup Referentni brojevi će se koristiti u potvrdu, unesite vrijednost za prefiks, raspon Referentni brojevi i sufiks.|

### <a name="validations"></a>Provjera valjanosti

|Svojstvo|Opis |
|----|----|
|Vrsta poruke|Odredite vrstu poruka. Dovršenim svaki redak provjere valjanosti, drugi dodat će se automatski. Ako su navedeni nijedno pravilo, zatim u retku označen kao zadani služi za provjeru valjanosti.|
|Uređivanje provjere valjanosti|Potvrdite ovaj okvir da biste izvršili provjere valjanosti za uređivanje na vrste podataka, kao što je definirao uređivanje svojstava shemu, Duljina ograničenja, prazan podatkovnih elemenata i završne razdjelnika.|
|Prošireni provjere valjanosti|Potvrdite ovaj okvir da biste omogućili prošireni provjeru (XSD) interchanges poslao pošiljatelj razmjenu. To uključuje provjere valjanosti polja Duljina, optionality i broj ponavljanja osim XSD provjeru valjanosti vrsta podataka.|
|Dopusti/na kraju početne nule|Odaberite **Dopusti** da biste omogućili prored i na kraju nule; **NotAllowed** ne želite dopustiti suvišne i prored i na kraju nula ili **obrezivanje** da biste izdvojili početne nule.|
|Obrezivanje i na kraju početne nule|Potvrdite ovaj okvir da biste izdvojili sve nule uvodnih ili krajnjih|
|Na kraju razdjelnik pravila|Ako ne želite dopustiti završne graničnike i razdjelnike u programa za razmjenu poslao pošiljatelj razmjenu, odaberite **Nije dopušteno** . Ako razmjenu sadrži završne graničnike i razdjelnika, je prijavljen koji nisu valjani. Odaberite **Dodatno** da biste prihvatili interchanges sa ili bez završne graničnike i razdjelnici. **Obavezno** odaberite ako primljene razmjenu mora sadržavati završne graničnike i razdjelnike.|

### <a name="internal-settings"></a>Interna postavke

|Svojstvo|Opis |
|----|----|
|Ako završne razdjelnici dopušteno, stvorite prazan XML oznake|Potvrdite ovaj okvir da bi pošiljatelj razmjenu sadržavati prazan XML oznake za završne razdjelnici.|
|Dolazni grupnog slanja promjena obrada|Mogućnosti obuhvaćaju sljedeće:</br></br>**Podijeli razmjenu kao skupove transakcije – privremeno obustavljanje transakcije skupove pogreške**: raščlanjuje pojedine transakcije postavljanje u programa za razmjenu u zasebnim dokumentom XML primjenom odgovarajuće omotnice za postavljanje transakcije. Uz tu se mogućnost ako jedan ili više skupova transakcija u razmjenu ne prođu provjeru, zatim samo one transakcije skupovi su obustavljeno. Podjela razmjenu kao skupove transakcije - odgoditi za razmjenu pogreške: raščlanjuje pojedine transakcije postavljanje u programa za razmjenu u zasebnim dokumentom XML primjenom odgovarajuće omotnice. Uz tu se mogućnost ako transakcije jedan ili više skupova u razmjenu ne prođu provjeru, tada će biti obustavljeno cijelu razmjenu.</br></br>**Očuvanje razmjenu – privremeno obustavljanje transakcije skupove pogreške**: razmjenu ostaje netaknuta Stvaranje XML dokument za cijelu odbacivanja razmjenu. Uz tu se mogućnost ako jedan ili više skupova transakcija u razmjenu ne prođu provjeru, zatim samo one transakcije skupovi su obustavljena, dok se obrađuju sve druge skupove transakcije.</br></br>**Očuvanje razmjenu – privremeno obustavljanje razmjenu pogreške**: razmjenu ostaje netaknuta Stvaranje XML dokument za cijelu odbacivanja razmjenu. Uz tu se mogućnost ako transakcije jedan ili više skupova u razmjenu ne prođu provjeru, potom obustavljeno je cijeli razmjenu.|

Da se slažete spreman je za rukovanje dolazne poruke u skladu sa postavke koje ste odabrali.

Konfiguriranje postavki koje rukovati porukama koje šaljete na partnere:  
10. Odaberite **Pošalji postavke** da biste konfigurirali kako se poruke koje se šalju putem ovog ugovora treba obraditi.  

Kontrolu slanje postavke je podijeljen u sljedećim odjeljcima, uključujući identifikatora, potvrdu, sheme, omotnice, skupovi znakova i razdjelnici, kontrola brojeve i provjere valjanosti. 

Ovdje je prikaz te kontrole. Odaberite kako želite rukovati poruke koje šaljete partnerima putem ovog ugovora na temelju:   
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-3.png)    
11. Odaberite gumb **u redu** da biste spremili postavke.  

### <a name="identifiers"></a>Identifikatori
|Svojstvo|Opis |
|----|----|
|UNB1.2 (sintaksa verzija)|Odaberite vrijednost između **1** i **4**.|
|UNB2.3 (pošiljatelj obrnutim usmjeravanje adresa)|Unesite alfanumerički vrijednost s najmanje jedan znak i najviše 14 znakova.|
|UNB3.3 (adresa primatelja obrnutim usmjeravanje)|Unesite alfanumerički vrijednost s najmanje jedan znak i najviše 14 znakova.|
|UNB6.1 (primatelja referenca lozinka)|Unesite alfanumerički vrijednost s najmanje jednu i najviše 14 znakova.|
|UNB6.2 (razdjelnik primatelja referenca)|Unesite alfanumerički vrijednost s najmanje jedan znak i maksimalno dva znaka.|
|UNB7 (ID aplikacije referenca)|Unesite alfanumerički vrijednost s najmanje jedan znak i najviše 14 znakova|

### <a name="acknowledgment"></a>Potvrda
|Svojstvo|Opis |
|----|----|
|Potvrda poruke (CONTRL)|Ovaj okvir potvrdite ako glavnom računalu partnera očekuje primanje prima tehničke potvrdu (CONTRL). Tom se postavkom određuje glavnom računalu partner koji se šalje poruku, zahtjeva za potvrdom od partnera za goste.|
|Potvrđivanje (CONTRL)|Ovaj okvir potvrdite ako glavnom računalu partnera očekuje da biste primali funkcionalni potvrdu (CONTRL). Tom se postavkom određuje glavnom računalu partner koji se šalje poruku, zahtjeva za potvrdom od partnera za goste.|
|Generiranje SG1/SG4 petlje za skupove prihvaćenom transakcije|Ako odaberete da biste zatražili funkcionalni potvrđivanje, potvrdite ovaj okvir da biste nametnuli generacije SG1/SG4 petlje u funkcionalni potvrda CONTRL za skupove prihvaćenom transakcije.|

### <a name="schemas"></a>Shema
|Svojstvo|Opis |
|----|----|
|UNH2.1 (VRSTA)|Odaberite vrstu skup transakcije.|
|UNH2.2 (VERZIJA)|Unesite broj verzije poruke.|
|UNH2.3 (IZDANJE)|Unesite broj izdanje poruke.|
|SHEMA|Odaberite shemu da biste koristili. Sheme nalaze se na vašem računu integracije. Da biste pristupili vaše sheme, najprije povezati s računom integraciju u aplikaciju za logiku.|

### <a name="envelopes"></a>Omotnice
|Svojstvo|Opis |
|----|----|
|UNB8 (obradu prioriteta kod)|Unesite vrijednost za abecednim redoslijedom koji nije više od jednog znaka.|
|UNB10 (komunikacije ugovor)|Unesite alfanumerički vrijednost s najmanje jedan znak i maksimalno 40 znakova.|
|UNB11 (testiranje pokazatelj)|Potvrdite ovaj okvir da biste naznačili da je razmjenu generira probno podataka|
|Primjena UNA segmenta (servis niz savjet)|Potvrdite ovaj okvir da biste generirali UNA segment za razmjenu slati.|
|Primjena UNG segmenata (zaglavlje grupe – opis funkcije)|Potvrdite ovaj okvir da biste stvorili segmenata grupiranja u zaglavlju grupe functional na poruke poslane partnera za goste. Da biste stvorili segmenata UNG koriste se sljedeće vrijednosti:</br></br>Za **UNG1**unesite alfanumerički vrijednost s najmanje jedan znak i najviše šest znakova.</br></br>Za **UNG2.1**unesite alfanumerički vrijednost s najmanje jedan znak i najviše 35 znakova.</br></br>Za **UNG2.2**unesite alfanumerički vrijednosti, s najviše četiri znaka.</br></br>Za **UNG3.1**unesite alfanumerički vrijednost s najmanje jedan znak i najviše 35 znakova.</br></br>Za **UNG3.2**unesite alfanumerički vrijednosti, s najviše četiri znaka.</br></br>Za **UNG6**unesite alfanumerički vrijednost s najmanje jednu i najviše tri znaka.</br></br>Za **UNG7.1**unesite alfanumerički vrijednost s najmanje jedan znak i najviše tri znaka.</br></br>Za **UNG7.2**unesite alfanumerički vrijednost s najmanje jedan znak i najviše tri znaka.</br></br>Za **UNG7.3**unesite alfanumerički vrijednost s najmanje 1 znak i najviše 6 znakova.</br></br>Za **UNG8**unesite alfanumerički vrijednost s najmanje jedan znak i najviše 14 znakova.|

### <a name="character-sets-and-separators"></a>Znak skupova i razdjelnicima
Osim znak skupu, možete unijeti drukčiji skup graničnike koja će se koristiti za svaku vrstu poruka. Ako za određenu poruku shemu nije navedena skup znakova, koristi se zadani skup znakova.

|Svojstvo|Opis |
|----|----|
|UNB1.1 (sustav identifikator)|Odaberite skup da se primjenjuje na odlazne razmjenu EDIFACT znakova.|
|Shema|S padajućeg popisa odaberite shemu. Dovršenim svaki redak dodat će se novi redak. Za odabranu shemu, odaberite razdjelnici postavljena za uporabu:</br></br>**Komponenta element razdjelnik** – Enter pojedinačni znak za razdvajanje elemenata složenih podataka.</br></br>**Razdjelnik Element podataka** – Enter pojedinačni znak za razdvajanje elemenata jednostavne podatke unutar elemenata složenih podataka.</br></br></br></br>**Zamjenski znak** – potvrdite ovaj okvir ako opseg podataka sadrži znakove koji se koriste kao podatke, segment ili komponente razdjelnika. Zatim možete unijeti zamjenski znak. Pri stvaranju odlazne poruke EDIFACT sve instance znakove razdjelnika u podacima tereta zamjenjuje navedeni znak.</br></br>**Kraj segmenta** – Enter pojedinačni znak da biste označili kraj za uređivanje segmenta.</br></br>**Nastavak** – odaberite znak koji se koristi s identifikatorom segmenta. Ako odredite sufiks element segmenta kraj podataka može biti prazna. Ako kraj segmenta ostavite prazno, morate odrediti sufiks.|

### <a name="control-numbers"></a>Kontrola brojeva
|Svojstvo|Opis |
|----|----|
|UNB5 (broj kontrole za razmjenu)|Unesite prefiks raspon vrijednosti za kontrole broj razmjenu i sufiks. Ove vrijednosti se koriste za generiranje odlazne razmjenu. Prefiks i sufiks nije obavezno; potreban je broj kontrola. Za svaku novu poruku, koji se povećava broj kontrole Prefiks i sufiks ostaju isti.|
|UNG5 (grupiranje kontrola broj)|Unesite prefiks raspon vrijednosti za kontrole broj razmjenu i sufiks. Ove vrijednosti se koriste za generiranje kontrolu broja grupe. Prefiks i sufiks nije obavezno; potreban je broj kontrola. Broj kontrole se povećava za svaku novu poruku dok se dosegne maksimalne vrijednosti; Prefiks i sufiks ostaju isti.|
|UNH1 (poruka zaglavlja referentni broj)|Unesite prefiks raspon vrijednosti za kontrole broj razmjenu i sufiks. Ove vrijednosti se koriste za generiranje referentni broj zaglavlja poruke. Prefiks i sufiks nije obavezno; potreban je referentni broj. Referentni broj se povećava za svaku novu poruku; Prefiks i sufiks ostaju isti.|

### <a name="validations"></a>Provjera valjanosti
|Svojstvo|Opis |
|----|----|
|Vrsta poruke|Tom se mogućnošću omogućuje Provjera valjanosti na razmjenu primatelja. Ova provjera valjanosti izvodi provjere valjanosti za uređivanje na transakcije skupu podataka elemenata, provjera valjanosti vrste podataka, Duljina ograničenja i prazan podatkovnih elemenata i obuka razdjelnika.|
|Uređivanje provjere valjanosti|Potvrdite ovaj okvir da biste izvršili provjere valjanosti za uređivanje na vrste podataka, kao što je definirao uređivanje svojstava shemu, Duljina ograničenja, prazan podatkovnih elemenata i završne razdjelnika.|
|Prošireni provjere valjanosti|Tom se mogućnošću omogućuje prošireni provjeru interchanges primili pošiljatelja razmjenu. To uključuje provjere valjanosti polja Duljina, optionality i broj ponavljanja osim XSD provjeru valjanosti vrsta podataka. Možete omogućiti provjere valjanosti za nastavak bez omogućivanja provjere valjanosti za uređivanje, ili obrnuto.|
|Dopusti prored i na kraju nule|Tom se mogućnošću određuje da programa za razmjenu uređivanje poslao strana ne ne prođu provjeru ako element podataka u programa za razmjenu uređivanje nije usklađen sa njegov duljine zahtjeva jer ili završne razmake, ali sukladnosti njegov duljine zahtjev kad se oni se uklanjaju.|
|Obrezivanje i na kraju početne nule|Tom se mogućnošću će obrezati na početku i kraju nule.|
|Na kraju razdjelnika|Tom se mogućnošću određuje programa za razmjenu uređivanje poslao strana ne ne prođu provjeru ako element podataka u programa za razmjenu uređivanje nije usklađen sa njegov duljine zahtjev zbog nule uvodnih (ili krajnjih) ili završne razmake, ali sukladnosti njegov duljine zahtjev kad se oni se uklanjaju.</br></br>Ako ne želite dopustiti završne graničnike i razdjelnike u programa za razmjenu poslao pošiljatelj razmjenu, odaberite **Nije dopušteno** . Ako razmjenu sadrži završne graničnike i razdjelnika, je prijavljen koji nisu valjani.</br></br>Odaberite **Dodatno** da biste prihvatili interchanges sa ili bez završne graničnike i razdjelnici.</br></br>**Obavezno** odaberite ako primljene razmjenu mora sadržavati završne graničnike i razdjelnike.|

Nakon što instalirate na otvorene plohu odaberite **u redu** :  
12. Odaberite pločicu **ugovore** na plohu Integracija računa i vidjet ćete novododani ugovor na popisu.  
![](./media/app-service-logic-enterprise-integration-edifact/EDIFACT-4.png)   

## <a name="learn-more"></a>uči više
- [Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija")  
