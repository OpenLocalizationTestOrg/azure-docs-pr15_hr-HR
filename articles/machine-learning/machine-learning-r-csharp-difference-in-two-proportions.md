<properties 
    pageTitle="Razlike u testu proporcije | Microsoft Azure" 
    description="Razlike u testu proporcije" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Razlike u testu proporcije


Razlikuju se dva proporcije statistically? Pretpostavimo da korisnik želi da biste usporedili dvije filmova da biste odredili ima li jednu film znatno veću udio "sviđanja" kada u usporedbi s drugim. S velikim uzorka, što može biti statistically Glavna razlika između proporcije 0.50 i 0.51. Pomoću malog oglednog možda nema dovoljno podataka da biste odredili ako su te proporcije zapravo drukčiji. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

[Web-servisa]( https://datamarket.azure.com/dataset/aml_labs/prop_test) obavlja test pretpostavke razlike u dva proporcije ovisno o korisničkom unosu broj uspjeha i ukupan broj probne verzije za grupe 2 usporedbe. U jedan scenarij moguće ovo web-servisa možda zove iz u aplikaciji film usporedbe, koja upozorava korisnika li jednu od filmova je zaista "sviđa mi" češće od druge, na temelju ocjena film.

>Ovo web-servisa nije moguće troše korisnicima – potencijalno putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, na primjer. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete se objaviti trgovine Windows Azure i troše korisnicima i uređajima širom svijeta s postavljanjem bez infrastrukture prema autoru web-servisa.


##<a name="consumption-of-web-service"></a>Potrošnje web-usluge

Taj servis prihvaća 4 argumenata i ne na pretpostavke testiranje od proporcije.

Ulazni argumenti su:

* Successes1 – broj uspjeha događaja u primjeru 1.
* Successes2 – broj uspjeha događaja u primjeru 2.
* Total1 - veličina uzorka 1.
* Total2 - veličina uzorka 2.

Izlaz iz servisa je rezultat pretpostavku testirajte uz chi-square statistike, df, p vrijednost i proporcije Ogledna granica 1/2 i interval pouzdanosti.

>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (aplikaciju primjer je [u nastavku](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

Iz unutar Azure strojnog učenja, novi prazan eksperiment stvorena je pomoću dva [Izvršavanje skripte R] [ execute-r-script] module. U prvom se definira shemu podataka modul dok drugi modul koristi prop.test naredbu unutar R da biste izvršili pretpostavke test za 2 proporcije. 


###<a name="experiment-flow"></a>Tijek eksperiment:

![Tijek eksperiment][2]


####<a name="module-1"></a>Modul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Modul 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Ograničenja 

Ovo je vrlo jednostavne primjer test razlike u 2 proporcije. Kao što je vidljivo iz Šifra za primjer, bez pogreške toga implementirana i usluge pretpostavlja da su sve varijable neprekinuti.

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
