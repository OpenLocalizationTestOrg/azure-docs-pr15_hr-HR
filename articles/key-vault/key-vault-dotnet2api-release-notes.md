<properties
   pageTitle="Ključ sigurnog .NET 2.x API napomene | Microsoft Azure"
   description="Razvojni inženjeri .NET će koristiti taj API kod za Azure ključ sigurnog"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure ključa sigurnog .NET 2.0 – napomene i vodič za migraciju

Su sljedeće bilješke i upute za razvojne inženjere rad s .NET sigurnog Azure ključ / C# biblioteke. U okvir Promijeni 1.0 verzije 2.0 verziju broj ažuriranja uvedena taj će zahtijevaju migracije rad u kodu u redoslijedu prednosti funkcionalni poboljšanja i značajka dodatke kao što je ključ sigurnog certifikati podrška.

Omogućuje sigurnog ključ potvrde podrška za upravljanje sustava x509 potvrde i ponašanja za sljedeće:  

-   Omogućuje vlasnik certifikat Stvaranje certifikata za kroz postupak stvaranja sigurnog ključ ili putem uvoz postojeći certifikat. To uključuje i samopotpisane i ustanove za izdavanje certifikata koje generira certifikata.

- Omogućuje vlasnik web ključ sigurnog certifikat za implementaciju sigurna pohrana i upravljanje X509 certifikata bez interakcije s privatnim ključem materijalom.  

-   Omogućuje vlasnik certifikat za stvaranje pravila koja upućuje sigurnog ključ za upravljanje životnog ciklusa certifikat.  

-   Omogućuje vlasnici certifikat možete unijeti podatke za kontakt za obavijesti o događajima životnog ciklusa isteka i obnavljanje certifikata.  

-   Podržava automatsko obnavljanje s odabranog kreditnih - ključ sigurnog partnera X509 certifikata davatelji / ustanovama za izdavanje certifikata.
    - Imajte na UMU - koje nisu partnered davatelji/izvora i dopušteno, ali, ne podržava značajku Automatsko obnavljanje.


## <a name="net-support"></a>Podrška za .NET
- **.NET 4.0** ne podržava 2.0 verziju .NET Azure ključ sigurnog / C# biblioteke
- **.NET Core** podržava 2.0 verziju .NET Azure ključ sigurnog / C# biblioteke

## <a name="namespaces"></a>Prostori naziva
- Prostor za naziv **modela** mijenja se iz **Microsoft.Azure.KeyVault** **Microsoft.Azure.KeyVault.Models**.
- Prostor za naziv **Microsoft.Azure.KeyVault.Internal** se prekida.
- Prostor za naziv ovisnosti Azure SDK mijenjaju iz **Hyak.Common** i **Hyak.Common.Internals** **Microsoft.Rest** i **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Promjena
- *Tajna* mijenjaju se u *SecretBundle*
- *Rječnik* mijenjaju se u *IDictionary*
- *Popis<T>, niz []* mijenjaju se *IList<T> *
- *NextList* mijenjaju se u *NextPageLink*


## <a name="return-types"></a>Vrsta povrata
- Vratit će **KeyList** i **SecretList** *IPage<T> * umjesto *ListKeysResponseMessage*
- Generirani **BackupKeyAsync** će vratiti *BackupKeyResult* koja sadrži *vrijednost* (sigurnosne kopije blob). Prije nego što je način omotan i povratkom samo vrijednost.

## <a name="exceptions"></a>Iznimke
- *KeyVaultClientException* mijenja se *KeyVaultErrorException*
- Pogreška servisa mijenja se iz *iznimke. Pogreška* za *iznimke. Body.Error.Message*.
- Dodatne informacije o se uklanjaju iz poruka o pogrešci za **[JsonExtensionData]**.

## <a name="constructors"></a>Constructors
- Umjesto prihvaćanja *HttpClient* kao argument Graditelj, na Graditelj prihvaća samo *HttpClientHandler* ili *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Preuzeta paketa  
Kada klijentsko obrađuje zavisnost o ključ sigurnog sljedeće su preuzete
#### <a name="previous-package-list"></a>Prethodni popis paketa
- pakiranje id="Hyak.Common" verzija = "1.0.2" targetFramework = "net45"
- pakiranje id="Microsoft.Azure.Common" verzija = "2.0.4" targetFramework = "net45"
- pakiranje id="Microsoft.Azure.Common.Dependencies" verzija = "1.0.0" targetFramework = "net45"
- pakiranje id="Microsoft.Azure.KeyVault" verzija = "1.0.0" targetFramework = "net45"
- pakiranje id="Microsoft.Bcl" verzija = "1.1.9" targetFramework = "net45"
- pakiranje id="Microsoft.Bcl.Async" verzija = "1.0.168" targetFramework = "net45"
- pakiranje id="Microsoft.Bcl.Build" verzija = "1.0.14" targetFramework = "net45"
- pakiranje id="Microsoft.Net.Http" verzija = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Trenutni popis paketa
- pakiranje id="Microsoft.Azure.KeyVault" verzija = "2.0.0-preview" targetFramework = "net45"
- pakiranje id="Microsoft.Rest.ClientRuntime" verzija = "2.2.0" targetFramework = "net45"
- pakiranje id="Microsoft.Rest.ClientRuntime.Azure" verzija = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Promjena klase

- Uklonjena je **UnixEpoch** klase
- Klase **Base64UrlConverter** preimenovana je u **Base64UrlJsonConverter**

## <a name="other-changes"></a>Ostale promjene

- Podrška za konfiguraciju KV operacija pokušaj pravila tranzitne pogreške dodana je ova verzija na API-JA.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Operacija koja vraća *sigurnog*vrsta povrata je klasu koja sadrži sigurnog svojstvo. Vrsta povrata je sada *zbirke ključeva*.
- *PermissionsToKeys* i *PermissionsToSecrets* su sada *Permissions.Keys* *Permissions.Secrets*
- Neke promjene vrsta povrata primijeniti na kontrolu-ravnini kao i.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Paket dolazi do oštećenja **Microsoft.Azure.KeyVault.Extensions** i **Microsoft.Azure.KeyVault.Cryptography** operacija šifriranja.
