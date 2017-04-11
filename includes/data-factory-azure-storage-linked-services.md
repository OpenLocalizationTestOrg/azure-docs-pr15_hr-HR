## <a name="azure-storage-linked-service"></a>Azure prostora za pohranu povezane usluge

**Prostor za pohranu Azure povezana servisa** omogućuje povezivanje poslovnog subjekta Azure prostora za pohranu na tvorničke Azure podataka korištenjem **ključ za račun**. To omogućuje tvorničke podataka globalni pristup Azure za pohranu. Sljedeća tablica sadrži opis elemenata JSON specifične za servis za pohranu Azure povezani.

| Svojstvo | Opis | Obavezno |
| :-------- | :----------- | :-------- |
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **AzureStorage** | Da |
| connectionString | Odredite podatke koji su potrebni za povezivanje s Azure prostora za pohranu za svojstvo connectionString. | Da |

U članku sljedeće korake za prikaz/Kopiraj ključ za račun za pohranu u Azure: [Prikaz, Kopiraj i regenerate prostora za pohranu pristupnih tipki](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Primjer:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure prostora za pohranu Sas povezana servisa  
Zajednički pristup potpis (SAS) omogućuje prijenos ovlasti pristup resursa u račun za pohranu. To znači da možete dodijeliti klijent ograničene dozvole za objekte na vašem računu za pohranu za određeno vrijeme i određeni skup dozvola, bez potrebe za zajedničko korištenje ključeva za pristup računu. Na SAS je URI koji obuhvaća u parametara upita autentičnost sve informacije potrebne za pristup resursu za pohranu. Da biste pristupili resursi za pohranu na SAS, klijent samo treba prenesite na SAS odgovarajući Graditelj ili način. Detaljne informacije o SAS potražite u članku [zajednički pristup potpisi: osnove modela SAS](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
Servis za Azure prostora za pohranu SAS povezane omogućuje povezivanje poslovnog subjekta Azure prostora za pohranu na tvorničke Azure podataka pomoću zajednički pristup potpis (SAS). To tvorničke podataka omogućuje pristup ograničene/vrijeme-granica sve/određene resursa (blob na spremnik) u prostora za pohranu. Sljedeća tablica sadrži opis JSON elementi specifične za servisa Azure prostora za pohranu SAS povezani. 

| Svojstvo | Opis | Obavezno |
| :-------- | :----------- | :-------- |
| Vrsta | Svojstvo vrsta mora biti postavljeno na: **AzureStorageSas**  | Da |
| sasUri | Odredite URI potpis za pristup zajedničko korištenje resursa za pohranu Azure kao što su blob, spremnik ili tablicu. Pogledajte bilješke niže detalje. | Da | 


**Primjer:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Prilikom stvaranja **SAS URI**, preporučuje se sljedeće:  

- Azure tvorničke podataka podržava samo **SAS servisa**, ne SAS računa. Detalje o tih dviju vrsta potražite u članku [Vrste od zajednički pristup potpisa](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) .
- Odgovarajuće **dozvole** moraju biti postavljena na objekte koji se temelji na kako povezane usluge (čitanje, pisanje, čitanje/pisanje) koristit će se u vašem tvorničke podataka.
- **Vrijeme isteka** mora biti postavljena na odgovarajući način. Provjerite je li pristup za pohranu Azure objekata ne istječu unutar aktivni razdoblja kanal.
- URI treba kreirati na razini tablice na temelju potreba ili desni spremnik/blob. SAS Uri za blobova platforme Azure omogućuje tvorničke podataka servisa za pristup tom određeni blob. SAS Uri u spremniku blobova platforme Azure omogućuje servis podataka tvorničke iteracija kroz blob-ova u njih. Ako je potrebno omogućiti pristup više manje objekata kasnije ili ažurirati SAS URI Imajte na umu da biste ažurirali servis povezane s novi URI.   
