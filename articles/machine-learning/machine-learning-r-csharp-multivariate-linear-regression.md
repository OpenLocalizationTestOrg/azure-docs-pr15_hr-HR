<properties 
    pageTitle="Multivariate linearna regresija | Microsoft Azure" 
    description="Multivariate linearna regresija" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multivariate linearna regresija   
 

 
Recimo da imate skup podataka, a želite brzo predviđanje zavisne varijable (y) za svakog korisnika (i) na temelju nezavisnu varijablu. Linearna regresija je popularne Statistička tehnika za pretraživanje kao predviđanja. Ovdje y zavisne varijable pretpostavlja se da neprekinuti vrijednost.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Jednostavni scenarij može biti gdje se researcher pokušava predviđanje debljine pojedinac (y) na temelju njihove visine (x). Naprednije scenarij može biti gdje se researcher ima dodatne informacije za pojedinačne (kao što su Debljina, spol trkaći) i pokušava predviđanje debljine pojedinačne. [Web-servisa]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) odgovara linearna regresija model podataka, a proizvodi predviđene vrijednosti (y) za svaki opažanja u podacima.

>Ovo web-servisa nije moguće troše korisnicima – potencijalno putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, na primjer. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete se objaviti trgovine Windows Azure i troše korisnicima i uređajima širom svijeta s postavljanjem bez infrastrukture prema autoru web-servisa.  

##<a name="consumption-of-web-service"></a>Potrošnje web-usluge  
U ovom web-servisa daje predviđene vrijednosti na temelju nezavisnu varijablu za sve u opažanja zavisne varijable. Web-servisa očekuje krajnjeg korisnika za unos podataka kao niz kojima reci će biti razdvojeni zarezima (,) i stupaca su razdvojenih točkom sa zarezom (;). Web-servisa očekuje 1 redak po, a očekuje prvi stupac zavisne varijable. Na primjer skup podataka može izgledati ovako:

![Ogledni podaci][1]

Opažanja bez zavisne varijable treba unos kao "N/d" za y. Podaci za unos za skup podataka iznad bio sljedeći niz: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, n/d"; "3"; "4". Rezultat je predviđene vrijednosti za svaki od redaka na temelju nezavisnu varijablu. 

>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (aplikaciju primjer je [u nastavku](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Iz unutar Azure strojnog učenja, novi prazan eksperiment stvorena i dva [Izvršavanje skripte R] [ execute-r-script] moduli su koji se povlače u radnom prostoru. U ovom web-servisa pokreće pokusa Azure strojnog učenja s skripte podlozi R. Postoje 2 dijelova ovaj eksperiment: definicija sheme i obuka modela + bilježenje rezultata. Modul za prvu definira očekivani strukturu unos skupu podataka, gdje je prvi varijabla zavisne varijable i preostale varijable su nezavisne. Modul za drugi odgovara generički linearna regresija modela za unos podataka.  
  
![Tijek eksperiment][3]

####<a name="module-1"></a>Modul 1:
 
####<a name="schema-definition"></a>Definicija sheme  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Modul 2:
####<a name="lm-modeling"></a>Modeliranje LM   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Ograničenja
Ovo je vrlo jednostavne primjera više web-servisa linearne regresije. Kao što je vidljivo iz Šifra za primjer, bez pogreške toga implementirana i usluge pretpostavlja da sve je neprekinuti varijabla (bez categorical značajke dopušteni), kao servisa numeričke vrijednosti unose samo prilikom stvaranja ovog web-servisa. Servis trenutno bavi i veličina ograničeni podataka, krajnji za odgovor na/zahtjev priroda poziva web-servisa i činjenica da model je u tijeku Prilagodi svaki put jest web-servisa. 

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
