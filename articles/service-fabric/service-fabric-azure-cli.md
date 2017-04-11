<properties
   pageTitle="Interakcija s klastere tkanina servis pomoću EŽA | Microsoft Azure"
   description="Kako koristiti Azure EŽA za interakciju sa servisa tkanina klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Korištenje EŽA Azure interakciju s klaster tkanina servisa

Možete raditi s klaster tkanina servisa iz Linux strojeva pomoću EŽA Azure na Linux.

Prvi korak je Dohvati najnoviju verziju EŽA iz predstavnik brojka i postaviti ga u putu pomoću sljedeće naredbe:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Za svaku naredbu podržava, možete upisati naziv naredbe da biste dobili pomoć za tu naredbu. Samodovršetak – nije podržana za naredbe. Na primjer, sljedeću naredbu vam pomoći za sve naredbe aplikacije. 

```sh
 azure servicefabric application 
```

Dodatno možete filtrirati pomoći za određenu naredbu, kao u sljedećem primjeru:

```sh
 azure servicefabric application  create
```

Da biste omogućili Samodovršetak u na EŽA, pokrenite sljedeće naredbe:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Sljedeće naredbe povezati klaster i pokazati vam čvorove u klasteru:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Imenovani parametara, pronaći i koristiti što su, možete upisati – pomoći nakon naredbe. Ako, na primjer:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Prilikom povezivanja s većim brojem strojne grupe klaster strojno **odnosno nije dio klaster**, koristite sljedeću naredbu:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Zamijenite oznaku PublicIPorFQDN realni IP ili FQDN po potrebi. Prilikom povezivanja s većim brojem strojne grupe klaster strojno **odnosno dio klaster**, koristite sljedeću naredbu:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Možete koristiti PowerShell ili EŽA interakciju s svoj klaster za servis tkanina Linux stvorene pomoću portala za Azure. 

**Upozorenje:** Ove klastere nisu sigurne, dakle, vam možda biti otvaranje prema gore za jedan-okvir dodavanjem na javnu IP adresu u manifestu klaster.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Povezivanje na servis tkanina klaster pomoću EŽA Azure

Sljedeće naredbe Azure EŽA opisuju kako povezati sigurne klaster. Detalji o certifikatu mora podudarati certifikat na čvorove klaster.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Ako je vaš certifikat ustanovama za izdavanje certifikata (CA), morate dodati parametra – ustanove za izdavanje certifikata put kao u sljedećem primjeru: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Ako imate više ca, koristiti zarez kao graničnik.
 
Ako naziv uobičajenih na certifikatu odgovaraju krajnju točku veze, nije moguće koristiti parametar `--strict-ssl` da biste zaobišli potvrda kao što je prikazano u sljedeću naredbu: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Ako želite da biste preskočili provjeru CA, možete dodati – Odbaci neovlašteno parametra kao što je prikazano u sljedeću naredbu: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Kada povežete, moći pokrenuti drugih EŽA naredbi za interakciju s klaster. 

## <a name="deploying-your-service-fabric-application"></a>Implementacija aplikacije servisa tkanina

Izvođenje sljedeće naredbe Kopiraj, registrirati i pokretanje servisne aplikacije za tkanina:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Nadogradnja aplikacije

Postupak je sličan [proces u sustavu Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Stvaranje, kopiranje, registrirati i stvaranje aplikacija iz projekta korijenski direktorij. Ako vašoj instanci aplikacije pod nazivom tkanina: / MySFApp i vrstu MySFApp, naredbe bio na sljedeći način:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Unesite željene promjene u aplikaciju i ponovno stvoriti izmijenjene servisa.  Ažuriranje datoteka manifesta izmijenjene servisa (ServiceManifest.xml) ažurirane verzije za servis (i kod ili Config ili podatke po potrebi). S brojem ažuriranu verziju za aplikacija i servisa izmijenjenu izmijeniti manifest aplikacije (ApplicationManifest.xml).  Sada možete kopirati i registrirati ažurirane aplikacije pomoću sljedeće naredbe:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Sada možete pokrenuti nadogradnju aplikacije pomoću sljedeće naredbe:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Sada je moguće nadzirati pomoću SFX nadogradnju aplikacije. Za nekoliko minuta, aplikacija će je ažuriran.  Pokušajte ažurirane aplikacije poruku o pogrešci i provjerite funkcionalnost vraćanje automatski tkanina servisa.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopiranje paketa aplikacija nije uspjela

Ako `openssh` je instaliran. Prema zadanim postavkama, radna površina Ubuntu nema instaliran. Instalirajte ga pomoću naredbe za sljedeće:

```
 sudo apt-get install openssh-server openssh-client**
```

Ako se problem nastavi pojavljivati, pokušajte onemogućiti PAM za ssh tako da promijenite datoteku **sshd_config** pomoću sljedeće naredbe:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Ako se problem i dalje pojavljuje, povećajte broj ssh sesije izvršavanjem sljedeće naredbe:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Pomoću tipki za ssh provjere autentičnosti (umjesto lozinke) nije još podržano (jer platforme koristi ssh da biste kopirali paketa), pa upotrijebite provjeru autentičnosti lozinke.


## <a name="next-steps"></a>Daljnji koraci

Postavljanje okruženja za razvoj i implementaciju aplikacija za servis tkanina Linux klaster.
