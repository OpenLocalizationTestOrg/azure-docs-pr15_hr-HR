<properties
    pageTitle="Korištenje uzoraka skupa podataka u strojnog učenja Studio | Microsoft Azure"
    description="Opisi skupa podataka koji se koriste u modelima uzorka obuhvaćeno ML Studio. Ove skupova podataka možete koristiti za svoje eksperimenata."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Korištenje uzoraka skupa podataka u Azure strojnog učenja Studio

[top]: #machine-learning-sample-datasets

Prilikom stvaranja novog radnog prostora u Azure strojnog učenja, po zadanom obuhvaća broj skupova podataka i eksperimenata. Mnoge od tih skupova podataka koji se koristi modelima uzorak u [Galeriju obavještavanje Cortana za Azure](http://gallery.cortanaintelligence.com/), a drugi uključene su kao primjeri različitih vrsta podataka obično se koristi u strojnog učenja.

Neke od tih skupova podataka dostupni su u spremište blobova platforme Azure. Za ove skupa podataka u tablici u nastavku nudi izravnu vezu. Možete koristiti te skupa podataka u vašem eksperimenata pomoću [Uvoz podataka] [ import-data] modul.

Ostatak ovih skupova podataka navedene su u odjeljku **Spremili skupove podataka** u paleti modulu s lijeve strane eksperiment crtanja kada otvorite ili stvorite novi eksperiment u ML Studio.
Možete koristiti bilo koju od tih skupova podataka u vlastiti eksperiment povlačenjem vaše eksperiment područje crtanja.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Naziv skupa podataka</th>
  <th align=left>Opis skup podataka</th>
</tr>

<tr>
  <td valign=top>Odrasle dataset statistiku dobit binarni klasifikacija</td>
  <td valign=top>
Podskup 1994 statistiku bazu podataka, rad odrasli putem starost 16 s indeksa prilagođeni dobit > 100.<p> </p><b>Korištenje:</b> klasifikaciju osobama koje koriste demografskih podataka za predviđanje li osoba donosi veće od 50K godišnje.<p> </p><b>Srodni Istraživanje:</b> Kohavi, R., Becker, b, (1996). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Skup podataka kodovi zračna luka</td>
  <td valign=top>
Kodovi zračna luka SAD-a.<p> </p>Ovaj skup podataka sadrži jedan redak za svaki luka SAD-a, koja omogućuje airport ID broj i ime uz mjesto gradu i županiji.
  </td>
</tr>

<tr>
  <td valign=top>Područja cijena podataka (Raw)</td>
  <td valign=top>
Informacije o automobiles tako provjerite i model, uključujući cijenu, značajke kao što su broj cilindre i MPG, kao i u rezultatu osiguranja rizika.<p> </p>Rezultat rizika prethodno vezan uz automatsko cijena i prilagodbe za stvarni rizika u postupku poznati actuaries kao symboling. Vrijednost +3 označava da je automatsko opasan, a vrijednost -3 da je vjerojatno vrlo sigurni.<p> </p><b>Korištenje:</b> predviđanje rezultat rizika značajke, pomoću regresije ili multivariate klasifikacija. <p> </p><b>Srodni Istraživanje:</b> Schlimmer, J.C. (1987). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Dataset najam UCI za bicikle</td>
  <td valign=top>
Najam bicikle UCI skup podataka koji se temelji na realni podataka tvrtke Bikeshare veliko slovo koji se održava najam mreže za bicikle u Kontroler Washington.<p> </p>Skup podataka sadrži jedan redak za svaki sat svakog dana u 2011 i 2012, za Ukupno 17,379 redaka. Raspon zaračunava bicikle i usluge najma je od 1 do 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Myhosting RGB slike</td>
  <td valign=top>
Javno objavljivanje slikovne datoteke se pretvaraju u CSV podatke.<p> </p>Kod za pretvaranje slika navedeni su na stranici za <strong>boja quantization pomoću K znači Klasteriranje</strong> modela detalja.
  </td>
</tr>

<tr>
  <td valign=top>Krvnog donacije podataka</td>
  <td valign=top>
Podskup podataka iz baze podataka donatora krvnog na Transfusion krvnog usluge centra Hsin Chu Grad, Tajvanu.<p> </p>Donatora podaci sadrže mjeseci od posljednjeg donacije), učestalost, ili ukupan broj donacije, vrijeme od posljednjeg donacije, a količinu krvnog donirano.<p> </p><b>Korištenje:</b> cilj je za predviđanje putem klasifikacija li donatora donirano krvnog u ožujku 2007, gdje 1 označava donatora tijekom razdoblja cilj i 0 nije-donatora. <p> </p><b>Srodni Istraživanje:</b> Yeh I.C., (2008). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti <p> </p>Yeh, mogu-Cheng, Yang, kralja Jang i mogućeni, dodirnuti Ming "znanja otkrivanje na model RFM pomoću niza Bernoullijevih" stručnjak sustavi s aplikacijama, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Pregledi adresara iz Amazon</td>
  <td valign=top>
Pregledi s uklonjenim zaglavljem IDNarudžbe u Amazon, s web-mjesta okvir zauzima istraživači University Pennsylvania su (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">šalju</a>). Istraživanje papira, u odjeljku "Biographies, Bollywood, Boom okvire i Blenders: adaptacijski domene za klasifikaciju šalju" Nevena Blitzer, Označi Dredze i Fernando Pereira; Pridruživanje od računalne Lingvistička (ACL) 2007.<p> </p>Izvorni dataset ima 975K recenzije s rangiranje 1, 2, 3, 4 i 5. Na recenzije su pisane na engleskom i s vremenskom razdoblju 1997 2007. Ovaj skup podataka je uzorkovanja dolje da biste 10K recenzije.
  </td>
</tr>

<tr>
  <td valign=top>Breast cancer podataka</td>
  <td valign=top>
Jedna od tri vezane uz cancer skupove podataka koje ste dobili od Institut za Oncology koji se često pojavljuje u literature strojnog učenja. Kombinira dijagnostičke informacije sa značajkama iz laboratorijska analizu oko 300 postavke automatskog uzorka.<p> </p><b>Korištenje:</b> klasifikaciju vrstu cancer, na temelju 9 atribute neke od kojih su linearni i neke su categorical. <p> </p><b>Srodni Istraživanje:</b> Wohlberg, W.H., ulica, W.N. i Mangasarian, O.L. (1995). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Značajke Cancer breast <td valign=top>
Skupu podataka sadrži podatke za 102K sumnjiva regije (kandidata) X-ray slike, svaki opisane 117 značajke. Značajke su vlasničkih i njihova značenja se površina prikazati tako da kreatora skup podataka (Siemens Zdravstvena Zaštita). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Informacije o Cancer breast</td>
  <td valign=top>
Skup podataka sadrži dodatne informacije za svako područje sumnjiva X-ray slike. Svaki primjer pruža informacije (npr., natpis, pacijenta ID-a, koordinate zakrpu odnosu cijelu sliku) o odgovarajući broj redaka u skupu podataka Breast Cancer značajke. Pacijent ima nekoliko primjera. Za bolesnike koji dolaze koji imaju na cancer, primjere su pozitivni i neke su negativni. Za bolesnike koji dolaze koje nemaju na cancer, svim primjerima nije negativna Skup podataka sadrži primjere 102K. Skup podataka je usmjeren, % 0,6 naglaske su pozitivni, dok su ostale negativne. Skup podataka je učinio dostupnima Zdravstvena zaštita Siemens.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>CRM Appetency natpise zajedničko korištenje</td>
  <td valign=top>
Oznake iz KDD Cup 2009 kupca odnos predviđanje test (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM Churn natpise zajedničko korištenje</td>
  <td valign=top>
Oznake iz KDD Cup 2009 kupca odnos predviđanje test (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>Skup podataka CRM zajedničko korištenje</td>
  <td valign=top>
Ove podatke dolazi iz KDD Cup 2009 kupca odnos predviđanje test (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Skup podataka sadrži 50K Kupci iz tvrtke Telecom francuski Narančasto. Svaki korisnik ima 230 anonymized značajke 190 koje su numeričke i 40 su categorical. Značajke su vrlo kratke.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>CRM Upselling natpise zajedničko korištenje</td>
  <td valign=top>
Oznake iz KDD Cup 2009 kupca odnos predviđanje test (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Podaci regresije učinkovitosti energiju</td>
  <td valign=top>
Zbirka Simulirani energiju profila, koji se temelji na 12 drugoj zgradi oblicima. Zgrade s respect se razlikovati razlikuju 8 značajke, kao što su glazing područje, glazing područje raspodjele i usmjerenje.<p> </p><b>Korištenje:</b> koristiti regresije ili klasifikacije za predviđanje učinkovitosti energiju ocjena na temelju kao jedan od dva odgovora real vrijednosti. Za klasifikaciju više predmete je okrugle varijabla odgovor na najbliži cijeli broj. <p> </p><b>Srodni Istraživanje:</b> Xifara, A. & Tsanas, A. (2012). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Leta odgađa podataka</td>
  <td valign=top>
Putnika leta na vrijeme podataka o performansama uzeti skup podataka TranStats sad odjel od prijevoza, (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">Na vrijeme</a>).<p> </p>Skup podataka pokriva vremensko razdoblje Travanj listopad 2013. Prije prijenosa Azure ML Studio skupu podataka je obrađuje na sljedeći način:<ul><li>Skup podataka je filtrirane tako da prekrije samo 70 busiest zračnih luka u SAD-a continental</li><li>Otkazani letovi su označene kao odgoditi za više od 15 minuta</li><li>Filtrira su diverted letova</li><li>Sljedeće stupce odabranima: godini, mjesecu, DayofMonth, DayOfWeek, prijenosni, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, Cancelled</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Performanse leta na vrijeme (Raw)</td>
  <td valign=top>
Zapisi aviona leta arrivals i odlascima unutar Sjedinjenih Američkih Država iz listopad 2011.<p> </p><b>Korištenje:</b> predviđanje kašnjenja letova. <p> </p><b>Srodni Istraživanje:</b> iz SAD Dept. od prijevoza <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Skup stabala aktivira podataka</td>
  <td valign=top>
Sadrži vremenske prognoze podatke, kao što su temperature i humidity indekse i brzina vjetra iz područja sjeveroistočnom ogranku Portugal, u kombinaciji s zapisa skupa stabala fires.<p> </p><b>Korištenje:</b> Ovo je zadatak teško regresije, pri čemu je ciljem za predviđanje snimljenih područje skupa stabala fires. <p> </p><b>Srodni Istraživanje:</b> Cortez, P., i Morais, A. (2008). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti <p> </p>[Cortez i Morais, 2007] P. Cortez i A. Morais. Na podataka Mining pristup predviđanje pomoću Meteorological podataka Fires skupa stabala. U J. Neves, s. F. Prisutnosti i J. Machado Eds., novi trendova u Artificial Inteligencija, radova s konferencija 13. EPIA 2007 – portugalski konferenciji na Artificial Inteligencija, prosinac, Guimarães, Portugal, pp. 512-523, 2007. APPIA, ISBN 13 978-989-95618-0 DO 9. Dostupan je na: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Skup podataka UCI njemački kreditne kartice</td>
  <td valign=top>
UCI Statlog (njemački kreditne kartice) skup podataka (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + njemački + odobrenja + podataka</a>), pomoću datoteke german.data.<p> </p>Skup podataka raspoređuje osobe opisan skup atribute kao rizika malo ili visoke kreditne kartice. Svaki primjer predstavlja neke osobe. Postoje 20 značajke, numeričkih i categorical, i natpis binarni (rizika vrijednost odobrenja). Visoke odobrenja rizika stavke imaju oznaku = 2, stavke rizika malo odobrenja imaju oznaku = 1. Trošak misclassifying primjer rizika malo kao visoko je 1, dok je trošak misclassifying primjer visoke rizika kao niskog 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>Naslovi IMDB filma</td>
  <td valign=top>
Skup podataka ne sadrži podatke o filmovima koji su ocjenjivanje u Twitter tweets: IMDB film ID-a, naziv filmske i stupca, godine radnog. Postoje 17K filmova u skupu podataka. Skup podataka je uvedena u papira "S. Dooms, T. De Pessemier i L. Martens. MovieTweetings: film ocjena skup podataka prikupljenih Twitter. Radionicu na Crowdsourcing i Ljudski izračuni za sustave Recommender CrowdRec pri RecSys 2013."
  </td>
</tr>

<tr>
  <td valign=top>IRIS dva klase podataka</td>
  <td valign=top>
Ovo je možda najbolje poznati baze podataka koju želite pronaći u onu prepoznavanja uzorak. Skup podataka je razmjerno malo, koji sadrži 50 Primjeri svaki svijetlosivi mjere iz tri iris postoje vrste.<p> </p><b>Korištenje:</b> predviđanje vrste iris iz mjere.  <p> </p><b>Srodni Istraživanje:</b> Fisher, R.A. (1988). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Film Tweets</td>
  <td valign=top>
Skup podataka je proširenu verziju film Tweetings skupu podataka. Skup podataka sadrži 170K ocjene filmova, dobivenih iz dobro strukturiranim tweets na Twitteru. Svaku instancu predstavlja tweet, a je n-torke: korisnički ID, IMDB film ID, ocjena, vremenska oznaka, numer favorita za tweet i broj retweets ovaj tweet. Skup podataka je učinio dostupnima A. rečeno, S. Dooms, B. Loni i D. Tikk u 2014 Recommender sustavi test..
  </td>
</tr>

<tr>
  <td valign=top>MPG podaci za razne automobiles</td>
  <td valign=top>
Ovaj skup podataka je malo izmijenjene verzija dataset nudi biblioteku StatLib Carnegie Mellon University. Skup podataka korišten u pridruživanje Exposition 1983 American Statistika.<p> </p>Podatke popisa potrošnje goriva za različite automobiles u milja po galon uz podatke takve broj cilindre, engine pomaknuće, Konjska snaga, Ukupna Masa i ubrzanja.<p> </p><b>Korištenje:</b> predviđanje potrošnje goriva ovisno o 3 s više vrijednosti pojedinačnog atributi i 5 neprekinuti atribute. <p> </p><b>Srodni Istraživanje:</b> StatLib Carnegie Mellon University, (1993). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti </td>
</tr>

<tr>
  <td valign=top>Binarni klasifikacije Diabetes Indians Pima skup podataka</td>
  <td valign=top>
Podskup podataka iz nacionalna Institut za Diabetes i Digestive i Kidney Diseases baze podataka. Skup podataka nije filtriran prema fokus na ženski bolesnike koji dolaze Pima indijske nasljeđa. Podaci obuhvaćaju mjehuričaste podataka kao što su glukoze u krvi i insulin razine, kao i čimbenika stilu života.<p> </p><b>Korištenje:</b> predviđanje ima li predmet diabetes (binarni klasifikacija). <p> </p><b>Srodni Istraživanje:</b> Sigillito protiv (1990). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti </td>
</tr>

<tr>
  <td valign=top>Restoran korisničkih podataka</td>
  <td valign=top>
Skup metapodataka o klijentima, uključujući demografskih podataka i postavki.<p> </p><b>Korištenje:</b> koristi ovaj skup podataka u kombinaciji s drugim dva restoran skupove podataka, obuka i testiranje recommender sustava. <p> </p><b>Srodni Istraživanje:</b> Bache, K. i Lichman, polja (2013). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti.
  </td>
</tr>

<tr>
  <td valign=top>Restoran značajka podataka</td>
  <td valign=top>
Skup metapodataka o restorani i njihove značajke, kao što su vrste hrane, dining stil i mjesto.<p> </p><b>Korištenje:</b> koristi ovaj skup podataka u kombinaciji s drugim dva restoran skupove podataka, obuka i testiranje recommender sustava. <p> </p><b>Srodni Istraživanje:</b> Bache, K. i Lichman, polja (2013). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti.
  </td>
</tr>

<tr>
  <td valign=top>Ocjene restoran</td>
  <td valign=top>
Sadrži ocjene daje korisnicima restorani u rasponu od 0 do 2.<p> </p><b>Korištenje:</b> koristi ovaj skup podataka u kombinaciji s drugim dva restoran skupove podataka, obuka i testiranje recommender sustava. <p> </p><b>Srodni Istraživanje:</b> Bache, K. i Lichman, polja (2013). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti.
  </td>
</tr>

<tr>
  <td valign=top>Čelik dataset više klase Annealing</td>
  <td valign=top>
Ovaj skup podataka sadrži skup zapisa iz čelik annealing pokušaji fizičke atributa (Širina, Debljina, vrstu (coil lista, itd.) na rezultat steel vrste.<p> </p><b>Korištenje:</b> predviđanje bilo koji od dva atributa numeričke predmete; hardness ili granica lomljenja. Možda analizirati i korelacija među atribute.<p> </p>Steel ocjena slijedite skup standard, definira SAE i drugim tvrtkama ili ustanovama. Tražite određenu 'ocjene"(varijabla klase) i želite da biste shvatili vrijednosti potrebne. <p> </p><b>Srodni istraživanja:</b> sterlinga, D. & Buntine w (n/d). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University Kaliforniji, školske informacije i računalnih znanosti <p> </p>Vodič kroz korisne steel ocjena Ovdje možete pronaći: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Telescope podataka</td>
  <td valign=top>
Zapisi visoke energiju gama čestica bursts uz pozadinski Šum, oba Simulirani Monte Carlo postupkom.<p> </p>Cilj simulacije je radi poboljšanja točnosti utemeljen na dna atmosfere Cherenkov gama telescopes, korištenje statističkim postupcima za razlikovanje željeni signal (Cherenkov radiation showers) i pozadinski Šum (hadronic showers pokrenuo cosmic zrake u gornjem atmosferu).<p> </p>Podaci su unaprijed obrađeni da biste stvorili elongated klaster u dugu je usmjeren prema centru za kameru osi. Među slika parametre koje je moguće koristiti za diskriminacije su karakteristike ovaj Elipsa (često se nazivaju i parametara Hillas).<p> </p><b>Korištenje:</b> predviđanje predstavlja li slika zabavu signal i pozadinske buke.<p> </p><b>Napomene:</b> od klasifikaciju pozadine događaja kao što je signal worse od klasifikaciju signal događaj kao pozadinu jednostavne klasifikacija točnost nije smisleni za te podatke. Za Usporedba različitih classifiers želite koristiti ROC grafikonu. Vrijednost vjerojatnosti prihvatiti pozadine događaja, kao što je signal mora biti ispod nekog od sljedećih pragovi: 0,01, 0,02, 0,05, 0,1 ili 0,2.<p> </p>Osim toga, imajte na umu da underestimated je broj događaja pozadine (h za hadronic showers), dok je u realne mjere predmete h ili Šum predstavlja Većina događaja. <p> </p><b>Srodni istraživanja:</b> Bock, R.K. (1995). UCI strojnog učenja spremište <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University od Kaliforniji, školske informacije </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Skup podataka vremenske prognoze</td>
  <td valign=top>
Svaki sat sustava sustavom vremenske prognoze opažanja iz NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">spojenih podataka iz 201304 za 201310</a>).<p> </p>Vremenske prognoze podataka pokriva opažanja stvoren od zračna luka vremenske prognoze stanice, prekrivajući vremensko razdoblje Travanj listopad 2013. Prije prijenosa Azure ML Studio skupu podataka je obrađuje na sljedeći način:<ul><li>Vremenske prognoze stanice ID-a nisu mapirani odgovarajuće airport ID-a</li><li>Filtrira su vremenske prognoze stanice nije povezana ni s 70 busiest zračne luke</li><li>Stupac datum je podijelite u zasebne stupce godinu, mjesec i dan</li><li>Sljedeće stupce odabranima: AirportID, godinu, mjesec, dan, vrijeme, vremenska zona, SkyCondition, vidljivost, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, WindSpeed, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedije SP 500 skup podataka</td>
  <td valign=top>
Podaci proizlazi iz Wikipedije (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) koji se temelji na članke svaki S & P 500 tvrtke ili ustanove pohranjeni kao XML podataka.<p> </p>Prije prijenosa Azure ML Studio skupu podataka je obrađuje na sljedeći način:<ul><li>Izdvajanje tekstni sadržaj za svaki određene tvrtke</li><li>Uklanjanje oblikovanja wiki</li><li>Uklonite znakove koji nisu alfanumerički</li><li>Pretvaranje sav tekst u mala slova.</li><li>Dodani kategorije poznate tvrtke</li></ul><p> </p>Imajte na umu da se za neke companies članka nije moguće pronaći, pa je broj zapisa manja od 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Skup podataka sadrži podatke o klijentu i indications o odgovor Izravni kampanjom primatelja e-pošte. Svaki redak predstavlja klijenta. Skup podataka sadrži 9 značajke o demografskih podataka korisnika i prošle ponašanje i 3 natpisa stupca (posjetite pretvorbe i provedu).  Posjetite stranicu je binarni stupac koji upućuje na to da se klijent posjetili nakon marketinške kampanje, pretvorbe upućuje na to klijenta kupili nešto i provedu je iznos koji je utrošeno.  Skup podataka je učinio dostupnima Kevin Hillstrom za MineThatData e-pošte analize i podataka Mining test.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Značajke test primjerima u skupu podataka RCV1 V2 Reuters novosti. Skup podataka sadrži članke novosti 781K zajedno s identifikacijskim (prvi stupac skupu podataka). Svaki članak je tokenized stopworded, i stemmed. Skup podataka je učinio dostupnima Nevena. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Značajke obuka primjerima u skupu podataka RCV1 V2 Reuters novosti. Skup podataka sadrži članke novosti 23K zajedno s identifikacijskim (prvi stupac skupu podataka). Svaki članak je tokenized stopworded, i stemmed. Skup podataka je učinio dostupnima Nevena. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Skup podataka iz KDD Cup 1999 znanja otkrivanje i podataka Dubinska analiza Alati konkurencije (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Skup podataka preuzet i pohranjene u Azure blobova (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>), a uključuje i obuka i testiranje skupova podataka. Obuka dataset približno sadrži 126K redaka i 43 stupaca, uključujući oznake; dio informacija natpisa su 3 stupca, a 40 stupaca koji se sastoji od numeričke i niz categorical značajke dostupne su za osposobljavanje modela. Testiranje podataka sadrži približno 22.5K testiranje primjere s istom 43 stupca kao obuka podataka.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Tema dodjele članke novosti u skupu podataka RCV1 V2 Reuters interesne grupe. U članku novosti možete dodijeliti nekoliko teme. Odaberite oblik svakog retka "<topic name>  <document id> 1". Skup podataka sadrži dodjele 2.6M temu. Skup podataka je učinio dostupnima Nevena. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Ove podatke dolazi iz KDD Cup 2010 učenika performanse procjenu test (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">Procjena studentskih performanse</a>). Podaci koji se koriste se skup obuka Algebra_2008_2009 (Stamper, J., Niculescu-Mizil, S. A., Ritter, Gordon, G.J. i Koedinger, K.R. (2010). Algebra li 2008 2009. Test zadani skup podataka iz KDD Cup 2010 obrazovna podataka Mining test. Saznajte <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> ili <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Skup podataka preuzet i pohranjene u Azure blobova (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) i sadrži datoteka zapisnika s učenik tutoring sustava. Navedeni značajke obuhvaćaju ID problema i njegov kratak opis, ID učenika, vremenska oznaka i koliko pokušaja učenika unijeli prije desnom način rješavanja problema. Izvorni dataset ima 8.9M zapise, ovaj skup podataka je uzorkovanja dolje na prvi redak 100K. Skup podataka sadrži 23 tabulatorom stupaca s različitim vrstama: categorical, numeričke i vremenske oznake.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
