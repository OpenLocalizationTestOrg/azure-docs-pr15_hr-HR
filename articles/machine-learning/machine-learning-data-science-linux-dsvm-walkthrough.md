<properties 
    pageTitle="Znanstvena podataka na Linux podataka znanstvenog virtualnog računala | Microsoft Azure" 
    description="Upute za izvođenje nekoliko uobičajenih podataka znanstvenog zadataka s VM znanosti za podataka Linux." 
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Znanstvena podataka na Linux podataka znanstvenog virtualnog računala

Ovaj vodič prikazuje kako izvršiti nekoliko uobičajenih podataka znanstvenog zadataka s VM znanosti za podataka Linux. Linux podataka znanstvenog virtualnog računala (DSVM) je dostupan na Azure koji je instalirana s skup alata koji se obično koristi za analize podataka i strojnog učenja slika virtualnog računala. Komponente ključa softver su detaljni popis u temi [dodjele Linux podataka znanstvenog virtualnog računala](machine-learning-data-science-linux-dsvm-intro.md) . Slika VM pojednostavnjuje početak učinite znanstvenog podataka u minutama bez potrebe za instalaciju i konfiguriranje svakog alata pojedinačno. Možete jednostavno proširenja VM, ako je potrebno i prekinuti kada nije u upotrebi. Ovaj resurs tako da je elastic i učinkovito trošak. 

Znanstvena zadacima podataka planirati ovaj vodič slijedite korake navedene u [Timu podataka znanstvenog postupak](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Ovaj postupak omogućuje sustavan pristup znanstvenog podataka koji omogućuje timovima fizičari podataka da biste učinkovito surađivati putem životnim ciklusom stvaranje Inteligentna aplikacija. Znanstvena postupka podataka nudi i iteracije framework za znanstvenog podataka koji se mogu slijediti pojedinac.

Ne možemo analiza [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) skup podataka u prikazu. Ovo je skup poruke e-pošte koje su označene kao neželjene e-pošte ili ham (što znači da se ne nalaze neželjene e-pošte), a sadrži i nekoliko statistiku za sadržaj u porukama e-pošte. Statistika uključiti se spominju u sljedeći, ali jedne sekcije. 


## <a name="prerequisites"></a>Preduvjeti

Da biste koristili Linux podataka znanstvenog virtualnog računala, morate imati sljedeće:

- Za **Azure pretplate**. Ako nije već postoji, potražite u članku [Stvaranje pomoću besplatnog Azure računa danas](https://azure.microsoft.com/free/).
- [**Linux podataka znanstvenog VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Informacije o ovom VM za dodjelu resursa, potražite u članku [Dodjeljivanje Linux podataka znanstvenog virtualnog računala](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) instaliran na računalo i otvoriti sesiju XFCE. Informacije o instaliranju i konfiguriranju **X2Go klijenta**, potražite u članku [Instaliranje i konfiguriranje X2Go klijenta](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- **AzureML računa**. Ako nemate onaj koji se registrirati za novi na [početnu stranicu AzureML](https://studio.azureml.net/). Postoji besplatne korištenje sloju koji će vam pomoći da započnete.


## <a name="download-the-spambase-dataset"></a>Preuzmite spambase skupu podataka

Skup podataka [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) je razmjerno malo skupa podataka koji sadrži samo 4601 primjere. Ovo je praktičan veličine za korištenje prilikom takvi da neke ključne značajke programa VM znanstvenog podataka kao što je stalno uvjete resursa modest.

>[AZURE.NOTE] Ovaj vodič stvorena na na D2 v2 veličine Linux podataka znanstvenog virtualnog računala. Veličina DSVM se može obrade postupci u ovom vodiču.

Ako je potrebno više prostora za pohranu, možete stvoriti dodatne diskova i priložite vaše VM. Tih diskova koristi trajnog Azure prostor za pohranu, da bi se njegovi podaci zadržava se čak i kad je poslužitelj brišu se zbog promjene veličine ili se isključi. Da biste dodali na disku, a zatim priložite vaše VM, slijedite upute u članku [Dodavanje disku Linux VM](../virtual-machines/virtual-machines-linux-add-disk.md). Ove korake koristite Azure sučelja naredbenog retka (Azure EŽA), koji je već instaliran na DSVM. Pa ove postupke mogu se izvršiti potpuno iz VM sam. Da biste koristili [Azure datoteke](../storage/storage-how-to-use-files-linux.md)je i mogućnost da biste povećali prostor za pohranu.

Da biste preuzeli podatke, otvorite prozor terminal i pokrenite sljedeću naredbu:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Preuzete datoteke ne sadrži redak zaglavlja, možemo stvoriti druge datoteke koje imaju zaglavlja. Pokrenite sljedeću naredbu da biste stvorili datoteku s odgovarajućim zaglavlja:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Zatim spajanje dvije datoteke zajedno s naredbu:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Skup podataka sadrži nekoliko vrsta Statistika na svakom e-pošte: 

- Kao što su stupci ***word\_frekvencija\_WORD*** naznačiti postotak riječi u e-pošte koje odgovaraju *WORD*. Na primjer, ako *word\_frekvencija\_provjerite* je 1, a zatim su 1% svih riječi u e-poštu *provjerite*. 
- Kao što su stupci ***znak\_frekvencija\_znak*** naznačiti postotak svi znakovi u e-pošte koje su bile *znak*. 
- ***Veliko\_pokrenite\_duljina\_najdulje*** je najdulje duljina niza velika slova. 
- ***Veliko\_pokrenite\_duljina\_Prosječna*** je prosječna duljina sve nizove velika slova. 
- ***Veliko\_pokrenite\_duljina\_ukupni*** je ukupna duljina sve nizove velika slova. 
- ***neželjene pošte*** označava je li e-poštu smatra neželjene e-pošte ili ne (1 = neželjene e-pošte, 0 = nije neželjena pošta).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Istražite skup podataka s R Otvori

Pogledajmo pregledajte podatke i učinite neki osnovni strojnog učenja s R. Znanstvena VM podataka u sklopu [R Otvori](https://mran.revolutionanalytics.com/open/) unaprijed instalirano. Biblioteka s usporednim nitima matematičkih izraza u ovoj verziji programa R nude bolje performanse od različite verzije jedne niti. R Otvori omogućuje reproducibility pomoću brze snimke u spremištu CRAN paketa.

Da biste dobili kopije primjere koda koji se koriste u ovom vodiču, Kloniraj spremište **Azure-strojno-učenje-podataka – znanstvenog** pomoću brojka, u kojem je instalirana na na VM. U naredbeni redak brojka pokrenite:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Otvorite prozor terminal i pokrenite novu sesiju R s konzole interaktivne R.

>[AZURE.NOTE] RStudio možete koristiti i za sljedeće postupke. Da biste instalirali RStudio, izvršavanje tu naredbu na je terminal:`./Desktop/DSVM\ tools/installRStudio.sh`

Da biste uvezli podatke i postavlja okruženje, pokrenite:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Da biste vidjeli sažetak statističkih podataka o svakom stupcu:

    summary(data)

Za drugi prikaz podataka:

    str(data)

Ovo prikazuje vrstu svakoj varijabli i prvi nekoliko vrijednosti u skupu podataka. 

Stupac *neželjene pošte* je pročitana kao cijeli broj, ali je zapravo je categorical varijabla (ili faktor). Da biste postavili vrstu:

    data$spam <- as.factor(data$spam)

Da biste učinili neke exploratory analizu, pomoću značajke pakiranja [ggplot2](http://ggplot2.org/) , popularne graphing biblioteka za R na kojem je već instaliran na VM. Imajte na umu iz zbirnih podataka prikazuju neke starije verzije, imamo sažetak statističkih podataka o učestalosti znak uskličnik. Pogledajmo iscrtati te učestalosti pomoću sljedeće naredbe:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Budući da traci nula je skewing na crtež, uklonit ćemo je:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Postoji koje nisu trivial gustoće iznad 1 koji izgleda zanimljive. Pogledajmo samo podatke:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Zatim ga podijelite tako da ham Dodavanje veze za vanjskih neželjene e-pošte:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

U ovim se primjerima trebate omogućiti da biste slične iscrtavaju drugih stupaca za istraživanje podataka koji se nalaze u njima.


## <a name="train-and-test-an-ml-model"></a>Obuka i testiranje modelu ML

Sada ćemo uvježbati nekoliko strojnog učenja modela za klasifikaciju poruke e-pošte u skupu podataka kao da sadrže raspon ili ham. Ne možemo uvježbati model stabla odlučivanja i model slučajni skupa stabala u ovom odjeljku, a zatim testirajte svoje točnosti njihove predviđanja. 

>[AZURE.NOTE] Paket rpart (rekurzivne particija i regresije stabala) koristi u sljedeći kod je već instalirana na znanstvenog VM podataka.


Najprije ćemo podijeliti skupu podataka u obuka i testiranje skupova:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

A zatim stvorite stabla odlučivanja za klasifikaciju u porukama e-pošte.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Evo i rezultata:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Da biste odredili koliko će se dobro izvodi na skup obuka, koristite sljedeći kod:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Da biste odredili koliko će se dobro izvodi na skup test:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Također Pokušajmo slučajni skupa stabala modela. Slučajni šuma obuka više odlukama i izlaz predmete koji je način klasifikacije iz svih pojedinačne odlukama. Pružaju je jače strojnog učenja pristup kao što su ispravljati srednju vrijednost modela stabla odlučivanja za overfit obuka skup podataka. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Implementacija model Azure ML

[Azure strojnog učenja Studio](https://studio.azureml.net/) (AzureML) je servis u oblaku koja olakšava stvorite i implementirajte modelima predvidljivu analize. Jedna od obzirni značajki AzureML je sposobnost da biste objavili bilo koju funkciju R kao web-servisa. Paket AzureML R olakšano implementacije učiniti izravno iz naših R sesije na na DSVM. 

Da biste implementirali kod stabla odlučivanja iz prethodnog odjeljka, morate prijaviti na Azure strojnog učenja Studio. Potreban je identifikacijskog Broja za radni prostor i oznaku ovlaštenja sigh u. Da biste pronašli te vrijednosti i pokretanje varijable AzureML s njima:

Odaberite **Postavke** na izborniku s lijeve strane. Imajte na umu **ID radnog prostora**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Odaberite **Autorizacija tokena** na izborniku indirektni i zabilježite vaš **Primarni autorizacije tokena**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Učitavanje paketa **AzureML** , a zatim postavite vrijednosti varijabli s tokena i radni prostor ID-om u sesiji R na na DSVM:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Pogledajmo pojednostavniti model da biste olakšali implementirati ovu demonstraciju. Odaberite tri varijable u najbliži korijenskom stabla odluke i izraditi novi stabla pomoću samo onih tri varijable:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Potrebna predviđanje funkcija koja se koristi značajke kao ulaz i vraća predviđene vrijednosti:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Objavljivanje funkciju predictSpam AzureML pomoću funkcije **publishWebService** : 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Ova funkcija koristi funkciju **predictSpam** , stvara web-servisa pod nazivom **spamWebService** s definirani unosa i izlaze i vraća informacije o novi krajnje točke.

Prikaz detalja o objavljenu web-usluge, uključujući njezin krajnju točku API-JA i pristupne ključeve pomoću naredbe:

    spamWebService[[2]]

Da biste isprobali neku na prvoj 10 redaka test postaviti:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Korištenje druge alate

Preostali sekcije pokazuju kako da biste koristili neki od alata instalirano VM znanosti za podataka Linux. Slijedi popis alata koji se spominju:

- XGBoost
- Python
- Jupyterhub
- Rattle
- PostgreSQL & Squirrel SQL
- SQL Server Data Warehouse


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) alat je koji omogućuje brz i točne boosted stabla implementacije.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost također možete pozvati iz python ili naredbeni redak.

## <a name="python"></a>Python

Za razvoj pomoću Python distribucija Anaconda Python 2.7 i 3.5 instaliran u na DSVM. 

>[AZURE.NOTE] Raspodjele Anaconda obuhvaća [Condas](http://conda.pydata.org/docs/index.html), koji se može koristiti za stvaranje prilagođene okruženja za Python koji imaju različite verzije i/ili paketa instaliranih na njima.

Recimo pročitajte u nekim spambase dataset i klasifikaciju u porukama e-pošte s računalima vektorski podršku scikit – Saznajte:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Da biste unijeli predviđanja:

    clf.predict(X.ix[0:20, :])

Da biste prikazali kako objaviti krajnje AzureML, recimo provjerite jednostavniji modela tri varijable kao smo kada smo prethodno objavljena R modela. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Da biste objavili modela na AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] Dostupna je samo za python 2.7 i još nije podržana na 3.5. Pokretanje s **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

U sklopu distribuciju Anaconda u na DSVM Jupyter bilježnice, okruženje za različite platforme za zajedničko korištenje Python "," R "ili" Julia i analiza. Bilježnice Jupyter se pristupa putem JupyterHub. Prijavljujete pomoću lokalne Linux korisničko ime i lozinku na ***https://\<VM DNS naziv ili IP adresa\>: 8000 /***. Sve datoteke konfiguracije JupyterHub nalaze se u imeniku **/etc/jupyterhub**.

Nekoliko oglednih bilježnica već su instalirane na na VM:

- Pogledajte [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) za bilježnicu Python uzorka.
- Primjer **R** bilježnice potražite u članku [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) .
- Pogledajte [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) za drugu bilježnicu **Python** uzorka.

>[AZURE.NOTE] Jezik Julia je dostupno i iz naredbenog retka na VM znanosti za podataka Linux.


## <a name="rattle"></a>Rattle

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (u R analitičkih alata za dodatne jednostavno) je grafičkog R alata za dubinsku analizu podataka. Ima intuitivno sučelje koja olakšava učitavanje, istraživanje, i pretvaranje podataka i stvaranje i procjenu modela.  U članku [Rattle: A podataka Mining GUI za R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) nudi vodič koji pokazuje njegove značajke.

Instalirajte i pokrenite Rattle pomoću sljedeće naredbe:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Instalacija nije obavezno na na DSVM. No Rattle može od vas zatražiti da biste instalirali dodatne paketa pri učitavanju.

Rattle koristi sučelje za utemeljen na kartici. Većina kartice koje odgovaraju koraka u [Proces znanstvenog podataka](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), kao što su učitavanja podataka ili je Istraživanje. Proces znanstvenog podataka teče slijeva nadesno po karticama. No kartice zadnjeg sadrži zapisnik naredbi R pokrenuti Rattle. 


Učitavanje i konfiguriranje skupu podataka:

- Da biste učitali datoteku, odaberite karticu **Podaci** pa 
- Odaberite izbornik pokraj **naziva datoteke** , a zatim odaberite **spambaseHeaders.data**. 
- Da biste učitali datoteku. Odaberite **izvrši** u gornjem retku gumba. Trebali biste vidjeti sažetak svakog stupca, uključujući vrstu identificirani podataka, hoće li je ulazni, cilj ili druge vrste varijabla i broj jedinstvene vrijednosti.
- Rattle pravilno otkrio stupcu **neželjene e-pošte** kao cilj. Odaberite stupac neželjene e-pošte, a zatim postavite **Ciljna vrsta podataka** na **Categoric**.

Za istraživanje podataka: 

- Odaberite karticu **Istraživanje** . 
- Kliknite **Sažetak**, a zatim **izvrši**, da biste vidjeli neke informacije o varijable vrste i neke statistički sažetak. 
- Da biste vidjeli druge vrste statističkih podataka o svakoj varijabli, odaberite druge mogućnosti kao što su **opišite** ili **Osnove**.

Na kartici **Istraži** omogućuje i generiranje mnogo insightful iscrtavaju. Da biste iscrtajte histogram podataka:


- Odaberite **distribucije**.
- Provjera **Histogram** **word_freq_remove** i **word_freq_you**.
- Odaberite **izvršiti**. Trebali biste vidjeti oba gustoće iscrtavaju u prozoru u jednom grafikonu gdje je Očisti da riječ "vi" pojavljuje mnogo češće u porukama e-pošte od "Ukloni".

Crtanje korelacije su zanimljive. Da biste je stvorili:


- Zatim odaberite **korelacije** kao **Vrsta** 
- Odaberite **izvršiti**. 
- Rattle će vas upozoriti preporučuje najviše 40 varijabli. Odaberite **da** da biste pogledali na crtež. 

Postoje neka zanimljive korelacija koje se spominju: "tehnologije" je svakako povezanog "HP" i "labs", na primjer. Ga je i svakako povezanog za "650", jer je pozivni broj donatora dataset 650.

U prozoru Istraži dostupni su numeričke vrijednosti za korelacija između riječi. To je zanimljive bilješke, na primjer, je li "tehnologije" negativno usklađeni s "na" i "novac".

Rattle možete transformirati skup podataka za rukovanje neke od uobičajenih problema. Ako, na primjer, omogućuje ponovno Skaliraj značajke, impute vrijednosti koje nedostaju, rukovati outliers i uklanjanje varijable ili opažanja nedostaju podacima. Rattle možete prepoznati pravila pridruživanja između opažanja i/ili varijabli. Karticama su iz opseg za ovaj uvodni vodič.

Rattle možete izrađivati klaster analize. Pogledajmo izuzimanje neke značajke da biste olakšali čitanje izlaz. Na kartici **Podaci** odaberite **Zanemari** uz svaku od varijable osim ovih deset istaknutih stavki:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- neželjene e-pošte

Prijeđite natrag na karticu **klaster** odaberite **KMeans**i postavite *broj klastere* za 4. Zatim **izvršiti**. Rezultati prikazuju se u izlaznom prozoru. Jedan klaster ima velika učestalost "Goran" i "hp", a vjerojatno valjane poslovnu e-poštu.

Da biste sastavili jednostavne odluka stabla strojno učenje modela: 

- Odaberite karticu **modela** 
- Kao **vrstu**odaberite **stabla** . 
- Odaberite **izvrši** za prikaz stabla u obliku teksta u izlaznom prozoru. 
- Odaberite gumb za **Crtanje** da biste pogledali grafički verziju. To sliči prilično stabla smo dobili neke starije verzije pomoću *rpart*.

Jedna od obzirni značajki Rattle je sposobnost da biste pokrenuli nekoliko metoda strojnog učenja i brzoj procjeni. Evo postupak:

- Odaberite **sve** za **vrste**. 
- Odaberite **izvršiti**. 
- Nakon što završi klikom bilo koje jednu **vrstu**, kao što su **SVM**i prikaz rezultata. 
- Možete usporediti i performanse modelima na provjere valjanosti postavljanje putem kartice **procijeni** . Ako, na primjer, odabir **Pogreške matrice** prikazuje vam zbrku matrice, cjelokupan pogreške i pogreške klase računa prosječna vrijednost za svaku model skup provjere valjanosti. 
- Možete i crtanje krivulje ROC, izvođenje simbol e-pošte i učinite druge vrste modela procjene.

Kada završite sa sastavljanjem modela, odaberite karticu **zapisnika** da biste vidjeli kod R pokrenuti Rattle tijekom sesije. Možete odabrati gumb **Izvoz** da biste je spremili. 

>[AZURE.NOTE] Postoji pogreška u trenutnom izdanju sustava Rattle. Da biste izmijenili skriptu ili ga koristiti za kasnije ponovite, morate umetnuti znak # ispred *Izvoz zapisnika...* u tekst zapisnika. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL

U sklopu sustava DSVM PostgreSQL instaliran. PostgreSQL je relacijske baze složene, Otvori izvor podataka. U ovom se odjeljku objašnjava umetnite naš dataset neželjene pošte u PostgreSQL i zatim je upit.

Prije nego što možete učitali podatke, morate dopustiti provjeru autentičnosti lozinke iz na localhost. U naredbeni redak:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Pri dnu konfiguracijska datoteka su nekoliko redaka detalja dopuštene veze:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Promjena u retku "IPv4 lokalne veze" za značajku md5 ident, pa ne možemo možete prijaviti pomoću korisničkog imena i lozinke za:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

I ponovno pokrenite servis postgres:

    sudo systemctl restart postgresql

Da biste pokrenuli psql programa terminal interactive za PostgreSQL kao korisnik ugrađene postgres s upitom pokrenite sljedeću naredbu:

    sudo -u postgres psql

Stvaranje novog korisničkog računa, pomoću istog korisničkog imena kao račun Linux trenutno prijavljeni ste kao i joj dodijeliti lozinku:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Zatim se prijavite da biste psql kao korisniku:

    psql

I uvezli podatke u novu bazu podataka:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Sada Upoznajmo podataka i Pokreni neke upita pomoću **Squirrel SQL**, grafičkog alata koji vam omogućuje interakciju s bazama podataka putem upravljački program za JDBC.

Da bismo započeli, pokrenite Squirrel SQL na izborniku aplikacije. Da biste postavili upravljački program:

- Odaberite **Windows**, zatim **upravljačke programe za prikaz**. 
- Desnom tipkom miša kliknite **PostgreSQL** , a zatim odaberite **Upravljački program za izmjenu**. 
- Odaberite **Dodatne predmete put**, a zatim **Dodaj**. 
- Uz **naziv datoteke** unesite ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** i 
- Odaberite **Otvori**.
- Odaberite upravljačke programe za popis, a zatim odaberite **org.postgresql.Driver** **Naziv klase**pa odaberite **u redu**.

Da biste postavili vezu s lokalnog poslužitelja:
 
- Odaberite **Windows**, zatim **Prikaz pseudonima.** 
- Odaberite na **+** gumb da biste novi pseudonim. 
- Naziv *baze podataka neželjene e-pošte*, odaberite **PostgreSQL** u **upravljački program za** padajući popis.
- Postavite URL *jdbc:postgresql://localhost/spam*. 
- Unesite *korisničko ime* i *lozinku*. 
- Kliknite **u redu**. 
- Da biste otvorili prozor **veze** , dvokliknite pseudonim za ***neželjenu poštu baze podataka*** . 
- Odaberite **Poveži**.

Da biste pokrenuli neki upiti:

- Odaberite karticu **SQL** .
- Unesite jednostavnog upita kao što su `SELECT * from data;` u tekstni okvir upit pri vrhu kartice SQL. 
- Pritisnite **Ctrl-unesite** da biste ga pokrenuti. Prema zadanim postavkama Squirrel SQL vraća prvih 100 redaka iz upita. 

Postoji mnogo više upita možete pokrenuti da biste istražili te podatke. Na primjer, kako ne učestalost opaska *provjerite* razlikuju neželjene e-pošte i ham?

    SELECT avg(word_freq_make), spam from data group by spam;

Ili koje su značajke e-pošte koje često sadrže *3d*?

    SELECT * from data order by word_freq_3d desc;

Većina poruke e-pošte koje sadrže visoke pojavljivanja *3d* su su neželjene e-pošte, pa ga može biti korisna značajka za izgradnju predvidljivu model za klasifikaciju u porukama e-pošte.

Ako želite izvršiti strojnog učenja s podacima koji se pohranjuju u bazi podataka PostgreSQL, preporučujemo da koristite [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server Data Warehouse

Azure SQL Data Warehouse je u oblaku, skala iz baze podataka može obrade pretraživanje velikog količine podataka, relacijski i koje nisu relacijske. Dodatne informacije potražite u članku [što je Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Povezivanje s podacima skladištu i stvorite tablicu, pokrenite sljedeću naredbu iz naredbenog retka:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Zatim naredbenom sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Kopirajte podatke s bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] Crte kod u preuzete datoteke su Windows stil, ali bcp očekuje UNIX stilu tako moramo znati bcp koji zastavicom - r.

I upit s sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Nije moguće upita i s Squirrel SQL. Slijedite korake za slične za PostgreSQL, koristeći Microsoft MSSQL poslužitelja JDBC upravljački program, koju je moguće pronaći u ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Daljnji koraci

Pregled tema koje će vas voditi kroz zadatke koje čine postupka znanstvenog podataka u Azure, potražite u članku [Timu podataka znanstvenog postupak](http://aka.ms/datascienceprocess).

Opis druge završetka do kraja vodiči kojima se ilustrira koraka u postupku znanstvenog timu podataka za nekim konkretnim scenarijima potražite u članku [timu podataka znanstvenog procese](data-science-process-walkthroughs.md). Prikazi prikazuju i upute za kombiniranje oblaka i lokalnog alate i usluge u tijeku rada ili kanal za stvaranje Inteligentna aplikacije.

