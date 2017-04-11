<properties
    pageTitle="Objavljivanje aplikacije na udaljenom klaster s Visual Studio | Microsoft Azure"
    description="Saznajte kako objaviti aplikaciju na klaster tkanina daljinskog servisa pomoću Visual Studio."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Objavljivanje aplikacija na udaljenom klaster pomoću Visual Studio

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Tkanina servisa Azure proširenje za Visual Studio omogućuju jednostavno kontrolirani te s podrškom za skripte za objavljivanje aplikaciju servisa tkanina klaster.

## <a name="the-artifacts-required-for-publishing"></a>Artefakte potrebne za objavljivanje

### <a name="deploy-fabricapplicationps1"></a>Implementacija FabricApplication.ps1

Ovo je skriptu PowerShell koji koristi put profila Objavi kao parametar za objavljivanje tkanina servisa aplikacije. Budući da Ova skripta je dio aplikacije, koje su dobrodošlice da biste izmijenili prema potrebi aplikacije.

### <a name="publish-profiles"></a>Objavljivanje profila

Mapa u programu project aplikacije servisa tkanina naziva **PublishProfiles** sadrži XML datoteke pohranite ključne informacije za objavljivanje aplikacija, kao što su:

- Parametri veze klaster tkanina servisa
- Put do datoteke parametar aplikacije
- Postavke nadogradnje

Prema zadanim postavkama vašeg računala neće sadržavati stavku dva objavljivanje profila: Local.xml i Cloud.xml. Kopiranjem i lijepljenjem neke datoteke zadane možete dodati više profila.

### <a name="application-parameter-files"></a>Datoteka za parametar aplikacije

Mapa u programu project aplikacije servisa tkanina naziva **ApplicationParameters** sadrži XML datoteke za korisnički definiran aplikacije manifesta parametar vrijednosti. Datoteka manifesta aplikacije mogu biti parametrizirane da bi mogli koristiti različite vrijednosti za implementaciju postavke Da biste saznali više o parameterizing aplikacije, potražite u članku [Upravljanje više okruženja u tkanina servisa](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Za servise glumca, trebali biste sastavljanje projekta najprije prije pokušaja uređivati datoteku u uređivaču ili putem dijaloški okvir Objavi. To je jer dio manifesta datoteke bit će načinjena tijekom sastavljanje.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>Da biste objavili aplikacije pomoću dijaloškog okvira objavljivanje servisne aplikacije za tkanina

Sljedeći koraci pokazuju kako objaviti aplikacije pomoću dijaloškog okvira **Objavljivanje servisne aplikacije za tkanina** nudi Visual Studio servisa tkanina Tools.

1. Na izborniku prečacu projekta servisne aplikacije za tkanina odaberite **Objavi...** Da biste prikazali dijaloški okvir **Objavljivanje servisne aplikacije za tkanina** .

    ![Na ** dijaloški okvir za objavljivanje servisa tkanina aplikacije **][0]

    Datoteka odabrana u okvir **ciljne profila** padajućeg popisa je mjesta sve postavke osim **manifesta verzijama**, spremanja. Možete koristiti postojeći profil ili stvorite novi tako da odaberete **< Upravljanje profilima... >** u okvir **ciljne profila** padajućeg popisa. Kad odaberete profil za objavljivanje, njegov sadržaj će se pojaviti u odgovarajućim poljima u dijaloškom okviru. Da biste spremili promjene u bilo kojem trenutku, odaberite vezu za **Spremanje profila** .    

2. U odjeljku **krajnja točka povezivanja** navedite na lokalnom ili udaljenom servisa tkanina klaster, objavljivanje krajnjoj točki. Da biste dodali ili promijenili krajnju točku vezu, kliknite na **Krajnjoj točki veze** padajućeg popisa. Na popisu prikazuje dostupne klaster servisa tkanina veze krajnje točke na koju možete objaviti na temelju vašeg Azure subscription(s). Imajte na umu da ako već niste prijavljeni u Visual Studio, od vas će se zatražiti da biste to učinili.

    Pomoću dijaloškog okvira klaster odabira na raspolaganju skup dostupnih pretplate i klastere.

    ![Na ** dijaloški okvir za odabir servisa tkanina klaster **][1]

    >[AZURE.NOTE] Ako želite da biste objavili na krajnje proizvoljne (kao što su klaster strana), u odjeljku **Objavljivanje za krajnje proizvoljne klaster** ispod.

    Kada odaberete krajnje, Visual Studio Provjeri valjanost veza odabrane klaster tkanina servisa. Ako ne klaster sigurne, Visual Studio možete povezati ga odmah. Međutim, ako sigurna klaster, morat ćete instalirati certifikat na lokalnom računalu prije nastavka. Dodatne informacije potražite u članku [kako konfigurirati sigurnu vezu](service-fabric-visualstudio-configure-secure-connections.md) . Kada završite, odaberite gumb **u redu** . U dijaloškom okviru **Objavljivanje servisne aplikacije za tkanina** pojavit će se odabrane klaster.

3. U okviru popisa padajući izbornik **Datoteka parametar aplikacije** pronađite datoteku parametar aplikacije. Parametar datoteku aplikacije sadrži korisnički definiran vrijednosti za parametre u datoteci manifesta aplikacije. Da biste dodali ili promijenili parametar, odaberite gumb **Uredi** . Unesite ili promijenite vrijednost parametra u rešetki **parametara** . Kada završite, odaberite gumb **Spremi** .

    ![Na ** dijaloškog okvira za uređivanje parametara **][2]

4. Korištenje **nadogradnju aplikacije** potvrdni okvir da biste odredili hoće li to objaviti akcija nije nadogradnju. Nadogradnja objavljivanje akcije razlikovati od normalno objavljivanje akcije. Potražite u članku [Servis tkanina aplikacije nadograditi](service-fabric-application-upgrade.md) popis razlike. Da biste konfigurirali postavke nadogradnje, odaberite vezu **Konfiguriranje postavki za nadogradnju** . Pojavit će se uređivač nadogradnje parametar. Potražite u članku [Konfiguriranje nadogradnju aplikacije servisa tkanina](service-fabric-visualstudio-configure-upgrade.md) dodatne informacije o nadogradnji parametara.

5. Odaberite **manifesta verzije...** gumb za prikaz dijaloškog okvira **Uređivanje verzije** . Trebate ažurirati aplikacija i servisa verzije za nadogradnju na snagu. U odjeljku [aplikacije servisa tkanina nadograditi Praktični vodič](service-fabric-application-upgrade-tutorial.md) da biste saznali kako aplikacija i servisa manifesta utjecaj verzije programa postupak nadogradnje.

    ![Na ** dijaloškog okvira za uređivanje verzije **][3]

    Ako aplikacija i servisa verzije semantičkog rad s verzijama kao što su 1.0.0 ili brojčanih vrijednosti koristiti u obliku 1.0.0.0, odaberite mogućnost **automatski ažurirati aplikacija i servisa verzije** . Kada odaberete tu mogućnost, a zatim na servis i brojevi verzija aplikacije će se automatski ažurirati kad god kod, config ili podaci pakiranje verziju će se ažurirati. Ako biste radije da biste uredili verzije ručno, poništite potvrdni okvir da biste onemogućili tu značajku.

    >[AZURE.NOTE] Za sve unose paketa da se prikazuju u projektu glumca sastavljanje projekta za generiranje stavke na popisu servisa Manifest datoteke.

6. Kada završite koji navodi sve potrebne postavke, odaberite gumb **Objavi** da biste objavili aplikacije da biste odabrani servis tkanina klaster. Postavke koje ste naveli primjenjuju se na postupak objavljivanja.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Objavljivanje u krajnje proizvoljne klaster (uključujući klastere strana)

Visual Studio sučelje za objavljivanje optimiziran je za objavljivanje na udaljenom klastere povezane s jednim od pretplate Azure. Međutim, moguće je objavljivanje proizvoljne krajnje točke (primjerice klastere davatelja usluge tkanina) tako da izravno uređivanje profila Objavi XML. Prethodno opisan, dva objavljivanje profili nudi zadani –**Local.xml** i **Cloud.xml**–, ali ste Dobro došli u stvoriti dodatne profile u različitim okruženjima. Na primjer, možda ćete morati stvoriti profil za objavljivanje klastere strana možda pod nazivom **Party.xml**.

Ako se povezujete s nezaštićenu klaster, koje je potrebno je krajnja točka povezivanja klaster, kao što su `partycluster1.eastus.cloudapp.azure.com:19000`. U tom slučaju krajnju točku veze u profilu Objavi izgleda otprilike ovako:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Ako se povezujete s zaštićenim klaster, i morat ćete unijeti detalje certifikata klijenta s lokalnom spremištu koja će se koristiti za provjeru autentičnosti. Dodatne informacije potražite u članku [Konfiguriranje sigurnu vezu servisa tkanina klaster](service-fabric-visualstudio-configure-secure-connections.md).

  Kada postavite svoj profil za objavljivanje, možete je referenca u dijaloškom okviru Objavi kao što je prikazano u nastavku.

  ![Nova objava profila u dijaloški okvir objavljivanje][4]

  Imajte na umu da, u tom slučaju novi profil Objavi upućuje na jedan od datoteka parametar zadane aplikacije. To je prikladno ako želite objaviti istu konfiguraciju aplikacije na broj okruženja. Za razliku od toga u slučajevima gdje želite da drugi konfiguracije za svako okruženje koje želite objaviti ga smisla da biste stvorili datoteku za parametar odgovarajuće aplikacije.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako automatizirati postupak objavljivanja u okruženju neprekinuti integracije, potražite u članku [Postavljanje neprekinuti Integracija tkanina servisa](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
