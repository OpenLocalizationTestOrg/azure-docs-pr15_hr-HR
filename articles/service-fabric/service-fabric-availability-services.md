<properties
   pageTitle="Dostupnost servisa tkanina servisa | Microsoft Azure"
   description="U članku se opisuje kvara otkrivanje, prebacivanje i oporavak za servise"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="availability-of-service-fabric-services"></a>Dostupnost servisa tkanina servisa
Azure servisa tkanina services može biti s praćenjem stanja ili bez praćenja stanja. U ovom se članku daje pregled kako servisa tkanina održava dostupnost usluge u slučaju pogreške.

## <a name="availability-of-service-fabric-stateless-services"></a>Dostupnost servisa tkanina bez praćenja stanja servisa
Bez praćenja stanja servisa je servis za aplikacije koje sadrže sve [lokalne stalni stanje](service-fabric-concepts-state.md).

Stvaranje bez praćenja stanja servisa zahtijeva definiranje instance broj, što je broj instanci bez praćenja stanja servisa koji treba imati u klasteru. To je broj primjeraka logike aplikacije koji će se instancirati u klasteru. Povećava broj instanci je preporučeni način promjene veličine gore bez praćenja stanja servisa.

Kada se na kvara otkrije na bilo kojoj instanci bez praćenja stanja servisa, novu instancu stvara se na neki drugi uvjete čvor u klasteru.

## <a name="availability-of-service-fabric-stateful-services"></a>Dostupnost servisa tkanina s praćenjem stanja servisa
S praćenjem stanja servisa sadrži neke stanje pridružen. U tkanina servisa, kao skup replike katalog je modeliran s praćenjem stanja servisa. Svaki replike je instanca kod usluge koja ima kopiju stanja. Čitanje i pisanje operacije se izvode na jedan replike (naziva se primarni). Promjena stanja iz pisanje operacije su *replicirati* na više drugih replike (naziva se aktivni secondaries). Kombinacija primarnih i aktivni sekundarnog replike je skup replike servisa.

Može imati samo jedan primarni replike održavanje za čitanje i pisanje zahtjeve, ali može biti više aktivni sekundarnog replike. Broj aktivnog sekundarnog replike je konfigurirati i veći broj replike možete tolerate veći broj Istodobni softvera i hardvera pogreške.

U slučaju kvara (kada je funkcionira primarni replike) tkanina servisa postaje jedan aktivni sekundarnog replike nove primarni replike. U ovom aktivni sekundarnog replike već ima ažuriranu verziju stanje (putem *replikacije*), a možete nastaviti obradu daljnje čitanje i pisanje operacije.

Tog koncepta – replike primarni ili Aktivno sekundarni – zove replike ulogu.

### <a name="replica-roles"></a>Uloge replike
Uloga kopiju služi za upravljanje životnog ciklusa stanja upravlja te replike. Replike čiju ulogu je primarni services pročitajte zahtjeva. Zahtjevi za pisanje je services i ažuriranje stanje i replikaciju promjene na aktivni secondaries u skupu njegov replike. Uloga Aktivni sekundarni je primanje stanje promjene koje je replicirati primarni replike i ažuriranje njezin prikaz stanja.

>[AZURE.NOTE] Više razine programiranje modele, primjerice [pouzdanog Glumci framework](service-fabric-reliable-actors-introduction.md) apstraktnog Odsutan pojam replike uloga proizvođača.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o servisa tkanina koncepata pogledajte sljedeće:

- [Skalabilnost servisa tkanina servisa](service-fabric-concepts-scalability.md)

- [Stvaranje particija servisa tkanina services](service-fabric-concepts-partitioning.md)

- [Definiranje i upravljanje stanja](service-fabric-concepts-state.md)
