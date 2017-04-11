<properties
    pageTitle="Aplikacije servisa za Azure tarife detaljnije pregled | Microsoft Azure"
    description="Saznajte kako se aplikacije servisa za tarife za posao aplikacije servisa za Azure te objašnjenje kako mogu pridonijeti sučelje za upravljanje."
    keywords="aplikacije servisa, azure aplikacije servisa, mjerilo skalabilni, plan aplikacije servisa, trošak servisa aplikacija"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Aplikacije servisa za Azure tarife detaljnije pregled#

Tarifu aplikacije servisa predstavlja skupa značajki i kapacitet koji možete omogućiti zajedničko korištenje svim više aplikacijama. Pokrenite tarifu aplikacije servisa za web-aplikacije, mobilne aplikacije, funkcija aplikacije ili API aplikacije, [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714) sve. Planova podržava pet cijene razine: *slobodno*, *zajedničko korištenje*, *Basic*, *standardni*i *Premium*. Svaki sloju ima vlastite mogućnosti i kapacitet. Aplikacija u iste pretplate te geografsku lokaciju možete zajednički koristiti plan. Sve aplikacije plan za zajedničko korištenje možete koristiti sve mogućnosti i značajke koji su definirani u planu sloju. Sve aplikacije koje su vezane uz plan se izvoditi na resurse koji definira plan.

Ako, na primjer, ako vaša tarifa je konfiguriran za korištenje dvije instance "mala" u sloju standardnog servisa, sve aplikacije koje su vezane uz taj plan pokrenuti na oba i pristup sloju funkciji standardnog servisa. Planiranje instance koristite aplikacije su potpuno upravljanih i vrlo dostupna.

U ovom se članku istražuje ključa karakteristike, primjerice sloju i skaliranje tarifu aplikacije servisa i kako se isporučuju u Reproduciraj tijekom upravljanja aplikacijama.

## <a name="apps-and-app-service-plans"></a>Aplikacije i aplikacije servisa za tarife

Aplikaciju u aplikacije servisa za mogu se pridružiti samo jedan aplikacije servisa za planiranje u bilo kojem trenutku.

Aplikacije i tarife nalaze se u grupu resursa. Grupa resursa služi kao životni ciklus granicu za svaki resurs koji se nalazi unutar njega. Grupa resursa možete koristiti da biste upravljali sve dijelove aplikacija zajedno.

Jer je grupa jedan resursa možete imati više aplikacije servisa za tarife, može dodijeliti različite aplikacije na drugu fizičke resurse. Na primjer, možete razdvojiti resursi među razvojni, testiranje i radnog okruženja. Imate zasebnom okruženja za proizvodnju i razvojni i testiranje možete izdvojiti resursi. Na taj način učitavanja testiranje u odnosu na novu verziju aplikacije ne se natječu za iste resurse kao radnog aplikacija, koji su posluživanje realni klijentima.

Ako imate više tarifa u grupi pojedinačni resurs, možete definirati i aplikacije koja obuhvaća zemljopisnim područjima. Ako, na primjer, iznimno dostupnih aplikacija izvodi u dva područja sadrži najmanje dva tarife, jedan za svaku regiju i jednu aplikaciju pridružen svakom tarifi. U takvim situacija sve kopije aplikaciju pa nalaze se u grupi jedan resurs. Imate grupu resursa pomoću više tarife i aplikacija za više olakšava upravljanje, upravljanje i prikaz stanja aplikacije.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Stvaranje tarifu aplikacije servisa ili korištenje postojeće

Kada stvorite aplikaciju, razmislite o stvaranju grupu resursa. S druge strane, ako je aplikaciju koju ćete stvoriti komponenta za veće aplikaciju, aplikacije trebalo stvoriti unutar grupa resursa dodijeljen za tu aplikaciju za veće.

Hoće li se nova aplikacija je potpuno novu aplikaciju ili dio veći jedan, možete koristiti postojećeg plana aplikacije servisa za vođenje ga ili stvorite novi. Ova se odluka je više pitanje kapacitetu i očekivani opterećenja.

Ako nova aplikacija će koristiti mnogo resursa i imaju različite skaliranja čimbenika iz drugih aplikacija smješten u postojeću tarifu, preporučujemo da vam izdvojiti u vlastiti plan.

Prilikom stvaranja plana možete dodijeliti novi skup resursa za aplikacije i dobiti veću kontrolu nad Dodjela resursa jer se svaki plan dobiti vlastiti skup instance.

Jer aplikacija možete premjestiti sve tarife, možete promijeniti način na koji će se Resursi raspoređivati preko veći aplikacije.

Na kraju, ako želite stvoriti aplikaciju u nekoj drugoj regiji, a to područje nema postojećeg plana, stvorite plan u tom području da biste mogli hostirati pokrenite aplikaciju.

## <a name="create-an-app-service-plan"></a>Stvaranje tarifu aplikacije servisa

>[AZURE.TIP] Ako imate okruženju servisa aplikacija možete pregledati u dokumentaciji specifične za aplikaciju servisa okruženja u kojima se ovdje: [Stvaranje programa aplikacije servisa Plan u okruženju servisa aplikacija](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Možete stvoriti prazan plan za aplikacije servisa za sučelje za pregled aplikacije servisa za planiranje ili kao dio stvaranja aplikacija.

[Portal za Azure](https://portal.azure.com)kliknite **Novo** > **Web + mobile**, i zatim odaberite **Web-aplikacije** ili drugu aplikaciju aplikacije servisa za.
![Stvaranje aplikacije za Azure portalu.][createWebApp]

Zatim možete odabrati ili stvaranje plana aplikacije servisa za nove aplikacije.

 ![Stvorite plan sustava aplikacije servisa.][createASP]

Da biste stvorili novu tarifu aplikacije servisa, kliknite **[+] Stvori novo**, upišite naziv **aplikacije servisa za planiranje** i zatim odaberite odgovarajuće **mjesto**. Kliknite **određivanje cijena sloju**, a zatim odgovarajuću cijene sloju za servis. Odaberite **Prikaži sve** da biste vidjeli dodatne mogućnosti cijene, kao što su **besplatno** i **zajedničko korištenje**. Nakon što ste odabrali cijene sloju, kliknite gumb **Odaberi** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Premještanje aplikacije na drugu tarifu aplikacije servisa

Aplikaciju možete premjestiti na tarifu za drugu aplikaciju servisa [Azure portal](https://portal.azure.com). Aplikacije možete premještati između tarife dok god planovi nalaze u istoj grupi resursa i zemljopisnom području.

Da biste premjestili aplikaciju na neku drugu tarifu, otvorite aplikaciju koju želite premjestiti. Na izborniku **Postavke** potražite **Plan za promjenu aplikacije servisa**.

**Promjena tarifa za servis aplikacije** otvorit će se izbornik **aplikacije servisa za planiranje** . Sada možete odabrati postojeću tarifu ili stvorite novi. Prikazuju se samo valjani tarife (u istoj grupi resursa i geografski).

![Aplikacije servisa za planiranje birača.][change]

Svaki tarifa sadrži vlastitu cijene sloju. Ako, na primjer, kada prijeđete na web-mjesto iz besplatne sloju za standardne sloj aplikacije sada možete koristiti sve značajke i resursima standardne sloju.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Kloniraj aplikacije na drugu tarifu za aplikacije servisa
Ako želite da biste premjestili aplikaciju na drugo područje, jedan zamjenski je aplikacija kloniranje. Kloniranje čini kopiju aplikacije u novu ili postojeću aplikacije servisa za plan ili okruženje za aplikacije servisa za sve regije.

 ![Kloniraj aplikacije.][appclone]

**Kloniraj aplikaciju** možete pronaći na izborniku **Alati** .

Kloniranje ima određena ograničenja koja možete pročitati na [kloniranje aplikacije servisa za aplikaciju Azure pomoću portala za Azure](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Promjena veličine tarifu aplikacije servisa

Da biste skalirali plan na tri načina:

- **Promjena u planu cijene sloju**. Ako, na primjer, na tarifu iz osnovne sloju moguće pretvoriti u standardni prikaz ili Premium sloju, a sve aplikacije koje su vezane uz taj plan sada možete koristiti značajke koje nudi novog servisnog sloja sustava.
- **Promjena veličine u planu instance**. Kao primjer tarifu u sloju osnovni koje koristi mali instance možete promijeniti da biste koristili velike instance. Sve aplikacije koje su vezane uz taj plan sada možete koristiti dodatnu memoriju i resursa procesora koji nudi veći instance.
- **Promijenite broj instanci u planu**. Ako, na primjer, standardne plan koji je neproporcionalno odgovor na tri instance možete skalirana na 10 instance. Premium plan možete skalirana odgovor na 20 instance (primjenjuje dostupnost). Sve aplikacije koje su vezane uz taj plan sada možete koristiti dodatnu memoriju i resursa procesora koji nudi veći broj instanci.

Cijene sloju i instancu veličine možete promijeniti klikom na stavku **Skaliranje prema gore** u odjeljku postavke za aplikaciju ili aplikacije servisa za planiranje. Promjene primjenjuju se na željenu tarifu aplikacije servisa i utječe na sve aplikacije koje se hostira.

 ![Postavljanje vrijednosti skaliranja aplikacije.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Planiranje usluga čišćenje aplikacije
**Aplikacije servisa za tarife** koje nema aplikacije povezane s njima su i dalje plaćati naknada jer nastavljaju rezerviranje kapaciteta računalnim konfiguriran u svojstvima aplikacije servisa za planiranje mjerilo.
Da biste izbjegli neočekivane naknade nakon brisanja aplikaciju zadnjeg smješten u tarifu aplikacije servisa, rezultirajući prazan aplikacije servisa za planiranje briše se i.


## <a name="summary"></a>Sažetak

Aplikacije servisa za tarife predstavljaju skupa značajki i kapacitet koji možete zajednički koristiti putem aplikacije. Tarife za aplikacije servisa za što je fleksibilnosti dodijeliti određeni aplikacije da bi se skup resursa i optimizirati vaše Upotreba Azure resursa. Na taj način, ako želite spremiti novac o okruženju sustava testiranja možete zajednički koristiti plan preko više aplikacija. Možete i Maksimiziraj propusnost okruženju sustava radnog po skaliranje preko više područja i tarife.

## <a name="whats-changed"></a>Što se promijenilo

* Vodič za promjenu iz aplikacije servisa za web-mjestima, potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
