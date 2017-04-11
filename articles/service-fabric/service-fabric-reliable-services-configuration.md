<properties
   pageTitle="Pregled pouzdanog Services tkanina servisa Azure konfiguracije | Microsoft Azure"
   description="Informirajte se o konfiguriranju s praćenjem stanja pouzdani servisi u tkanina servisa Azure."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Konfiguriranje servisa za pouzdan s praćenjem stanja

Postoje dva skupa konfiguracijske postavke za pouzdan servise. Jedan skup je globalni za sve servise pouzdanog u klasteru dok je iz skupa specifične za određenu pouzdanog servisa.

## <a name="global-configuration"></a>Globalna konfiguracija

Konfiguraciju globalni pouzdanog servisa naveden je u manifest klaster skupine u odjeljku KtlLogger. Omogućuje konfiguraciju mjesto zajedničke zapisnika i veličine plus ograničenja globalni memorije koristi zapisivaču. Manifest klaster je jedan XML datoteku koja sadrži postavke i konfiguracije koji se odnose na sve čvorove i servise u klasteru. Datoteke se obično naziva ClusterManifest.xml. Vidjet ćete klaster manifesta za svoj klaster pomoću naredbe ljuske powershell Get-ServiceFabricClusterManifest.

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


## <a name="service-specific-configuration"></a>Konfiguracija određene servisa
S praćenjem stanja pouzdanog servisa zadane konfiguracije možete izmijeniti pomoću konfiguracije paketa (Config) ili implementaciji servisa (kod).

+ **Config** - konfiguracija putem paketa config se postiže tako da promijenite Settings.xml datoteke koje generira se u korijenskoj mapi paketa Microsoft Visual Studio u odjeljku Config mape za svaku uslugu u aplikaciji.
+ **Kod** - konfiguracija putem koda se postiže stvaranjem ReliableStateManager pomoću ReliableStateManagerConfiguration objekta sa skupom odgovarajuće mogućnosti.

Prema zadanim postavkama, izvođenja tkanina servisa Azure traži nazivi unaprijed definirane sekcija u datoteci Settings.xml i troši konfiguracijskih vrijednosti prilikom stvaranja osnovne komponente runtime.

>[AZURE.NOTE] Nemoj **ne** Izbriši naziv sekcije od sljedećih konfiguracija u na Settings.xml odnosno datoteke generirane u Visual Studio rješenja osim ako ne namjeravate konfiguriranje usluge putem koda.
Konfiguracija paketa ili sekciju naziva potrebno promjenu u kodu prilikom konfiguriranja na ReliableStateManager.


### <a name="replicator-security-configuration"></a>Konfiguracija umnažanje sigurnosti
Konfiguracija sigurnosti umnažanje koriste se za sigurnog kanala komunikaciju koja se koristi tijekom replikacije. To znači da services nećete moći vidjeti promjene koje su drugi replikacije promet, osiguravanje podataka koji je stvoren iznimno dostupan je i sigurne. Prema zadanim postavkama konfiguracije odjeljak praznu sigurnosnu sprječava replikacije sigurnost.

### <a name="default-section-name"></a>Zadani naziv sekcije
ReplicatorSecurityConfig

>[AZURE.NOTE] Da biste promijenili taj naziv sekcije, nadjačati parametar replicatorSecuritySectionName za Graditelj ReliableStateManagerConfiguration pri stvaranju ReliableStateManager za taj servis.


### <a name="replicator-configuration"></a>Konfiguriranje umnažanje
Konfiguracija umnažanje konfigurirati umnažanje odgovorna za podnošenje s praćenjem stanja pouzdanog servisa stanja iznimno pouzdanog replikaciju i persisting stanje lokalno.
Zadana konfiguracija je generirao predložak za Visual Studio i trebali biste suffice. U ovom se odjeljku govori o dodatna konfiguracija koji su dostupni za ugađanje na umnažanje.

### <a name="default-section-name"></a>Zadani naziv sekcije
ReplicatorConfig

>[AZURE.NOTE] Da biste promijenili taj naziv sekcije, nadjačati parametar replicatorSettingsSectionName za Graditelj ReliableStateManagerConfiguration pri stvaranju ReliableStateManager za taj servis.


### <a name="configuration-names"></a>Konfiguriranje imena
|Ime|Jedinica|Zadana vrijednost|Napomene|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekundi|0.015|Vremensko razdoblje za koje umnažanje na sekundarnom čekanje nakon primanja operacije prije slanja natrag na potvrđivanje u primarni. Drugi acknowledgements slati za operacije obrađuju u tom razdoblju šalju kao jedan odgovor.|
|ReplicatorEndpoint|N/D|Zadani – obavezan parametar|IP adresa i priključaka kojima umnažanje primarni/sekundarni će se koristiti za komunikaciju s drugim replicators replike skupa. To trebalo da upućuje na TCP resursa krajnje točke u manifestu servisa. Pogledajte [manifesta resursi za servisnu](service-fabric-service-manifest-resources.md) potražite dodatne informacije o definiranju krajnjoj točki resursa u manifestu za servis. |
|MaxPrimaryReplicationQueueSize|Broj operacije|8192|Maksimalan broj operacije u redu čekanja za primarni. Operacija oslobodi nakon primarni umnažanje prima programa potvrđivanje iz sekundarne replicators. Ta vrijednost mora biti veći od 64 i na potenciju broja 2.|
|MaxSecondaryReplicationQueueSize|Broj operacije|16384|Maksimalan broj operacije u redu čekanja za sekundarne. Operacija oslobodi nakon toga stanje vrlo je dostupan putem postojanost. Ta vrijednost mora biti veći od 64 i na potenciju broja 2.|
|CheckpointThresholdInMB|MB|50|Količina prostora datoteka zapisnika nakon kojeg se checkpointed stanje.|
|MaxRecordSizeInKB|KB|1024|Najveći veličina zapisa koje možda na umnažanje pisati u zapisniku. Ta vrijednost mora biti višekratnik broja 4 i veće od 16.|
|MinLogSizeInMB|MB|0 (sustav određuje)|Minimalna veličina transakcijskih zapisnika. Zapisnik neće moći će se skratiti veličinu ispod tu postavku. 0 označava da u umnažanje određivanje minimalne zapisnik. Mogućnost nastanka način djelomične kopije i rastuće sigurnosne kopije od vjerojatno odgovarajući zapisnika zapisa skraćivanje je spuštene povećate tu vrijednost povećava.|
|TruncationThresholdFactor|Analiza varijance|2|Određuje koje veličini zapisnik se pokrenuti obrezivanje. Rezanje prag ovisi o MinLogSizeInMB pomnožen sa TruncationThresholdFactor. TruncationThresholdFactor mora biti veći od 1. MinLogSizeInMB * TruncationThresholdFactor mora biti manja od MaxStreamSizeInMB.|
|ThrottlingThresholdFactor|Analiza varijance|4|Određuje koje veličini zapisnik replike počet će se ograničio vrijeme. Prag regulacije (u MB) ovisi o Max ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Prag regulacije (u MB) mora biti veći od praga obrezivanje (u MB). Rezanje prag (u MB) mora biti manja od MaxStreamSizeInMB.|
|MaxAccumulatedBackupLogSizeInMB|MB|800|Max akumulirani veličina (MB) sigurnosne kopije zapisnika u lanac navedeni sigurnosne kopije zapisnika. Rastuća zahtjeva za sigurnosne kopije neće uspjeti ako rastuće sigurnosne kopije želite generirati sigurnosne kopije zapisnika koja može izazvati zapisnike skupljena sigurnosne kopije od odgovarajuće potpuno sigurnosno kopiranje biti veća od ovu veličinu. U tim slučajevima, korisnik mora poduzeti potpuno sigurnosno kopiranje.|
|SharedLogId|GUID|""|Određuje jedinstveni GUID za prepoznavanje datoteka zapisnika za zajednički koristiti s ovom replike. Obično servise trebali biste koristiti tu postavku. Međutim, ako je naveden SharedLogId, zatim SharedLogPath mora se navesti.|
|SharedLogPath|Puni put naziv|""|Određuje puni put kojem će se stvoriti u datoteci zapisnika zajedničke ovaj replike. Obično servise trebali biste koristiti tu postavku. Međutim, ako je naveden SharedLogPath, zatim SharedLogId mora se navesti.|
|SlowApiMonitoringDuration|Sekundi|300|Postavlja interval nadzora za upravljani API poziva. Primjer: korisnik navedeni su sigurnosne kopije povratnog (opis funkcije). Nakon intervala, izvješća o stanju upozorenje poslat će se upravitelju stanja.|

### <a name="sample-configuration-via-code"></a>Konfiguriranje uzorka putem koda
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Ogledna konfiguracijska datoteka
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Napomene
Kašnjenje replikacije kontrole BatchAcknowledgementInterval. Vrijednost "0" rezultira najniže moguće kašnjenje, teret propusnost (kao što je više poruka potvrđivanja mora biti poslana i obrađuju, svaka sadrži manje acknowledgements).
Veća vrijednost za BatchAcknowledgementInterval, veći ukupni replikacije propusnost, teret veći Latencija operacija. Ovo izravno prevodi da biste Latencija pamti transakcije.

Vrijednost za CheckpointThresholdInMB određuje količinu prostora na disku na umnažanje možete koristiti za spremanje informacija o stanju u replike namjenski datoteke zapisnika. Time povećava vrijednosti veće od zadane može rezultirati brže ponovno konfiguriranje vremena kada se dodaje novi replike skupu. To su djelomično stanje prijenosa koja se odvija zbog dostupnost više povijest operacije u zapisniku. To potencijalno možete povećati vrijeme oporavak kopiju nakon rušenja.

Postavka MaxRecordSizeInKB definira maksimalne veličine zapis koji se na umnažanje zapisati u datoteke zapisnika. U većini slučajeva, zadana veličina 1024 KB zapis je optimalnih. Međutim, ako servis uzrokuje veće stavke podataka kao dio informacije o stanju, tu vrijednost bi dobro da se povećati. Postoji mnogo koristi u mijenjanju MaxRecordSizeInKB manja od najmanje 1024, kao što je manji zapis koristi samo prostor potreban za manje zapisa. Ne možemo očekivati tu vrijednost treba u slučajevima samo rijetko mijenjati.

Postavke SharedLogId i SharedLogPath uvijek koriste zajedno da biste koristiti odvojene zajedničke zapisnika iz zapisnika zajedničke zadane čvor. Za najbolje učinkovitosti proizvoljan broj usluge moguće navedite iste zajedničke zapisnika. Zajedničke datoteke zapisnika se mora nalaziti na disk koji se koriste isključivo za zajedničke zapisničke datoteke da biste smanjili glavni premještanja Nadmetanje. Ne možemo očekivati tu vrijednost treba u slučajevima samo rijetko mijenjati.

## <a name="next-steps"></a>Daljnji koraci
 - [Ispravljanje pogrešaka aplikacije usluge tkanina u Visual Studio](service-fabric-debugging-your-application.md)
 - [Referenca za razvojne inženjere za pouzdan servise](https://msdn.microsoft.com/library/azure/dn706529.aspx)
