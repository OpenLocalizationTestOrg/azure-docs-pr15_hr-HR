<properties 
    pageTitle="Paket binomna razdioba | Microsoft Azure" 
    description="Paket binomnu distribuciju." 
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


#<a name="binomial-distribution-suite"></a>Paket binomnu distribuciju.




Skup uzorak web-servisi ([Binomne Generator](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Vjerojatnost Kalkulator]( https://datamarket.azure.com/dataset/aml_labs/bdp4) [Kalkulator Quantile]( https://datamarket.azure.com/dataset/aml_labs/bdq5)) koji pomažu u generiranje i povezanima s binomna distribucija je paket binomnu distribuciju. Na servisi omogućuju generiranje nizu binomna razdioba sve duljine izračunavanje quantiles o dali vjerojatnosti i izračunavanje vjerojatnosti iz navedene quantile. Svaki servis emits različite izlaze koji se temelji na odabrani servis (pogledajte opis u nastavku). Paket binomna razdioba temelji se na qbinom funkcije R, rbinom i pbinom, koji su uključeni u paketu stat R. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Ove web-servisi može biti troše korisnika – potencijalno izravno na tržištu putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, primjerice. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete biti objavljeni trgovine Windows Azure i troše korisnicima i uređajima širom svijeta – potrebna je instalacijski program ne infrastrukture prema autoru web-servisa.

##<a name="consumption-of-web-service"></a>Potrošnje web-usluge
Paket binomna razdioba obuhvaća sljedeće 3 servise.

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulator Quantile binomnu distribuciju.
Taj servis prihvaća 4 argumente normalne distribucije i izračunava povezane quantile.
Ulazni argumenti su:

- p - jedan pridružuje vjerojatnost od više pokušaja.  
- Veličina - broj pokušaja.
- PROB - vjerojatnost uspjeha u probnom razdoblju.
- Strani - L donjoj strani razdiobu, U za gornjem dijelu distribuciju. 

Izlaz iz servisa je izračunati quantile koji je povezan s određenom vjerojatnosti.

###<a name="binomial-distribution-probability-calculator"></a>Kalkulator binomne izraze distribucije vjerojatnosti.
Taj servis prihvaća 4 argumente binomne distribucije i izračunava vjerojatnost povezana.
Ulazni argumenti su:

- pitanja-jedan quantile događaja pomoću binomne distribucije. 
- Veličina - broj pokušaja.
- PROB - vjerojatnost uspjeha u probnom razdoblju.
- strani - L donjoj strani razdiobu, U za gornjem dijelu raspodjele ili E koji je jednak jedan broj uspješnih pokušaja.

Izlaz iz servisa je vjerojatnost izračuna koji je povezan s određenom quantile.

###<a name="binomial-distribution-generator"></a>Generator binomnu distribuciju.
Taj servis prihvaća 3 argumente binomne distribucije i generira slučajni niza brojeva koji binomially distribuirati. Sljedeće argumente biti dostupni u nju unutar zahtjev:

- n - broj opažanja. 
- Veličina - broj pokušaja.
- PROB - vjerojatnosti uspjeha.

Izlaz iz servisa je niz duljine n sa binomnu distribuciju utemeljenu na veličinu i prob argumente.

>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (Ovdje su primjeru aplikacije: [Generator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Vjerojatnost Kalkulator](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx) [Kalkulator Quantile](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulator Quantile binomnu distribuciju.
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Kalkulator binomne izraze distribucije vjerojatnosti.
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Generator binomnu distribuciju.
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Kalkulator Quantile binomnu distribuciju.

![Stvaranje radnog prostora][4]

####<a name="module-1"></a>Modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Modul 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Kalkulator binomne izraze distribucije vjerojatnosti.

![Stvaranje radnog prostora][5]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Generator binomnu distribuciju.

![Stvaranje radnog prostora][6]

####<a name="module-1"></a>Modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Ograničenja 
To su vrlo jednostavne Primjeri oko binomnu distribuciju. Kao što je vidljivo iz Šifra za primjer, implementira se pogreška mnogo toga.

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
