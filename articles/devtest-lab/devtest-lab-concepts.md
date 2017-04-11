<properties
    pageTitle="DevTest Labs koncepata | Microsoft Azure"
    description="Naučite osnovni koncepti DevTest Labs i kako ga možete jednostavno stvaranje, upravljanje i praćenje Azure virtualnim strojevima"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

#<a name="devtest-labs-concepts"></a>DevTest Labs koncepti

> [AZURE.NOTE]
> Ovaj je članak dio 3 od 3 praktičnih:
> 
> 1. [Što je DevTest Labs?](devtest-lab-overview.md)
> 1. [Zašto DevTest Labs?](devtest-lab-why.md)
> 1. **[DevTest Labs koncepti](devtest-lab-concepts.md)**

##<a name="overview"></a>Pregled

Sljedeći popis sadrži koncepata DevTest Labs i definicija:

##<a name="artifacts"></a>Artefakte
Artefakte koriste se za uvođenje i konfigurirali aplikaciju nakon što je VM je dodijeljeni resursi. Artefakte mogu biti:

- Alati koji želite instalirati na VM – kao što su agenata, Fiddler i Visual Studio.
- Akcije koje želite pokrenuti na VM – kao što su kloniranje na repo.
- Aplikacije koje želite testirati.

Artefakte su [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md) JSON datoteke koje sadrže upute za izvođenje implementacije i Primjena konfiguracije. 

##<a name="artifact-repositories"></a>Artefakt spremišta
Artefakt spremištima su brojka spremištima gdje su artefakte prijavljene. Isti artefakt spremištima mogu dodati više labs u tvrtki ili ustanovi Omogućivanje ponovnog korištenja skretnice i zajedničko korištenje.

## <a name="base-images"></a>Osnovni slike
Osnovni slike su VM slike s alatima i postavke predinstaliran i konfigurirali da biste brzo stvorili na VM. Možete Dodjela resursa u VM odabira postojeće baze i dodavanjem artefakt da biste instalirali agent za testiranje. Dodjela resursa VM pa možete spremiti kao osnovu tako da se bez potrebe za ponovno instalirajte test agent za svaki dodjeljivanje na VM može se koristiti bazu.

##<a name="formulas"></a>Formula 
Formule, osim osnovni slike nude mehanizam za dodjelu resursa VM za brzo. Formula u DevTest Labs je popis vrijednosti nekretnina zadanom koristi za stvaranje Laboratorija VM. S formulama, VMs uz isti skup svojstava – kao što su osnovni slike, veličina VM, virtualne mreže i artefakte – možete stvoriti bez potrebe za određivanje tih svojstava svaki put. Kada stvarate na VM dobiti formulom, zadane vrijednosti mogu se koristiti kao-je ili izmijenjene.

##<a name="caps"></a>Tipka CAPS LOCK
Tipka CAPS LOCK je mehanizam da biste minimizirali otpad u vašem Laboratorija. Ako, na primjer, možete postaviti kapaciteta da biste ograničili broj VMs koje je moguće stvoriti po korisniku ili na Laboratorija.

##<a name="policies"></a>Pravila
Pomoć za pravila u kontroliranje trošak u vašem Laboratorija. Na primjer, možete stvoriti pravila da biste automatski isključili VMs koji se temelji na definirani raspored.

##<a name="security-levels"></a>Razine sigurnosti
Sigurnosni pristup određuje po Azure Role-Based pristup kontrola (RBAC). Da biste shvatili funkcioniranje programa access, lakše je razlike između dozvole, uloge i opseg kako je definirano parametrom RBAC. 

- Dozvole – dozvole je definirani pristup određene akcije (npr. – pristup za čitanje sve virtualnim strojevima). 
- Uloga - uloge je skup dozvola koje se mogu grupirati i Dodijeljeno korisniku. Na primjer, *vlasnik pretplate* uloga ima pristup svim resursima unutar pretplatu. 
- Opseg - opseg je razine u hijerarhiji Azure resursa – kao što su grupe resursa, jedan Laboratorija ili cijelu pretplatu).
 
Unutar opseg DevTest Labs, postoje dvije vrste uloga da biste definirali korisničkih dozvola: Laboratorija vlasnik i korisnik Laboratorija.

- Vlasnik Laboratorija - Laboratorija vlasnik ima pristup nikakve resurse unutar na Laboratorija. Zbog toga Laboratorija vlasnik možete izmijeniti pravila, čitanje i pisanje sve VMs, promjena virtualne mreže, itd. 
- Korisnik Laboratorija - korisnik Laboratorija možete prikazati sve Laboratorija resurse, kao što su VMs, pravila i virtualne mreže, ali ne možete mijenjati pravila ili sve VMs stvorili drugi korisnici. 


Da biste vidjeli kako stvoriti prilagođene uloge u DevTest Labs, potražite u članku [Dodjela korisničkih dozvola na određene Laboratorija pravila](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Budući da korisnik ima dozvole u određenim dosegu su hijerarhijski, rasponi, oni se automatski dobiti te dozvole u svakoj dosegu niže razine encompassed. Ako, primjerice, ako se korisniku Dodijeli ulogu vlasnik pretplate, zatim imaju pristup svim resursima u pretplatu, što obuhvaća sve virtualnim strojevima, sve virtualne mreže i sve labs. Stoga vlasnik pretplate automatski nasljeđuje ulogu Laboratorija vlasnik. Međutim, suprotno nije ispunjen. Vlasnik Laboratorija ima pristup Laboratorija koji je opseg manju od razinu pretplate. Stoga vlasnik Laboratorija će moći vidjeti virtualnim strojevima ili virtualne mreže ili bilo kojeg resursa koje su izvan na Laboratorija.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

##<a name="next-steps"></a>Daljnji koraci

[Stvaranje na Laboratorija u DevTest Labs](devtest-lab-create-lab.md)