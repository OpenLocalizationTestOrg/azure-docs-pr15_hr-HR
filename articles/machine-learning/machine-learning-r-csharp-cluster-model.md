<properties 
    pageTitle="Skupine modela | Microsoft Azure" 
    description="Model klaster" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Model klaster    

Kako ćemo predviđanje grupe odobrenja cardholders ponašanja kako bi se smanjila rizika trošak isključivanje kreditnih kreditne kartice? Kako ćemo definiranje grupa za osobnost publikom zaposlenika da biste poboljšali njihove performanse na poslu? Kako mogu doctors klasifikaciju bolesnike koji dolaze u grupe na temelju karakteristike njihove diseases? U načelo, sva ta pitanja možete odgovoriti putem klaster analize.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Analiza klaster raspoređuje skup opažanja u dva ili više isključivih Nepoznato grupe na temelju kombinacije varijabli. Svrha analizu klaster je s ciljem otkrivanja sustava organizacije opažanja, obično osoba ili njihove karakteristike u grupe, gdje zajednički grupe dijeliti svojstva. [Servis](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) koristi methodology K znači, često korištenih klasteriranja postupak, klaster proizvoljne podatke u grupe. U ovom web-servisa uzima podatke i broj k klaster kao ulaz i daje predviđanja koje k grupe kojoj pripada svaki opažanja. 

>Ovo web-servisa nije moguće troše korisnicima – potencijalno putem mobilne aplikacije putem web-mjesta ili čak i na lokalnom računalu, na primjer. Ali se i da bi služio kao primjer kako se Azure strojnog učenja možete koristiti za stvaranje web-servisi pri vrhu R kod Svrha web-servisa. Uz samo nekoliko redaka koda R i klika na gumb unutar Azure strojnog učenja Studio, pokusa mogu stvoriti kod R i objavili kao web-servisa. Web-servisa možete se objaviti trgovine Windows Azure i troše korisnicima i uređajima širom svijeta s postavljanjem bez infrastrukture prema autoru web-servisa.  

##<a name="consumption-of-web-service"></a>Potrošnje web-usluge   
U ovom web-servisa grupira podatke u skupu k grupe i šalje e-pošta za svaki redak. Web-servisa očekuje krajnjeg korisnika za unos podataka kao niz kojima reci će biti razdvojeni zarezima (,) i stupaca su razdvojenih točkom sa zarezom (;). Web-servisa očekuje 1 redak po jedan. Na primjer skup podataka može izgledati ovako:

![Ogledni podaci][1]

Pretpostavimo da korisnik željeli podijelite ove podatke u 3 isključivih grupe. Podaci za unos za skup podataka iznad bio sljedeći: vrijednost = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". Rezultat je predviđene korisnikova članstva u grupama za sve retke.

>Taj servis kao što je hostirane na trgovine Windows Azure je servis OData; te se možda naziva kroz objave ili PRONAĐITE načine. 

Više je načina za troše servisa automatskog način (aplikaciju primjer je [u nastavku](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>Početni C# kod za utrošak web servisa:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Iz unutar Azure strojnog učenja, novi prazan eksperiment stvorena i dva [Izvršavanje skripte R] [ execute-r-script] moduli koji se povlače u radnom prostoru. Shema podataka stvorena je pomoću jednostavnu [Izvršavanje skripte R][execute-r-script]. Pa je bio shemi podataka povezan sa klaster odjeljku model ponovno stvorena pomoću [Skripte za izvršavanje R][execute-r-script]. U [Izvršavanje skripte R] [ execute-r-script] koristi se za model klaster, web-servisa zatim koristi funkciju "k znači", koja je ugrađene u [Izvršavanje skripte R] [ execute-r-script] od Azure strojnog učenja.    
   

     
![Tijek eksperiment][3]

####<a name="module-1"></a>Modul 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Modul 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Ograničenja
Ovo je vrlo jednostavne primjera klasteriranja web-servisa. Kao što je vidljivo iz Šifra za primjer, bez pogreške toga implementirana i usluge pretpostavlja da sve je neprekinuti varijabla (bez categorical značajke dopušteni), kao servisa numeričke vrijednosti unose samo prilikom stvaranja ovog web-servisa. Servis trenutno bavi i veličina ograničeni podataka, krajnji za odgovor na/zahtjev priroda poziva web-servisa i činjenica da model je u tijeku Prilagodi svaki put jest web-servisa. 

##<a name="faq"></a>NAJČEŠĆA PITANJA
Najčešća pitanja na potrošnje web-servisa ili objavljivanje trgovine Windows Azure, potražite u članku [ovdje](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
