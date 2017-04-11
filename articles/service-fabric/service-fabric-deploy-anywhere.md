<properties
   pageTitle="Stvaranje klastere tkanina servisa Azure Windows Server i Linux | Microsoft Azure"
   description="Servis tkanina klastere pokrenuti Windows Server i Linux, što znači da ćete moći uvesti i bilo kojeg mjesta aplikacije tkanina servis glavnog računala možete pokrenuti Windows Server ili Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Stvaranje klastere tkanina servisa Windows Server ili Linux

Azure tkanina servisa omogućuje stvaranje klastere tkanina servisa na bilo kojem VMs ili računala sa sustavom Windows Server ili Linux. To znači da se moći uvesti i pokretanja aplikacije servisa tkanina u bilo kojem okruženju koje se skup Windows Server ili Linux računala koja se međusobno, biti informacije o lokalnom Microsoft Azure i s bilo kojeg davatelja usluga oblaka.

##<a name="create-service-fabric-clusters-on-azure"></a>Stvaranje klastere tkanina servisa na Azure

Stvaranje klaster Azure Završi ili putem predloška modela resursa ili Azure portal. Dodatne informacije pročitajte [Stvaranje klaster tkanina servis pomoću predloška Voditelj resursa](service-fabric-cluster-creation-via-arm.md) ili [Stvaranje klaster tkanina servisa na portalu Azure](service-fabric-cluster-creation-via-portal.md) .

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Podržani operacijski sustavi za klastere na Azure

Prikazuje vam se da biste stvorili klastere na VMs izvodi tih operacijskih sustava:

* Windows Server 2012 R2
* Windows Server 2016 (kada je objaviti kao načelu dostupan)
* Linux Ubuntu 16.04 (u javnom pretpregled) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Stvaranje servisa tkanina samostalne klastere na lokaciji ili s bilo kojeg davatelja usluga oblaka

Servis tkanina omogućuje paketa za instalaciju za stvaranje samostalne servisa tkanina klastere lokalnog ili na bilo kojem davatelju usluga oblaka

Dodatne informacije o postavljanju klastere samostalne tkanina za servisa u sustavu Windows Server, pročitajte [Stvaranje klaster tkanina servisa za Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Bilo koji implementacijama oblaka nasuprot lokalnim implementacijama
Postupak za stvaranje servisa tkanina klaster lokalnog je slična postupak stvaranja klaster na bilo kojem oblaka po izboru sa skupom VMs. Početna korake Dodjela na VMs mjerodavni su od davatelja oblaka ili lokalnog okruženja koji koristite. Nakon skup VMs imaju omogućeno između njih je veza s mrežom, zatim korake da biste postavili paket servisa tkanina uređivanje postavki klaster i pokretanje stvaranja klaster i upravljanje skripte jednake. Na taj način znanja i sučelje radi upravljanja klastere tkanina servisa i je li moguće prenijeti kada se odlučite za ciljnu novi hostinga okruženja.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Prednosti stvaranje samostalne servisa tkanina klaster
* Vi ste slobodni da biste odabrali bilo kojem davatelju usluga oblaka za hostiranje svoj klaster.
* Aplikacije servisa tkanina jednom napisali, može se pokrenuti u više hostinga okruženjima s minimalnim bez promjene.
* Poznavanje stvaranje aplikacija servisa tkanina prenosi iz jednog okruženja za hosting na drugi.
* Radu doživljaj operacijski sustav, a zatim upravljanje servisa tkanina klaster prenosi putem iz jednog okruženja u drugu.
* Općenite kupca razgovor je neograničeno hosting ograničenja okruženju.
* Dodatni sloju pouzdanosti i zaštitu od Primamo kvarove postoji jer možete premjestiti servisa drugog okruženje za implementaciju ako davatelj centar ili oblačić podataka na blackout.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Podržani operacijski sustavi za klastere samostalni
Prikazuje vam se da biste stvorili klastere na VMs ili računala s programom tih operacijskih sustava:

* Windows Server 2012 R2
* Windows Server 2016 (kada je objaviti kao načelu dostupan)
* Linux (uskoro dostupno)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Prednosti servisa tkanina klastere na Azure putem samostalne servisa tkanina klaster stvorili lokalnog

Sustavom klastere tkanina servisa Azure nudi prednosti u odnosu na lokalni mogućnost, pa ako nemate potrebe za gdje pokrenete klastere, a zatim predlažemo da ih pokrenete na Azure. Na Azure, dajemo Integracija druge Azure značajke i servise koji olakšava operacije i upravljanje klaster jednostavnije i više pouzdan.

* **Azure portal:** Portal za Azure olakšava stvaranje i upravljanje klastere.

* **Azure Voditelj resursa:** Korištenje Azure Voditelj resursa omogućuje jednostavno upravljanje svih resursa koji se koristi kao jedinica klaster i pojednostavljuje praćenja cijena i naplatu.
* **Servis tkanina klaster kao Azure resurs** Servis tkanina klaster je resursa u OBLAK tako da možete je model kao ostali resursi OKVIRA u Azure.
* **Integracija s infrastruktura za Azure** Servis tkanina koordinate s podlozi Azure infrastrukture za OS, mreže i druge nadogradnje da biste poboljšali dostupnosti i pouzdanosti aplikacija.  
* **Dijagnostiku:** Na Azure, dajemo Integracija Azure Dijagnostika i analize zapisnika.
* **Automatsko skaliranje:** Za klastere na Azure dajemo ugrađene funkcije automatsko skaliranje zbog skaliranje skupove virtualnog računala. Lokalni i u drugim oblaka okruženju morate sastavljanje vlastite automatsko skaliranje značajka skaliranje ili ručno pomoću API-ji servisa tkanina izlaže skaliranja klastere.

## <a name="next-steps"></a>Daljnji koraci
Stvaranje klaster na web-mjesto VMs ili u okvir za računala sa sustavom Windows Server: [Stvaranje klaster tkanina servisa za Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Stvaranje klaster na web-mjesto VMs ili u okvir za računala sa sustavom Linux: [Tkanina servisa na Linux](service-fabric-linux-overview.md)
