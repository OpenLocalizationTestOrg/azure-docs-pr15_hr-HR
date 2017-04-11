<properties
    pageTitle="Dodavanje spremište blobova platforme Azure poveznika u aplikacijama za logiku | Microsoft Azure"
    description="Pregled blobova platforme Azure poveznik s parametrima REST API-JA"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="anneta"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-azure-blob-storage-connector"></a>Početak rada s poveznik za spremište blobova platforme Azure
Azure blobova je servis za pohranu velikih količina podataka nestrukturirane. Izvršite različite radnje kao što su prijenos, ažuriranje, dobiti i brisanje blob-ova u spremište blobova platforme Azure. 

S spremište blobova platforme Azure vam:

- Stvaranje tijekova rada prijenosom novih projekata ili početak datoteke koje ste nedavno ažurirane.
- Korištenje akcija da biste dobili metapodataka datoteku, izbrišite datoteke, kopiju datoteke i više. Ako, na primjer, alat ažuriranja programa Azure web-mjestu (okidač) pa ažurirajte datoteke u spremište blobova (akcije). 

U ovoj se temi objašnjava konektora blob prostora za pohranu u aplikaciji logike i i popise akcije.

>[AZURE.NOTE] Ovu verziju članka primjenjuje se na logike aplikacije Općenito dostupan (GA). 

Da biste saznali više o aplikacijama logike, potražite u članku [što su logike aplikacije](../app-service-logic/app-service-logic-what-are-logic-apps.md) i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Povezivanje sa spremištem blobova platforme Azure

Logika aplikacije možete pristupiti bilo koji servis, prvo stvorite *vezu* sa servisom. Veze navedene veze između logike aplikacije i drugih servisa. Ako, na primjer, da biste se povezali s računom za pohranu, stvaranja prostora za pohranu blob *veze*. Da biste stvorili vezu, unesite vjerodajnice obično koristite za pristup servisu se povezujete s. Pa Azure prostor za pohranu, unesite vjerodajnice na račun servisa za pohranu za stvaranje veze. 

#### <a name="create-the-connection"></a>Stvaranje veze

>[AZURE.INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]
 
## <a name="use-a-trigger"></a>Koristite okidač

Ovaj poveznik ne sadrži sve okidača. Pomoću drugih okidača da biste pokrenuli aplikaciju logike, kao što su okidač ponavljanja, okidača HTTP Webhook, a zatim okidača dostupno u sklopu drugih poveznika i drugo. [Stvaranje aplikacije za logiku](../app-service-logic/app-service-logic-create-a-logic-app.md) navedeni primjer.

## <a name="use-an-action"></a>Koristite akciju
    
Akciju je postupak provodi definirano u aplikaciji logike tijeka rada.

1. Odaberite znak plus. Pogledajte nekoliko mogućnosti: **Dodaj akciju**, **Dodaj uvjet**ili neke **Dodatne** mogućnosti.

    ![](./media/connectors-create-api-azureblobstorage/add-action.png)

2. Odaberite **Dodaj akciju**.

3. U tekstni okvir upišite "bloba" da biste dobili popis dostupnih akcija.

    ![](./media/connectors-create-api-azureblobstorage/actions.png) 

4. U našem primjeru odaberite **AzureBlob - dobiju metapodatke datoteke pomoću puta**. Ako se veza već postoji, zatim odaberite **...** Gumb (Prikaži birača) da biste odabrali datoteku.

    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)

    Ako su za podatke o vezi od vas zatraži, unesite detalje da biste stvorili vezu. [Stvaranje veze](connectors-create-api-azureblobstorage.md#create-the-connection) u ovoj se temi opisuju tih svojstava. 

    > [AZURE.NOTE] U ovom primjeru ćemo dobiti metapodataka datoteke. Da biste vidjeli metapodatke, dodajte drugih akcija stvara novu datoteku pomoću drugog poveznik. Na primjer, dodajte akciju OneDrive koji stvara novu "testiranje" datoteku na temelju metapodataka. 

5. **Spremite** promjene (gornjem lijevom kutu na alatnoj traci). Pokrenite aplikaciju logike se sprema i možda je automatski omogućena.

> [AZURE.TIP] [Prostor za pohranu](http://storageexplorer.com/) je odličan alat za upravljanje računima više prostora za pohranu.

## <a name="technical-details"></a>Tehničke pojedinosti

## <a name="storage-blob-actions"></a>Spremište blobova platforme akcije

|Akcija|Opis|
|--- | ---|
|[Dohvaćanje datoteka metapodataka](connectors-create-api-azureblobstorage.md#get-file-metadata)|Ovaj postupak može vidjeti metapodataka datoteke pomoću id datoteke.|
|[Ažuriranje datoteka](connectors-create-api-azureblobstorage.md#update-file)|Ovaj postupak ažurira datoteku.|
|[Brisanje datoteke](connectors-create-api-azureblobstorage.md#delete-file)|Ovaj postupak briše datoteke.|
|[Dohvaćanje datoteka metapodataka pomoću puta](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path)|Ovaj postupak može vidjeti metapodataka datoteke pomoću put.|
|[Sadržaj datoteke pomoću puta](connectors-create-api-azureblobstorage.md#get-file-content-using-path)|Ovaj postupak može vidjeti sadržaj datoteke pomoću put.|
|[Sadržaj datoteke](connectors-create-api-azureblobstorage.md#get-file-content)|Ovaj postupak može vidjeti sadržaj datoteke pomoću ID-a.|
|[Stvaranje datoteke](connectors-create-api-azureblobstorage.md#create-file)|Ovaj postupak prenosite datoteke.|
|[Kopiranje datoteke](connectors-create-api-azureblobstorage.md#copy-file)|Ovaj postupak kopira datoteke spremište blobova platforme Azure.|
|[Izdvajanje Arhivske mape](connectors-create-api-azureblobstorage.md#extract-archive-to-folder)|Ovaj postupak izvlači arhivsku datoteku u mapu (primjer: .zip).|

### <a name="action-details"></a>Detalji o akcija

U ovom ćete odjeljku potražite u članku određene detalje o svaku akciju, uključujući obavezan ili nije unos svojstva i sve odgovarajuće izlaz pridružene poveznik.

#### <a name="get-file-metadata"></a>Dohvaćanje datoteka metapodataka
Ovaj postupak može vidjeti metapodataka datoteke pomoću id datoteke.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|ID *|Datoteka|Odaberite datoteku|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
BlobMetadata

| Naziv svojstva | Vrsta podataka |
|---|---|
|ID-a|niz|
|Ime|niz|
|Riješiti problem|niz|
|Put|niz|
|LastModified|niz|
|Veličina|cijeli broj|
|Vrste medija|niz|
|IsFolder|Booleove vrijednosti|
|E-oznake|niz|
|FileLocator|niz|


#### <a name="update-file"></a>Ažuriranje datoteka
Ovaj postupak ažurira datoteku.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|ID *|Datoteka|Odaberite datoteku|
|tijelo *|Sadržaj datoteke|Sadržaj datoteke da biste ažurirali|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
BlobMetadata

| Naziv svojstva | Vrsta podataka |
|---|---|
|ID-a|niz|
|Ime|niz|
|Riješiti problem|niz|
|Put|niz|
|LastModified|niz|
|Veličina|cijeli broj|
|Vrste medija|niz|
|IsFolder|Booleove vrijednosti|
|E-oznake|niz|
|FileLocator|niz|


#### <a name="delete-file"></a>Brisanje datoteke
Ovaj postupak briše datoteke.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|ID *|Datoteka|Odaberite datoteku|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="get-file-metadata-using-path"></a>Dohvaćanje datoteka metapodataka pomoću puta
Ovaj postupak može vidjeti metapodataka datoteke pomoću put.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|put *|Put datoteke|Odaberite datoteku|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
BlobMetadata

| Naziv svojstva | Vrsta podataka |
|---|---|
|ID-a|niz|
|Ime|niz|
|Riješiti problem|niz|
|Put|niz|
|LastModified|niz|
|Veličina|cijeli broj|
|Vrste medija|niz|
|IsFolder|Booleove vrijednosti|
|E-oznake|niz|
|FileLocator|niz|


#### <a name="get-file-content-using-path"></a>Sadržaj datoteke pomoću puta
Ovaj postupak može vidjeti sadržaj datoteke pomoću put.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|put *|Put datoteke|Odaberite datoteku|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="get-file-content"></a>Sadržaj datoteke
Ovaj postupak može vidjeti sadržaj datoteke pomoću ID-a.  

|Naziv svojstva| Vrsta podataka|Opis|
| ---|---|---|
|ID *|niz|Odaberite datoteku|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
Ništa.


#### <a name="create-file"></a>Stvaranje datoteke
Ovaj postupak prenosite datoteke.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|folderPath *|Put do mape|Odaberite mapu|
|Naziv *|Naziv datoteke|Naziv datoteke za prijenos|
|tijelo *|Sadržaj datoteke|Sadržaj datoteke za prijenos|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
BlobMetadata

| Naziv svojstva | Vrsta podataka | 
|---|---|
|ID-a|niz|
|Ime|niz|
|Riješiti problem|niz|
|Put|niz|
|LastModified|niz|
|Veličina|cijeli broj|
|Vrste medija|niz|
|IsFolder|Booleove vrijednosti|
|E-oznake|niz|
|FileLocator|niz|


#### <a name="copy-file"></a>Kopiranje datoteke
Ovaj postupak kopira datoteke spremište blobova platforme Azure.  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Izvor *|Izvorišni url|Određivanje URL-a izvornu datoteku|
|odredište *|Put datoteke odredište|Odredite put odredište datoteke, uključujući ciljnog naziva datoteke|
|Želite li izbrisati|Želite li izbrisati?|Trebali biste postojeće odredišnu datoteku prebrisati (true i false)?  |

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
BlobMetadata

| Naziv svojstva | Vrsta podataka |
|---|---|
|ID-a|niz|
|Ime|niz|
|Riješiti problem|niz|
|Put|niz|
|LastModified|niz|
|Veličina|cijeli broj|
|Vrste medija|niz|
|IsFolder|Booleove vrijednosti|
|E-oznake|niz|
|FileLocator|niz|

#### <a name="extract-archive-to-folder"></a>Izdvajanje Arhivske mape
Ovaj postupak izvlači arhivsku datoteku u mapu (primjer: .zip).  

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Izvor *|Put datoteke za arhiviranje izvora|Odaberite arhivsku datoteku|
|odredište *|Put do odredišne mape|Odabir sadržaja za izdvajanje|
|Želite li izbrisati|Želite li izbrisati?|Trebali biste postojeće odredišnu datoteku prebrisati (true i false)?|

Zvjezdicu (*), znači da je svojstvo obavezno.

##### <a name="output-details"></a>Detalji o Izlaz
BlobMetadata

| Naziv svojstva | Vrsta podataka |
|---|---|
|ID-a|niz|
|Ime|niz|
|Riješiti problem|niz|
|Put|niz|
|LastModified|niz|
|Veličina|cijeli broj|
|Vrste medija|niz|
|IsFolder|Booleove vrijednosti|
|E-oznake|niz|
|FileLocator|niz|


## <a name="http-responses"></a>HTTP odgovora

Kada se upućivanje poziva za različite akcije, mogla bi vam se određene odgovore. Sljedeća tablica prikazuje odgovore i njihovi opisi:  

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvatili|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do nepoznate pogreške|
|Zadani|Nije uspjelo.|

## <a name="next-steps"></a>Daljnji koraci

[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Istražite ostale dostupne poveznika u logiku aplikacija u našem [popisu API-ji](apis-list.md).



