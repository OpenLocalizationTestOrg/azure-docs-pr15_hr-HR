<properties
   pageTitle="Sigurne klaster sustavom Windows pomoću sigurnost sustava Windows | Microsoft Azure"
   description="Saznajte kako konfigurirati čvor čvor i klijent čvor sigurnosti na samostalni klaster sustavom Windows pomoću sigurnost sustava Windows."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Sigurne samostalne klaster u sustavu Windows pomoću sigurnost sustava Windows

Da biste spriječili neovlašteni pristup servisa tkanina klaster morate sigurne, osobito kada je radnih opterećenja radnog izvodi na njemu. U ovom se članku objašnjava kako konfigurirati sigurnosti čvor čvor i klijent čvor pomoću sigurnost sustava Windows u datoteci *ClusterConfig.JSON* i odgovara na korak Konfiguriranje sigurnosnih [stvaranje samostalne klaster sustavom Windows](service-fabric-cluster-creation-for-windows-server.md). Dodatne informacije o kako tkanina servis koristi sigurnost sustava Windows, potražite u članku [scenariji klaster sigurnost](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Odabir sigurnosti za sigurnost čvor čvor Pažljivo razmotrite, jer nema klaster nadogradnje iz jednog sigurnost izbor u drugu. Promjena odabira za sigurnost je potrebna potpuna klaster izvođenje.

## <a name="configure-windows-security"></a>Konfiguriranje sigurnost sustava Windows
Konfiguracijska datoteka *ClusterConfig.Windows.JSON* ogledne slike u album s [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) samostalne klaster paket sadrži predložak za konfiguriranje sigurnost sustava Windows.  U odjeljku **Svojstva** je konfiguriran sigurnost sustava Windows:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Postavke konfiguracije**|**Opis**|
|-----------------------|--------------------------|
|ClusterCredentialType|Postavljanjem parametar **ClusterCredentialType** na *Windows*je omogućena sigurnost sustava Windows.|
|ServerCredentialType|Postavljanjem parametar **ServerCredentialType** na *Windows*je omogućena sigurnost sustava Windows za klijente. To znači da se klijenti klaster i klaster sam radi unutar domena aktivnog imenika.|
|WindowsIdentities|Sadrži identiteta klaster i klijenta.|
|ClusterIdentity|Konfigurira čvor čvor sigurnost. Popis odvojenih zarezom grupe upravljanih računa servisa ili naziva računala.|
|ClientIdentities|Konfigurira klijent čvor sigurnost. Polje klijent korisničkih računa.|
|Identitet|Identitet klijent, korisnik domene.|
|IsAdmin|TRUE određuje domene ima li korisnik administratorske klijentski pristup false za klijentski pristup za korisnika.|

Postavljanjem pomoću **ClusterIdentity**je konfiguriran [čvor čvor sigurnost](service-fabric-cluster-security.md#node-to-node-security) . Da biste sastavili odnos pouzdanosti između čvorove, oni mora izvršiti svjesni međusobno povezani. To je moguće napraviti na dva načina: Navedite grupi upravljani račun servisa koji uključuje sve čvorove u klasteru ili navedite identiteta čvor domene sve čvorove u klasteru. Preporučujemo korištenje pristup [Grupi upravljanih računa servisa (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) osobito veće klastere (više od 10 čvorove) ili za klastere koje su vjerojatno Povećaj ili Smanji.
Taj se način omogućuje čvorove da biste dodali ili uklonili gMSA, bez promjene manifest klaster. Taj se način ne zahtijeva stvaranje grupa domene za koje administratori klaster odobren prava pristupa za dodavanje i uklanjanje članova. Dodatne informacije potražite u članku [Uvod u grupe upravljanih računa servisa](http://technet.microsoft.com/library/jj128431.aspx).

[Klijent čvor sigurnost](service-fabric-cluster-security.md#client-to-node-security) je konfiguriran pomoću **ClientIdentities**. Da biste uspostavili pouzdanosti između klijenta i klaster, morate konfigurirati klaster znati koji klijent identiteta koji je pouzdanima. To možete učiniti na dva načina: Navedite korisnike grupe domene koje se mogu povezati ili navedite korisnike čvor domene koji je moguće povezati. Servis tkanina podržava dvije vrste kontrola drugačiji pristup za klijente koji su povezani s servisa tkanina klaster: administrator i korisnika. Kontrola pristupa pruža mogućnost za administratora klaster ograničiti pristup određene vrste klaster postupci za različite grupe korisnika, a zatim dodatna klaster zaštita.  Administratori imaju puni pristup mogućnostima upravljanja (uključujući mogućnosti za čitanje/pisanje). Korisnici, po zadanom imaju samo za čitanje za mogućnosti upravljanja (na primjer, mogućnosti upita) i mogućnost da biste riješili aplikacija i servisa.

Sljedeći primjer odjeljak **Sigurnost** konfigurira sigurnost sustava Windows i određuje da su strojeva u *ServiceFabric/clusterA.contoso.com* dio klaster te sadrži li *CONTOSO\usera* klijentski pristup za administratore:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Daljnji koraci

Nakon konfiguriranja sigurnost sustava Windows u datoteci *ClusterConfig.JSON* , nastavili postupak stvaranja klaster u [stvaranje samostalne klaster sustavom Windows](service-fabric-cluster-creation-for-windows-server.md).

Dodatne informacije o sigurnosti čvor čvor, klijent čvor sigurnost i kontrola pristupa na temelju uloga potražite u članku [scenariji sigurnost klaster](service-fabric-cluster-security.md).

Primjeri povezati pomoću programa PowerShell ili FabricClient potražite u članku [Povezivanje sigurne klaster](service-fabric-connect-to-secure-cluster.md) .
