<properties 
    pageTitle="Kako stvoriti programa ILB elika i mala slova pomoću predložaka Azure resursima | Microsoft Azure" 
    description="Saznajte kako stvoriti programa Interna učitavanja opterećenja elika i mala slova pomoću predložaka Azure Voditelj resursa." 
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
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Kako stvoriti programa ILB elika i mala slova pomoću predložaka Azure Voditelj resursa

## <a name="overview"></a>Pregled ##
Aplikacije servisa za okruženja u kojima se mogu se kreirati pomoću virtualne mreže interne adrese umjesto javno VIP.  Ovaj interne adrese navedeni su Azure komponenta naziva na interni opterećenja (ILB).  Na ILB elika i mala slova možete stvoriti pomoću portala za Azure.  Ga i moguće stvoriti pomoću automatizacije putem Voditelj resursa Azure predložaka.  U ovom se članku vodi kroz korake i sintaksa potrebne za stvaranje programa elika i mala slova ILB s predlošcima Azure Voditelj resursa.

Prilikom automatske stvaranje programa elika i mala slova ILB su tri koraka:
1. Osnovni elika i mala slova je stvoreno u virtualne mreže pomoću adrese opterećenja Interna opterećenja umjesto javno VIP.  Kao dio ovaj korak, naziv korijenske domene je dodijeljen elika i ILB u mala slova.
2. Nakon stvaranja elika i ILB u mala slova, SSL certifikata prijenosa.  
3. Preneseni SSL certifikat izričito dodijeljene elika i ILB u mala slova kao njegov "Zadano" SSL certifikata.  SSL certifikat će se koristiti za SSL promet aplikacije na elika i ILB u mala slova prilikom aplikacije u članku se korištenjem uobičajenih korijensku domenu dodijeljena elika i za mala slova (npr. https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>Stvaranje baze ILB elika i mala slova ##
Predložak programa primjer Voditelj resursa Azure i njegove datoteke pridružene parametre dostupni na GitHub [ovdje][quickstartilbasecreate].

Većina parametara u datoteci *azuredeploy.parameters.json* su uobičajeni za stvaranje i ILB ASEs, kao i ASEs vezana uz javno VIP.  Na popisu u nastavku pozive izvan parametara posebno bilješke ili koji su jedinstveni, prilikom stvaranja programa elika i mala slova ILB:


- *interalLoadBalancingMode*: U većini slučajeva to 3, što znači i HTTP/HTTPS promet na priključke 80 i 443 i kontrola/podataka kanala priključke Uvažili putem usluge FTP na elika i mala slova, će biti vezani skup programa ILB dodijeliti virtualne mreže interne adrese.  Ako je ovo svojstvo već postavljeno na 2, zatim samo FTP usluge povezane priključke (kontrola i podataka kanala) će biti povezan sa adresu ILB dok promet HTTP/HTTPS će ostati na javno VIP.
-  *dnsSuffix*: ovaj parametar određuje zadani korijensku domenu koji će se dodijeliti u elika i mala slova.  U javnim varijacija aplikacije servisa za Azure korijensku domenu zadane za sve web-aplikacije je *azurewebsites.net*.  No Budući da je elika i mala slova ILB je interni virtualne mreže klijenta, nema smisla da biste koristili javnog servisa zadani korijensku domenu.  Umjesto toga programa elika i mala slova ILB mora sadržavati zadanu domenu za korijenske koji vam odgovara za korištenje unutar tvrtke internoj mreži virtualne.  Na primjer, hipotetska Tvrtka Contoso mogu koristiti u korijensku domenu zadani *interni contoso.com* aplikacije koje se trebaju biti samo resolvable i dostupan unutar tvrtke Contoso, virtualne mreže. 
-  *ipSslAddressCount*: ovaj parametar automatski postaviti vrijednost 0 u datoteci *azuredeploy.json* jer ILB ASEs imati samo jedan ILB adresa.  Postoje bez izričitog IP SSL adresa programa ILB elika i mala slova, dakle skupna SSL za IP adresa za programa elika i mala slova ILB mora biti postavljeno na nulu, u suprotnom će se pojaviti dodjele resursa pogreška. 

Kada datoteku *azuredeploy.parameters.json* popunjeno za programa ILB elika i mala slova, elika i ILB u mala slova pa moguće stvoriti pomoću sljedećih Powershell koda.  Promjena datoteke putova tako da odgovara gdje se nalaze datoteke predloška Azure Voditelj resursa na vašem računalu.  Ne zaboravite navesti vlastite vrijednosti za Voditelj resursa Azure implementacije ime i naziv grupe resursa.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nakon Voditelj resursa Azure je predložak poslan će trebati i nekoliko sati u elika i mala slova ILB će biti stvoren.  Nakon stvaranja dovrši, elika i ILB u mala slova prikazat će se na portalu UX na popisu okruženja aplikacije servisa za pretplatu koja ga je pokrenula uvođenje.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Prijenos i konfiguriranje SSL certifikata "Zadano" ##

Nakon stvaranja elika i ILB u mala slova, SSL certifikat mora biti povezan s elika i u mala slova jer "Zadano" SSL certifikat koji se koristi za uspostavljanje veze SSL aplikacije.  Nastavite s hipotetska Tvrtka Contoso primjeru, ako u elika i mala slova, zadani DNS-a sufiks je *Interna contoso.com*, a zatim vezu *https://some-random-app.internal-contoso.com* potreban je SSL certifikat koji vrijedi za **.internal contoso.com*. 

Postoje na različite načine da biste dobili valjane SSL certifikat uključujući interne ca, kupite certifikat od vanjskih izdavača i pomoću samopotpisanog certifikata.  Bez obzira na to izvora SSL certifikat, potrebno je konfigurirati pravilno sljedećim atributima certifikata:

- *Subject*: atribut mora biti postavljeno na **.your-korijenski-domene-here.com*
- *Predmetni Naziv zamjenski*: atribut mora sadržavati i * *.your-korijenski-domene-here.com*i * *.scm.your-korijenski-domene-here.com*.  Razlog drugom unosu je SSL veze na web-mjesto IO/Kudu pridružene svaku aplikaciju će se načiniti pomoću adrese na obrascu *your-app-name.scm.your-root-domain-here.com*.

Pomoću valjane SSL certifikata u rukom, potrebne su dva dodatna pripremnih koraka.  SSL certifikat mora biti pretvoriti/Spremi kao .pfx datoteka.  Imajte na umu mora uključiti sve Srednja i korijenski certifikati .pfx datoteka, a i mora biti zaštićen lozinkom.

Zatim konačni .pfx datoteka mora se pretvoriti u niz base64 jer SSL certifikat će biti prenesene pomoću predloška Azure Voditelj resursa.  Budući da su predlošci Voditelj resursa Azure tekstne datoteke, .pfx datoteka mora se pretvoriti u niz base64 tako da se može uključiti kao parametar predloška.

Powershell koda ispod prikazuje primjer stvaranje samopotpisanog certifikata, izvoza potvrde kao .pfx datoteka pretvaranja .pfx datoteka u na base64 kodirani niza i spremanja za base64 kodirani niza u zasebnu datoteku.  Kod ljuske Powershell za base64 kodiranje je prilagoditi s [Bloga skripti komponente Powershell][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Kada SSL certifikat je uspješno generira i pretvoriti u base64 kodirani niza, predložak primjer Voditelj resursa Azure na GitHub za [Konfiguriranje SSL certifikata zadane] [ configuringDefaultSSLCertificate] može se koristiti.

Parametri u datoteci *azuredeploy.parameters.json* navedena su u nastavku:

- *appServiceEnvironmentName*: naziv u ILB elika i mala slova konfiguriranje.
- *existingAseLocation*: tekstni niz koji sadrži Azure regija u kojoj je uveden elika i ILB u mala slova.  Na primjer: "Jug središnje NAM".
- *pfxBlobString*: U based64 kodirani niz predstavljanje .pfx datoteka.  Pomoću koda ranije prikazane bi kopirajte niz koji se nalazi u "exportedcert.pfx.b64" i zalijepite ih u kao vrijednost atributa *pfxBlobString* .
- *Lozinka*: sigurnost .pfx datoteka koristi lozinku.
- *certificateThumbprint*: otisak prsta potvrde.  Ako dohvatite tu vrijednost iz Powershell (npr. *$certificate. Otisak prsta* iz starije koda), možete koristiti vrijednost kao-je.  No ako vrijednost kopirali u dijaloškom okviru Potvrda Windows, ne zaboravite uklanjanje out suvišne razmake.  *CertificateThumbprint* trebao bi izgledati otprilike ovako: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: identifikatora neslužbeni niz vlastite odabiru koriste za identificiranje certifikata.  Naziv se koristi kao dio Jedinstveni identifikator Azure Voditelj resursa za *Microsoft.Web/certificates* entitet koji predstavlja SSL certifikata.  Naziv **mora** kraju sa sljedećim sufiks: \_yourASENameHere_InternalLoadBalancingASE.  Na portalu koristi ovaj sufiks kao pokazatelja certifikat koristi za osiguravanje programa elika i ILB omogućen u mala slova.


Primjer skraćeni *azuredeploy.parameters.json* je prikazano u nastavku:


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Kada datoteku *azuredeploy.parameters.json* popunjeno, SSL certifikat zadani mogu konfigurirati pomoću sljedeće komponente Powershell koda.  Promjena datoteke putova tako da odgovara gdje se nalaze datoteke predloška Azure Voditelj resursa na vašem računalu.  Ne zaboravite navesti vlastite vrijednosti za Voditelj resursa Azure implementacije ime i naziv grupe resursa.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nakon Voditelj resursa Azure je predložak poslan trebat će otprilike četrdeset minuta minuta po elika i mala slova sučelja da biste primijenili promjene.  Na primjer, s na zadane veličine elika i mala slova pomoću dva prednju završava, predložak se oko jedan sat i dvadeset minuta da biste dovršili.  Dok se izvodi predložak u elika i mala slova nećete moći skalirana.  

Nakon dovršetka predložak aplikacije na elika i ILB u mala slova možete pristupiti putem HTTP i veze će biti zaštićen SSL certifikat zadani.  SSL certifikat zadani koristit će se kada se aplikacija na elika i ILB u mala slova adresirana pomoću kombinacije naziv aplikacije plus zadani naziv glavnog računala.  Na primjer *https://mycustomapp.internal-contoso.com* koristi zadani SSL certifikat za **.internal contoso.com*.

Međutim, baš kao i aplikacije koji se izvode na javnog servisa više klijentu, razvojni inženjeri možete konfigurirati i prilagođeni nazivi glavnog računala za pojedinačne aplikacije i konfiguriranje jedinstvenih povezivanja SNI SSL certifikat za pojedinačne aplikacije.  


## <a name="getting-started"></a>Početak rada

Da biste započeli aplikacije servisa okruženja, potražite u članku [Uvod u okruženje za aplikaciju servisa](app-service-app-service-environment-intro.md)

Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
