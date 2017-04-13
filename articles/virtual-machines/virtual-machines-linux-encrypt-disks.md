<properties
   pageTitle="Šifriranje diskova na Linux VM | Microsoft Azure"
   description="Kako šifrirati diskova na Linux VM pomoću EŽA Azure i model implementacije Voditelj resursa"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Šifriranje diskova na Linux VM pomoću EŽA Azure
Poboljšana virtualnog računala (VM) sigurnost i usklađenost, virtualne diskova Azure možete šifriraju na ostale. Diskova šifrirane pomoću šifriranja ključeva koji su zaštićeni u programa Azure ključ sigurnog. Kontrola ključeve šifriranja i možete nadzirati njihova upotreba. U ovom se članku detaljno kako šifrirati virtualne diskova na Linux VM pomoću EŽA Azure i model implementacije Voditelj resursa.


## <a name="quick-commands"></a>Brzi naredbe
Ako morate brzo obaviti zadatak, sljedeće sekcije logaritma naredbe za šifriranje virtualne diskova na vašem VM. Detaljnije informacije i kontekst svakog koraka možete pronaći ostatku dokumenta, [počevši ovdje](#overview-of-disk-encryption).

Potreban vam je [Najnovija Azure EŽA](../xplat-cli-install.md) instaliran i prijavljeni pomoću MOD upravitelja resursa na sljedeći način:

```
azure config mode arm
```

U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjeru sadrže `myResourceGroup`, `myKeyVault`, i `myVM`.

Prvo, omogućivanje sigurnog ključ Azure davatelja unutar pretplate Azure i stvorite grupu resursa. Sljedeći primjer stvara naziv grupe resursa `myResourceGroup` u na `WestUS` mjesto:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Stvaranje Azure ključa sigurnog. Sljedeći primjer stvara ključ sigurnog pod nazivom `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Stvaranje ključa šifriranja u vašem sigurnog ključ i omogućiti za šifriranje. Sljedeći primjer stvara ključ naziva `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Stvaranje krajnje pomoću servisa Azure Active Directory za rukovanje provjeru autentičnosti i razmjenu šifriranja tipki iz zbirke ključeva ključ. Na `--home-page` i `--identifier-uris` ne moraju biti stvarni usmjeravati adresa. Za najvišu razinu sigurnosti tajne klijent treba koristiti umjesto lozinke. Azure EŽA trenutno nije moguće generirati tajne klijenta. Klijent tajne moguće je generirati samo na portalu za Azure. Sljedeći primjer stvara krajnje Azure Active Directory pod nazivom `myAADApp` i koristi lozinku za `myPassword`. Odredite vlastitu lozinku na sljedeći način:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Napomena u `applicationId` prikazani u Izlaz iz prethodne naredbe. U ovom ID aplikacije koriste se u na sljedeći način:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Dodajte na disku podataka u postojeće VM. Sljedeći primjer dodaje podatkovni disk VM pod nazivom `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Pregledajte pojedinosti svoje sigurnog ključ i ključ koji ste stvorili. Potreban ključ sigurnog ID, URI i ključ URL-a u posljednjem koraku. U sljedećem primjeru preglede detalje za ključ sigurnog pod nazivom `myKeyVault` i ključ pod nazivom `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Šifriranje vaše diskova na sljedeći način, unos vlastite nazive parametar cijeloj:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Azure EŽA ne nudi opširno pogreške tijekom postupka šifriranje. Dodatne informacije o otklanjanju poteškoća, pregledajte `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Prethodni naredba ima raznim čimbenicima, a ne može se koliko oznaka kao Zašto postupak ne uspije, naredba dovršeno primjer bio na sljedeći način:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Na kraju, pregledajte stanje šifriranja da biste potvrdili da vaše virtualne diskova sada kodirana. Sljedeći primjer provjerava status VM pod nazivom `myVM` u na `myResourceGroup` grupa resursa:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Pregled šifriranje
Virtualna diskova na Linux VMs šifrirane na ostale pomoću [dm crypt](https://wikipedia.org/wiki/Dm-crypt). Postoji bez naknade za šifriranje virtualne diskova Azure. Cryptographic tipke spremaju se u Azure ključ sigurnog pomoću značajke zaštite za softver ili moguće je uvesti i generiranje ključeva u hardver sigurnost modula (HSMs) certificirani za FIPS standarde razine 2 140-2. Nadzor nad ključeve šifriranja, a možete nadzirati njihova upotreba. Šifrirana koriste se za šifriranje i dešifriranje virtualne diskova priložiti vaše VM. Krajnje Azure Active Directory sadrži sigurne mehanizam za izdavanje ključeve šifriranja, kao što je VMs se pokreće uključiti i isključiti.

Postupak za šifriranje s VM je na sljedeći način:

1. Stvaranje ključa šifriranja u programa sigurnog ključ Azure.
2. Konfiguriranje šifriranja ključ moći koristiti za šifriranje diskova.
3. Da biste pročitali šifriranja ključ iz zbirke ključeva za Azure ključ, stvorite krajnje Azure Active Directory pomoću odgovarajuće dozvole.
4. Izdati naredbu za šifriranje virtualne diskova, navodeći Azure Active Directory krajnjoj točki i odgovarajuću tipku šifriranja koja će se koristiti.
5. Krajnja točka Azure Active Directory zahtjeve potreban ključ šifriranja iz zbirke ključeva Azure ključ.
6. Virtualna diskova šifrirane pomoću navedenih šifriranja ključa.


## <a name="supporting-services-and-encryption-process"></a>Podrške proces servisa i šifriranje
Šifriranje ovisi o sljedeće dodatne komponente:

- **Azure ključ sigurnog** - koristi da biste zaštitili šifriranja tipke i tajne koji se koriste za šifriranje/dešifriranje postupka disk. 
  - Ako ona postoji, možete koristiti postojeće Azure ključ zbirke ključeva. Ne morate odvojiti zbirke ključeva ključa za šifriranje diskova.
  - Da biste razdvojili administratora ograničenja i ključne vidljivost, možete stvoriti namjenski sigurnog ključ.
- **Azure Active Directory** – rukuje sigurne razmjena potrebna šifriranja tipke i provjere autentičnosti za tražene akcije. 
  - Obično možete koristiti postojeće instance Azure Active Directory za housing aplikacije. 
  - Aplikacija je od krajnje točke za ključ sigurnog i virtualnog računala usluge zahtjev i dobiti izdavanja odgovarajuće šifriranja tipke. Ne radite stvarni aplikacije koja integrira Azure Active Directory.


## <a name="requirements-and-limitations"></a>Preduvjeti i ograničenja
Podržani scenariji i preduvjeti za šifriranje diska:

- Sljedeći Linux poslužitelj SKU-ove - Ubuntu, CentOS, SUSE i SUSE Linux Enterprise Server (SLES) i crveno je vaša Enterprise Linux.
- Svi resursi (kao što je ključ sigurnog, račun za pohranu i VM) moraju biti iste Azure regija i pretplate.
- Standardni odgovora, D, DS, G i Oznaka niz VMs.

Šifriranje trenutno nisu podržani u sljedećim scenarijima:

- Osnovni sloju VMs.
- VMs stvoren pomoću model implementacije klasični.
- Onemogućivanje OS šifriranje na Linux VMs.
- Ažuriranje šifriranja tipke na već šifrirane VM za Linux.


## <a name="create-the-azure-key-vault-and-keys"></a>Stvaranje sigurnog ključ Azure i ključevi
Da biste dovršili ostatak ovaj vodič, potrebno vam je [Najnovija Azure EŽA](../xplat-cli-install.md) instaliran i prijavljeni pomoću MOD upravitelja resursa na sljedeći način:

```
azure config mode arm
```

Cijeloj Primjeri naredbi zamijenite sve parametre primjer vlastite nazive, mjesto i vrijednosti ključa. U sljedećem se primjeru koristi konvencija od `myResourceGroup`, `myKeyVault`, `myAADApp`, itd.

U prvi je korak da biste stvorili programa Azure ključ zbirke ključeva za pohranu ključeva šifriranja. Azure sigurnog ključ možete spremiti tipke, tajne ili lozinki koje omogućuju sigurno implementirati u aplikacijama i servisima. Za šifriranje virtualne diska koristite sigurnog ključ za pohranu ključa šifriranja koja se koristi za šifriranje ili dešifriranje vaše virtualne diskova. 

Omogućivanje sigurnog ključ Azure davatelja za Azure pretplatu, a zatim Stvori grupu resursa. Sljedeći primjer stvara grupu resursa pod nazivom `myResourceGroup` u na `WestUS` mjesto:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Sigurnog ključ Azure koja sadrži šifrirane tipke i pridruženi računalnim resurse kao što su prostora za pohranu i VM sam mora se nalaziti na istom području. Sljedeći primjer stvara sigurnog ključ Azure koja se naziva `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Pohranite šifriranja tipke softver ili zaštita hardver sigurnost Model (HSM). Korištenje programa HSM zahtijeva premium sigurnog ključ. Postoji dodatni trošak za stvaranje premium sigurnog ključ umjesto standardne sigurnog ključ koji pohranjuje softver zaštićeni tipke. Da biste stvorili premium ključ sigurnog u prethodnom koraku dodajte `--sku Premium` naredbi. U sljedećem primjeru pomoću tipke softver zaštićeni jer smo stvorili standardne sigurnog ključ. 

Za obje modele zaštitu Azure platforme mora odobriti pristup da biste zatražili šifriranja tipke kada se pokrene u VM dešifrirati virtualne diskova. Stvaranje ključa za šifriranje unutar vaše sigurnog ključ, a zatim omogućiti za korištenje s virtualne šifriranje. Sljedeći primjer stvara ključ naziva `myKey` i zatim je omogućuje za šifriranje diska:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Stvaranje aplikacije servisa Azure Active Directory
Kad virtualne diskova su šifrirane ili dešifrirati, koristite krajnje učiniti provjeru autentičnosti i razmjenu šifriranja tipki iz zbirke ključeva ključ. U ovom krajnje programa Azure Active Directory omogućuje Azure platforme za upućivanje zahtjeva za tipke odgovarajući šifriranja ime na VM. Zadane instance Azure Active Directory dostupna u vašoj pretplati iako mnoge organizacije imaju namjenski direktorija Azure Active Directory.

Dok ne stvarate punu aplikaciju za Azure Active Directory u `--home-page` i `--identifier-uris` parametara u sljedećem primjeru ne moraju biti stvarni usmjeravati adresa. U sljedećem primjeru i određuje s operacijskim sustavom tajna umjesto generiranje ključeva s portala za Azure. Kao ovaj put generiranje ključeva ne može učiniti s EŽA Azure. 

Stvaranje aplikacije servisa Azure Active Directory. Sljedeći primjer stvara aplikacije pod nazivom `myAADApp` i koristi lozinku za `myPassword`. Odredite vlastitu lozinku na sljedeći način:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Obratite pozornost na `applicationId` koja se vraća u Izlaz iz prethodne naredbe. U ovom ID aplikacije se koristi za neke od preostale korake. Nakon toga stvoriti glavni naziv servisa (SPN) tako da se aplikacija nije dostupna u okruženju sustava. Da biste uspješno šifriranje ili dešifriranje virtualne diskova, dozvole na tipku šifriranja pohranjene u sigurnog ključ mora biti postavljeno na dopušta Azure Active Directory aplikacije da biste pročitali tipki. 

Stvaranje na SPN i postavite odgovarajuće dozvole na sljedeći način:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Dodavanje virtualne disku i pregledajte stanje šifriranja
Zapravo šifriranje neke virtualne diskova omogućuje dodavanje na disku postojeće VM. Dodati na disku 5Gb podataka postojeće VM na sljedeći način:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Virtualna diskova trenutno nije šifrirana. Pregled trenutnog statusa šifriranje vaše VM na sljedeći način:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Šifriranje virtualne diskova
Da biste odmah šifrirali virtualne diskova, koje objediniti prethodne komponente:

1. Navedite aplikacije Azure Active Directory i lozinku.
2. Navedite sigurnog ključ za pohranu metapodataka za vaše šifrirane diskova.
3. Navedite šifriranja tipke za stvarni šifriranje i dešifriranje.
4. Odredite želite li šifriranje OS disk, diskova podataka ili sve.

Omogućuje pregledajte pojedinosti svoje sigurnog ključ Azure i ključ koji ste stvorili, kao što je potreban ključ ID sigurnog URI, a zatim tipku URL-a u posljednjem koraku:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Šifriranje vaše virtualne diskova pomoću Izlaz iz na `azure keyvault show` i `azure keyvault key show` naredbe na sljedeći način:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Prethodni naredba ima raznim čimbenicima, u sljedećem primjeru je naredba kao dovršeno reference:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Azure EŽA ne nudi opširno pogreške tijekom postupka šifriranje. Dodatne informacije o otklanjanju poteškoća, pregledajte `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` na VM su šifriranje.

Na kraju, omogućuje pregled stanja šifriranje da biste potvrdili da vaše virtualne diskova sada kodirana:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Dodavanje dodatnih podataka diskova
Kada imate šifrirane diskova vaše podatke, možete naknadno dodavanje dodatnih virtualne diskova vaše VM i šifriranja ih. Kada pokrenete u `azure vm enable-disk-encryption` naredbu, povećali pomoću niza verziju na `--sequence-version` parametar. Ovaj parametar redoslijeda verzija omogućuje izvođenje koji se ponavljaju operacija na istom VM.

Na primjer, omogućuje dodavanje drugi virtualni disk na VM na sljedeći način:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Ponovno pokrenite naredbu za šifriranje virtualne diskova dodavanje vremena na `--sequence-version` parametar i povećava vrijednosti iz naših prvog pokretanja na sljedeći način:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o upravljanju sigurnog ključ Azure, uključujući brisanje šifriranja tipke i sefovi, potražite u članku [Upravljanje ključ sigurnog korištenja EŽA](../key-vault/key-vault-manage-with-cli.md).
- Dodatne informacije o šifriranje, kao što su Priprema šifrirane prilagođene VM da biste prenijeli na Azure, potražite u članku [Azure šifriranje](../security/azure-security-disk-encryption.md).