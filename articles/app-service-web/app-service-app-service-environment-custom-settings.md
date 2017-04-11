<properties
    pageTitle="Prilagođene postavke za aplikaciju servisa okruženja"
    description="Postavke prilagođene konfiguracije za aplikaciju servisa okruženja"
    services="app-service"
    documentationCenter=""
    authors="stefsch"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="stefsch"/>

# <a name="custom-configuration-settings-for-app-service-environments"></a>Postavke prilagođene konfiguracije za aplikaciju servisa okruženja

## <a name="overview"></a>Pregled ##
Budući da su aplikacije servisa okruženja odvajanja jednog klijentu, postoje određene konfiguracijske postavke koje se mogu primijeniti isključivo za aplikaciju servisa okruženja. U ovom se članku dokumenti različite specifične prilagodbe koje su dostupne za aplikacije servisa okruženja.

Ako nemate okruženju servisa aplikacije, potražite u članku [Stvaranje okruženja aplikacije servisa](app-service-web-how-to-create-an-app-service-environment.md).

Prilagodbe aplikacije servisa okruženje možete spremiti pomoću polja u novi atribut **clusterSettings** . Taj atribut nalazi se u rječniku "Svojstva" *hostingEnvironments* entitet Azure Voditelj resursa.

Sljedeće skraćeni isječak predložak resursima prikazuje atribut **clusterSettings** :


    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Atribut **clusterSettings** možete uvrstiti u predlošku Voditelj resursa da biste ažurirali okruženje za aplikaciju servisa.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Koristite Azure resursa Explorer da biste ažurirali okruženju servisa aplikacija
Osim toga, možete ažurirati okruženje za aplikaciju servisa pomoću [Programa Explorer Azure resursa](https://resources.azure.com).  

1. U programu Explorer resursa, idite na čvor za servis okruženja aplikacije (**pretplate** > **resourceGroups** > **davatelji** > **Micrososft.Web** > **hostingEnvironments**). Zatim kliknite određene okruženje servisa aplikacije koje želite ažurirati.

2. U desnom oknu kliknite **Čitanje/pisanje** u gornjem alatne trake da biste omogućili interaktivne uređivanja u programu Explorer resursa.  

3. Kliknite plavi **Uređivanje** gumb da biste predložak Voditelj resursa koje je moguće uređivati.

4. Pomaknite se do dna u desnom oknu. Atribut **clusterSettings** je na samom dnu, gdje možete unijeti i ažurirati njegovom vrijednošću.

5. Upišite (ili kopirajte i zalijepite) polja konfiguracije vrijednosti iz atributa **clusterSettings** .  

6. Kliknite na zeleni **STAVITI** gumb koji sadrži koja se nalazi pri vrhu okna desno da biste potvrdili promjene okruženje servisa aplikacija.

No pošaljete promjena, traje otprilike 30 minuta pomnoži broj sučelja u okruženju aplikacije servisa za promjene stupile na snagu.
Na primjer, ako okruženju aplikacije usluge sadrži četiri sučeljima, trebat će otprilike dva sata za ažuriranje konfiguracije da biste završili. Tijekom promjene konfiguracije izvršavanja, bez skaliranja operacije ili operacije promjena konfiguracije može obaviti u okruženju servisa aplikacija.

## <a name="disable-tls-10"></a>Onemogućivanje TLS 1.0 ##
Ponavljajuća pitanje klijenata, osobito korisnike radite PCI usklađenost revizije, je izričito onemogućivanju TLS 1.0 za svojih aplikacija.

TLS 1.0 može se onemogućiti kroz sljedeće stavke **clusterSettings** :

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Promijeni TLS snaga paket redoslijed ##
Drugi pitanje klijenata je ako ih možete izmijeniti popis ciphers dogovori njihove poslužitelja, a to se može postići izmjenom **clusterSettings** kao što je prikazano u nastavku. Na popisu snaga pakete dostupna koje će biti dohvaćeni iz [članka MSDN] (https://msdn.microsoft.com/library/windows/desktop/aa374757(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [AZURE.WARNING]  Ako su neispravnih vrijednosti postavljena snaga paket koji ne razumijete SChannel, sve TLS komunikacije s poslužiteljem može prestati raditi. U tom slučaju, morat ćete ukloniti *FrontEndSSLCipherSuiteOrder* stavku iz **clusterSettings** i slanje ažurirane resursima predložak da biste vratili zadane postavke paketa snaga.  Koristite ovu funkciju upozorenje.

## <a name="get-started"></a>Početak rada
Voditelj resursa za Azure brzi početak rada predloška web-mjesta sadrži predložak s osnovnim definicijom za [Stvaranje okruženja aplikacije servisa](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).


<!-- LINKS -->

<!-- IMAGES -->
