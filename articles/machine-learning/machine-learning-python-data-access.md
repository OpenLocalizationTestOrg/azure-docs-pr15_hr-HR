<properties 
    pageTitle="Pristup skupove podataka s bibliotekom klijentskog računala učenje Python | Microsoft Azure" 
    description="Instalirajte i koristite biblioteku Python klijenta za pristup i upravljanje podacima Azure strojnog učenja sigurno iz lokalnog okruženja Python." 
    services="machine-learning" 
    documentationCenter="python" 
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
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Skupove podataka programa Access s Python pomoću Azure strojnog učenja Python klijentska biblioteka 

Pretpregled Microsoft Azure strojnog učenja Python klijentska biblioteka omogućuje siguran pristup vašem Azure strojnog učenja skupove podataka iz lokalnog okruženja Python te omogućuje stvaranje i upravljanje skupova podataka u radnom prostoru.

Ova tema sadrži upute o tome kako:

* Instalacija strojnog učenja Python klijentska biblioteka 
* pristup i prijenos skupova podataka, uključujući upute za početak autorizacije pristup Azure strojnog učenja skupove podataka s lokalnom okruženju Python
*  pristup Srednja skupove podataka iz eksperimenata
*  pomoću biblioteke klijent Python Nabrajanje skupova podataka, pristup metapodataka, pročitajte sadržaj skup podataka, stvoriti novi skupova podataka i ažuriranje postojećih skupova podataka

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Preduvjeti

Klijentska biblioteka Python testirano u odjeljku okruženja za sljedeće:

 - Windows, Mac i Linux
 - Python 2.7 3,3 i 3.4

To je u opsegu paketi za sljedeće:

 - Zahtjevi za
 - Python dateutil
 - pandas

Preporučujemo korištenje raspodjele Python kao što su navedene [Anaconda](http://continuum.io/downloads#all) ili [Canopy](https://store.enthought.com/downloads/), koji se isporučuju uz Python, IPython i tri paketi instalirani. Iako isključivo potreban je IPython, je odličan okruženja za rukovanje i interaktivno vizualizacija podataka.


###<a name="installation"></a>Kako instalirati Azure strojnog učenja Python klijentska biblioteka

Klijentska biblioteka Azure strojnog učenja Python mora biti instaliran i da biste dovršili zadatke navedene u ovoj temi. Nije dostupan iz [Python paketa indeksa](https://pypi.python.org/pypi/azureml). Da biste instalirali u svom okruženju Python, pokrenite sljedeću naredbu iz vaše lokalne Python okruženja:

    pip install azureml

Osim toga, možete preuzeti i instalirati s izvora na [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Ako imate brojka instaliran na vašem računalu, možete koristiti točaka da biste instalirali izravno iz spremišta brojka:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Pristup skupove podataka pomoću koda Studio

Klijentska biblioteka Python omogućuje vam programski pristup vaše postojeće skupove podataka iz eksperimenata koje ste pokrenuta.

Iz web-sučelja Studio možete generirati koda koji sadrže sve potrebne informacije da biste preuzeli i ukloniti serijski broj skupova podataka kao Pandas DataFrame objekti na mjesto na računalu.

### <a name="security"></a>Sigurnost za pristup podacima

Koda nudi Studio za korištenje s bibliotekom Python klijent obuhvaća radni prostor id i autorizacije tokena. Te navedite puni pristup radnog prostora i mora biti zaštićene, kao što su lozinke.

Sigurnosnih vam razloga funkciju isječak koda dostupna samo korisnicima koji imaju njihove uloge postaviti kao **vlasnik** radnog prostora. Vaša uloga se prikazuje u Azure strojnog učenja Studio na stranici **KORISNICI** u odjeljku **Postavke**.

![Sigurnost][security]

Ako vaša uloga nije postavljena kao **vlasnik**, možete ga ili zahtjev za ponovnim pozivanjem vlasnik ili zatražite od vlasnika radnog prostora da vam pruži isječak koda.

Da biste nabavili u ovlaštenja, možete učiniti nešto od sljedećeg:



- Zatražite token od vlasnika. Vlasnici možete pristupiti njihove tokeni autorizacije sa stranice Postavke njihove radnog prostora u Studio. U lijevom oknu odaberite **Postavke** , a zatim kliknite **AUTORIZACIJE TOKENI** da biste vidjeli tokeni primarnih i sekundarnih.  Iako primarni ili sekundarni autorizacije tokeni mogu se koristiti u isječak koda, preporučuje se vlasnici dijeljene i tokeni sekundarne autorizacije.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Dobiti ulogu uloga vlasnika zatražite od.  Da biste to učinili, trenutni vlasnik radnog prostora mora najprije uklanjanje radni prostor, zatim ponovno pozvati koje je vlasnik.

Kada razvojnim inženjerima ste dobili radni prostor id i autorizacije tokena, oni će moći pristupati radni prostor putem koda bez obzira na to njihove uloge.

Tokeni autorizacije upravlja se na stranici **TOKENI AUTORIZACIJE** u odjeljku **Postavke**. Ih možete obnovi, no taj postupak opoziva pristup prethodne token.

### <a name="accessingDatasets"></a>Skupove podataka programa Access iz lokalne Python aplikacije

1. U strojnog učenja Studio kliknite **skupova podataka** na navigacijskoj traci na lijevoj strani.

2. Odaberite skup podataka kojima želite omogućiti pristup. U skupove podataka možete odabrati na popisu **Moji skupove podataka** ili s popisa **UZORKA** .

3. Na donjoj alatnoj traci kliknite **Generiraj podataka pristupnu šifru**. Ako su podaci u obliku koji je kompatibilan s bibliotekom Python klijent, taj gumb je onemogućen.

    ![Skupove podataka][datasets]

4. Odaberite isječak koda u prozoru koji će se prikazati i kopiranje u međuspremnik.

    ![Pristupni kod][dataset-access-code]

5. Zalijepite kod u bilježnici vaše lokalne Python aplikacije.

    ![Bilježnice][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Pristup Srednja skupove podataka iz strojnog učenja eksperimenata

Nakon pokusa se izvodi u učenje Studio računalu, moguće je da biste pristupili Srednja skupove podataka iz izlaza čvorove module. Srednji skupove podataka su podataka koji je stvoren i koristi za Srednja korake kada je pokrenuta alat za model.

Srednji skupove podataka može pristupiti dok god oblik podataka kompatibilan s bibliotekom Python klijenta.

Podržani su sljedeći oblici (konstante za te se nalazite u `azureml.DataTypeIds` klase):

 - Običan tekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

Oblik možete utvrditi tako iznad čvor modul izlaz. Prikazuje se uz naziv čvor u zaslonskom opisu.

Neke od module, kao što su [podijeljene] [ split] modula izlazni oblik naziva `Dataset`, koji ne podržava Python klijentska biblioteka.

![Oblik za skup podataka][dataset-format]

Morate koristiti modul za pretvorbu, kao što je [pretvoriti u CSV][convert-to-csv], da biste na Izlaz podržani oblik zapisa.

![Oblikovanje GenericCSV][csv-format]

Na sljedeći način prikažite primjer u kojem se stvara pokusa, pokreće se i pristupa Srednja skupu podataka.

1. Stvorite novi eksperiment.

2. Umetnite modul za **odrasle statistiku dobit binarni klasifikacija skup podataka** .

3. Umetanje [Podijeli] [ split] modula, te je povežite njegov ulaz modul izlazni skup podataka.

4. Umetanje je [pretvoriti u CSV] [ convert-to-csv] modul i povezivanje njegov unos u neku od [Podijeli] [ split] modul proizvodi.

5. Spremanje pokusa, pokrenite ga i pričekajte da se završi izvođenje.

6. Kliknite čvor Izlaz na [pretvorili CSV] [ convert-to-csv] modul.

7. Kada se pojavi na kontekstnom izborniku odaberite **Generiraj podataka pristupnu šifru**.

    ![Kontekstni izbornik][experiment]

8. Odaberite isječak koda i kopiranje u međuspremnik u prozoru koji će se prikazati.

    ![Pristupni kod][intermediate-dataset-access-code]

9. Zalijepite kod u bilježnici.

    ![Bilježnice][ipython-intermediate-dataset]

10. Možete vizualni prikaz podataka pomoću matplotlib. Prikazat će se u histogramu stupca dob:

    ![Histogram][ipython-histogram]


##<a name="clientApis"></a>Pomoću biblioteke klijent strojnog učenja Python možete pristupiti, čitanje, stvaranje i upravljanje skupova podataka

### <a name="workspace"></a>Radni prostor

Radni prostor je ulaza za biblioteku Python klijenta. Navedite na `Workspace` klase s ID-a radnog prostora i autorizacije token da biste stvorili instance:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Nabrajanje skupova podataka

Da biste numeriranje sve skupove podataka u radnom prostoru za dani:

    for ds in ws.datasets:
        print(ds.name)

Da biste numeriranje samo na stvorili kao korisnik skupove podataka:

    for ds in ws.user_datasets:
        print(ds.name)

Da biste numeriranje samo na primjer skupove podataka:

    for ds in ws.example_datasets:
        print(ds.name)

Skup podataka možete pristupiti s nazivom (što je velika i mala slova):

    ds = ws.datasets['my dataset name']

Ili, možete joj pristupiti pomoću indeksa:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metapodataka

Skupove podataka sadrži metapodatke, uz sadržaj. (Srednji skupove podataka su iznimke to pravilo i nemaju sve metapodatke)

Neke vrijednosti metapodataka dodjeljuju se korisnik u vrijeme stvaranja:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Ostali su vrijednosti dodijelio Azure ML:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Pogledajte na `SourceDataset` predmete dodatne informacije o dostupna metapodataka.


### <a name="read-contents"></a>Čitanje sadržaja

Koda automatski nudi strojnog učenja Studio preuzmite i ukloniti serijski broj dataset Pandas DataFrame objekt. To možete učiniti s na `to_dataframe` metoda:

    frame = ds.to_dataframe()

Ako biste radije da biste preuzeli sirovim podacima i izvesti na deserijalizacija, to je mogućnost. U trenutku, to je samo mogućnost za oblike kao što su "ARFF", koji se ne može ukloniti serijski broj u biblioteku Python klijenta.

Da biste pročitali sadržaj kao tekst:

    text_data = ds.read_as_text()

Da biste pročitali sadržaj kao binarni:

    binary_data = ds.read_as_binary()

Možete otvoriti samo strujanje sadržaj:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Stvaranje novog skupa podataka

Klijentska biblioteka Python omogućuje prijenos skupove podataka iz programa Python. Ove skupove podataka pa su dostupni za upotrebu u radnom prostoru.

Ako imate podatke u Pandas DataFrame, koristite sljedeći kod:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Ako vaši podaci već serijalizirani, možete koristiti:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Biblioteka klijentski Python je moći serijalizirati Pandas DataFrame za sljedeće oblike (konstante za to su u na `azureml.DataTypeIds` klase):

 - Običan tekst
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Ažuriranje postojeći skup podataka

Ako pokušate učitati novi skup podataka s nazivom koji odgovara postojeći skup podataka, trebali biste dobiti sukoba pogreške.

Da biste ažurirali postojeći skup podataka, najprije morate upućivanje na postojeći skup podataka:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Zatim se poslužite `update_from_dataframe` Serijalizacija i zamijeniti sadržaj skupa podataka na Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Ako želite serijalizirati podataka u drugi oblik, odrediti vrijednost za neobavezna `data_type_id` parametar.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Novi opis po želji možete postaviti tako da navedete vrijednost u `description` parametar.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Po želji možete postaviti novi naziv tako da navedete vrijednost u `name` parametar. Od tog trenutka, ćete dohvatiti skup podataka pomoću samo novi naziv. Sljedeći kod ažuriranja podataka, naziv i opis.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

U `data_type_id`, `name` i `description` parametara nisu obavezni i prema zadanim postavkama njihove prethodne vrijednosti. Na `dataframe` potreban je parametar uvijek.

Ako vaši podaci već serijalizirani, koristite `update_from_raw_data` umjesto `update_from_dataframe`. Ako u proći `raw_data` umjesto `dataframe`, funkcionira na isti način.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
