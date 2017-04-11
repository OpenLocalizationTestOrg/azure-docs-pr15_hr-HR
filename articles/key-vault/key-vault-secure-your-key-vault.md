<properties
    pageTitle="Sigurne vaše ključa sigurnog | Microsoft Azure"
    description="Upravljanje dozvolama za pristup za ključne zbirke ključeva za upravljanje sefovi i ključeva i tajne. Provjera autentičnosti i autorizacije modela za ključne sigurnog te o tome radi zaštite vaše ključa sigurnog"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Sigurne vaše ključa zbirke ključeva

Azure sigurnog ključ je servis u oblaku koji zaštitu ključeva za šifriranje i tajne (kao što su certifikati nizove veze, lozinke) za aplikacije oblaka. Budući da je povjerljive podatke i business ključnih, želite siguran pristup sustava vaše ključa sefovi tako da samo ovlašteni aplikacija i korisnika možete pristupiti ključa zbirke ključeva. Ovaj članak sadrži pregled modela ključa sigurnog pristupa, objašnjava provjere autentičnosti i autorizacije i u članku se opisuje kako siguran pristup sustava ključa zbirke ključeva za oblak aplikacije s primjerom.

## <a name="overview"></a>Pregled

Pristup ključa sigurnog kontroliraju se kroz dva zasebna sučelja: Upravljanje ravnini i ravnini podataka. Za obje ravnine proper provjere autentičnosti i autorizacije potreban je prije pozivatelja (korisnik ili aplikacija) možete dobiti pristup ključa zbirke ključeva. Provjera autentičnosti uspostavlja identiteta pozivatelja, dok autorizacije određuje koje operacije pozivatelj moći izvršiti.

Za provjeru autentičnosti upravljanje ravnini i ravnini podatke pomoću servisa Azure Active Directory. Za provjeru autentičnosti, upravljanje ravnini koristi kontrola pristupa na temelju uloga (RBAC) dok ravnini podataka koristi pravilnik o pristupu ključa zbirke ključeva.

Ovdje je Kratak pregled tema prekriveno:

[Provjera autentičnosti pomoću servisa Azure Active Directory](#authentication-using-azure-active-direcrory) – u ovom se odjeljku objašnjava kako pozivatelj potvrđuje s Azure Active Directory za pristup ključa sigurnog putem upravljanja ravnini i ravnini podataka. 

[Upravljanje ravnini i ravnini podataka](#management-plane-and-data-plane) – upravljanje ravnini i ravnini podataka su dvije ravnine pristup koji se koristi za pristup vašem ključa sigurnog. Svaki ravnini pristupa podržava određene operacije. U ovom se odjeljku opisuju krajnje točke pristupa, operacije podržane i pristup kontrola načina koji se koristi tako da svaki ravnini. 

[Kontrola pristupa za upravljanje ravnini](#management-plane-access-control) – u ovom odjeljku ne možemo ćete pregled dopuštanja pristupa operacija ravnini upravljanja pomoću kontrola pristupa na temelju uloga.

[Podaci ravnini kontrola pristupa](#data-plane-access-control) – u ovom se odjeljku opisuje kako pomoću ključa sigurnog pristupa pravila za kontrolu pristupa ravnini podataka.

[Primjer](#example) – u ovom se primjeru opisuje kako postaviti kontrolu pristupa za ključne zbirke ključeva da biste omogućili tri različitih timova (Sigurnost tima, razvojni inženjeri/operatori i revizore) za izvođenje određenih zadataka za razvoj, upravljanje i praćenje aplikacija u Azure.


## <a name="authentication-using-azure-active-directory"></a>Provjera autentičnosti pomoću servisa Azure Active Directory

Kada stvorite ključa sigurnog Azure pretplate, je automatski povezuju s klijentom Azure Active Directory svoje pretplate. Sve pozivatelji (korisnicima i aplikacijama) mora biti registriran u tog klijenta za pristup ovog ključa sigurnog. Aplikaciju ili korisnik mora provjeru s Azure Active Directory da biste pristupili ključa zbirke ključeva. To se odnosi na oba upravljanje ravnini i podataka access ravnini. U oba slučaja aplikaciju možete pristupiti ključa sigurnog na dva načina:

-  **korisniku + aplikacije programa access** - obično to vrijedi za aplikacije koje pristup ključa sigurnog ime korisnika za prijavljeni u. Azure PowerShell, Azure Portal nekoliko primjera vrsta programa access. Da biste omogućili pristup korisnicima dva načina: jedan pokrenuta je da biste omogućili pristup korisnicima tako da možete pristupiti ključa sigurnog u bilo kojoj aplikaciji i drugi način je da biste dodijelili korisničkog pristupa ključa sigurnog samo prilikom korištenja određenu aplikaciju (naziva se složene identiteta). 
-  **pristup samo za aplikacije** - za aplikacije koji se pokreću daemon services pozadinske zadatke itd. Identitet aplikacije je omogućen pristup ključa zbirke ključeva.

U obje vrste aplikacija aplikacije potvrđuje s Azure Active Directory pomoću bilo koje [podržava metoda provjere autentičnosti](../active-directory/active-directory-authentication-scenarios.md) i nabaviti token. Način provjere autentičnosti koji se koristi ovisi o vrsti aplikacije. Zatim aplikacija koristi ovaj token i šalje zahtjev REST API-JA ključa zbirke ključeva. U slučaju upravljanje ravnini pristup zahtjeve su usmjerena kroz Voditelj resursa Azure krajnjoj točki. Prilikom pristupa podacima ravnini talks aplikacije izravno u ključ sigurnog krajnjoj točki. Da biste vidjeli dodatne detalje na [tijek cijeli provjere autentičnosti](../active-directory/active-directory-protocols-oauth-code.md). 

Naziv resursa za koju aplikaciju zahtjeve token razlikuje se ovisno o tome hoće li aplikacija pristupa upravljanje ravnini ili ravnini podataka. Dakle naziv resursa je ili upravljanje ravnini ili podataka ravnini krajnjoj točki opisan u tablici u nastavku, ovisno o Azure okruženju.

Imate jedan jedan mehanizam za provjeru autentičnosti za upravljanje i ravnini podataka sadrži vlastitu pogodnosti:

- Tvrtke i ustanove središnje možete upravljati pristupom sve ključne sefovi u tvrtki ili ustanovi
- Ako korisnik napusti, oni trenutačno izgubiti pristup svim ključa sefovi u tvrtki ili ustanovi
- Tvrtke i ustanove mogu prilagoditi provjera autentičnosti putem mogućnosti u Azure Active Directory (na primjer, omogućivanjem višestruke provjere autentičnosti radi dodatne sigurnosti)

## <a name="management-plane-and-data-plane"></a>Upravljanje ravnini i ravnini podataka

Azure sigurnog ključ je dostupno putem model implementacije Voditelj resursa Azure Azure usluga. Kada stvorite ključa sigurnog, prikazat će se virtualne spremnika u kojem možete stvoriti druge objekte kao što su tipke, tajne i certifikati. Zatim pristupiti vašem ključa sigurnog pomoću upravljanja ravnini i podataka ravnini izvođenje određenih operacija. Sučelje za upravljanje servisima ravnini služi za upravljanje sustava ključa sigurnog odvojeno, kao što su stvaranje, brisanje, ažuriranje ključa sigurnog atributi i postavljanje pravilnika za pristup za ravnini podataka. Sučelje za ravnini podataka koristi se za dodavanje, brisanje, izmijeniti i pomoću tipke, tajne i certifikati pohranjene u vašem ključa sigurnog.

Upravljanje ravnini i podataka ravnini sučelja se pristupa putem različitih krajnje točke (pogledajte tablicu). Drugi stupac u tablici opisuju DNS naziva za te krajnje točke u različitim okruženjima Azure. Treći stupac opisuju operacije možete izvesti iz svakog ravnini programa access. Svaki ravnini access ima vlastiti mehanizam za kontrolu pristupa: za kontrolu pristupa za upravljanje ravnini postavljena pomoću kontrola pristupa na Azure Resource Manager Role-Based (RBAC), dok za kontrolu pristupa ravnini podataka postavljena putem ključa sigurnog pristupa pravila.

| Pristup ravnini | Pristup krajnje točke | Operacije | Mehanizam za kontrolu pristupa|
|--------------|------------------|--------------------|--------|
| Upravljanje ravnini|**Globalni:**<br> Management.Azure.com:443<br><br> **Azure Kina:**<br> Management.chinacloudapi.CN:443<br><br> **Azure vlade sad:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Njemačka:**<br> Management.microsoftazure.de:443 | Stvaranje/pročitano/ažuriranje i brisanje ključa sigurnog <br> Postavljanje pristupa pravilnika za ključne zbirke ključeva<br>Postavljanje oznake za ključne zbirke ključeva | Kontrola pristupa na temelju uloga upravitelja Azure resursa (RBAC) |
| Ravnini podataka | **Globalni:**<br> &lt;sigurnog naziv&gt;. vault.azure.net:443<br><br> **Azure Kina:**<br> &lt;sigurnog naziv&gt;. vault.azure.cn:443<br><br> **Azure vlade sad:**<br> &lt;sigurnog naziv&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Njemačka:**<br> &lt;sigurnog naziv&gt;. vault.microsoftazure.de:443 | Za tipke: Dešifrirati, šifriranje, UnwrapKey, WrapKey, provjerite je li, prijavite se, se, popisa, ažuriranje, stvaranje, uvoz, brisanje, sigurnosno kopirali, vraćanje<br><br> Za tajne: dobivanje, popisa, postavljanje, a zatim Izbriši | Pravilnik o pristupu ključa zbirke ključeva|

Upravljanje ravnini i podataka ravnini pristup kontrole rade. Na primjer, ako želite dopustiti aplikacije programa access za korištenje tipki u ključa sigurnog, što je potrebno da biste dodijelili ravnini dozvole za pristup podacima pomoću ključa sigurnog pristupa pravila i bez pristupa ravnini upravljanje potreban je za ovu aplikaciju. Nasuprot tome, ako želite da korisnik da biste mogli čitati sigurnog svojstva i oznake, no nećete imati pristup tipke, tajne ili certifikata, možete dodijeliti taj korisnik 'pročitajte"pristupa pomoću RBAC i potreban je pristup ravnini podataka.

## <a name="management-plane-access-control"></a>Kontrola pristupa ravnini upravljanja

Upravljanje ravnini sastoji se od operacije koje utječu na ključa sigurnog sam. Ako, na primjer, stvaranje ili brisanje ključa sigurnog. Popis sefovi možete dobiti u pretplatu. Možete dohvatiti svojstva ključa sigurnog (kao što su SKU-om, oznake) i postaviti ključa sigurnog pristupa pravila koja određuju korisnicima i aplikacijama koje mogu pristupiti tipke i tajne u ključa sigurnog. Kontrola pristupa za upravljanje ravnini koristi RBAC. Pogledajte popis svih ključa sigurnog operacije koje možete izvršiti putem upravljanja ravnini u tablici u prethodni sekcije. 

### <a name="role-based-access-control-rbac"></a>Kontrola pristupa na temelju uloga (RBAC)
Azure pretplate ima Azure Active Directory. Korisnike, grupe i aplikacijama iz imenika možete dobiti pristup za upravljanje resursima u Azure pretplate koje koriste model implementacije Azure Voditelj resursa. Ove vrste kontrola pristupa se nazivaju se na temelju uloga kontrole pristupa (RBAC). Da biste upravljali taj pristup, možete koristiti [Azure portal](https://portal.azure.com/), [Alati za Azure EŽA](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)ili [Azure resursima REST API -ji](https://msdn.microsoft.com/library/azure/dn906885.aspx).

S modelom upravljanja resursima Azure stvarate vaše ključa sigurnog u resursa grupa te upravljanje pristupom za upravljanje ravnini s ovog ključa sigurnog pomoću servisa Azure Active Directory. Na primjer, možete dodijeliti korisnike ili grupe mogućnost da biste upravljali ključa sefovi u određene grupe resursa.

Dopustiti pristup za korisnike, grupe i aplikacije, na određene opseg dodjelom prikladne RBAC uloge. Na primjer, da biste omogućili pristup korisniku da biste upravljali ključa sefovi želite dodijeliti unaprijed definirane ulogu 'ključa sigurnog Suradnik' za tog korisnika u određenim dosegu. Opseg u tom slučaju bi pretplatu, grupu resursa ili samo određene ključa sigurnog. Uloga dodijeliti razini pretplate primjenjuje se na sve grupe resursa i resurse te pretplate. Uloga dodijeliti razini grupu resursa primjenjuje se na sve resursa u toj grupi resursa. Uloga dodijeljeni za određeni resurs odnosi samo na taj resurs. Postoji nekoliko unaprijed definiranih uloga (potražite u članku [RBAC: ugrađene uloge](../active-directory/role-based-access-built-in-roles.md)), a ako unaprijed definirane uloge odgovara vašim potrebama možete definirati vlastite uloge.

>[AZURE.IMPORTANT] Imajte na umu da ako korisnik ima suradničke dozvole (RBAC) u ravnini upravljanja ključem sigurnog, she mogu dodijeliti herself pristup ravnini podataka pravilima postavljanje ključa sigurnog pristupa, koji omogućuje pristup ravnini podataka. Dakle, preporučuje se čvrsto kontrolirati tko ima pristup 'Suradnik' vaše ključa sefovi da biste bili sigurni samo ovlašteni osobe možete pristupiti i upravljanje sefovi tipki, tipke, tajne i certifikata.

## <a name="data-plane-access-control"></a>Kontrola pristupa ravnini podataka

Podaci ravnini ključa sigurnog sastoji se od operacije koje utječu na objekte u ključa sigurnog, kao što su tipke, tajne i certifikata.  To obuhvaća ključ operacija kao što su stvaranje, uvoz, ažuriranje, popisa, sigurnosnog kopiranja i vraćanja tipke, šifrirane operacija kao što su znak, provjerite je li, šifriranje, dešifrirati, prelamanje i Ukini prelamanje i postavljanje oznake i druge atribute za ključeve. Isto tako, tajne uključuje, se, postavite popis, izbrišite.

Pristup podacima ravnini moguć je postavljanjem pristup pravila za ključne zbirke ključeva. Korisniku, grupi ili aplikacije morate imati dozvole Suradnik (RBAC) za upravljanje ravnini za ključne sigurnog da biste mogli postaviti pravilnike za pristup za taj ključa sigurnog. Korisniku, grupi ili aplikacije možete dobiti pristup za izvođenje određenih operacija tipke ili tajne u ključa zbirke ključeva. ključne sigurnog podržava do 16 stavki pravilnik pristupa za ključne zbirke ključeva. Stvorite sigurnosnu grupu sustava Azure Active Directory i dodavanje korisnika u toj grupi da biste dodijelili pristup podacima ravnini nekoliko korisnika za ključne sigurnog.

### <a name="key-vault-access-policies"></a>Ključni sigurnog pristupa pravila

Ključni sigurnog pristupa pravila zasebno dodijelili dozvole tipke, tajne i certifikata. Na primjer, možete dati korisnički pristup samo tipke, no bez dozvole za tajne. Međutim, dozvole za pristup tipke ili tajne ili certifikati su na razini zbirke ključeva. Drugim riječima, pravilnik o pristupu ključa sigurnog ne podržava razine dozvola za objekte. [Azure portal](https://portal.azure.com/), [Alati za Azure EŽA](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)ili [ključa sigurnog upravljanje REST API -ji](https://msdn.microsoft.com/library/azure/mt620024.aspx) možete koristiti da biste postavili pravila programa access za ključne sigurnog.

>[AZURE.IMPORTANT] Imajte na umu da se ključa sigurnog pristupa pravila primjenjuju na razini zbirke ključeva. Kada korisnik je imaju dozvolu za stvaranje i brisanje tipke, u tom ključa sigurnog Ana možete izvršiti te operacije na sve tipke.

## <a name="example"></a>Primjer

Recimo da su razvoj aplikacija koja koristi certifikat za SSL, Azure prostora za pohranu za pohranu podataka, a koristi ključa RSA 2048-bitni za prijavu operacije. Recimo da radi ovu aplikaciju u VM (ili postaviti na skaliranje VM). Koristite ključa zbirke ključeva za pohranu sve tajne aplikacije i koristite ključa zbirke ključeva za pohranu Samopokretanje certifikat koji se koristi aplikacija za provjeru s Azure Active Directory.

Tako, ovo je sažetak ključeva i tajne pohraniti u ključa zbirke ključeva.

- **SSL certifikata** - koristi za SSL
- **Ključ za pohranu** – da biste dobili pristup računu za pohranu
- **Ključ RSA 2048-bitni** - koristi za prijavu operacije
- **Samopokretanje certifikat** - provjerava autentičnost na temelju Azure Active Directory, da biste pristupili ključa zbirke ključeva za dohvaćanje ključa za pohranu i korištenje RSA ključ za potpis.

Sada ćemo zadovoljavaju s osobama koje su upravljanje, uvođenje i nadzor ove aplikacije. U ovom primjeru ćemo pomoću tri uloge.

- **Sigurnost timu** – to su obično IT osoblju iz 'office od CSO (obavijest sigurnost Šef)' ili ekvivalentnu, odgovorni da biste ih sačuvali proper tajne kao što su SSL certifikata, RSA ključevi koji se koriste za potpisivanje niza veze za baze podataka, tipke račun za pohranu.
- **Razvojni inženjeri/operatori** – to su osobe koji razvijaju ovu aplikaciju i zatim je implementirati u Azure. Obično nisu dio od tima za sigurnost i zato treba ne imaju pristup osjetljive podatke, kao što su SSL certifikata, RSA tipke, ali računala mogu uvesti moraju imati pristup onima.
- **Revizore** – to je obično drugačiji skup Izolirani razvojnim inženjerima i općenite IT osoblju osobe. Njihove odgovornosti tako da pregledajte pravilna Upotreba i održavanje certifikata, tipke, itd., a zatim provjerite usklađenost sa standardima sigurnost podataka. 

Postoji jedan više ulogu koji je izvan opsega ovu aplikaciju, ali odgovarajući ovdje biti navedeno koje bi pretplate (ili grupu resursa) administratora. Pretplata administrator postavlja dozvole za pristup početne za tim sigurnost. Ovdje pretpostavkom da je administrator pretplate je omogućen pristup tim za sigurnost u grupu resursa u koje svih resursa potrebne za ovu aplikaciju reside.

Sada Pogledajmo akcije koje izvodi ulogama u kontekstu ove aplikacije.

- **Sigurnost tima**
    - Stvaranje ključa sefovi
    - Uključuje ključ sigurnog zapisivanje
    - Dodavanje tipke/tajne
    - Stvaranje sigurnosne kopije ključeva za oporavak Izrada
    - Postavljanje pravilnika za pristup ključa sigurnog da biste dodijelili dozvole korisnicima i aplikacijama za izvođenje određene operacije
    - Povremeno snimljene fotografije tipke/tajne
- **Razvojni inženjeri/operatora**
    - Reference na povezati i SSL certifikati o (thumbprints), ključa za pohranu (tajna URI) i potpisivanje ključ (URI ključa) s tim sigurnosti
    - Razvoj i implementaciju aplikacija za programski pristupa tipke i tajne
- **Revizore**
    - Pregled zapisnike korištenja da biste potvrdili pravilna Upotreba ključ/tajna i usklađenost sa standardima sigurnost podataka

Sada Pogledajmo što prava pristupa ključa sigurnog su vam potrebne po ulogama (i aplikacija) za izvođenje dodijeljenih zadataka. 

| Korisnička uloga    | Dozvole za upravljanje ravnini | Dozvole za ravnini podataka |
|--------------|------------------------------|------------------------|
|Sigurnost tima|Ključni sigurnog suradnika|Ključevi: sigurnosno kopirati, stvaranje, brisanje, se, uvoz, popisa, vraćanje <br> Tajne: sve|
|Razvojni inženjeri/Operator| Ključni sigurnog implementacija dozvole tako da VMs mogu uvesti možete dohvaćati tajne iz ključa zbirke ključeva | Ništa |
|Revizore| Ništa | Strelicama: popis<br>Tajne: popis|
|Aplikacija| Ništa | Strelicama: prijava<br>Tajne: početak |

>[AZURE.NOTE] Revizore potrebno popis dozvola za tipke i tajne tako da ih možete provjeriti atributa tipke i tajne koji se ne čuje u zapisnicima, kao što su oznake, aktivacija i istek datuma.

Osim dozvolu za ključne sigurnog sve tri uloge i potreban pristup ostali resursi. Na primjer, da biste mogli implementirati VMs (ili Web Apps itd.) Razvojni inženjeri/operatori i potreban pristup 'Suradnik' te vrste resursa. Revizore mora imati pristup za čitanje na račun za pohranu pohranjuju zapisnike ključa sigurnog.

Budući da fokus je ovog članka je zaštita pristup vašem ključa sigurnog, ne možemo samo ilustriraju važne dijelove vezani uz u ovoj temi i preskočiti detalje o implementaciji certifikata, pristup tipke i tajne programski itd.. Te detalje već možete pronaći u nekom drugom mjestu. Implementacija certifikati pohranjene u ključa zbirke ključeva za VMs opisana su u odjeljku s [bloga](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/), a postoji [uzorak koda](https://www.microsoft.com/download/details.aspx?id=45343) dostupna koji pokazuje kako koristiti Samopokretanje certifikat za provjeru autentičnosti Azure AD da biste pristupili ključa zbirke ključeva.

Većina dozvole za pristup se mogu dodijeliti pomoću portala za Azure, no da biste dodijelili dozvole zrnastog možda ćete morati koristiti Azure PowerShell (ili Azure EŽA) da biste postigli željeni rezultat. 

Pretpostavimo da sljedeće isječci PowerShell:

- Administrator servisa Azure Active Directory je stvorio sigurnosnih grupa koji predstavljaju tri uloge, odnosno Contoso sigurnost tima, Devops aplikacije Contoso, revizore aplikacije Contoso. Administrator je dodan korisnicima i grupama pripadaju.

- **ContosoAppRG** je grupa resursa u kojoj se nalazite u člancima. **contosologstorage** se pohranjuju zapisnike. 

- **ContosoKeyVault** i pohranu ključa sigurnog račun koji se koristi za ključne sigurnog zapisnika **contosologstorage** mora biti na istom mjestu Azure


Najprije administrator pretplate dodjeljuje 'ključa sigurnog Suradnik' i ' korisnički pristup administratorske ' uloge timu za sigurnost. Time se omogućuje tima sigurnost za upravljanje pristupom na druge resurse i upravljanje ključa sefovi u grupi resursa ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Sljedeću skriptu prikazuje kako sigurnost tima možete stvoriti ključa sigurnog postavljanje prijava i postavljanje dozvola pristupa za druge uloge i računala. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Prilagođene uloge definirali, je samo koju valja dodijeliti s pretplatom kojem je stvorena ContosoAppRG grupu resursa. Ako se isti prilagođene uloge će se koristiti za projekata u druge pretplate, njegova opseg može imati više pretplata dodali.

Dodjela prilagođene uloge za razvojni inženjeri/operatori za dozvole "implementacija/akcije" implementaciju ograničen je na grupu resursa. Na taj način samo VMs stvorene u grupi resursa 'ContosoAppRG' će dobiti tajne (SSL certifikata i Samopokretanje certifikata). Bilo koji VMs koje od tima za razvojni/ops stvara u drugu grupu resursa nećete moći pristupiti te tajne čak i ako su znale tajnu ji.

U ovom se primjeru prikazuje jednostavni scenarij. Možda je scenarije stvarnih vijek složenije, a možda ćete morati prilagoditi dozvole za vaše ključa sigurnog ovisno o vašim potrebama. Ako, na primjer, u našem primjeru pretpostavkom da je taj tim sigurnost će nudi ključ i tajnu reference (ji i thumbprints) te razvojnim inženjerima/operatori tim morate postaviti referencu u svojim aplikacijama. Dakle, oni ne morate dodijeliti razvojnim inženjerima/operatori sve ravnini pristupa podacima. Osim toga, imajte na umu da u ovom primjeru usredotočuje se na Zaštita vaše ključa zbirke ključeva. Slične razmotriti trebali biste dobiti radi zaštite [vaše VMs](https://azure.microsoft.com/services/virtual-machines/security/), [račune za pohranu](../storage/storage-security-guide.md) i ostale resurse za Azure previše.

>[AZURE.NOTE] Napomena: Ovom je primjeru prikazano kako ključa sigurnog pristupa će se zaključati prema dolje u radnog. Za razvojne inženjere trebali biste dobiti vlastitih pretplatu ili resourcegroup kojemu imaju potpune dozvole upravljanja njihove sefovi, VMs i račun za pohranu gdje su razvili aplikacije.


## <a name="resources"></a>Resursi

-   [Kontrola pristupa na temelju uloga Azure Active Directory](../active-directory/role-based-access-control-configure.md)

    U ovom se članku objašnjava kontrolu pristupa utemeljen na Azure Active Directory uloga i kako funkcionira.

-   [RBAC: Ugrađeni uloge](../active-directory/role-based-access-built-in-roles.md)

    U ovom se članku detaljno sve ugrađene uloge dostupne u RBAC.

-   [Razumijevanje implementacije Voditelj resursa i klasični implementacije](../resource-manager-deployment-model.md)

    U ovom se članku objašnjava implementacije Voditelj resursa i uvođenje klasičnog modela i objašnjava prednosti korištenja grupe Voditelj resursa i resursa

-    [Upravljanje kontrola pristupa na temelju uloga s Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)

     U ovom se članku objašnjava kako upravljati kontrola pristupa na temelju uloga s Azure PowerShell

-   [Upravljanje pristupom utemeljeno na ulogama kontrole s REST API-JA](../active-directory/role-based-access-control-manage-access-rest.md)

    U ovom se članku objašnjava korištenje REST API-JA za upravljanje RBAC.

-   [Kontrola pristupa na temelju uloga za Microsoft Azure s Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Ovo je veza s videozapisom na kanal 9 iz konferencije Ignite 2015 MS. U ovoj sesiji oni objasniti pristup upravljanja i mogućnosti izvješćivanja u Azure i istraživanje najbolje prakse oko zaštita pristup Azure pretplata pomoću servisa Azure Active Directory.

-   [Autorizirajte pristup web-aplikacije pomoću OAuth 2.0 i Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)

    U ovom se članku opisuju cijeli tijek OAuth 2.0 za provjeru autentičnosti s Azure Active Directory.

-   [Upravljanje REST API-ji sigurnog ključa.](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Dokument je vodič za REST API-ji za upravljanje sustava ključa sigurnog programski, uključujući postavljanje pravilnik o pristupu ključa sigurnog.

-   [Ključni sigurnog REST API-ji](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Veza na ključa sigurnog referentnu dokumentaciju REST API-JA.

-   [Kontrola pristupa ključa](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Veza na ključ pristup kontrola referentnu dokumentaciju.

-   [Kontrola pristupa tajnu](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Veza na ključ pristup kontrola referentnu dokumentaciju.

-   [Postavljanje](https://msdn.microsoft.com/library/mt603625.aspx) i [Uklanjanje](https://msdn.microsoft.com/library/mt619427.aspx) ključa sigurnog pravilnik o pristupu pomoću komponente PowerShell

    Veze na referencu dokumentaciji PowerShell cmdleti za upravljanje pravilima ključa sigurnog pristupa.

## <a name="next-steps"></a>Daljnji koraci

Dohvaćanje rada vodič za administratora, potražite u članku [Početak rada s Azure sigurnog ključa](key-vault-get-started.md).

Dodatne informacije o korištenju zapisnika ključa sigurnog potražite u članku [zapisivanje Azure sigurnog ključa](key-vault-logging.md).

Dodatne informacije o korištenju ključeva i tajne s Azure ključa sigurnog potražite u članku [o tipke i tajne](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Ako imate pitanja o ključa sigurnog, posjetite [Azure ključa sigurnog forume](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
