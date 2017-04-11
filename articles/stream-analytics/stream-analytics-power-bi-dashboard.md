<properties
    pageTitle="Nadzorna ploča Power BI na strujanje analize | Microsoft Azure"
    description="Prikupljanje poslovno obavještavanje i analiza podataka visoku glasnoću iz analize toka posla pomoću u stvarnom vremenu nadzorne ploče strujanje Power BI."
    keywords="nadzornoj ploči analitike, u stvarnom vremenu nadzorne ploče"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Strujanje analize i Power BI: nadzorne ploče u stvarnom vremenu analytics za strujanje podataka

Azure strujanje Analytics omogućuje da iskoristite prednost nešto na početku alata za poslovnu inteligenciju, Microsoft Power BI. Saznajte kako koristiti Azure strujanje Analytics za analizu visoku glasnoću, strujanja podataka i dobivanje uvid u stvarnom vremenu nadzornoj ploči analitike Power BI.

Koristite [Microsoft Power BI](https://powerbi.com/) da biste brzo stvaranje uživo nadzorne ploče. [Pogledajte videozapis ilustraciju scenarija](https://www.youtube.com/watch?v=SGUpT-a99MA).

U ovom se članku saznati kako stvoriti vlastiti alata za prilagođene poslovnu inteligenciju pomoću Power BI kao na izlaz za Azure strujanje Analitika poslova i korištenje nadzorne ploče komponente u stvarnom vremenu.

## <a name="prerequisites"></a>Preduvjeti

* Račun za Microsoft Azure
* Unos za posao strujanje analize korištenja strujanja podataka iz. Analitički strujanje prihvaća unos iz koncentratora Azure događaj ili blobova platforme Azure prostora za pohranu.  
* Račun tvrtke ili škole za Power BI

## <a name="create-azure-stream-analytics-job"></a>Stvaranje zadatka Azure strujanje Analytics

[Azure klasični Portal](https://manage.windowsazure.com)kliknite **Novo, podatkovne usluge, strujanje analize, brzo stvaranje**.

Navedite sljedeće vrijednosti pa kliknite **Stvaranje analize strujanje posao**:

* **Naziv zadatka** - unesite naziv zadatka. Na primjer, **DeviceTemperatures**.
* **Područje** – odaberite područje gdje želite da se posao koja se nalazi. Razmotrite smještanje posla i događaja središte na istom području da biste omogućili optimalne performanse i izbjegli povećavanja troškove prijenos podataka između područja.
* **Račun za pohranu** – odaberite račun za pohranu koji želite koristiti za pohranu nadzora podataka za sve zadatke strujanje analize koji se izvodi u tom području. Imate mogućnost da biste odabrali postojeći račun za pohranu ili stvorite novi.

Kliknite **Analitika strujanje** u lijevom oknu za prikaz popisa zadataka strujanje analize.

![graphic1][graphic1]

> [AZURE.TIP] Novi zadatak će se prikazati sa statusom **Nije pokrenut**. Obratite pozornost na to je onemogućen gumb **Start** na dno stranice. Ovo je očekivano kao što je morate konfigurirati unos posla, izlazna, upit, i tako dalje prije no što počnete posao.

## <a name="specify-job-input"></a>Odredite unos zadatka

Za ovaj vodič smo su uz pretpostavku da koristite događaj koncentrator kao ulaz s JSON serijalizacije i UTF-8 šifriranje.

* Kliknite naziv zadatka.
* Kliknite **unosa** na vrhu stranice, a zatim **Dodajte unos**. Dijaloški okvir koji će se otvoriti vodit će vas kroz nekoliko koraka možete postaviti unos.
*   Odaberite **strujanja podataka**, a zatim kliknite na desni gumb.
*   Odaberite **Događaj koncentrator**, a zatim kliknite na desni gumb.
*   Upišite ili odaberite sljedeće vrijednosti na trećoj stranici:
  * **Unos pseudonim** – Upišite neslužbeni naziv za ovaj zadatak za unos. Imajte na umu da ćete koristiti taj naziv u upitu kasnije.
  * **Koncentrator događaj** – ako središtu događaj koji ste stvorili u okviru iste pretplate kao posao strujanje analize, odaberite je prostora za naziv koji se nalazi središtu događaja u.
*   Ako je koncentratora za događaj u neku drugu pretplatu, odaberite **Koristi događaj središte na drugu pretplatu** i ručno unesite podatke za **Polje naziva Bus servisa**, **Naziv događaja koncentrator**, **Naziv pravila koncentrator događaja**, **Ključ pravila koncentrator za događaj**i **Count particija koncentrator za događaj**.

> [AZURE.NOTE]  Ovaj primjer koristi zadani broj particije, koji je 16.

* **Naziv događaja koncentrator** - odaberite naziv središtu događaj Azure imate.
* **Naziv pravila koncentrator događaja** - odaberite pravila događaj koncentrator za središtu događaj koji koristite. Provjerite je li to pravilo ima Upravljanje dozvolama.
*   **Događaj koncentrator korisničke grupe** – možete ostavite prazan ili navesti grupu potrošača imate koncentratora za događaj. Imajte na umu svake grupe potrošača koncentratora za događaj može imati samo 5 čitatelji odjednom. Tako, sukladno tome odlučite grupi desnom potrošača za svoj posao. Ako polje ostavite prazno, će koristiti zadanu grupu korisnika.

*   Kliknite gumb desno.
*   Navedite sljedeće vrijednosti:
  * **Oblikovanje serijalizatora događaj** - JSON
  * **Kodiranje** - UTF8
*   Kliknite gumb Provjeri da biste dodali izvoru i da biste provjerili možete li strujanje Analytics uspješno povezati s koncentratora za događaj.

## <a name="add-power-bi-output"></a>Dodajte izlazna Power BI

1.  Kliknite **Izlaz** iz vrha stranice, a zatim **Dodajte izlaz**. Prikazat će se Power BI naveden kao mogućnost za izlaz.

    ![graphic2][graphic2]  

2.  Odaberite **Power BI** , a zatim kliknite gumb desno.
3.  Prikazat će se na zaslon, kao što je sljedeća:

    ![graphic3][graphic3]  

4.  U ovom ćete koraku navedite računa tvrtke ili škole za posao izlaz strujanje analize. Ako već imate račun za Power BI, odaberite **Autorizirali odmah**. Ako nije, odaberite **prijavite se odmah**. [Evo dobar bloga prolaska kroz pojedinosti o registracija za Power BI](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Sljedeće prikazat će se na zaslon, kao što je sljedeća:

    ![graphic4][graphic4]  

Sadrže vrijednosti kao ispod:

* **Pseudonim za izlaz** – možete pohraniti sve izlazne pseudonim koji je lako odnose. Pseudonim za izlaz je osobito korisni ako odlučite da bi više izlaza za svoj posao. U tom slučaju morate potražite u ovom Izlaz u upitu. Na primjer, pretpostavimo koristite vrijednost pseudonim Izlaz = "OutPbi".
* **Naziv skupa podataka** - Navedite naziv skupa podataka koje želite servisa Power BI izlaz da bi. Na primjer, pretpostavimo koristiti "pbidemo".
*   **Naziv tablice** - Navedite naziv tablice u odjeljku dataset izlaz Power BI. Recimo da ne možemo ga pozovite "pbidemo". Trenutno Power BI Izlaz iz strujanje Analitika poslova može imati samo jednu tablicu u skup podataka.
*   **Radni prostor** – odaberite radnog prostora u klijentu za Power BI u kojem će se stvoriti skupu podataka.

>   [AZURE.NOTE] Nemojte izričito određivati ovaj skup podataka i tablicu na računu servisa Power BI. Oni će se automatski stvara kada pokrenete posla strujanje analize i posao započinje pumping Izlaz u Power BI. Ako posao upit ne pronađe rezultate, skup podataka i tablicu ne stvorit će se.

*   Kliknite **u redu**, **Testiraj vezu** , a sada pošaljete configuraiton dovršetka.

>   [AZURE.WARNING] Također imajte na umu da ako Power BI već imate skup podataka i tablicu s istim nazivom kao navedena u ovom zadatku strujanje analize, postojeće podatke će prebrisati.


## <a name="write-query"></a>Pisanje upita

Idite na karticu **upit** vaš posao. Upit i izlaz iz koje želite u dodatku Power BI za pisanje. Na primjer, Razlog može biti nešto kao što su sljedeće SQL upita:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Pokrenite posla. Provjerite valjanost koncentratora za događaj prima događaje te upit generira očekivane rezultate. Ako je upit proizvodi 0 redaka, skup podataka za Power BI i tablica će neće se automatski stvarati.

## <a name="create-the-dashboard-in-power-bi"></a>Stvaranje nadzorne ploče u dodatku Power BI

Otvorite [Powerbi.com](https://powerbi.com) i prijavite se pomoću računa tvrtke ili obrazovne ustanove. Ako upit posao strujanje analize proizvodi rezultata, vidjet ćete svoje dataset već stvorili:

![graphic5][graphic5]

Za stvaranje nadzorne ploče, idite na mogućnost nadzornih ploča i stvorite novu nadzornu ploču.

![graphic6][graphic6]

U ovom primjeru smo ćete lable je "Pokazni videozapis nadzorne ploče".

Sada kliknite dataset stvorila analize toka posla (pbidemo u našem primjeru trenutni). Bit ćete usmjereni na stranicu da biste stvorili grafikon pri vrhu ovog skupa podataka. Ovo je osim jednog primjera izvješća možete stvoriti:

Odaberite Σ temp i vremena polja. Automatski se može vrijednosti i osi grafikona:

![graphic7][graphic7]

Uz to, automatski će se grafikon kao ispod:

![graphic8][graphic8]

U odjeljku vrijednosti kliknite na padajućem izborniku prema dolje za temp i odaberite **Prosječna** temperatura i na grafikonu, kliknite na **vizualizaciju** i odaberite **linijski grafikon**:

![graphic9][graphic9]

Sada će dobiti linijski grafikon od prosjeka tijekom vremena.  Pomoću mogućnosti PIN-a kao dolje možete prikvačiti to nadzorne ploče koju ste prethodno stvorili:

![graphic10][graphic10]

Sada kada pregledavate nadzorna ploča s prikvačene izvješće, vidjet ćete ažuriranje izvješća u stvarnom vremenu. Pokušajte tako promjena podataka u svoje događaje – temp šiljka ili nešto i će vidjeti u stvarnom vremenu učinak koje odražavaju nadzorne ploče.

Napomena ovog praktičnog vodiča planirati je kako stvoriti, ali jedne vrste grafikona za skup podataka. Power BI omogućuju stvaranje druge korisnike alata za poslovnu inteligenciju za tvrtku ili ustanovu. Drugi primjer primjene nadzorne ploče komponente Power BI u pogledajte videozapis [Uvod u Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Dodatne informacije o konfiguriranju Power BI izlazne i mogu koristiti Power BI grupe, pregledajte [sekcije Power BI](stream-analytics-define-outputs.md#power-bi) [Razumijevanje strujanje analize proizvodi](stream-analytics-define-outputs.md "Proizvodi razumijevanje strujanje analize"). Ako neki drugi resurs korisno da biste saznali više o stvaranju nadzornih ploča pomoću dodatka Power BI je [nadzornih ploča u dodatku Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Ograničenja i najbolje prakse

Power BI uključuje istodobnosti i propusnost ograničenja, kao što je opisano u nastavku: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Cijene za Power BI")

Zbog one Power BI stane sam najčešće prirodan u slučajevima gdje Azure strujanje analize ne smanjena učitavanja značajan podataka.
Preporučujemo korištenje TumblingWindow ili HoppingWindow da biste bili sigurni da automatske podataka biti najviše 1 automatske/sekundi i da stane upit unutar preduvjeti propusnost – pomoću sljedeće jednadžbe za izračun vrijednosti da bi se dobilo prozora u sekundama:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Kao primjer – ako imate 1000 uređaja slanje podataka u sekundi, uključeni u Power BI Pro SKU koji podržava 1,000,000 redaka/h, a želite dobiti prosječnu podataka po uređaj u dodatku Power BI možete učiniti najviše na automatske svaki 4 sekunde po uređaj (kao što je prikazano u nastavku):  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Što znači da promijenit ćemo izvorni upit:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>Osvježavanje prikaza PowerBI

Uobičajena pitanja je "Zašto ne na nadzornoj ploči automatski ažurirati u PowerBI?".

Da biste to postigli, PowerBI koriste pitanja i odgovora i postavljati pitanja kao što su "Maksimalna vrijednost po temp gdje je vremenska oznaka danas" i prikvačivanje pločice nadzorne ploče.

### <a name="renew-authorization"></a>Obnavljanje autorizacija

Morat ćete ponovno autentičnost računa za Power BI ako lozinku promijenjena jer je stvoren ili zadnje autentičnost posla. Ako je višestruke provjere autentičnosti (MFA) konfiguriran na klijentu za Azure Active Directory (AAD) i morat ćete obnoviti autorizaciju Power BI svaki dva tjedna. Simptoma tog problema je bez izlaz posla i "provjere autentičnosti korisnika pogreška" u zapisnicima postupak:

![graphic12][graphic12]

Isto tako, posao pokuša pokrenuti dok je istekao token, pojavit će se pogreška i početka posla neće uspjeti. Pogreška će izgledati kao što su ispod:

![Pogreška pri provjeri valjanosti PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

Da biste riješili taj problem, zaustavljanje pokrenute posla, a zatim odaberite izlaz Power BI.  Kliknite vezu "Obnavljanje autorizacije" pa ponovno pokrenite posla s prekinuli posljednjeg da biste izbjegli gubitak podataka.

![Obnavljanje PowerBI provjere valjanosti](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Nakon osvježavanja autorizacija pomoću dodatka Power BI prikazat će se zelena upozorenja u području autorizacije:

![Obnavljanje PowerBI provjere valjanosti](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
