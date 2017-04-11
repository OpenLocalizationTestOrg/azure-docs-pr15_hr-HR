### <a name="compression-support"></a>Podrška za spajanje  
Obrada velikih skupova podataka može uzrokovati grla/i i mreže. Stoga sažete podatke u trgovine možete ne samo ubrzavanje prijenos podataka putem mreže i Štednja prostora na disku, ali premjestiti i poboljšanja performansi za značajan u obrada velikih skupova podataka. Trenutno sažimanja nije podržana za služi za pohranu datoteka na temelju podataka kao što su blobova platforme Azure i lokalnih datotečnom sustavu.  

> [AZURE.NOTE] Postavke kompresije nisu podržani za podatke u **AvroFormat**, **OrcFormat**ili **ParquetFormat**. 

Da biste odredili sažimanja za skup podataka, koristiti ovo svojstvo **sažimanja** u skupu podataka JSON kao u sljedećem primjeru:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
**Sažimanje** sekciji sastoji se od dva svojstva:  
  
- **Vrsta:** sažimanja kodek, što može biti **GZIP**, **Deflate** ili **BZIP2**.  
- **Razinu:** omjer spajanja, što može biti **Optimal** ili **najbrže**. 
    - **Najbrže:** Operacija spajanja provedite treba brzo kao što je to moguće, čak i ako rezultat ne optimalnog komprimiranja datoteke. 
    - **Optimal**: operaciju spajanja mora biti optimalnog spojene, čak i ako se postupak traje dulje da biste dovršili. 
    
    Dodatne informacije potražite u članku [Razinu kompresije](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) temu. 

Pretpostavimo da iznad uzorka skupu podataka koristi se kao rezultat Kopiraj aktivnosti aktivnosti Kopiraj komprimira za izlazne podatke s GZIP kodek pomoću optimalnih omjer i zatim zapisati sažete podatke u datoteku pod nazivom pagecounts.csv.gz u spremište blobova platforme Azure.   

Kada odredite svojstvo sažimanja u unos dataset JSON, kanal možete čitati sažete podatke iz izvora i navedite svojstvo u izlaz dataset JSON, aktivnosti kopiju možete napisati sažete podatke na odredište. Evo nekoliko oglednih scenarija: 

- Čitanje GZIP sažete podatke iz Azure blobova platforme dekomprimiranje ga, a zapisivanje podataka rezultata s bazom podataka Azure SQL. U ovom slučaju definirati unos dataset blobova platforme Azure s spajanja JSON svojstvo. 
- Čitanje podataka iz datoteke običnog teksta s lokalnim datotečnog sustava, sažmite je pomoću GZip oblika i sažeti podaci za pisanje blobova platforme Azure. U ovom slučaju definirati programa izlaz blobova platforme Azure skup slogova pomoću spajanja JSON svojstvo.  
- Čitanje podataka GZIP spojene blobova platforme Azure, dekomprimiranje ga, sažmite je pomoću BZIP2 i zapisivanje podataka rezultat blobova platforme Azure. Definiranje unos dataset blobova platforme Azure s vrstom sažimanja postavite GZIP i izlazni skup podataka s vrstom sažimanja u tom slučaju postavljena na BZIP2.   
