<properties 
    pageTitle="O analize pomoću Azure strojnog učenja | Microsoft Azure" 
    description="O analizi događaj pojavu vjerojatnosti" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>O analizi 

U odjeljku mnogo scenarija glavni rezultat u odjeljku procjenu je vrijeme događaja koje vas zanimaju. Drugim riječima, pitanje "kada se ovaj događaj će se pojaviti?" se od vas zatraži. Kao primjeri, razmislite o slučajevima gdje podatke opisuje Proteklog vremena (dana, godine, potrošnje, itd.) do događaja koje vas zanimaju (relapse Media Reporting stupanj Doktorski primili, pogreška kočnica tipkovnica) pojavljuje. Svaku instancu podataka predstavlja određenog objekta (s strpljivi učenik, u automobilu, itd.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

[Web-servisa]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) odgovora na pitanje "koji je vjerojatnost događaja koje vas zanimaju dolazi sa n vremena za objekt x?" Unosom model za analizu o ovom web-servisa omogućuje korisnicima izvor podataka uvježbati model, a zatim ga testirate. Glavni temu pokusa je model Proteklog vremena dok se ne dogodi događaj koji vas zanimaju. 

>Ovo web-servisa nije moguće troše korisnicima – potencijalno putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, na primjer. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete se objaviti trgovine Windows Azure i troše korisnicima i uređajima širom svijeta s postavljanjem bez infrastrukture prema autoru web-servisa.  

##<a name="consumption-of-web-service"></a>Potrošnje web-usluge

U tablici u nastavku prikazan je shema ulaznih podataka web-servisa. Šest dijelovi informacija potrebnih kao unos: obuka podataka, testiranje podataka, vrijeme koje vas zanimaju, indeks "vrijeme" dimenzije, indeks "događaj" dimenzije i varijable vrste (neprekinuti ili faktor). Obuka podataka predstavlja nizom, pri čemu retke odvojene zarezima, a stupaca su ih razdvojite točkom sa zarezom. Prilagodljivo je broj značajki podataka. Svi elementi u niz mora biti brojčana. U podataka obuka dimenzije "vrijeme" ukazuje na broj vremenske jedinice (dana, godine, potrošnje, itd.) proteklih od početnu točku slučaja (strpljivi primanje zloupotrebu pokusa programe, slučaja Doktorski početni učenika, automobila pokretanje da biste se hibridnih, itd.) do događaja koje vas zanimaju (u strpljivi povratkom na korištenje zloupotrebu, učenika stjecanje stupanj Doktorski, automobil pojavljuju pogreške kočnica tipkovnice itd.) Kada se dogodi. Dimenzije "događaj" označava pojavljuje li se na kraju na studije događaja koje vas zanimaju. Vrijednost "događaj = 1" znači da interesa događaj u trenutku označenu dimenziju "vrijeme" "događaj = 0" znači da se događaj koji vas zanimaju nije pojavila po vremenu označenu dimenzije "vrijeme".

- trainingdata - niz znakova. Reci će biti razdvojeni zarez, a stupaca su ih razdvojite točkom sa zarezom. Svaki redak sadrži dimenzije "vrijeme", "događaj" dimenzije i predictor varijabli.
- testingdata – jedan redak podataka koja sadrži varijable predictor za određenog objekta.
- time_of_interest - Proteklog vremena n kamata.
- index_time - indeks stupca dimenzije "vrijeme" (počevši od 1).
- index_event - indeks stupca (počevši od 1) dimenzije "događaj".
- variable_types - znakovnom nizu točkom sa zarezom kao razdjelnika u njoj. 0 predstavlja neprekinuti varijable, a 1 faktor varijabli.


Rezultat je vjerojatnost događaja koje su se pojavile tako da određeno vrijeme. 

>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (aplikaciju primjer je [u nastavku](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Tumačenja ovaj test je na sljedeći način. Cilj podatke uz pretpostavku da je model Proteklog vremena do povratak na korištenje zloupotrebu za bolesnike koji dolaze koji su primili jedan od dva postupka programa. Izlaz iz servisa čitanja web: za bolesnike koji dolaze 35 godina u tijeku, imate prethodne zloupotrebu pokusa 2 puta neobično dugo zajedničkog pokusa program i korištenje heroin i cocaine vjerojatnost povratkom na korištenje zloupotrebu je % 95.64 po dan 500.

##<a name="creation-of-web-service"></a>Stvaranje web-usluge

>U ovom web-servisa stvorena je pomoću Azure strojnog učenja. Za besplatnu probnu verziju, kao i uvodni videozapisi o stvaranju eksperimenata i [Objavljivanje web-servisi](machine-learning-publish-a-machine-learning-web-service.md), pročitajte članak [azure.com/ml](http://azure.com/ml). U nastavku je snimka zaslona pokusa stvorenih web kod usluge i primjere za svaki od modula unutar pokusa.

Iz unutar Azure strojnog učenja, novi prazan eksperiment stvorena i dva [Izvršavanje skripte R] [ execute-r-script] moduli su koji se povlače u radnom prostoru. Shema podataka stvorena je pomoću jednostavnu [Izvršavanje skripte R][execute-r-script], koji definira shemu ulaznih podataka za web-servisa. Ovaj modul tada povezan s drugom [Izvršavanje skripte R] [ execute-r-script] modula, glavnih rad. U ovom modul ne pretprocesnih podataka, model sastavnih i predviđanja. U koraku pretprocesnih podataka ulaznih podataka predstavlja niz dugo transformacije i pretvoriti u okvir podataka. U koraku sastavnih modela paketa vanjskih R "survival_2.37 7.zip" prve instalacije za vođenje o analizi. Zatim se izvršava funkciju "coxph" nakon zadatke za obradu podataka niz. Detalje o funkciju "coxph" za analizu o mogu čitati iz R dokumentacije. U koraku predviđanje testiranja instancu dolazi u obučeni model s funkcijom "surfit", a krivulje o za ovu instancu testiranja koje je kao varijabla "krivulje". Na kraju, dobivaju se vjerojatnost vrijeme koje vas zanimaju. 

###<a name="experiment-flow"></a>Tijek eksperiment:

![Isprobajte tijek][1]

####<a name="module-1"></a>Modul 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Modul 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Ograničenja

U ovom web-servisa može potrajati samo numeričke vrijednosti kao značajka varijable (stupce). Stupac "događaj" može potrajati samo vrijednost 0 ili 1. Stupac "vrijeme" mora biti pozitivni cijeli broj.

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
