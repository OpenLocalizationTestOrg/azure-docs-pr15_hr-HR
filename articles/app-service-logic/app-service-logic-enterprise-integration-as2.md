<properties 
    pageTitle="Naučite kako stvoriti AS2 ugovor za integraciju paket Enterprise" 
    description="Naučite kako stvoriti AS2 ugovor za integraciju paket Enterprise | Aplikacije servisa za Microsoft Azure" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Enterprise Integracija s AS2

## <a name="create-an-as2-agreement"></a>Stvorite ugovor AS2
Da biste mogli koristiti značajke za tvrtke u aplikacijama logike, prvo morate stvoriti ugovore. 

### <a name="heres-what-you-need-before-you-get-started"></a>Evo što vam je potrebno prije nego što počnete
- [Račun za integraciju](./app-service-logic-enterprise-integration-accounts.md) definirano u pretplatu za Azure  
- Najmanje dva [partnere](./app-service-logic-enterprise-integration-partners.md) već definiran na vašem računu Integracija  

>[AZURE.NOTE]Kada stvorite ugovor, sadržaj u datoteci ugovor mora podudarati s vrstom ugovor.    


Nakon što ste [stvorili račun integracije](./app-service-logic-enterprise-integration-accounts.md) i [dodati partnera](./app-service-logic-enterprise-integration-partners.md), možete stvoriti ugovor slijedeći ove korake:  

### <a name="from-the-azure-portal-home-page"></a>Iz Azure početnoj stranici portala

Nakon prijave na [portal za Azure](http://portal.azure.com "Azure portal"):  
1. Na izborniku na lijevoj strani odaberite **Pregledaj** .  

>[AZURE.TIP]Ako ne vidite vezu **Pregledavanje** , morate najprije proširite izbornik. To tako da odaberete vezu **Prikaz izbornika** koji se nalazi u gornjem lijevom kutu sažeti izbornika.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integracija* upišite u okvir za pretraživanje filtar, a zatim odaberite **Računi za integraciju** s popisa rezultata.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. U **Račune za integraciju** plohu koji se otvara odaberite račun za integraciju u kojem ćete stvoriti ugovor. Ako ne vidite računa za integraciju popisa, [stvorili prvu](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Odaberite pločicu **ugovore** . Ako ne vidite ugovore pločica, najprije ga dodati.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Odaberite gumb **Dodaj** u plohu ugovore koji će se otvoriti.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Unesite **naziv** za svoje ugovor, a zatim odaberite **Partnera glavno računalo**, **Identitet glavno računalo**, **Partnera za goste**, **Goste identiteta**u plohu ugovore koji će se otvoriti.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Evo nekoliko detalja koje se može pronaći korisne prilikom konfiguriranja postavki da se slažete: 
  
|Svojstvo|Opis|
|----|----|
|Glavno računalo partnera|Ugovor mora glavno računalo i goste partnera. Glavno računalo partnera predstavlja tvrtke ili ustanove koji konfigurira ugovor.|
|Glavno računalo identiteta|Identifikator za partnera glavnog računala. |
|Partner za goste|Ugovor mora glavno računalo i goste partnera. Partnera za goste predstavlja tvrtke ili ustanove radi suradnji s partnerom glavnog računala.|
|Identitet goste|Identifikator za goste partnera.|
|Postavke primanja|Tih svojstava Primijeni na sve poruke koje se primio ugovor|
|Slanje postavke|Tih svojstava Primijeni na sve poruke koje šalje ugovor|  
Prijeđite:  
7. Odaberite **Primanje postavke** da biste konfigurirali kako se poruke koje su stigle putem ovog ugovora treba obraditi.  
 
 - Po želji možete nadjačati svojstva dolazne poruke. Da biste to učinili, odaberite potvrdni okvir za **Zanemarivanje svojstva poruke** .
  - Ako želite da se Traži sve dolazne poruke treba potpisati, potvrdite okvir **poruke moraju biti prijavljeni** . Ako odaberete tu mogućnost, također ćete morati odaberite **certifikat** koji će se koristiti za provjeru valjanosti potpis u poruke.
  - Po želji možete tražiti poruke kao i šifriranje. Da biste to učinili, potvrdite okvir **mora biti šifrirane poruke** . Zatim potrebni odaberite **certifikat** koji će se koristiti za dekodiranje dolazne poruke.
  - Možete tražiti i poruka da se sažeti. Da biste to učinili, potvrdite okvir **mora biti spojene poruke** .  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Pogledajte tablicu u nastavku ako želite da biste saznali više o na primanje postavke omogućuju.  

|Svojstvo|Opis|
|----|----|
|Zanemarivanje svojstva poruke|To odaberite da biste naznačili da se možete nadjačati svojstava u primljenim porukama |
|Poruke se moraju biti prijavljeni|Omogućivanje to obavezne digitalno potpisanih poruka|
|Mora biti šifrirane poruke|Omogućivanje to obavezne poruke šifriranje. Osobe koje nisu šifrirane poruke će biti odbijene.|
|Mora biti spojene poruke|Omogućivanje to obavezne poruke se sažeti. Osobe koje nisu komprimirane poruke će biti odbijene.|
|MDN teksta|To je zadana MDN slati pošiljatelju poruke|
|Slanje MDN|Omogućivanje ovo da biste omogućili MDNs slati.|
|Slanje traje MDN|Omogućivanje to obavezne MDNs treba potpisati.|
|Algoritam Mikrofona||
|Slanje asinkronog MDN|Omogućivanje to obavezne asinkrono slanje poruka.|
|URL-A|To je URL adresa na koju se šalju poruke.|
Sada ćemo nastavak:  
8. Odaberite **Pošalji postavke** da biste konfigurirali kako se poruke koje se šalju putem ovog ugovora treba obraditi.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Pogledajte tablicu u nastavku ako želite da biste saznali više o koje Pošalji postavke omogućuju.  

|Svojstvo|Opis|
|----|----|
|Omogućivanje potpisivanje poruka|Potvrdite ovaj okvir da biste omogućili sve poruke poslane s ugovor treba potpisati.|
|Algoritam Mikrofona|Odaberite algoritam za korištenje u potpisivanje poruka|
|Certifikat|Odaberite certifikat za korištenje u potpisivanje poruka|
|Omogućivanje šifriranje poruka|Potvrdite ovaj okvir da biste šifrirali sve poruke poslane s ovog ugovora.|
|Algoritam za šifriranje|Odaberite algoritam za šifriranje da biste koristili u šifriranje poruka|
|Unfold HTTP zaglavlja|Potvrdite ovaj okvir da biste unfold HTTP zaglavlje vrsta sadržaja u jedan redak.|
|Zahtjev za MDN|Omogući ovaj okvir da biste zatražili programa MDN za sve poruke poslane s ovog ugovora|
|Zahtjev za traje MDN|Omogući da biste zatražili da sve MDNs poslao ovaj ugovor prijavljeni|
|Zahtjev za asinkronog MDN|Omogućite da bi se zatražila asinkronog MDN slati ovog ugovora|
|URL-A|URL koji će se slati MDNs|
|Omogućivanje NRR|Potvrdite ovaj okvir da biste omogućili Osigurano priznavanje potvrde|
Ne možemo gotovo gotovo!  
9. Odaberite pločicu **ugovore** na plohu Integracija računa i vidjet ćete novododani ugovor na popisu.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

