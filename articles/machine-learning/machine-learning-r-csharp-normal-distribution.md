<properties 
    pageTitle="Paket za servis normalnu razdiobu Web | Microsoft Azure" 
    description="Paket za servis Web normalnu distribuciju." 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 

#<a name="normal-distribution-suite"></a>Paket normalnu distribuciju.



Paket normalnu razdiobu je skup uzorak web-servisi ([Generator]( https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile Kalkulator]( https://datamarket.azure.com/dataset/aml_labs/ndq5) [Vjerojatnost Kalkulator]( https://datamarket.azure.com/dataset/aml_labs/ndp5)) koji pomažu u generiranje i obrađuju distribucija normalno. Usluge omogućuju generiranje nizu normalnu razdiobu sve duljine, izračun quantiles s određenom vjerojatnost i izračunavanje vjerojatnosti s određenom quantile. Svaki servis emits različite izlaze koji se temelji na odabrani servis (pogledajte opis u nastavku). Paket normalnu razdiobu temelji se na qnorm funkcije R, rnorm i pnorm, koji su uključeni u paketu stat R.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Ovo web-servisa nije moguće troše korisnicima – potencijalno putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, na primjer. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete se objaviti trgovine Windows Azure i troše korisnicima i uređajima širom svijeta s postavljanjem bez infrastrukture prema autoru web-servisa.  
 

##<a name="consumption-of-web-service"></a>Potrošnje web-usluge
Paket normalnu razdiobu obuhvaća sljedeće 3 servise.

###<a name="normal-distribution-quantile-calculator"></a>Kalkulator Quantile normalnu distribuciju.
Taj servis prihvaća 4 argumente normalne distribucije i izračunava povezane quantile.

Ulazni argumenti su:

* p-jedan vjerojatnost događaja s normalnom razdiobom. 
* Sredina - Srednja normalnu distribuciju.
* SD – standardna devijacija normalnu distribuciju. 
* Strani - L donjoj strani distribuciju i U za gornjem dijelu distribuciju.

Izlaz iz servisa je izračunati quantile koji je povezan s određenom vjerojatnosti.

###<a name="normal-distribution-probability-calculator"></a>Kalkulator normalne distribucije vjerojatnosti.
Taj servis prihvaća 4 argumente normalne distribucije i izračunava vjerojatnost povezana.

Ulazni argumenti su:

* pitanja-jedan quantile događaja s normalnom razdiobom. 
* Sredina - Srednja normalnu distribuciju.
* SD – standardna devijacija normalnu distribuciju. 
* Strani - L donjoj strani distribuciju i U za gornjem dijelu distribuciju.

Izlaz iz servisa je vjerojatnost izračuna koji je povezan s određenom quantile.

###<a name="normal-distribution-generator"></a>Generator normalnu distribuciju.
Taj servis prihvaća 3 argumente normalne distribucije i generira slučajni niza brojeva koji se normalno raspodijeljen. Sljedeće argumente biti dostupni u nju unutar zahtjev:

* n - broj opažanja. 
* Sredina - Srednja normalnu distribuciju.
* SD – standardna devijacija normalnu distribuciju. 

Izlaz iz servisa je niz duljine n s argumentima Srednja i sd na temelju normalne distribucije.

>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (Ovdje su primjeru aplikacije: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Vjerojatnost Kalkulator](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx) [Kalkulator Quantile](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:

###<a name="normal-distribution-quantile-calculator"></a>Kalkulator Quantile normalnu distribuciju.
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Kalkulator normalne distribucije vjerojatnosti.
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Generator normalnu distribuciju.
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>U ovom web-servisa stvorena je pomoću Azure strojnog učenja. Za besplatnu probnu verziju, kao i uvodni videozapisi o stvaranju eksperimenata i [Objavljivanje web-servisi](machine-learning-publish-a-machine-learning-web-service.md), pročitajte članak [azure.com/ml](http://azure.com/ml). 

U nastavku je snimka zaslona pokusa stvorenih web kod usluge i primjere za svaki od modula unutar pokusa.

###<a name="normal-distribution-quantile-calculator"></a>Kalkulator Quantile normalnu distribuciju.

Tijek eksperiment:

![Tijek eksperiment][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Kalkulator normalne distribucije vjerojatnosti.
Tijek eksperiment:

![Tijek eksperiment][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Generator normalnu distribuciju.
Tijek eksperiment:

![Tijek eksperiment][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Ograničenja 

To su vrlo jednostavne Primjeri oko normalnu distribuciju. Kao što je vidljivo iz Šifra za primjer, implementira se pogreška mnogo toga.

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
