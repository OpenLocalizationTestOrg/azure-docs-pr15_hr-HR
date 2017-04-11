<properties
   pageTitle="Postavljanje servisa tkanina klaster pomoću Visual Studio | Microsoft Azure"
   description="U članku se opisuje kako postaviti servisa tkanina klaster pomoću predloška Azure Voditelj resursa stvorenu u projektu programa Azure grupa resursa u Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Postavljanje servisa tkanina klaster pomoću Visual Studio
U ovom se članku opisuje kako postaviti programa klaster tkanina servisa Azure pomoću Visual Studio i predložak Azure Voditelj resursa. Koristit ćemo grupni projekt resursa za Visual Studio Azure da biste stvorili predložak. Nakon stvaranja predloška ga može uvesti izravno u Azure s Visual Studio. Može se koristiti i putem skripte ili kao dio funkcijom neprekinuti Integracija (CI).

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Stvaranje predloška klaster tkanina servis pomoću u grupi projekt Azure resursa
Da bismo započeli, otvorite Visual Studio i stvaranje projekta grupi Azure resursa koja se (je dostupan u mapi **oblaka** ):

![Novi dijaloški okvir projekta s programom project grupa resursa Azure odabrana][1]

Možete stvoriti novo Visual Studio rješenje za taj projekt ili ga dodati u postojeće rješenje.

>[AZURE.NOTE] Ako ne vidite grupni projekt Azure resursa u odjeljku čvor oblaka, nemate SDK Azure instaliran. Pokretanje instalacijskog programa za platformu Web ([sada ga instalirati](http://www.microsoft.com/web/downloads/platform.aspx) ako već nije), zatim potražite "Azure SDK za .NET" i instalirati verziju kompatibilan s vašom verzijom programa Visual Studio.

Nakon što kliknete gumb u redu, Visual Studio zatražit će da biste odabrali predložak Voditelj resursa koju želite stvoriti:

![Odabir predloška Azure dijaloški uz odabran predložak servisa tkanina klaster][2]

Odaberite predložak klaster tkanina servisa i klik na gumb u redu. Sada je stvoriti projekta i resursima predložak.

## <a name="prepare-the-template-for-deployment"></a>Priprema predložak za implementaciju
Prije nego što je predložak implementiran da biste stvorili klaster, navedite vrijednosti za parametre potreban predložak. Ove vrijednosti parametara su za čitanje iz na `ServiceFabricCluster.parameters.json` datoteka koje se nalazi u na `Templates` mapu projekta grupu resursa. Otvorite datoteku i navedite sljedeće vrijednosti:

|Naziv parametra           |Opis|
|-----------------------  |--------------------------|
|adminUserName            |Naziv administratorski račun za servis tkanina strojeva (čvorove).|
|certificateThumbprint    |Otisak prsta certifikat koji secures klaster.|
|sourceVaultResourceId    |*ID resursa* ključa sigurnog pohranjuju certifikat koji secures klaster.|
|certificateUrlValue      |URL klaster sigurnosni certifikat pouzdanim.|

Voditelj resursa za Visual Studio servisa tkanina predložak stvara sigurno klaster zaštićen certifikat. Taj certifikat označena parametri zadnja tri predložaka (`certificateThumbprint`, `sourceVaultValue`, i `certificateUrlValue`), a moraju se nalaziti u **Azure ključ sigurnog**. Dodatne informacije o stvaranju klaster sigurnosni certifikat pouzdanim, potražite u članku [servis tkanina skupine sigurnost scenariji](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) članka.

## <a name="optional-change-the-cluster-name"></a>Neobavezno: Promjena naziva klaster
Svaki servis tkanina klaster ima naziv. Tkanina klaster stvaranja u Azure klaster naziv određuje (zajedno s područjem Azure) naziv upravljanje pravima na informacije (DNS-Domain Name System) za klaster. Na primjer, ako je naziv svoj klaster `myBigCluster`, i mjesto (Azure regija) grupa resursa koji će biti smješteno novi klaster Istočni SAD-a, bit će DNS naziva klaster `myBigCluster.eastus.cloudapp.azure.com`.

Prema zadanim postavkama naziv klaster je automatski generira i stvoren jedinstveni Pridruživanjem slučajni sufiks "klaster" prefiks. To olakšava vrlo da biste koristili predložak kao dio sustava **Neprekinuti Integracija** (CI). Ako želite koristiti određene naziv svoj klaster jedan smislenog, skup vrijednosti u `clusterName` varijabilnih u datoteku predloška Voditelj resursa (`ServiceFabricCluster.json`) da biste odabrani naziv. To je prvi varijabla definirano u toj datoteci.

## <a name="optional-add-public-application-ports"></a>Neobavezno: dodavanje javno aplikacije priključci
Možda želite promjena javno aplikacije priključke za klaster prije njegove implementacije kod. Prema zadanim postavkama, predložak se otvara se samo dva javna TCP priključci (80 i 8081). Ako trebate više za aplikacije, izmijenite definiciju Azure opterećenja u predlošku. Definicija pohranjen u datoteci glavni predloška (`ServiceFabricCluster.json`). Otvorite datoteku, a zatim potražite `loadBalancedAppPort`. Svaki priključak je povezan s tri artefakte:

1. Varijable predloška koji definira TCP priključak vrijednost za priključak:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. Na *probni* koji određuje koliko se često i koliko Azure opterećenja opterećenja nije pokušava koristiti određene čvor tkanina servisa prije Neuspjeh na neku drugu. Na probes su dio opterećenja resursa. Evo definiciju probni za prvi zadani je priključak računala.

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. *Pravilo za ujednačavanje opterećenja* koji zajedno ties priključak i probni omogućuje u skupu rezultata čvorove klaster tkanina servisa za ujednačavanje opterećenja:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Aplikacije koje namjeravate implementacija klasteru potrebna dodatne priključke, možete ih dodati stvaranje dodatnih probni i definicije pravila za ujednačavanje opterećenja. Dodatne informacije o radu s Azure opterećenja predlošcima resursima potražite u članku [Početak rada prilikom stvaranja za interne opterećenja pomoću predloška](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Uvođenje predloška pomoću Visual Studio
Nakon što ste spremili sve vrijednosti parametra potrebna u na`ServiceFabricCluster.param.dev.json` datoteku, spremni ste za uvođenje predloška i stvorite svoj klaster tkanina servisa. Desnom tipkom miša kliknite grupni projekt resursa u pregledniku rješenja za Visual Studio, a zatim odaberite **uvođenja | Novi implementacije...**. Ako je potrebno, Visual Studio vode dijaloški okvir **uvođenja grupu resursa** upitom za provjeru autentičnosti Azure:

![Implementacija dijaloški okvir grupa resursa][3]

Dijaloški okvir možete odabrati postojeću grupu resursa resursima za klaster te vam nudi mogućnost da biste stvorili novi. U normalnim smisla koristiti grupu zasebnom resursa za servis tkanina klaster.

Nakon što kliknete gumb uvođenja, Visual Studio će vas da biste potvrdili vrijednosti parametara predložak. Kliknite gumb **Spremi** . Jedan parametar nema postojanog vrijednost: lozinku administratora računa za klaster. Morate navesti vrijednost lozinkom kada Visual Studio traži jednu.

>[AZURE.NOTE] Počevši od Azure SDK 2.9, Visual Studio podržava čitanje lozinke iz **Zbirke ključeva ključ Azure** tijekom implementacije. U dijaloškom okviru parametri predložak Primijetit ćete da se `adminPassword` parametar tekstni okvir sadrži mala ikona "tipke" na desnoj strani. Ta ikona omogućuje vam da biste odabrali postojeći tajna za ključne sigurnog kao administratorske lozinke za klaster. Samo se pobrinite da biste prvi Omogući pristup Azure Voditelj resursa za predložak implementacije u pravilnike za napredne pristup vašem ključa sigurnog. 

Možete pratiti napredak postupka implementacije u izlaznom prozoru Visual Studio. Nakon dovršetka uvođenje predloška svoj novi klaster spremna je za korištenje!

>[AZURE.NOTE] Ako je PowerShell nikada niste koristili za administriranje Azure s računala koje trenutno koristite, morate učiniti malo kućanstva.
>1. Omogućivanje PowerShell skriptiranje tako da pokrenete u [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) naredbe. Za razvoj strojeva, obično prihvatljiva je "neograničeno" pravilo.
>2. Odlučite želite li dopustiti prikupljanje dijagnostičkih podataka iz naredbe ljuske PowerShell Azure i pokretanje [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) ili [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) prema potrebi. To će izbjegavanje nepotrebnih upite tijekom uvođenje predloška.

Ako su sve pogreške, otvorite [portal za Azure](https://portal.azure.com/) i otvorite grupu resursa koji je implementiran na. Kliknite **sve postavke**, a zatim kliknite **implementacijama** plohu postavke. Nije uspjelo uvođenje grupa resursa ima ostavlja dijagnostičkih informacija.

>[AZURE.NOTE] Servis tkanina klastere zahtijevaju određenog broja čvorove se prema gore da biste zadržali dostupnosti i očuvanje stanje - se nazivaju "održavanje kvorum". Zbog toga nije siguran da biste isključili sve strojeva u klasteru osim u slučaju da najprije obavite [cjelovite sigurnosne kopije stanje](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Daljnji koraci
- [Dodatne informacije o postavljanju servisa tkanina klaster pomoću portala za Azure](service-fabric-cluster-creation-via-portal.md)
- [Saznajte kako upravljati i Implementacija servisa tkanina aplikacije koje se koriste Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
