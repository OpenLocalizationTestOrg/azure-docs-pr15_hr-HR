<properties 
    pageTitle="Leksikon temelji šalju analizu | Microsoft Azure" 
    description="Leksikon temelji šalju analizu" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Leksikon temelji šalju analizu 

Kako možete ćete mjeriti mišljenja korisnika i attitudes prema marke ili teme u mreži društvenih mreža, kao što su Facebook objava, tweets, preglede itd.? Analiza šalju omogućuje način za analizu takve pitanja.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Obično se za analizu šalju i na dva načina. Jedan koristi supervised učenje algoritam, a drugi mogu se smatrati unsupervised učenje. Algoritam za učenje supervised obično sastavlja klasifikacija modela na velike označenim corpus. Njegov točnost uglavnom temelju kvalitete primjedbe i obično postupka osposobljavanja će potrajati. Osim toga, kad ne možemo primijeniti algoritam na drugu domenu rezultat nije obično dobro. U usporedbi s supervised učenje, koji se temelji na Leksikon unsupervised učenje koristi šalju rječnika koja ne zahtijeva pohrani velike podatkovne corpus i obuka – što čini mnogo brže cijelog procesa. 

Naših [usluga](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) se temelji na MPQA Subjectivity Leksikon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), što je jedna od najčešće korištenih subjectivity lexicons. Postoje 5097 negativni i 2533 pozitivan riječi u MPQA. Te su dodavati sve ove riječi napomene kao dobra ili loša polaritet. Cijeli corpus je u odjeljku Općenito GNU javno licence. Web-servisa primjenjuje se na kratke rečenice, kao što su tweets i Facebook objave. 

>U ovom web-servisa nije moguće troše korisnicima – potencijalno putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, primjerice. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete se objaviti trgovine Windows Azure i troše korisnicima i uređajima širom svijeta s postavljanjem bez infrastrukture prema autoru web-servisa.

##<a name="consumption-of-web-service"></a>Potrošnje web-usluge

Unos podataka može biti bilo tekst, ali web-servisa radi bolje kratke rečenice. Rezultat je numeričku vrijednost između -1 i 1. Bilo koja vrijednost ispod 0 označava je li šalju tekst negativan; pozitivni ako je iznad 0. Apsolutna vrijednost argumenta rezultat označava jačinu pridružene šalju. 

>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (aplikaciju primjer je [u nastavku](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



Unos je "Danas je dobar dan." Rezultat je "1", koja pokazuje pozitivan šalju pridružene unos rečenicu. 

##<a name="creation-of-web-service"></a>Stvaranje web-usluge
>U ovom web-servisa stvorena je pomoću Azure strojnog učenja. Za besplatnu probnu verziju, kao i uvodni videozapisi o stvaranju eksperimenata i [Objavljivanje web-servisi](machine-learning-publish-a-machine-learning-web-service.md), pročitajte članak [azure.com/ml](http://azure.com/ml). U nastavku je snimka zaslona pokusa stvorenih web kod usluge i primjere za svaki od modula unutar pokusa.


Iz unutar Azure strojnog učenja, novi prazan eksperiment stvorena. Na slici u nastavku prikazuje tijek eksperiment utemeljen na Leksikon šalju analize. "Sent_dict.csv" datoteka je subjectivity Leksikon MPQA i postavljena kao jedan od unosa [Izvršavanje skripte R][execute-r-script]. Drugi unos je sampled pregled iz Amazon pregled skupa podataka za testiranje, gdje ćemo izvršiti odabir izmjene naziva stupaca, i Podjela operacije. Koristimo raspršivanje paket za pohranu Leksikon subjectivity u memoriji i accelerate postupka rezultat izračuna. Cijeli tekst će biti tokenized u paketu "tm" i uspoređuje s riječ u rječnik šalju. Na kraju, rezultat će biti izračunat dodavanjem debljine svake subjektivna riječi u tekstu. 

###<a name="experiment-flow"></a>Tijek eksperiment:

![Isprobajte tijek][2]


####<a name="module-1"></a>Modul 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Instalirajte paket raspršivanje
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Ograničenja

Iz perspektive na algoritam utemeljen na Leksikon šalju analizu je alat za analizu Općenito šalju, koji može imati bolje način klasifikacije određenih polja. Negativni broj problem se ne i riješili. Ne možemo hardcode nekoliko negativni broj riječi u naš program, ali bolji način je pomoću negativni broj rječnik i izraditi neka pravila. Web-servisa provodi bolje kratki i jednostavno rečenice, kao što su tweets i objave Facebook od na dugi i složene rečenice kao što su pregledi Amazon. 

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
