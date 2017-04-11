Pomoću upravitelja Azure resursa, definiranje parametara za vrijednosti koje želite da biste odredili kada je implementiran u predložak. Predložak uključuje nazivaju parametri sekciju koja sadrži sve vrijednosti parametra.
Morate definirati parametar za te vrijednosti koje će se razlikovati ovisno o projektu implementirate ili ovise o okruženju implementacije da biste. Definiranje parametara za vrijednosti koje se uvijek će ostati isto. Svaku vrijednost parametra se koristi u predlošku da biste definirali resurse koji su raspoređeni. 

Pri definiranju parametara polje **allowedValues** koristiti da biste odredili vrijednosti koje korisnik pružaju tijekom implementacije. Polje **defaultValue** koristiti se dodjeljuje vrijednost za parametar, ako je vrijednost tijekom implementacije.

Ne možemo opisane su parametra u predlošku.

### <a name="sitename"></a>NazivWebMjesta

Naziv web-aplikacije koje želite stvoriti.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

Naziv aplikacije servisa za namjeravate koristiti za hostiranje web-aplikaciji.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU

Cijene sloju hostinga plana.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Predložak se definira vrijednosti koje su dopuštene taj parametar, a dodjeljuje zadana vrijednost (S1) ako je naveden nijedna vrijednost.

### <a name="workersize"></a>workerSize

Veličina instancu hostinga plan (mala, Srednje ili velike).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Predložak se definira vrijednosti koje su dopuštene taj parametar (0, 1 ili 2), a dodjeljuje zadana vrijednost (0) ako je naveden nijedna vrijednost. Vrijednosti koje odgovaraju mali, Srednje i velike.
