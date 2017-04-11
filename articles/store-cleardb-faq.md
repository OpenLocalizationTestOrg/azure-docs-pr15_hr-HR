<properties
    pageTitle="Najčešća pitanja vezana uz za baze podataka ClearDB MySql sa servisom Azure aplikacije | Microsoft Azure"
    description="Odgovori na najčešća pitanja o korištenju baze podataka ClearDB MySQL sa servisom Azure aplikacije."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Najčešća pitanja vezana uz za baze podataka ClearDB MySql sa servisom Azure aplikacije

U ovim najčešćim Pitanjima navedeni odgovori na najčešća pitanja o korištenju i kupnju baze podataka ClearDB MySQL Azure web-aplikacijama.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Mogućnosti koje imaju za MySQL na Azure?

Imate nekoliko mogućnosti:

* [Baze podataka MySQL zajednički ClearDB](/marketplace/partners/cleardb/databases/)

* [Klastere ClearDB MySQL Premium](/marketplace/partners/cleardb-clusters/cluster/)

* [Klaster MySQL sustavom VM programa Azure](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Jednu instancu MySQL sustavom VM programa Azure](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB je MySQL hostiranje i upravlja MySQL infrastrukture za vas. Kada pokrenete vlastite MySQL klaster ili u bazi podataka na programa Azure virtualnog računala, morate postaviti poslužitelj MySQL i nastavak ažurirati zakrpa.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Moram li kreditne kartice za web-aplikacije + predloška MySQL u trgovine Windows Azure?

To ovisi o vrsti pretplate koju koristite. Ovo su neke vrste najčešće korištenih pretplate:

* [Plaćanje kao vam Idi](/offers/ms-azr-0003p/): zahtijeva kreditnu karticu, pa kad kupite plaćenu baze podataka MySQL se naplaćuje kreditne kartice.

* [Besplatna probna verzija](https://azure.microsoft.com/pricing/free-trial/): obuhvaća zahvale za korištenje s Microsoft Azure services, ali ne dopušta kupnju resursa drugih proizvođača. Da biste kupili servisa drugih proizvođača ili plaćenu baze podataka MySQL morate koristiti kreditnu karticu omogućen pretplate. Web-aplikacijama možete stvoriti bazom podataka MySQL BESPLATNE ClearDB.

* [Pretplate na MSDN](/pricing/member-offers/msdn-benefits/) i **MSDN razvojni Test platiti kada otvorite**: slično besplatnu probnu verziju, pretplate na MSDN morate imati kreditne kartice na kupnju plaćenu MySQL rješenje od ClearDB.

* [Ugovor Enterprise (EA)](/pricing/enterprise-agreement/): EA kupci naplatu na temelju njihove EA tromjesečju za sve svoje Nabava trgovine Windows Azure (proizvođača) na fakturi zasebne, Konsolidirani. Se naplaćuju izvan novčani izvršenja za nabavu sve trgovine. Napominjemo da u isto vrijeme iz trgovine Azure nije dostupna korisnicima koji su se prijavili Azerbajdžan, Hrvatska, Norveška i Portoriko. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): samo baze podataka BESPLATNE ClearDB možete stvoriti web-aplikacijama. Nema ograničenja broja baze podataka MySQL besplatne ClearDB možete stvoriti. Imajte na umu da besplatne baza podataka nije će se koristiti za proizvodnju web-aplikacijama kao što je ovaj servis je namijenjena samo za probnu verziju.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Zašto je naplatiti $3.50 za web-aplikacijama + MySQL iz trgovine Azure?

Zadana mogućnost baze podataka je značajke Titan iz, što je 3.50 $. Ne Pokazat ćemo trošak tijekom stvaranja baze podataka, a možete kupiti pogrešno bazu podataka koju niste namjeravate. Pokušavamo da biste pronašli način da biste poboljšali iskustvo u, ali tek onda morate potvrdite sve svoje odabrane cijene razine za web-aplikacije i baza podataka prije nego što kliknete **Stvaranje** i pokretanje implementacije resurse.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Sustav MySQL na vlastitu Azure virtualnog računala. Moguće istodobno povezati Azure web aplikacije Moji baza podataka?

Da. Web-aplikaciju programa možete povezati bazu podataka kao vaše VM Azure dao daljinski pristup na web-aplikaciju. Dodatne informacije potražite u članku [Instalacija MySQL na virtualnog računala](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>U kojem države su ClearDB Premium MySQL klastere podržane?

[ClearDB Premium MySQL klastere](/marketplace/partners/cleardb-clusters/cluster/) dostupne su u svim Azure regijama diljem svijeta osim Indija, Australija, Brazil Južna i Kini.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Je li moguće stvoriti novu klaster prije stvaranja baze podataka ClearDB premium klaster rješenjem?

Ne, stvaranje prazan klastere ClearDB nije podržana. Portal za Azure omogućuje stvaranje baze podataka u skupini koje mogu stvarati nove klaster u isto vrijeme.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Hoće li primati upozorenja pokušam da biste izbrisali baze podataka ClearDB MySQL koja se koristi za jedan svoje aplikacije?

Ne, Azure ne upozorit će vas ako izbrišete kupnju trgovine koja ovisi o aplikaciji.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Koje područja je li moguće stvoriti ClearDB baze podataka u?

Azure Marketplace nije dostupna korisnicima koji su se prijavili Azerbajdžan, Hrvatska, Norveška ili Portoriko. ClearDB nije dostupna u te regijama.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Što cijene sloju odaberite za za proizvodnju web app i baze podataka?

Korištenje Basic ili noviji sloju cijene za web-aplikacije. ClearDB, preporučujemo tarifa Saturn ili Jupiter. Pregledajte značajke i ograničenja svaki cijene sloju za [Web-aplikacije](/pricing/details/app-service/) i [baza podataka ClearDB MySQL](/marketplace/partners/cleardb/databases/) da biste odabrali onu koja odgovara vašim potrebama.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Kako nadograditi moju ClearDB bazu podataka s jedne tarife na drugu?

[Portal za Azure](https://portal.azure.com)Skaliraj kopije ClearDB zajedničke pohrane baze podataka. Pročitajte ovaj [članak](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) da biste saznali više. Trenutno ne podržavamo nadogradnju za klastere ClearDB Premium na portalu za Azure.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Ne mogu vidjeti moju ClearDB bazu podataka na portalu Azure?

Ako ćemo stvoriti bazu podataka ClearDB pomoću upravitelja resursa Azure ili [Novom portalu Azure](https://portal.azure.com), neće biti vidljiv na [Stari Azure Portal](https://manage.windowsazure.com). Da biste rada – oko to je da biste se povezali bazu podataka ručno web-aplikaciji. Na sličan način ako stvorite bazu podataka ClearDB [stari portal](https://manage.windowsazure.com) neće moći da biste vidjeli bazu podataka na [Novom portalu Azure](https://portal.azure.com). Postoji bez rada – oko potonjem scenarija.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Tko se obratiti za podršku kada je Moja baza podataka prema dolje?

Kontakt [ClearDB podrška](https://www.cleardb.com/developers/help/support) za sve baze podataka povezan problema. Budite spremni i pošaljite im uz svoje podatke za Azure pretplate.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Je li moguće stvoriti ostale korisnike za moj rješenje klaster za baze podataka ClearDB MySQL? 

ne. Ne možete stvoriti ostale korisnike, no možete stvoriti dodatne baze podataka na svoj klaster ClearDB baze podataka.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Možete Basic/Pro niz baza podataka biti nadograđena lokalno slično danas Planetary tarife portala za ClearDB?

Da, osnovni niz baze podataka može biti nadograđena na istome mjestu (osnovni 60 kroz osnovne 500). Pro niz može biti nadograđena na istome mjestu (Pro 125 do Pro 1000) osim Pro 60. Ne podržavamo trenutno nadogradnje Pro 60 baze podataka. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Kada migrirati Moje resursa iz jedne pretplate na drugi ne moje baze podataka ClearDB MySQL se migrirati i? 

Prilikom izvođenja migracije resursa preko pretplate, određena [ograničenja](./app-service-web/app-service-move-resources.md) primjenjuju. Bazom podataka ClearDB MySQL je servis drugih proizvođača i zato nisu se preneseni tijekom migracije Azure pretplate. Ako imate upravljate Migracija baze podataka MySQL prije migracije Azure resursa, baze podataka ClearDB MySQL može se onemogućiti. Ručno najprije migrirajte baze podataka programa, a zatim izvedite migracije Azure pretplatu za web-aplikacije. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Mogu li prenijeti ClearDB baze podataka iz pretplate kreditnom karticom EA pretplatu?

Postojeće baze podataka ClearDB koristite kreditne kartice pridružene postojeće pretplate. Da biste koristili pretplatu EA trebate migrirati podataka za novu bazu podataka:

* Kupite novu bazu podataka ClearDB s pretplatom EA.
* Migriranje podataka u novu bazu podataka.
* Ažuriranje aplikacije da biste koristili novu bazu podataka.
* Izbrišite stari ClearDB baze podataka.

Kada stvorite novu web-aplikaciju s MySQL (ClearDB) ili stvaranje baze podataka MySQL (ClearDB), pretplatu odaberete određuje koliko će platiti servis. Uz pretplatu na EA smo neće blokirati Nabava servisa drugih proizvođača kao što su ClearDB na portalu za Azure. EA pretplata se naplaćuju izvan novčani izvršenja i naplatiti kvartalno i arrears. Korisnička EA promijenile da biste postavili način plaćanja, primjerice kreditne kartice za plaćanje servisi trgovine drugih proizvođača.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Gdje se naknade za ClearDB resursa u pretplatu na EA mogu vidjeti?

Za kupce Izravni EA naknade za Azure Marketplace su vidljive na portalu Enterprise. Imajte na umu da sve trgovine nabavu i potrošnje se naplaćuju izvan novčani izvršenja i naplatiti tromjesečno i arrears. EA korisnici morati platiti izravno davateljima usluga drugih proizvođača, a možete učiniti tako da omogućite način plaćanja, primjerice kreditne kartice sa svojim računom EA.

INDIRECT EA klijentima možete pronaći svoje pretplate trgovine Windows Azure na stranici **Upravljanje pretplatama** portala za Enterprise, ali cijene je skriven. Korisnici potrebno obratiti njihove LSP-a informacije o naknade trgovine.

Pristup servisu Azure Marketplace za servisa drugih proizvođača upravlja se vaš administratori registraciju EA Azure. Možete onemogućivanju i ponovnom omogućivanju pristupa 3 Nabava sudionika iz trgovine u Upravljanje korisničkim računima i pretplate u odjeljku računi na portalu za Enterprise.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Tko se obratiti za pitanja o računa za servise ClearDB u EA pretplate?

Koja se odnosi na naplata u odjeljku njihove EA registraciju, obratite se [Službi za Enterprise](http://aka.ms/AzureEntSupport) . Portal za podršku EA će odgovor na pitanje ili riješiti problem.

 



## <a name="more-information"></a>Dodatne informacije

[Najčešća pitanja vezana uz Azure Marketplace](/marketplace/faq/)
