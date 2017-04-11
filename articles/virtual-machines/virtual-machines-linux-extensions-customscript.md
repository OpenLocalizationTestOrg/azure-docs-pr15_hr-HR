<properties
   pageTitle="Prilagođene skripte na Linux VMs | Microsoft Azure"
   description="Automatiziranje zadataka konfiguracije Linux VM pomoću proširenja za prilagođene skripte"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Proširenje Azure prilagođene skripte pomoću Linux virtualnim strojevima

Proširenje za prilagođene skripte preuzimanja i izvršava skripte na Azure virtualnih računala. Ovaj kućni broj je korisno za konfiguraciju implementacije objavu, instalacija softvera ili druge konfiguracije / zadatak upravljanja. Skripte možete preuzeti s Azure prostora za pohranu ili druge pristupačnih internetsko mjesto ili za nastavak vrijeme izvođenja. Proširenje za prilagođene skripte integrira s predlošcima Voditelj resursa Azure i može se pokrenuti i pomoću EŽA Azure, PowerShell, Azure portal ili Azure virtualnog računala REST API-JA.

Ovaj dokument detalje o kako koristiti naziv prilagođene skripte nastavka iz EŽA Azure i predložak Azure Voditelj resursa, a i detalje o korake za otklanjanje poteškoća u sustavima Linux.

## <a name="extension-configuration"></a>Proširenje konfiguracija

Konfiguriranje prilagođene skripte proširenje određuje elemente kao što su skripte mjesto i naredbu za pokretanje. Tu konfiguraciju moguće pohraniti u konfiguracijske datoteke naveden u naredbenom retku ili u predlošku Azure Voditelj resursa. Povjerljive podatke može se spremiti u zaštićenom konfiguraciji koja je šifrirana i samo dešifrirati unutar virtualnog računala. Zaštićeni konfiguracije je korisno kada naredbu izvođenja sadrži tajne kao što su lozinke.

### <a name="public-configuration"></a>Javni konfiguracija

Sheme:

- **commandToExecute**: (obavezan, niz) skripte točke unosa za izvođenje
- **fileUris**: (nije obavezno, polje niza) URL-ovi za datoteke za preuzimanje.
- **Vremenska oznaka** (nije obavezno, cijeli broj) polje koristiti samo za pokretanje ponovno pokrenite skripte tako da promijenite vrijednost tog polja.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Zaštićeni konfiguracija

Sheme:

- **commandToExecute**: (nije obavezno, niz) skripte točke unosa za izvršavanje. Ako naredba sadrži tajne kao što su lozinke, umjesto toga koristite to polje.
- **storageAccountName**: (nije obavezno, niz) naziv računa za pohranu. Ako navedete vjerodajnice za pohranu, sve fileUris mora biti URL-ova za Azure blob-ova.
- **storageAccountKey**: (nije obavezno, niz) tipka za pristup računu za pohranu.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure EŽA

Prilikom korištenja EŽA Azure pokretanje prilagođene skripte datotečni nastavak, stvorite konfiguracijska datoteka ili datoteke koja sadrži barem uri datoteke i izvođenja naredbu skripte.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Po želji možete se naredba izvršiti pomoću na `--public-config` i `--private-config` mogućnost koja omogućuje konfiguraciju da bude navedena tijekom izvođenja i bez zasebnom konfiguracije datoteke.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Primjeri Azure EŽA

**Primjer 1** – javno konfiguraciju skriptna datoteka.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Naredba Azure EŽA:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Primjer 2** – javno konfiguraciju bez skriptna datoteka.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Naredba Azure EŽA:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Primjer 3** – javno konfiguracije datoteke koja se koristi za određivanje skriptna datoteka URI i datoteke zaštićene konfiguracije se koristi za određivanje naredbu koju želite izvršiti.

Javni konfiguracijska datoteka:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Zaštićeni konfiguracijska datoteka:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Naredba Azure EŽA:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Voditelj resursa predloška

Proširenje Azure prilagođene skripte pokretati trenutku implementacije virtualnog računala pomoću predloška Voditelj resursa. Da biste to učinili, dodajte ispravno oblikovane JSON predložak implementacije.

### <a name="resource-manager-examples"></a>Primjeri Voditelj resursa

**Primjer 1** – javno konfiguracije.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Primjer 2** – Izvršavanje naredbe u zaštićenom konfiguracije.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

U odjeljku .net Core glazbu spremište dijaloški okvir svojstva za dovršavanje primjer – [Pokazni videozapis glazbu iz trgovine](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Otklanjanje poteškoća

Kada se pokrene proširenja za prilagođene skripte, skripta je stvorili ili preuzeli u direktorij slično kao u sljedećem primjeru. Naredba izlazne i sprema se u taj imenik u `stdout` i `stderr` datoteku.

```none
/var/lib/azure/custom-script/download/0/
```

Proširenje skripte Azure stvara zapisnik, koji se nalazi se ovdje.

```none
/var/log/azure/customscript/handler.log
```

Stanje izvođenja proširenja za prilagođene skripte je moguće dohvatiti i s EŽA Azure.

```none
azure vm extension get <resource-group> <vm-name>
```

Rezultat izgleda kao sljedeći tekst:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Daljnji koraci

Informacije o drugim datotečnim nastavcima skripte VM potražite u članku [Pregled Azure skripte proširenje Linux](./virtual-machines-linux-extensions-features.md).