<properties
    pageTitle="Stvaranje tekst analize modela dodatka Azure strojnog učenja Studio | Microsoft Azure"
    description="Kako stvoriti tekst modelima analize u pomoću modula za tekst pretprocesnih, N gramima, ili značajka raspršivanje Azure strojnog učenja Studio"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Stvaranje tekst analize modela dodatka Azure strojnog učenja Studio

Omogućuje stvaranje i operationalize modela analize tekst možete koristiti Azure strojnog učenja. Ove modelima može pomoći riješiti, na primjer, dokument klasifikacija ili šalju analize problema.

U eksperiment za analize tekst, što činite obično:

 1. Čišćenje i preprocess dataset teksta
 2. Izdvajanje numerički značajka vektori od unaprijed obrađeni teksta
 3. Klasifikacija ili regresije model vlaku
 4. Rezultat i provjera valjanosti modela
 5. Implementacija model radnog

Pomoću ovog praktičnog vodiča saznat ćete sljedeće korake kao detaljnih kroz pomoću dataset Amazon knjige pregledava model analizu šalju (Ova dokumentacija za istraživanje potražite u članku "Biographies, Bollywood, Boom okvire i Blenders: adaptacijski domene za klasifikaciju šalju" Nevena Blitzer, Označi Dredze i Fernando Pereira; Pridruživanje od računalne Lingvistička (ACL) 2007.) Ovaj skup podataka sastoji se od pregled rezultata (1-2 ili 4-5) i prostoručni tekst. Cilj je za predviđanje rezultat pregled: niska (1 - 2) ili od visoke (4-5).

Možete pronaći eksperimenata obuhvaćeno ovog praktičnog vodiča pri Cortana obavještavanje Galerija:

[Predviđanje adresara recenzije] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Predviđanje adresara recenzije - predvidljivu eksperiment] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Korak 1: Čišćenje i preprocess dataset teksta

Početak pokusa dijeljenjem pregled rezultata u categorical memorijski blokovi gornju i donju da biste formulate problem kao klasifikacija dva predmete. Koristimo [Uređivanje metapodataka] (https://msdn.microsoft.com/library/azure/dn905986.aspx) i moduli [grupa Categorical Values] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Stvaranje oznaka](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Zatim ćemo očistiti tekst pomoću modula [Preprocess tekst] (https://msdn.microsoft.com/library/azure/mt762915.aspx). Za čišćenje smanjuje Šum u skupu podataka, pomoći pronalaženje najvažnije značajke i poboljšajte točnost konačni modela. Ne možemo ukloniti stopwords - česte riječi kao što su "u" ili "web -" i brojeve, posebne znakove, dupliciranu znakova, adrese e-pošte i URL-ova. Ne možemo i pretvaranje teksta u mala slova, lemmatize riječi i otkriti rečenice ograničenja koja zatim označena "|||" simbol u unaprijed obrađeni teksta.

![Preprocess teksta](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Ako želite koristiti prilagođeni popis stopwords? Prosljeđivanje u kao neobavezno unos. Možete koristiti prilagođene C# sintaksa Uobičajeni izraz da biste zamijenili podnizova, te ukloniti riječi riječi: imenice, glagoli ili pridjeva.

Po dovršetku na pretprocesnih ćemo podijeliti s podacima u vlaku i testiranje skupove.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Korak 2: Izdvajanje numerički značajka vektori od unaprijed obrađeni teksta

Da biste sastavili modela za tekstne podatke, obično imate prostoručni teksta pretvoriti numeričke značajka vektori. U ovom primjeru koristimo [izdvojiti N gramatike značajke iz teksta] (https://msdn.microsoft.com/library/azure/mt762916.aspx) modul za pretvorbu teksta podataka kao što je oblik. U ovom modul uzima stupac razmak razdvojene riječi i formula izračunava rječnika riječi ili N-gramima riječi koje se pojavljuju u vašem skupu podataka. Nakon toga Broji koliko vremena svakog riječ ili N-gramatike, pojavljuje se svaki zapis, a stvara značajka vektori od onih broji. U ovom ćete praktičnom vodiču ne možemo postaviti N gramatike veličina 2, tako da naš vektori značajke obuhvaćaju pojedinačne riječi i kombinacija dva sljedeće riječi.

![Izdvojiti N gramima](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Ne možemo primijeniti TF * IDF (termina učestalost inverzni dokument učestalost) weighting N gramatike broji. Taj se način dodaje debljine riječi koje se najčešće pojavljuju u jedan zapis, ali se rijetko preko cijeli skup podataka. Druge mogućnosti obuhvaćaju binarnu TF, i grafikona koja teže.

Značajke kao što su tekst često imaju visoke dimensionality. Ako, na primjer, ako je vaš corpus 100 000 jedinstvenih riječi, prostor za vaše značajka promijenile 100 000 dimenzije ili više njih ako koriste N gramima. Modul za izdvojiti N gramatike značajke daje sa skupom mogućnosti da biste smanjili na dimensionality. Možete odabrati da biste izuzeli riječi koje su kratko ili dugo, ili previše neuobičajeno ili previše često korištenih značajan predvidljivu vrijednost. U ovom ćete praktičnom vodiču smo izuzimanje N gramima koje se pojavljuju u manje od 5 zapisa ili više od 80% zapisa.

Osim toga, možete koristiti odabir značajki da biste odabrali samo one značajke koje su najčešće povezanog s vašeg cilja predviđanje. Da biste odabrali 1000 značajke koristimo odabir značajki hi-kvadrata. Pojmovnik odabrane riječi ili N gramima možete vidjeti klikom na desnu izlaz izdvojiti N-gramima modula.

Možete koristiti značajku raspršivanje modul kao zamjenski pristup, pomoću značajke N gramatike za izdvajanje. Imajte na umu iako se taj [značajka raspršivanje] (https://msdn.microsoft.com/library/azure/dn906018.aspx) imati značajka Sastavi odabira mogućnosti ili TF * koja teže IDF.

## <a name="step-3-train-classification-or-regression-model"></a>Korak 3: Obuka klasifikacija ili regresije model

Sada tekst su logaritme numerički značajka stupce. Skup podataka i dalje sadrži niz stupce iz prethodne faze tako da koristimo odabir stupaca u skupu podataka da biste ih isključili.

Zatim koristimo [dva predmete logistika regresije] (https://msdn.microsoft.com/library/azure/dn905994.aspx) za predviđanje naš cilj: velikom ili malom pregled rezultata. Sada analize problema tekst sadrži su pretvoreni u pravilnim klasifikacije problem. Da biste poboljšati model, poslužite se alatima u Azure strojnog učenja. Ako, na primjer, Eksperimentirajte s različitim classifiers da biste saznali kako točne rezultate daju ili koristiti hyperparameter usklađivanje da biste poboljšajte točnost.

![Vlaku i rezultat](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Korak 4: Rezultatu i provjera valjanosti modela

Kako bi provjerili obučeni model? Ne možemo rezultatu na temelju test dataset i procjenu točnost. Međutim, model naučili rječnik N gramima i njihovih debljine iz skupa podataka za obuku. Zbog toga treba koristimo taj rječnik i one debljine kada izdvajanje značajke iz podataka test, umjesto stvaranja novi u rječnik. Stoga ćemo dodati modul izdvojiti N gramatike značajke bilježenja rezultata granu pokusa, povezivanje rječnik za izlaz iz podružnice obuka i postavite rječnik način rada samo za čitanje. Ne možemo i onemogućite filtriranja N gramima po učestalosti tako da postavite minimalne na 1 instancu i maksimum na 100% i isključivanje odabir značajki.

Nakon tekstni stupac u testiranje podataka su logaritme numerički značajka stupce, ne možemo izostavljanje niz stupaca iz prethodne faza kao što je obuka grani. Zatim koristimo modul modela rezultat da biste predviđanja i procjenu modela modula za procjenu točnost.

## <a name="step-5-deploy-the-model-to-production"></a>Korak 5: Implementacija model radnog

Model je gotovo biti implementirano u proizvodnje. Kada uveden kao web-servisa, ga vodi prostoručni tekstni niz kao ulaz i povratna predviđanje "Visoka" ili "niskog." Learned rječnik N gramatike koristi za pretvorbu teksta u značajke i obučeni logistika regresije model da biste stvarali predviđanje od tih značajki. 

Da biste postavili predvidljivu pokusa, ne možemo prvo spremanje vokabular N gramatike kao skup podataka i obučeni logistika regresijskog modela iz grana obuka pokusa. Zatim ćemo spremite pokusa pomoću naredbe "Spremi kao" da biste stvorili na grafikonu eksperiment za predvidljivu eksperiment. Ne možemo uklanjanje modul za podjelu podataka i obuka granu pokusa. Ne možemo pa povezati prethodno spremljene N gramatike rječnik i model izdvojiti N gramatike značajke i moduli rezultat Model, odnosno. Modul za procjenu modela ćemo i ukloniti.

Ne možemo umetnite odabir stupaca u modulu Dataset prije modul Preprocess tekst da biste uklonili oznaku stupca i poništavanje odabira mogućnosti "Dodavanjem rezultat stupac dataset" u modulu rezultat. Na taj način web-servisa nije zahtjev oznake koje pokušava predviđanje, a ne nije jeka unos značajke u odgovoru.

![Predvidljivu eksperiment](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Sad imamo pokusa koje se mogu objavili kao web-servisa i pod nazivom pomoću odgovor na zahtjev ili obradu izvođenja API-ji.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o moduli analize tekst iz [MSDN dokumentaciju] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
