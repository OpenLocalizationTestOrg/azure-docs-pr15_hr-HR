<properties
   pageTitle="Pregled konfiguracije Azure servisa tkanina pouzdanog Glumci KVSActorStateProvider | Microsoft Azure"
   description="Informirajte se o konfiguriranju tkanina servisa Azure s praćenjem stanja Glumci vrste KVSActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Konfiguriranje pouzdanog Glumci – KVSActorStateProvider
Zadana konfiguracija KVSActorStateProvider možete izmijeniti tako da promijenite settings.xml datoteke koje generira se u korijenskoj mapi paketa Microsoft Visual Studio u odjeljku Config mape za navedeni glumca.

Izvođenje tkanina servisa Azure traži nazivi unaprijed definirane sekcija u datoteci settings.xml i troši konfiguracijskih vrijednosti prilikom stvaranja osnovne komponente runtime.

>[AZURE.NOTE] Učinite **ne** brisanje ili izmjenu nazivi sekcija od sljedećih konfiguracija u datoteci settings.xml koje se generiraju u Visual Studio rješenja.

## <a name="replicator-security-configuration"></a>Konfiguracija umnažanje sigurnosti
Konfiguracija sigurnosti umnažanje koriste se za sigurnog kanala komunikaciju koja se koristi tijekom replikacije. To znači da services ne mogu vidjeti promjene koje su drugi replikacije promet, osiguravanje podataka koji je stvoren iznimno dostupan je i sigurne.
Prema zadanim postavkama konfiguracije odjeljak praznu sigurnosnu sprječava replikacije sigurnost.

### <a name="section-name"></a>Naziv sekcije
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Konfiguriranje umnažanje
Konfiguracija umnažanje konfigurirati umnažanje odgovorna za podnošenje stanje glumca stanje davatelja iznimno pouzdan.
Zadana konfiguracija je generirao predložak za Visual Studio i trebali biste suffice. U ovom se odjeljku govori o dodatna konfiguracija koji su dostupni za ugađanje na umnažanje.

### <a name="section-name"></a>Naziv sekcije
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfiguriranje imena

|Ime|Jedinica|Zadana vrijednost|Napomene|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekundi|0.015|Vremensko razdoblje za koje umnažanje na sekundarnom čekanje nakon primanja operacije prije slanja natrag na potvrđivanje u primarni. Drugi acknowledgements slati za operacije obrađuju u tom razdoblju šalju kao jedan odgovor.|
|ReplicatorEndpoint|N/D|Zadani – obavezan parametar|IP adresa i priključaka kojima umnažanje primarni/sekundarni će se koristiti za komunikaciju s drugim replicators replike skupa. To trebalo da upućuje na TCP resursa krajnje točke u manifestu servisa. Pogledajte [manifesta resursi za servisnu](service-fabric-service-manifest-resources.md) potražite dodatne informacije o definiranju krajnjoj točki resursa u manifestu servisa. |
|RetryInterval|Sekundi|5|Vremensko razdoblje nakon kojeg se umnažanje ponovno prenosi poruke ako se pojavljuje se potvrđivanje za operaciju.|
|MaxReplicationMessageSize|Bajtova|50 MB|Maksimalna veličina replikacije podataka koji se mogu prenijeti u jednu poruku.|
|MaxPrimaryReplicationQueueSize|Broj operacije|1024|Maksimalan broj operacije u redu čekanja za primarni. Operacija oslobodi nakon primarni umnažanje prima programa potvrđivanje iz sekundarne replicators. Ta vrijednost mora biti veći od 64 i na potenciju broja 2.|
|MaxSecondaryReplicationQueueSize|Broj operacije|2048|Maksimalan broj operacije u redu čekanja za sekundarne. Operacija oslobodi nakon toga stanje vrlo je dostupan putem postojanost. Ta vrijednost mora biti veći od 64 i na potenciju broja 2.|

## <a name="store-configuration"></a>Konfiguracija spremnika
Konfiguracija pohrane koriste se za konfiguriranje lokalno spremište koje se koriste za stanje u kojem se ponavlja se i dalje pojavljuje.
Zadana konfiguracija je generirao predložak za Visual Studio i trebali biste suffice. U ovom se odjeljku govori o dodatna konfiguracija koji su dostupni za ugađanje u lokalnom spremištu.

### <a name="section-name"></a>Naziv sekcije
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfiguriranje imena

|Ime|Jedinica|Zadana vrijednost|Napomene|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Milisekundama|200|Postavlja najviše grupnog slanja promjena intervala za pamti durable lokalno spremište.|
|MaxVerPages|Broj stranica|16384|Maksimalni broj verzije stranice u lokalnoj pohranite baze podataka. Određuje najveći je broj preostala transakcije.|

## <a name="sample-configuration-file"></a>Ogledna konfiguracijska datoteka

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Napomene

Parametar BatchAcknowledgementInterval kontrole kašnjenje replikacije. Vrijednost "0" rezultira najniže moguće kašnjenje, teret propusnost (kao što je više poruka potvrđivanja mora biti poslana i obrađuju, svaka sadrži manje acknowledgements).
Veća vrijednost za BatchAcknowledgementInterval, veći ukupni replikacije propusnost, teret veći Latencija operacija. Ovo izravno prevodi da biste Latencija pamti transakcije.
