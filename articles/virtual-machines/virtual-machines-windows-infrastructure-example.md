<properties
    pageTitle="Primjer infrastrukture vodič | Microsoft Azure"
    description="Saznajte više o ključa dizajna i implementaciju smjernice za implementaciju infrastruktura za primjer servisu Azure."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Primjer Azure infrastrukture vodič

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

U ovom se članku vodi kroz sastavnih out infrastruktura za aplikacije u primjeru. Ne možemo detaljno Osmišljavanje do infrastrukture za jednostavne mrežno pohrane koja objedinjuje smjernice i odluke oko konvencije imenovanja, skupovi dostupnost, virtualne mreže i učitavanje balancers i zapravo implementacija virtualnim strojevima (VMs).


## <a name="example-workload"></a>Primjer radno opterećenje

Adventure Works ciklusa želi da biste sastavili aplikaciju mrežno spremište u Azure koji se sastoji od:

- Dva IIS poslužitelje s programom sučelja u web razina klijenta
- Dva IIS poslužitelja obradu podataka i narudžbe u je razina aplikacije
- Dvije instance grupe dostupnosti AlwaysOn (dva SQL poslužitelja i zrcaljenja za čvor Većina) za pohranu podataka o proizvodima i narudžbe u sloju baze podataka Microsoft SQL Server
- Dva kontrolera domene servisa Active Directory za račune kupca i dobavljače u sloju je provjera autentičnosti
- Na poslužiteljima na kojima se nalaze u dva podmreže:
    - pristupne podmreže za web-poslužiteljima 
    - pozadinske podmreže za poslužitelja aplikacije, SQL klaster i kontrolera domena

![Dijagram različite razine za infrastrukturu aplikacije](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Dolazni sigurno web promet mora biti raspoređivačem među web-poslužiteljima kao klijentima potražite u mrežno spremište. Redoslijed obrade promet u obliku HTTP zahtjeve s weba poslužitelja mora biti raspoređen među poslužitelje aplikacije. Uz to, Infrastruktura mora biti namijenjena visoke dostupnosti.

Uključivanje morate dobivene dizajna:

- Azure pretplate i račun
- Grupa jedan resursa
- Računi za pohranu
- Virtualne mreže s dva podmreže
- Dostupnost skupovi za VMs slične uloge
- Virtualnim strojevima

Sve navedenog slijedite ove konvencije imenovanja:

- Adventure Works ciklusa koristi **[IT radno opterećenje]-[mjesta]-[Azure resursa]** kao prefiks
    - U ovom primjeru "**azos**" (trgovina Azure mrežni) je naziv radno opterećenje IT i "**Korištenje**" (Istočni sad 2) je mjesto
- Računi za pohranu koriste adventureazosusesa**[opis]**
    - 'adventure' dodan je prefiks koji će se unijeti jedinstvenosti i imena pohranu računa ne podržavaju pomoću spojnice.
- Virtualne mreže pomoću AZOS korištenje VN**[broj]**
- Dostupnost aparata koristi azos – korištenje-kao-**[uloga]**
- Nazivi virtualnog računala koriste azos – korištenje-vm -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Azure pretplate i računa

Adventure Works ciklusa koristi pretplate Enterprise, pod nazivom Adventure Works Enterprise pretplatu, možete unijeti naplatom za ovog IT posla.


## <a name="storage-accounts"></a>Računi za pohranu

Adventure Works ciklusa određuje da su potrebne dva računa za pohranu:

- **adventureazosusesawebapp** za standardne prostora za pohranu web-poslužiteljima, poslužitelji aplikacija i kontrolera domena i njihovih podataka diskova.
- **adventureazosusesasql** za pohranu Premium VMs za SQL Server i njihovih podataka diskova.


## <a name="virtual-network-and-subnets"></a>Virtualne mreže i podmreže

Jer virtualne mreže potrebna tijeku veza s lokalne mreže Adventure rad ciklusa, oni odlučili na samo oblak virtualne mreže.

Oni samo oblak virtualne mreže stvorenih pomoću portala za Azure sljedeće postavke:

- Naziv: AZOS – korištenje-VN01
- Lokacija: Istočni sad 2
- Prostor za adrese virtualne mreže: 10.0.0.0/8
- Prvi podmreže:
    - Naziv: sučelju
    - Adresa prostora: 10.0.1.0/24
- Drugi podmreže:
    - Naziv: pozadinskog
    - Adresa prostora: 10.0.2.0/24


## <a name="availability-sets"></a>Skupovi dostupnosti

Da biste zadržali visoke dostupnosti sve četiri razine, njihov mrežno spremišta, Adventure Works ciklusa odlučili na četiri skupa dostupnost:

- **azos koristi kao web** potražite na web-poslužiteljima
- **azos koristite kao aplikaciju** za poslužitelje aplikacije
- **azos koristi kao sql** za SQL Server
- **azos koristi kao kontroler** za kontrolera domena


## <a name="virtual-machines"></a>Virtualnim strojevima

Adventure Works ciklusa odlučili na sljedeća imena za svoje Azure VMs:

- **azos – korištenje-vm-web01** za prvi web-poslužitelj
- **azos – korištenje-vm-web02** za drugi web-poslužitelj
- **azos – korištenje-vm-app01** za prvi poslužitelj aplikacije
- **azos – korištenje-vm-app02** za drugi poslužitelj aplikacije
- **azos – korištenje-vm-sql01** za prvi poslužitelj sustava SQL Server u skupine
- **azos – korištenje-vm-sql02** za drugi poslužitelj sustava SQL Server u skupine
- **azos – korištenje-vm-dc01** za prvu kontrolerom domene
- **azos – korištenje-vm-dc02** za drugi kontrolerom domene

Evo dobivene konfiguracije.

![Konačni aplikacije infrastrukture implementiran u Azure](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Tu konfiguraciju sadrži:

- Samo oblak virtualne mreže s dva podmreže (sučelju i pozadinskog)
- Dva računa za pohranu
- Četiri skupa dostupnost jedan za svaki sloju mrežno spremišta
- Virtualnim računalima sustava četiri razine
- Skup vanjskih rasporediti opterećenje za web-mjesto utemeljeno na HTTPS promet s Interneta na web-poslužiteljima
- Interna učitavanja raspoređen postavljanje za šifrirane web promet s web-poslužitelja poslužitelje aplikacije
- Grupa jedan resursa


## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 