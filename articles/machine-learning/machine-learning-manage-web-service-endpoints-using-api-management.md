<properties
    pageTitle="Informirajte se o upravljanju AzureML web-servisi značajkom upravljanja API | Microsoft Azure"
    description="Vodič s prikazom načina za upravljanje AzureML web-servisi značajkom upravljanja API-JA."
    keywords="strojnog učenja upravljanje API-ja"
    services="machine-learning"
    documentationCenter=""
    authors="roalexan"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="roalexan" />


# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Informirajte se o upravljanju AzureML web-servisi značajkom upravljanja API-JA

##<a name="overview"></a>Pregled

Ovaj vodič pokazuje kako brzo početi koristiti Upravljanje API-JA za upravljanje web-servisi sustava AzureML.

##<a name="what-is-azure-api-management"></a>Što je upravljanje API Azure?

Azure upravljanja API je Azure usluga koja omogućuje upravljanje krajnje točke na REST API-JA definiranjem korisničkog pristupa, korištenje ograničavanje i nadzor nadzorne ploče. Kliknite [ovdje](https://azure.microsoft.com/services/api-management/) pojedinosti o upravljanje Azure API-JA. Kliknite [ovdje](../api-management/api-management-get-started.md) za vodič za početak rada s upravljanjem Azure API-JA. Druge vodič, koji se temelji ovaj vodič, obuhvaća dodatne teme, uključujući konfiguracije obavijesti, sloju cijene, obradu odgovora, provjere autentičnosti korisnika, stvaranje proizvoda, pretplate za razvojne inženjere i korištenje dashboarding.

##<a name="what-is-azureml"></a>Što je AzureML?

AzureML je servis za strojnog učenja koji omogućuje jednostavno stvaranje, uvođenje i zajedničko korištenje naprednom analitikom rješenja za Azure. Kliknite [ovdje](https://azure.microsoft.com/services/machine-learning/) pojedinosti o AzureML.

##<a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj vodič, potrebno je:

* Račun Azure. Ako nemate račun za Azure, kliknite [ovdje](https://azure.microsoft.com/pricing/free-trial/) detalje o stvaranju besplatnu probnu računa.
* Račun AzureML. Ako nemate račun AzureML, kliknite [ovdje](https://studio.azureml.net/) detalje o stvaranju besplatnu probnu računa.
* Radni prostor, servisa i api_key za pokusa AzureML u uveden kao web-servisa. Kliknite [ovdje](machine-learning-create-experiment.md) detalje o stvaranju pokusa AzureML. Kliknite [ovdje](machine-learning-publish-a-machine-learning-web-service.md) detalje o tome kako uvesti pokusa AzureML kao web-servisa. Umjesto toga dodatak A sadrži upute za stvaranje i testiranje jednostavne eksperiment AzureML i implementirajte ga u obliku web-servisa.

##<a name="create-an-api-management-instance"></a>Stvaranje instance upravljanje API-JA

U nastavku su navedeni koraci za korištenje upravljanje API-JA za upravljanje web-servisa na AzureML. Najprije stvorite instanca servisa. Prijavite se na [Portal za klasični](https://manage.windowsazure.com/) , a zatim kliknite **Nova** > **Aplikacije servisa** > **API upravljanja** > **Stvori**.

![Stvaranje instance](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Navedite jedinstveni **URL-a**. Ovaj vodič koristi **demoazureml** – morat ćete odabrati nešto drugo. Odaberite željeni **pretplate** i **regije** za vaše instanca servisa. Nakon toga odabira, kliknite gumb Dalje.

![Stvaranje-servis-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Odrediti vrijednost za **Naziv tvrtke ili ustanove**. Ovaj vodič koristi **demoazureml** – morat ćete odabrati nešto drugo. U polju **administrator e-pošte** unesite adresu e-pošte. Tu adresu e-pošte koristi se za obavijesti iz sustava upravljanja API-JA.

![Stvaranje servisa-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Kliknite potvrdni okvir da biste stvorili vaše instanca servisa. *Traje roku od trideset minuta za novi servis će biti stvoren*.

##<a name="create-the-api"></a>Stvaranje na API-JA

Nakon stvaranja instanca servisa, sljedeći je korak da biste stvorili na API-JA. API sastoji se od skupa operacije koje možete pozvati iz klijentske aplikacije. Operacije API su koji na postojeće web-servise. Ovaj vodič stvara API-ji taj proxy poslužitelj za postojeće AzureML RRS i BES web-usluge.

API-ji stvara i konfigurirali s portala za publisher API kojoj se pristupa putem klasične portala za Azure. Da biste dosegne portal za publisher, odaberite svoje instanca servisa.

![Odaberite instancu-servisa](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA.

![Upravljanje uslugom](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Kliknite **API-ji** na izborniku **Upravljanje API -JA** na lijevoj strani, a zatim **Dodavanje API -JA**.

![API-ja, upravljanje i izbornika](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Upišite **AzureML pokazni videozapis API** kao **Naziv Web API**. Upišite **https://ussouthcentral.services.azureml.net** kao **URL web-servisa**. Upišite **azureml pokazni** **sufiks web-URL API -JA**. Potvrdite **HTTPS** kao **Web-URL API** shemu. Odaberite **Starter** **proizvoda**. Kada završite, kliknite **Spremi** da biste stvorili na API-JA.

![Dodavanje – novi-API-ja](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

##<a name="add-the-operations"></a>Dodavanje operacije

Kliknite **Dodaj postupak** da biste dodali operacije na navedeni API.

![Dodavanje operacije](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Prozor **Nova operacija** će se prikazati, a po zadanom će biti odabran na kartici **potpis** .

##<a name="add-rrs-operation"></a>RRS postupak dodavanja

Najprije stvorite operacije servisa za AzureML RRS. Odaberite **OBJAVI** kao **HTTP glagolski**. Vrsta **/services/ /workspaces/ {radnog prostora} {servisa} / execute?api verziju = {apiversion} & pojedinosti = {pojedinosti}** kao **URL predloška**. Upišite **RRS izvršavanje** kao **zaslonsko ime**.

![Dodavanje rrs-operacija-potpis](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Kliknite **Odgovori** > lijeve i odaberite**DODAJ** **200 u redu**. Kliknite **Spremi** da biste spremili ovaj postupak.

![Dodavanje rrs-operacija-odgovora](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

##<a name="add-bes-operations"></a>Dodavanje BES operacije

Snimke zaslona nisu obuhvaćeni BES operacija kao što su vrlo su slične onima za dodavanje RRS postupak.

###<a name="submit-but-not-start-a-batch-execution-job"></a>Slanje (ali ne želite pokrenuti) grupe za izvršavanje posla

Kliknite da biste dodali postupka AzureML BES na API **postupak dodavanja** . Odaberite **OBJAVA** za **HTTP glagolski**. Vrsta **/services/ /workspaces/ {radnog prostora} {servisa} / jobs?api verziju = {apiversion}** **URL predloška**. **Zaslonski naziv**upišite **BES slanja** . Kliknite **Odgovori** > lijeve i odaberite**DODAJ** **200 u redu**. Kliknite **Spremi** da biste spremili ovaj postupak.

###<a name="start-a-batch-execution-job"></a>Pokretanje grupe za izvršavanje zadatka

Kliknite da biste dodali postupka AzureML BES na API **postupak dodavanja** . Odaberite **OBJAVA** za **HTTP glagolski**. Vrsta **/jobs/ /services/ {servisa} /workspaces/ {radnog prostora} {jobid} / start?api verziju = {apiversion}** **URL predloška**. **Zaslonski naziv**upišite **BES Start** . Kliknite **Odgovori** > lijeve i odaberite**DODAJ** **200 u redu**. Kliknite **Spremi** da biste spremili ovaj postupak.

###<a name="get-the-status-or-result-of-a-batch-execution-job"></a>Status ili rezultat izvođenja obrade posla

Kliknite da biste dodali postupka AzureML BES na API **postupak dodavanja** . Odaberite **Početak** za **HTTP glagolski**. Vrsta **/workspaces/ {radnog prostora} {servisa} /services/ /jobs/ {jobid} ?api verzija = {apiversion}** **URL predloška**. **Zaslonski naziv**upišite **BES stanje** . Kliknite **Odgovori** > lijeve i odaberite**DODAJ** **200 u redu**. Kliknite **Spremi** da biste spremili ovaj postupak.

###<a name="delete-a-batch-execution-job"></a>Brisanje grupe za izvršavanje posla

Kliknite da biste dodali postupka AzureML BES na API **postupak dodavanja** . Odaberite **Izbriši** za **HTTP glagolski**. Vrsta **/workspaces/ {radnog prostora} {servisa} /services/ /jobs/ {jobid} ?api verzija = {apiversion}** **URL predloška**. **Zaslonski naziv**upišite **BES brisanje** . Kliknite **Odgovori** > lijeve i odaberite**DODAJ** **200 u redu**. Kliknite **Spremi** da biste spremili ovaj postupak.

##<a name="call-an-operation-from-the-developer-portal"></a>Pozvati operaciju s portala za razvojne inženjere

Operacije naziva se izravno na portalu za razvojne inženjere, koji pruža jednostavan način za prikaz i testiranje operacije API. U ovom ćete koraku vodič podići metodu **RRS izvršavanje** koji je dodan **API AzureML pokazni videozapis**. Kliknite **portala za razvojne inženjere** na izborniku u gornjem desnom kutu klasični Portal.

![portala za razvojne inženjere](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Kliknite **API-ji** iz gornji izbornik, a zatim **API AzureML pokazni videozapis** da biste vidjeli dostupne operacije.

![demoazureml API-ja](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Odaberite **RRS izvođenje** operacije. Kliknite **isprobajte**.

![Pokušajte it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

Za parametre zahtjev, upišite vašeg **radnog prostora**, **servis**, **2.0** za **apiversion**i **true** **pojedinosti**. Nadzorna ploča službe za web AzureML možete pronaći **radnog prostora** i **usluge** (pogledajte **Test web-servisa** u dodatak A).

Zahtjev za zaglavlja, kliknite **Dodaj zaglavlje** i upišite **Vrsta sadržaja** i **aplikacije/json**, a zatim kliknite **Dodaj zaglavlje** i upišite **autorizacije** i **nošenja <YOUR AZUREML SERVICE API-KEY> **. Možete pronaći **ključ za api** na nadzornoj ploči AzureML web usluge (pogledajte **Test web-servisa** u dodatak A).

Vrsta **{"Unosa": {"input1": {"ColumnNames": ["st.2"], "vrijednosti": [["Ovo nazivamo dobar dan"]]}}, "GlobalParameters": {}}** za tijelo zahtjev.

![azureml api-pokazni videozapis](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Kliknite **Pošalji**.

![Pošalji](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

Kada se postupak poziva, portala za razvojne inženjere prikazuje **Tražena URL** iz servisa pozadinske, **status odgovora**, **odgovor zaglavlja**i bilo koji **sadržaj odgovor**.

![status odgovora](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

##<a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Dodatak A - stvaranja i testiranja jednostavne AzureML web-usluge

###<a name="creating-the-experiment"></a>Stvaranje pokusa

U nastavku su koraci za stvaranje jednostavne eksperiment AzureML i implementacija kao web-servisa. Na web servisa uzima kao stupac proizvoljne teksta za unos, a zatim vraća skup značajki koje su prikazane kao cijeli brojevi. Ako, na primjer:

Tekst | Hashed teksta
--- | ---
Ovo je dobar dan | 1 1 2 2 0 2 0 1

Najprije pomoću preglednika po izboru, dođite do: [https://studio.azureml.net/](https://studio.azureml.net/) i unesite vjerodajnice za prijavu. Zatim stvorite novi prazan eksperiment.

![pretraživanje eksperiment predložaka](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Preimenujte u **SimpleFeatureHashingExperiment**. Proširite **Spremili skupova podataka** i povucite **Recenzije adresara iz Amazon** na vašem eksperiment.

![jednostavno-značajka-raspršivanje-eksperiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Proširite i **Transformaciju podataka** te **rukovanje** i **Odabir stupaca u skupu podataka** povucite na eksperiment. Povezivanje **recenzije adresara iz Amazon** za **Odabir stupaca u skupu podataka**.

![Odabir stupaca](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Kliknite **Odabir stupaca u skupu podataka** , a zatim kliknite **pokretanje birač stupca** i odaberite **st.2**. Kliknite kvačicu da biste primijenili promjene.

![Odabir stupaca](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Proširite **Analize tekst** i povucite **Značajka raspršivanje** pokusa. Povezivanje **Odaberite stupce u skupu podataka** **značajke raspršivanje**.

![Povezivanje – projekta-stupaca](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Upišite **3** **Hashing bitsize**. To će stvoriti 8 (23) stupaca.

![Raspršivanje bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Sada možete **pokrenuti** da biste testirali pokusa.

![pokretanje](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

###<a name="create-a-web-service"></a>Stvaranje web-servisa

Sada stvorite web-servisa. Proširivanje **Web-servisa** i povucite **unosa** na vašem eksperiment. Povezivanje **unos** **raspršivanje značajke**. Povući i **Izlazni** vaše eksperiment. Povezivanje **Izlaz** **raspršivanje značajke**.

![Izlaz-na-značajka-raspršivanje](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Kliknite **Objavi web-servisa**.

![Objavljivanje – web-usluge](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Kliknite **da** da biste objavili pokusa.

![da-na-objavljivanje](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

###<a name="test-the-web-service"></a>Testiranje web-usluge

Na web-servisa AzureML sastoji se od RSS (zahtjeva i odgovora servisa) i krajnje točke BES (servis za obradu izvođenja). RSS je za sinkrono izvršavanje. BES je za izvršavanje asinkronog posao. Da biste testirali web-servisa s izvorom Python uzorka ispod, možda ćete morati preuzeti i instalirati Azure SDK za Python (pogledajte: [kako se instalira Python](../python-how-to-install.md)).

Također ćete **radnog prostora**, **usluge**i **api_key** vaše eksperimenta za izvor uzorka. Radni prostor i servisa možete pronaći tako da kliknete **Zahtjeva i odgovora** ili **Obradu izvođenja** za vaše eksperiment u nadzorna ploča službe za web.

![Pronalaženje radnog prostora – i – usluga](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

**Api_key** možete pronaći tako da kliknete vaše eksperiment u nadzorna ploča službe za web.

![Pronalaženje api ključa](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

####<a name="test-rrs-endpoint"></a>Krajnja točka RRS test

#####<a name="test-button"></a>Gumb za testiranje

Jednostavan način da biste testirali krajnju točku RRS je kliknite **testiranje** na nadzornoj ploči za servis za web.

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Upišite **Ovo je dobar dan** **st.2**. Kliknite kvačicu.

![unos podataka](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Vidjet ćete nešto poput

![Ogledna Izlaz](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

#####<a name="sample-code"></a>Ogledni kod

Drugi način za testiranje sustava RRS je iz koda klijenta. Ako kliknete **zahtjeva i odgovora** na nadzornoj ploči i pomaknite se do dna, vidjet ćete uzorak koda za C#, Python i R. Vidjet ćete i vidjet ćete da sintaksa zahtjeva za RRS, uključujući zahtjev za URI, zaglavlja i tijela.

Ovaj vodič prikazuje rad Python primjer. Morat ćete izmjena pomoću **radnog prostora**, **usluge**i **api_key** vaše eksperimenta.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

####<a name="test-bes-endpoint"></a>Krajnja točka BES test
Kliknite **grupe za izvršavanje** nadzorne ploče i pomaknite se do dna. Prikazat će se uzorak koda za C#, Python i R. Prikazat će se i vidjet ćete da sintaksa BES zahtjeva za slanje posla, pokretanje zadatka, dobiti status ili rezultate posla i brisanje posla.

Ovaj vodič prikazuje rad Python primjer. Ćete morati izmijeniti s **radnog prostora**, **servisa**i **api_key** vaše eksperimenta. Uz to, morate promijeniti **naziv računa za pohranu**, **ključ za račun za pohranu**i **naziv spremnika za pohranu**. Na kraju, morat ćete izmijeniti mjesto **ulazne datoteke** i mjesto **Izlazna datoteka**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
