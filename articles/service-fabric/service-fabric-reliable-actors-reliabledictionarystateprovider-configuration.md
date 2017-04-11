<properties
   pageTitle="Pregled konfiguracije Azure servisa tkanina pouzdanog Glumci ReliableDictionaryActorStateProvider | Microsoft Azure"
   description="Informirajte se o konfiguriranju tkanina servisa Azure s praćenjem stanja Glumci vrste ReliableDictionaryActorStateProvider."
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
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Konfiguriranje pouzdanog Glumci – ReliableDictionaryActorStateProvider
Zadana konfiguracija ReliableDictionaryActorStateProvider možete izmijeniti tako da promijenite datoteku settings.xml za navedeni glumca generira u korijenskoj mapi paketa Visual Studio u odjeljku Config mape.

Izvođenje tkanina servisa Azure traži nazivi unaprijed definirane sekcija u datoteci settings.xml i troši konfiguracijskih vrijednosti prilikom stvaranja osnovne komponente runtime.

>[AZURE.NOTE] Učinite **ne** brisanje ili izmjenu nazivi sekcija od sljedećih konfiguracija u datoteci settings.xml koja je nastala u Visual Studio rješenja.

Postoje i globalne postavke koje utječu na konfiguracije ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Globalna konfiguracija

Globalna konfiguracija naveden je u manifest klaster skupine u odjeljku KtlLogger. Omogućuje konfiguraciju mjesto zajedničke zapisnika i veličine plus ograničenja globalni memorije koristi zapisivaču. Imajte na umu da promjene u manifestu klaster utjecati na sve koji koriste ReliableDictionaryActorStateProvider i pouzdan s praćenjem stanja services.

Manifest klaster je jedan XML datoteku koja sadrži postavke i konfiguracije koji se odnose na sve čvorove i servise u klasteru. Datoteke se obično naziva ClusterManifest.xml. Vidjet ćete klaster manifesta za svoj klaster pomoću naredbe ljuske powershell Get-ServiceFabricClusterManifest.

### <a name="configuration-names"></a>Konfiguriranje imena

|Ime|Jedinica|Zadana vrijednost|Napomene|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|Kilobajta|8388608|Najmanji broj iz baze znanja za dodjelu u načinu za otklanjanje skupine zapisivaču pisanje međuspremnika na memorije. Ovaj skup memorije koristi se za predmemoriranje informacije o stanju prije pisanje na disk.|
|WriteBufferMemoryPoolMaximumInKB|Kilobajta|Nema ograničenja|Maksimalna veličina kojoj zapisivaču pisanje skup memorije međuspremnik može rasti.|
|SharedLogId|GUID|""|Određuje jedinstveni GUID za prepoznavanje zadani zajedničke datoteka zapisnika koristi sve pouzdane servise na sve čvorove u klasteru navesti na SharedLogId u njihovu određene konfiguraciju servisa. Ako je naveden SharedLogId, zatim SharedLogPath mora se navesti.|
|SharedLogPath|Puni put naziv|""|Određuje puni put gdje datoteka zapisnika zajednički koristi sve pouzdane servise na sve čvorove u klasteru navesti na SharedLogPath u njihovu određene konfiguraciju servisa. Međutim, ako je naveden SharedLogPath, zatim SharedLogId mora se navesti.|
|SharedLogSizeInMB|Megabajta|8192|Određuje broj MB prostora na disku statički dodijeliti za zajedničke zapisnika. Ta vrijednost mora biti 2048 ili veća.|

### <a name="sample-cluster-manifest-section"></a>Ogledna klaster manifesta sekcije
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Napomene
Zapisivaču ima globalni skup memorije iz memorije nestanično otklanjanje koji je dostupan za sve pouzdane usluge na čvor za predmemoriranje podatke o stanju prije koje se zapisuju zapisnik namjenski pridružene replike pouzdanog servisa. Veličina skup upravlja WriteBufferMemoryPoolMinimumInKB i WriteBufferMemoryPoolMaximumInKB postavke. WriteBufferMemoryPoolMinimumInKB određuje Početna veličina ovaj skup memorije i najmanje veličine koji mogu smanjiti skup memorije. WriteBufferMemoryPoolMaximumInKB je najveće veličina koje možda Povećaj skup memorije. Svaki replike pouzdanog servisa koja je otvorena mogu povećati veličinu skup memorije iznosom sustava određuje do WriteBufferMemoryPoolMaximumInKB. Ako postoji više zahtjev za memorije iz skup memorije nego što je dostupno, zahtjeve za memorije će se može odgoditi dok memorije. Stoga ako je skupinu memorije međuspremnika za pisanje presitan za određenu konfiguraciju zatim performanse mogu se.

Postavke SharedLogId i SharedLogPath uvijek koriste se za definiranje GUID i mjesto zajedničke zapisnik zadane za sve čvorove u klasteru. Zapisnik zadani zajednički koristi se za sve pouzdane servise koji odredite postavke settings.xml servisa za određene. Za najbolje performanse, zajedničke datoteke zapisnika se mora nalaziti na disk koji se koriste isključivo za zajedničke zapisničke datoteke da biste smanjili Nadmetanje.

SharedLogSizeInMB određuje količinu prostora na disku za preallocate za zadani zajedničke prijava sve čvorove.  SharedLogId i SharedLogPath ne moraju biti navedeno da bi SharedLogSizeInMB da bude navedena.

## <a name="replicator-security-configuration"></a>Konfiguracija umnažanje sigurnosti
Konfiguracija sigurnosti umnažanje koriste se za sigurnog kanala komunikaciju koja se koristi tijekom replikacije. To znači da services ne mogu vidjeti promjene koje su drugi replikacije promet, osiguravanje podataka koji je stvoren iznimno dostupna je sigurna.
Prema zadanim postavkama konfiguracije odjeljak praznu sigurnosnu sprječava replikacije sigurnost.

### <a name="section-name"></a>Naziv sekcije
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Konfiguriranje umnažanje
Konfiguracija umnažanje koriste se za konfiguriranje umnažanje odgovorna za podnošenje stanje glumca stanje davatelja iznimno pouzdanog replikaciju i persisting stanje lokalno.
Zadana konfiguracija je generirao predložak za Visual Studio i trebali biste suffice. U ovom se odjeljku govori o dodatna konfiguracija koji su dostupni za ugađanje na umnažanje.

### <a name="section-name"></a>Naziv sekcije
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfiguriranje imena

|Ime|Jedinica|Zadana vrijednost|Napomene|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekundi|0.015|Vremensko razdoblje za koje umnažanje na sekundarnom čekanje nakon primanja operacije prije slanja natrag na potvrđivanje u primarni. Drugi acknowledgements poslati na operacije obrađuju u tom razdoblju šalju kao jedan odgovor.||
|ReplicatorEndpoint|N/D|Zadani – obavezan parametar|IP adresa i priključaka kojima umnažanje primarni/sekundarni će se koristiti za komunikaciju s drugim replicators replike skupa. To trebalo da upućuje na TCP resursa krajnje točke u manifestu servisa. Pogledajte [manifesta resursi za servisnu](service-fabric-service-manifest-resources.md) potražite dodatne informacije o definiranju krajnjoj točki resursa u manifestu servisa. |
|MaxReplicationMessageSize|Bajtova|50 MB|Maksimalna veličina replikacije podataka koji se mogu prenijeti u jednu poruku.|
|MaxPrimaryReplicationQueueSize|Broj operacije|8192|Maksimalan broj operacije u redu čekanja za primarni. Operacija oslobodi nakon primarni umnažanje prima programa potvrđivanje iz sekundarne replicators. Ta vrijednost mora biti veći od 64 i na potenciju broja 2.|
|MaxSecondaryReplicationQueueSize|Broj operacije|16384|Maksimalan broj operacije u redu čekanja za sekundarne. Operacija oslobodi nakon toga stanje vrlo je dostupan putem postojanost. Ta vrijednost mora biti veći od 64 i na potenciju broja 2.|
|CheckpointThresholdInMB|MB|200|Količina prostora datoteka zapisnika nakon kojeg se checkpointed stanje.|
|MaxRecordSizeInKB|KB|1024|Najveći veličina zapisa koje možda na umnažanje pisati u zapisniku. Ta vrijednost mora biti višekratnik broja 4 i veće od 16.|
|OptimizeLogForLowerDiskUsage|Booleove vrijednosti|TRUE|Kada je true, zapisnik je konfiguriran tako da se replike namjenski datoteke zapisnika stvoren je pomoću datoteku kratke NTFS. To Spušta na stvarni prostor na disku za datoteku. Kada je vrijednost false, datoteka se stvara s fixed alokacija koji sadrže najbolje performanse za pisanje.|
|SharedLogId|GUID|""|Određuje jedinstveni guid za prepoznavanje datoteka zapisnika za zajednički koristiti s ovom replike. Obično servise trebali biste koristiti tu postavku. Međutim, ako je naveden SharedLogId, zatim SharedLogPath mora se navesti.|
|SharedLogPath|Puni put naziv|""|Određuje puni put kojem će se stvoriti u datoteci zapisnika zajedničke ovaj replike. Obično servise trebali biste koristiti tu postavku. Međutim, ako je naveden SharedLogPath, zatim SharedLogId mora se navesti.|


## <a name="sample-configuration-file"></a>Ogledna konfiguracijska datoteka

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
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

Parametar CheckpointThresholdInMB određuje količinu prostora na disku na umnažanje možete koristiti za spremanje informacija o stanju u replike namjenski datoteke zapisnika. Time povećava vrijednosti veće od zadane može rezultirati brže ponovno konfiguriranje vremena kada se dodaje novi replike skupu. To su djelomično stanje prijenosa koja se odvija zbog dostupnost više povijest operacije u zapisniku. To potencijalno možete povećati vrijeme oporavak kopiju nakon rušenja.

Ako OptimizeForLowerDiskUsage postavljena na true, prostor datoteka zapisnika bit će previše dodijeljenu tako da se aktivni replike možete spremiti dodatne informacije o stanju u svoje datoteke zapisnika dok Neaktivni replike će koristiti manje prostora na disku. To omogućuje za hostiranje više replike na čvor. Ako ste postavili OptimizeForLowerDiskUsage FALSE, informacije o stanju zapisuje u datoteke zapisnika brže.

Postavka MaxRecordSizeInKB definira maksimalne veličine zapis koji se na umnažanje zapisati u datoteke zapisnika. U većini slučajeva, zadana veličina 1024 KB zapis je optimalnih. Međutim, ako servis uzrokuje veće stavke podataka kao dio informacije o stanju, tu vrijednost bi dobro da se povećati. Postoji mnogo koristi u mijenjanju MaxRecordSizeInKB manja od najmanje 1024, kao što je manji zapis koristi samo prostor potreban za manje zapisa. Ne možemo očekivati tu vrijednost treba se promijeniti samo rijetko slučajeva.

Postavke SharedLogId i SharedLogPath uvijek koriste zajedno da biste koristiti odvojene zajedničke zapisnika iz zapisnika zajedničke zadane čvor. Za najbolje učinkovitosti proizvoljan broj usluge moguće navedite iste zajedničke zapisnika. Zajedničke datoteke zapisnika se mora nalaziti na disk koji se koriste isključivo za zajedničke zapisničke datoteke da biste smanjili glavni premještanja Nadmetanje. Ne možemo očekivati te vrijednosti treba se promijeniti samo rijetko slučajeva.
