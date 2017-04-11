<properties 
    pageTitle="Predviđanje-eksponencijalno izglađivanje | Microsoft Azure" 
    description="Web-usluge: predviđanje-eksponencijalne Smoothing" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="xueshzha"/> 


#<a name="forecasting---exponential-smoothing"></a>Predviđanje - eksponencijalno izglađivanje 

[Web-servisa]( https://datamarket.azure.com/dataset/aml_labs/ets) implementira modela eksponencijalno izglađivanje (ETS) da bi proizveo predviđanja koji se temelji na povijesnim podacima koje ste dobili od korisnika. Povećava li zahtjev za određeni proizvod ove godine? Mogu li predviđanje Moje Prodaja proizvoda za razdoblja božićne tako da mogu učinkovito planiranje Moje zaliha? Predviđanja modeli su Zemaljska adresa takve pitanja. Dani proteklih podataka te modelima pregledali skrivene trendova i sezonalnost za predviđanje buduće trendova.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 
>Ovo web-servisa nije moguće troše korisnicima – potencijalno putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, na primjer. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete se objaviti trgovine Windows Azure i troše korisnicima i uređajima širom svijeta s postavljanjem bez infrastrukture prema autoru web-servisa.
 
##<a name="consumption-of-web-service"></a>Potrošnje web-usluge 
 
Taj servis prihvaća 4 argumenata i izračunava predviđanja grafičke oznake.
Ulazni argumenti su:

* FREQUENCY – označava učestalost sirovim podacima (dnevnu/Tjedno/Mjesečno/tromjesečno i godišnje).
* Opseg - budućnost predviđanja vremensko razdoblje.
* Datum – dodavanje u novi niz podataka za vrijeme za vrijeme.
* Vrijednost - dodajte u nove vrijednosti vremena niza podataka.

Izlaz iz servisa je izračunate vrijednosti predviđanja.

Ogledni unos može biti: 

* FREQUENCY – 12
* Opseg – 12
* Datum - 1/15/2012 15/2/2012; 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012; 7/15/2012; 8 / 15/2012 9/15/2012; 10/15/2012; 11/15/2012; 12/15/2012; 1/15/2013 2/15/2013; 3/15/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013 9/15/2013; 15/10/2013; 11/15/2013; 12/15/2013; 1/15/2014. 2/15/2014.; 3/15/2014.; 4/15/2014.; 5/15/2014.; 6/15/2014.; 7/15/2014.; 8 / 15/2014.; 9/15/2014.
* Vrijednost - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509
 
>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (aplikaciju primjer je [u nastavku](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:
    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }



##<a name="creation-of-web-service"></a>Stvaranje web-usluge 

>U ovom web-servisa stvorena je pomoću Azure strojnog učenja. Za besplatnu probnu verziju, kao i uvodni videozapisi o stvaranju eksperimenata i [Objavljivanje web-servisi](machine-learning-publish-a-machine-learning-web-service.md), pročitajte članak [azure.com/ml](http://azure.com/ml). U nastavku je snimka zaslona pokusa stvorenih web kod usluge i primjere za svaki od modula unutar pokusa.

Iz unutar Azure strojnog učenja, novi prazan eksperiment stvorena. Ogledne ulazne podatke je učitana sa shemom unaprijed definirane podatke. Povezana sa shemom podataka se [Izvoditi skripte R] [ execute-r-script] modul koji stvara ETS predviđanje modela pomoću funkcija 'ets' i 'predviđanja iz R. 


![Tijek eksperiment][2]

####<a name="module-1"></a>Modul 1:
 
    # Add in the CSV file with the data in the format shown below 
![Uzorak podataka][3]   

####<a name="module-2"></a>Modul 2:
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- ets(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

 
##<a name="limitations"></a>Ograničenja 

Ovo je vrlo jednostavne primjer za predviđanje grafičke oznake. Kao što je vidljivo iz Šifra za primjer, bez pogreške toga je implementirana i usluge pretpostavlja da sve varijable su neprekinuti/pozitivne vrijednosti i učestalost mora biti cijeli broj veći od 1. Duljina vektori datum i vrijednost mora biti isti. Varijabla datum treba drži u oblik "mm/dd/gggg".

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img1.png
[2]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img2.png
[3]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
