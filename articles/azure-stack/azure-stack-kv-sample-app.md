<properties
    pageTitle="Omogućivanje aplikacije revtrieve Azure stogu ključ sigurnog tajne | Microsoft Azure"
    description="Korištenje aplikacije uzorka za rad s Azure stogu ključ sigurnog"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Pokrenite aplikaciju uzorka za sigurnog ključ 

U ovom vodiču koristit ćete probna aplikacija za dohvaćanje tajne i lozinke iz zbirke ključeva ključ.

## <a name="download-the-samples-and-prepare"></a>Preuzmite primjere i Priprema

Preuzmite klijent uzoraka Azure ključ sigurnog iz [sigurnog ključ Azure klijent uzoraka stranice](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Izdvajanje sadržaja .zip datoteku s vašim lokalnim računalom.

Čitanje datoteke **README.md** (to je tekstne datoteke), a zatim slijedite upute.

## <a name="run-sample-1--hellokeyvault"></a>Pokretanje uzorka #1 – HelloKeyVault
HelloKeyVault je aplikacija konzole koja vodi kroz ključni scenariji koje podržava sigurnog ključ:

  1. Stvaranje i uvoz ključ (HSM ili softver ključ)
  2. Tajna sadržaja ključem za šifriranje
  3. Prelamanje tipku sadržaja pomoću tipke sigurnog ključ
  4. Ukini prelamanje tipku sadržaja
  5. Dešifriranje na tajna
  6. Postavljanje programa tajna

Tu aplikaciju konzole trebale bi funkcionirati bez ikakvih promjena, osim što se odgovarajući konfiguracijske postavke u App.Config ažurirat će se prateći sljedeće korake:

1. Ažuriranje postavki Konfiguriranje aplikacije u HelloKeyVault\App.config s URL-a zbirke ključeva, glavni ID aplikacije i tajna. Podaci se generira prema želji pomoću **scripts\GetAppConfigSettings.ps1**.
2. Ažuriranje vrijednosti obavezno varijabli u GetAppConfigSettings.ps1.
3. Pokrenite Windows PowerShell prozora.
4. Pokrenuti skriptu GetAppConfigSettings.ps1 unutar prozora PowerShell.
5. Kopiranje rezultata skripta HelloKeyVault\App.config datoteku.


## <a name="next-steps"></a>Daljnji koraci

[Implementacija VM lozinkom sigurnog ključ](azure-stack-kv-deploy-vm-with-secret.md)

[Implementacija VM certifikatom sigurnog ključ](azure-stack-kv-push-secret-into-vm.md)