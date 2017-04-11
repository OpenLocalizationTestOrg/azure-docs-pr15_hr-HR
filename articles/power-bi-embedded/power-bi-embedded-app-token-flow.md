<properties
   pageTitle="Provjera autentičnosti i dopuštanja uz Power BI ugrađeni"
   description="Provjera autentičnosti i dopuštanja uz Power BI uloženi"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Provjera autentičnosti i dopuštanja uz Power BI ugrađeni

Servis Power BI ugrađene koristi **ključeva** i **Aplikacije tokeni** za provjeru autentičnosti i ovlaštenja, umjesto provjere autentičnosti eksplicitnih krajnjeg korisnika. U ovom modela aplikacija upravlja provjere autentičnosti i autorizacije za vaše krajnjim korisnicima. Kada je potrebno, pokrenite aplikaciju stvara i šalje tokeni aplikacije koja govori naših usluga za prikazivanje traženo izvješće. Ovaj dizajn ne zahtijeva aplikacije da biste koristili Azure Active Directory za provjeru autentičnosti korisnika i autorizacije, iako još uvijek ne možete.

## <a name="two-ways-to-authenticate"></a>Dva načina provjere autentičnosti

**Ključ** - tipki možete koristiti za sve pozive u Power BI ugrađene REST API-JA. Tipke pronaći ćete na **portal za Azure** tako da kliknete na **sve postavke** , a zatim **pristupnih tipki**. Uvijek tretirati ključ kao da je lozinka. Ove tipke imati dozvole za bilo koju REST API-JA poziva na zbirku određeni radni prostor.

Da biste koristili ključ na OSTALE poziv, dodajte zaglavlje autorizacije sljedeće:            

    Authorization: AppKey {your key}

**Aplikacija token** – aplikacije tokeni koriste se za sve zahtjeve za ugrađivanje. Osmišljene su će se pokrenuti klijentsko, tako da ih ograničena u jednom izvješće i je najbolji način da biste postavili neko vrijeme isteka.

Tokeni aplikacije su JWT (JSON Web tokena) koji je potpisao neku od svojih ključeva.

Vaš token aplikacije mogu sadržavati zahtjevima za sljedeće:

| Zahtjeva      | Opis        |
|--------------|------------|
| **ver**      | Verzija aplikacije token. 0.2.0 je trenutnu verziju.       |
| **aud**      | Svrhu primatelj token. Za Power BI ugrađene: "https://analysis.windows.net/powerbi/api".  |
| **ISS**      |  Niz koji označava aplikacija u kojoj je izdala token.    |
| **Vrsta**     | Vrsta tokena aplikacije koji je stvoren. Trenutni jedina podržana vrsta je **ugraditi**.   |
| **WCN**      | Naziv radnog prostora zbirke token izdavanja za.  |
| **RID**      | Radni prostor ID token izdavanja za.  |
| **Uklanjanje**      | ID izvješća token izdavanja za.     |
| **korisničko ime** (neobavezno) |  Koristi se s RLS, to je niz koji olakšava prepoznavanje korisnika prilikom primjene pravila RLS. |
| **uloge** (neobavezno)   |   Niz koji sadrži uloge da biste odabrali kad Primjena pravila sigurnosti na razini retka. Ako je prosljeđivanje više uloga mora biti proslijeđena kao sting polja.    |
| **Exp** (neobavezno)    |   Upućuje na to vrijeme u kojem će isteći token. Te treba proslijediti u obliku Unix vremenske oznake.   |
| **nbf** (neobavezno)    |   Upućuje na to vrijeme u kojem token pokreće se valjana. Te treba proslijediti u obliku Unix vremenske oznake.   |

Aplikacija token uzorka izgledat će ovako:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Kada dekodirati, izgledat će ovako:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Evo kako tijeka rada

1. Kopirajte tipki API-JA u aplikaciji. Tipke možete dobiti **Azure**portalu.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Token nametanja zahtjev i ima neko vrijeme isteka.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Token dobiva prijavljeni pomoću programa API pristupnih tipki.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Korisnik zahtjevi za prikaz izvješća.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Token se provjerava pomoću programa API pristupnih tipki.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI ugrađene šalje izvješća korisnika.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Nakon što **Power BI ugrađene** pošalje izvješća korisniku, korisnik može pregledavati izvješća u prilagođenu aplikaciju. Na primjer, ako ste uvezli [PBIX analiza podataka o prodaji uzorka](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), uzorak web-aplikaciji izgledati ovako:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Vidi također
- [Uvod u Microsoft Power BI ugrađene ogledne](power-bi-embedded-get-started-sample.md)
- [Uobičajeni scenariji za Microsoft Power BI ugrađeni](power-bi-embedded-scenarios.md)
- [Uvod u Microsoft Power BI ugrađeni](power-bi-embedded-get-started.md)
