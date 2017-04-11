<properties
   pageTitle="Objašnjenje računa | Microsoft Azure"
   description="Upute za čitanje i razumijevanje korištenja i računa za vašu pretplatu za Azure"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/31/2016"
   ms.author="erihur;genli"/>


# <a name="understand-your-bill-for-microsoft-azure"></a>Objašnjenje računa za Microsoft Azure

> [AZURE.NOTE] Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, ponovno se [obratiti službi za podršku](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) da biste dobili problem riješen brzo.

Naknade za pretplate na Microsoft Azure razlikuju se prema plan stopa. Neke cijenama, kao što su pretplatnike Visual Studio Enterprise (mreže Microsoftovih Partnera) obuhvaćaju mjesečni ekipa koje možete koristiti bilo koji servis za Azure ovisno o vašim potrebama.

Imajte na umu da se 24 sata latent korištenje iz prethodnih razdoblje naplate može biti prijavljene trenutno razdoblje naplate.

Dodatne informacije o potrošnje i cijenama potražite u članku [Mogućnosti za kupnju sustava Microsoft Azure stranice](https://azure.microsoft.com/pricing/purchase-options/).

<!-- The below links cover a complete list of all Microsoft Azure services.

<!-- - [Service Details list (csv1)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
<!-- - [Service Details list (csv2)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)

<!-- *NOTE: The **csv1** link refers to the column header names for csv version 1 and **csv2** link refers to the new column header names for csv version 2.  These files are updated monthly.*-->

### <a name="view-or-download-a-bill-for-microsoft-azure"></a>Prikaz i preuzimanje račun za Microsoft Azure:

1. Prijavite se u [Centar za račun](https://account.windowsazure.com/subscriptions) putem Microsoftova Account i ID tvrtke ili ustanove.

2. Kliknite pretplatu u koju želite da biste vidjeli detalje i korištenje.

3. Kliknite **povijesti naplate**

    ![Sažetak – -1 povijesti naplate](./media/billing-understand-your-bill/ContentViewaBillforMA1.png)

4. Odjeljak **Povijest naplate i** popise svojih izjava za prethodnih razdoblja naplate plus trenutno unbilled razdoblje. Naredba za trenutno razdoblje je procjena naplata od vremena generirana procijenjenu vrijednost. Ove informacije samo ažurira svakodnevno i ne obuhvaća sve korištenje nastale datum. Mjesečni račun mogu se razlikovati od ovaj članak Procjena uvjeta.  

    ![Sažetak naplata povijest 2](./media/billing-understand-your-bill/ContentViewaBillforMA2.png)

5. Kliknite **Prikaz trenutnog izjave** da biste pogledali Procjena naplata od vremena generirana procijenjenu vrijednost. Ove informacije samo ažurira svakodnevno i ne obuhvaća sve korištenje nastale datum. Mjesečni račun mogu se razlikovati od ovaj članak Procjena uvjeta.

    ![Sažetak naplata povijest 3](./media/billing-understand-your-bill/ContentViewaBillforMA3.png)

    ![Sažetak naplata povijest 4](./media/billing-understand-your-bill/ContentViewaBillforMA4.png)

6. Kliknite **Račun za preuzimanje** da biste pogledali kopiju prethodnog računa.

    ![Sažetak naplata povijest 5](./media/billing-understand-your-bill/ContentViewaBillforMA5.png)


> [AZURE.NOTE] Naknade za navedene na naplata naredbe za međunarodne klijenti su svrhe procjeni samo banaka imaju različite trošak Tečajevi pretvorbe.

Slijede neki izjave uzorka za dvije različite ponuda dostupne na Microsoft Azure.

 Vrsta ponude | Opis | Preuzimanje |
 :--------- |:-------- | :-------|
Pay-As-You-Go | U arrears plaćate mjesečno | [Ogledna datoteka](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_ccinvoice_Sample.pdf)
Ponuda izvršenja | Provedu deducted iz vaše pretplatnog izvršenja | [Ogledna datoteka](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_invoice_Sample.pdf)

## <a name="account-information"></a>Informacije o računu

U odjeljku informacije o račun označava važne informacije o korištenju i profila.

![Zaglavlje](./media/billing-understand-your-bill/Header.png)

| Termina                | Opis                                                                                         |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Broj faktura         | Fakture Jedinstveni identifikator radi praćenja                                                   |
| Ciklusa naplate       | Vremenski okvir u kojem korištenje dogodilo                                                       |
| Datum fakture        | Datum koji je generirana fakture                                                                 |
| Način plaćanja      | Vrsta plaćanja koristiti na računu (nakon primitka računa ili kreditnom karticom)                                   |
| Za naplatu             | Adresa uplate Microsoft Azure                                                                    |
| Pretplata na  | Vrsta pretplata koja je kupljena (Pay-As-You-Go BizSpark Plus, pristupni Azure, itd.) |
| E-pošte za vlasnika računa | Adresu e-pošte za račun koji je račun za Microsoft Azure registriran u odjeljku                      |

## <a name="understand-the-invoice-summary"></a>Razumijevanje sažetak računa

**Sažetak faktura** dio naplate navedene transakcije od zadnjeg računa i trenutni naknade za korištenje.

![Sažetak računa](./media/billing-understand-your-bill/InvoiceSummary.png)

Prethodni saldo, uplate i preostala saldo dio naplate navedene transakcije od zadnjeg računa.

| Termina                                              | Opis                                                                              |
|---------------------------------------------------|------------------------------------------------------------------------------------------|
| Prethodni saldo                                  | Ukupni Dospjeli iznos iz zadnjeg računa                                                 |
| Uplata                                          | Ukupna uplata primjenjuje se na zadnju računa                                                 |
| Preostalu saldo (iz prethodne ciklusa naplate) | Prilagođavanja računa (kredita ili salda) primjenjuje se na svoj račun od zadnjeg računa  |

## <a name="understand-the-current-charges"></a>Razumijevanje aktualnih naknada
U odjeljku aktualnih naknada naplate sadrži detalje o mjesečni troškovi. Veze su organizirane u Pododlomci koji slijede.

| Termina          | Opis                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Naknade za korištenje | Naknade za korištenje su ukupni mjesečni troškovi na pretplatu. Se naplaćuju u arrears za korištenje prošlog mjeseca.                                                                                                                                                                                                                                                                                                                                       |
| Popusta     | Servis popuste primjenjuje se na trenutni računa odrazit će se u tu stavku retka.                                                                                                                                                                                                                                                                                                                                              |
| Prilagodbe   | Usklađivanje razni su različiti kredita ili preostala naknada primjenjuje se na trenutni računa. Ako, na primjer, ako imate Visual Studio Enterprise s MSDN ponuda, pogledajte mjesečni kreditne kartice u tu stavku retka. Ako otkažete pretplatu, prikazat će naknade za mjesečni korištenja više od mjesečni odobrenja uključiti u ponudu od početka trenutno razdoblje naplate za vaše Datum otkazivanja pretplate.|

## <a name="footer-information"></a>Informacije u podnožje
![podnožje](./media/billing-understand-your-bill/footerinformation.png)

## <a name="understand-the-additional-information"></a>Razumijevanje dodatnih informacija
Dodatne informacije o stranica omogućuje reference na druge resurse da biste shvatili računa i veze za prikaz Upotreba i drugih relevantnih podataka za vaš račun.

![Dodatne informacije](./media/billing-understand-your-bill/AdditionalInformation.png)

### <a name="detailed-usage"></a>Detaljne korištenje
Veze u polje u odjeljku **Korištenje detaljne** usmjerava centar za račun gdje možete pregledati detaljne Upotreba za ovu pretplatu.  Sada postoje dvije verzije za preuzimanje: **.csv verzija 1** sadrži stare konvencije imenovanja i korištenje polja i **.csv verzija 2** sadrži nazive kupaca neslužbeni za svaku kategoriju plus dodatna polja koja će vam pomoći shvatiti što usluge koje koriste na Microsoft Azure. Imajte na umu da u .csv verzija 1 da ne postoje Voditelj resursa Azure Detalji. Azure resursima informacije pronaći ćete u verziji .csv 2.

### <a name="additional-information-and-useful-resources"></a>Dodatne informacije i korisne resurse
U ovom se odjeljku nalaze se veze na jednostavna pitanja vezane uz računalnim instancu veličine, SQL DB naknade i korisne veze da biste lakše pitanja Dodatno.

| Termina                 | Opis                                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Prodana              | To je prethodno s adresom profila na računu                                                                           |
| Upute za plaćanje | U ovom se odjeljku je upute za plaćanje gdje da biste poslali provjere, žičani prijenosa ili preko noći provjere ako je način plaćanja fakture |

## <a name="understand-detailed-usage-charges"></a>Razumijevanje detaljne ispravnim

Kao dio naš tijeku izvršenja korisnicima jednostavno upravljanje njihova Azure Upotreba olakšava, ne možemo ste poboljšani preuzetu datoteku korištenje izvješća o korištenju servisa Azure i troškove.  Veza za preuzimanje sadrži dvije verzije datoteke korištenje:

- **Verzija 1** koristi postojećih oblik

- **Verzija 2** sadrži dodatne informacije i ažurirati naziva stupaca u odjeljku dnevnih korištenje.  

Naknade za korištenje su ukupni **mjesečni** troškovi na pretplatu manje odobrenja ni popust. Se naplaćuju u arrears za korištenje prošlog mjeseca.  Gornji dio datoteke prikazuje detalje na servise koji su koji se naplaćuju za tijekom ciklusa naplate na prethodni mjesec.  U prethodnoj tablici popis naziva stupaca za svaku verziju .csv datoteke.

Verzija 1 |  Verzija 2  |  Opis|
:---------------| :---------------- | --------|
Razdoblje naplate | Razdoblje naplate | Razdoblje naplate kada je potrošena resurs.
Ime | Metar kategorija | Služi za identifikaciju najviše razine servisa za koji pripada korištenje.
Vrsta | Metar pod kategorija | Azure servis možda dalje definirane po vrsti u ovom stupcu koji mogu utjecati na stopa.
Resurs | Naziv metar | Služi za identifikaciju mjernu jedinicu resursa koji se potrošena.
Regija | Metar regija | Određuje mjesto s podatkovnim centrom za određene servise koje su cjenovnom ovisno o lokaciji podatkovnog centra.
SKU | SKU | Služi za identifikaciju jedinstveni sustava identifikator za svaki Azure resurs.
Jedinica | Jedinica | Označava jedinicu koju je uslugu naplatiti u. Na primjer, GB, radno vrijeme, 10,000s.
Potrošena | Potrošene količine | Sadrži iznos resurs koji sadrži potrošena tijekom razdoblja naplate.
Dio | Uključeni količina | Sadrži iznos resursa koji je sadržan bez troškova u trenutno razdoblje naplate.
Naplatu | Pokrivenosti količina | Ako je iznos Consumed premašuje uključene iznos, ovaj stupac prikazuje razliku. Se naplatiti za taj iznos. Za Pay-As-You-Go ponuda bez iznosa uključene ponudu, ukupan zbroj bit će jednaka onoj Consumed.
Unutar izvršenja | Unutar izvršenja | Sadrži troškove resursa koji su pala iz vaše iznosa izvršenja pridružene 6 ili 12 mjeseci ponudu. Naplata resursa su pala od iznosa izvršenja kronološkim redoslijedom.
Valuta | Valuta | Označava valutu odražavaju u trenutno razdoblje naplate.
Pokrivenost | Pokrivenost | Sadrži troškove resursa koji premašuje vaše iznos izvršenja pridružene 6 ili 12 mjeseci ponudu.
Stopa izvršenja | Stopa izvršenja | Sadrži stopu izvršenja koji se temelji na vaše izvršenja ukupni iznos pridružene 6 ili 12 mjeseci ponudu.
Stopa | Stopa | Stopa prikazuje stopa vam se naplatiti po jedinici za naplatu.
Vrijednost | Vrijednost | Prikazuje rezultat množenja naplatu stupca prema stupcu stopa. Ako je iznos Consumed ne prekoračuje uključene, pojavit će se bez troškova u tom stupcu.

## <a name="analyze-daily-usage-data"></a>Analiza dnevnih podataka o korištenju
Ovisno o korištenju, može biti tisuću redaka podataka s dnevnim korištenje. Ako želite da biste analizirali podatke, kliknite **Preuzimanje korištenje** i odaberite verziju odvojenih zarezom varijable datoteke (.csv) da biste vidjeli dnevnih podataka o korištenju za odgovarajuće razdoblje naplate.  Za referencu, možete preuzeti oglednu datoteku za svaku verziju donje za .csv.

 Ime | Preuzimanje |
 :----------:| :-------: |
  Detaljne korištenje .csv verzija 1|  [Ogledna datoteka](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
  Detaljne korištenje .csv verzija 2 | [Ogledna datoteka](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)



![csv2screenshot](./media/billing-understand-your-bill/csv2screenshot.png)



U datoteci .csv, stavke su podijeljene da bi se prikazao popis koliko se svaki resurs je potrošena unutar trenutno razdoblje naplate.

![CSV snimke](./media/billing-understand-your-bill/csvsnapshotportal.png)

Sljedeći stupci prikazuju pojedinosti koje utječu na tečajevima na početku razdoblje naplate:

Verzija 1 |   Verzija 2   |  Opis |
:---------------| :----------------| -----|
Korištenje datuma | Korištenje datuma | Datum kada je čuje resurs.
Ime | Metar kategorija | Služi za identifikaciju najviše razine servisa za koji pripada korištenje.
Resurse GUID | ID metar | Identifikator naplate metar.  Koristi se kao identifikator za cijene naplate korištenje.
Vrsta | Metar pod kategorija | Azure servis možda dalje definirane po vrsti u ovom stupcu koji mogu utjecati na stopa.
Resurs | Naziv metar | Služi za identifikaciju mjernu jedinicu resursa koji se potrošena.
Regija | Metar regija | Određuje mjesto s podatkovnim centrom za određene servise koje su cjenovnom ovisno o lokaciji podatkovnog centra.
Jedinica | Jedinica | Označava jedinicu koju je uslugu naplatiti u. Na primjer, GB, radno vrijeme, 10,000s.
Potrošena | Potrošene količine | Sadrži iznos resurs za taj dan.
Područje Sub | Lokacija resursa | Služi za identifikaciju s podatkovnim centrom na kojem se pokreće resurs.
Servis | Potrošenih servisa | Ovaj stupac koristi se za praćenje pojedinačnih platforme Azure servis možda neće se izričito prikazati u stupcu naziv. Taj servis označava stupac koji određeni servisa korištenje odnosi.
N/D | Grupa resursa | _**Zbrajanje novi stupac.**_ Grupa resursa distribuiranih resursa u kojem se izvodi u. Pogledajte [Pregled upravitelja resursa za Azure](../azure-resource-manager/resource-group-overview.md)
Komponenta | Instance ID | Identifikator za izvodi resursa. Identifikator sadrži ime koje navedete za resurs kada je stavka stvorena.
N/D | Oznaka | _**Zbrajanje novi stupac.**_ Nove vrste resursa u Azure omogućuju oznaka resursi. Pogledajte da biste [organizirali Azure resursa s oznakama](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/)
Dodatne informacije | Dodatne informacije | Dodatne metapodatke povezane s uslugom.
Informacije o servisu 1 | Informacije o servisu 1 | Ovaj stupac sadrži naziv projekta koji pripada servis pretplate na.
Informacije o servisu 2 | Informacije o servisu 2 | Ovo je naslijeđene polje koji opisuje neobavezno metapodataka specifične za servis.

Osim neke nova polja i promjene naziva csv verzija 2, još će biti standardizirani priručniku za oblikovanje podataka u na ispod polja:

- **Instance ID**: U ID Instance polje predstavlja korisnika naveden je identifikator za servis dodijeljena. Trenutno postoje dva oblika predstavlja Instance ID: je tekst naziv resursa ili potpuno kvalificiran ID resursa. Servisa Microsoft Azure su pri prijelazu na predstavljaju ID Instance u obliku standardizirani potpuno kvalificiran ID resursa _**(/subscriptions/<subscription id>/resourcegroups/<resourcegroupname>/providers/<providername>/<resourcename>)**_. Kako usluge prijelaz u novi oblik vidjet ćete podatkovno polje Instance ID promjena iz samo s nazivom resursa u ID resursa. ID resursa je oblik koji se koristi za prepoznavanje resursa u pretplatu [Azure resursima API](https://msdn.microsoft.com/library/azure/dn790567.aspx) .

![ID instance](./media/billing-understand-your-bill/instanceid.png)

- **Dodatne informacije**: stupac na dodatne informacije u .csv korištenje određuje metapodataka specifične za servis. Na primjer, vrstu slike za na VM. Trenutno servisa emits specifične za servis metapodataka u više stupaca: dodatne informacije, Info1 servisa i informacije o servisnom paketu 2 polja. Servisa Microsoft Azure će standardizing emitira specifične za servis metapodataka u samo stupac dodatne informacije.  U odjeljku u nastavku snimku standardiziranog oblika:

![additionalinfo_csv2](./media/billing-understand-your-bill/AdditionaInfo_csv2.png)

- **Oznaka**: stupac mora sadržavati korisnika koji je naveden resurs oznake. Oznake može se koristiti za grupiranje zapisa za naplatu. Ako, na primjer, možete koristiti oznake distribuirati troškove po odjelu korištenja servisa. Dodatne informacije o [korištenju oznake da biste organizirali Azure resursi](../resource-group-using-tags.md). Usluga koji podržavaju emitira oznake su:  

    - Virtualnim strojevima

    - Prostor za pohranu i

    - Povezivanje s mrežom servisa dodjeli pomoću [API resursima Azure](https://msdn.microsoft.com/library/azure/dn790567.aspx)

![oznaka](./media/billing-understand-your-bill/tags.png)


## <a name="next-steps"></a>Daljnji koraci

- [Postavljanje upozorenja za naplatu](../billing-set-up-alerts.md)

- [Upravljanje načinima plaćanja](../billing-how-to-change-credit-card.md)

- [Razumijevanje naknade za Azure Marketplace](../billing-understand-your-azure-marketplace-charges.md)

- [Najčešća pitanja o Azure naplatom i pretplatom](../billing-subscription-faq.md)

> [AZURE.NOTE] Ako i dalje imate dodatno pitanja, ponovno se [obratiti službi za podršku](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) da biste dobili problem riješen brzo.

<!--
OLD MSDN Articles
- [What do I do if my Azure subscription become disabled?](https://msdn.microsoft.com/library/azure/dn736049.aspx)
- [Edit payment information for an existing credit card](https://msdn.microsoft.com/library/azure/dn736053.aspx)
- [Add a new credit card to use as a payment method](https://msdn.microsoft.com/library/azure/dn736057.aspx)
- [Change the credit card on your Microsoft Azure account](https://msdn.microsoft.com/library/azure/dn736050.aspx)
- [Manage your payment method](https://msdn.microsoft.com/library/azure/dn736054.aspx)
-->



<!--Image references-->
