<properties
    pageTitle="Azure Active Directory Domain Services: Scenariji za implementaciju | Microsoft Azure"
    description="Scenariji za implementaciju za Azure servisa Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Scenariji za implementaciju i korištenje slučajeva
U ovom ćete odjeljku smo pogledajte nekoliko scenariji i korištenje slučajeve koje prednosti servisa Azure Active Directory (AD) domene.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Sigurna, jednostavno Administracija Azure virtualnih računala
Azure Active Directory Domain Services možete koristiti da biste upravljali Azure virtualnim strojevima pojednostavnjeno način. Azure virtualnim strojevima možete biti pridruženo upravljanih domenu zato što vam omogućuje da koristite tvrtke AD vjerodajnice za prijavu. Taj se način olakšava izbjegavanje gnjavaže upravljanje vjerodajnicama kao što su održavanje lokalne administratorske račune za svaku od Azure virtualnim strojevima sa sustavom.

Poslužitelj virtualnim strojevima pridruženim upravljanih domene možete upravlja i zaštićen pravila grupe. Možete primijeniti referente vrijednosti potrebno sigurnosno Azure virtualnim strojevima i ih zaključati skladu smjernice sigurnosti za tvrtku. Ako, na primjer, možete koristiti mogućnosti upravljanja pravilnika grupe da biste ograničili vrste aplikacije koje možete pokrenuti na te virtualnim računalima.

![Pojednostavnjeno Administracija Azure virtualnih računala](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Kao što je poslužitelja i druge infrastrukture dođe do završetka život, Contoso pomiče mnoge aplikacije koje se trenutno hostira lokalno u oblak. Svoje trenutne standardna IT mandates poslužiteljima kojima su smještene tvrtke aplikacije mora domene pridruženo i upravljanog putem pravila grupe. Contoso je IT administrator želi domene spoj virtualnim strojevima implementiran u Azure, da biste olakšali Administracija. Zbog toga administratorima i korisnicima možete prijaviti pomoću vjerodajnica za tvrtke. U isto vrijeme strojeva moguće je konfigurirati u skladu sa referente vrijednosti potrebno sigurnosno putem pravila grupe. Contoso radije da ne implementacije, praćenje i upravljanje kontrolera Azure sigurnost Azure virtualnih računala. Stoga Azure servisa Active Directory Domain Services je sjajno odgovara slučaj koristi.

**Uvođenje bilješke**

Imajte na umu sljedeće važne detalje za taj scenarij implementacije:

- Upravljani domene nudi Azure servisa Active Directory Domain Services dokumentu daju strukturu OU (Organizacijska jedinica) o jednom paušalni prema zadanim postavkama. Svim računalima domene pridružite se nalaziti u jednom paušalni OU. No možete stvoriti prilagođeni organizacijske jedinice.

- Azure servisa Active Directory Domain Services podržava jednostavne pravilnik grupe u obliku ugrađene GPO svaki za korisnike i računala spremnika. Nije moguće ciljani GP OU/odjel, izvođenje WMI filtriranja ili stvorite prilagođeni GPO.

- Azure servisa Active Directory Domain Services podržava osnovnu AD računalo objekt shemi. Nije moguće proširiti sheme računalni objekt.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Dizalica-i-shift lokalnog aplikacije koja koristi provjeru autentičnosti vezanja LDAP Azure infrastrukture servisima

![LDAP vezanja](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso je lokalnog aplikaciju koja je kupljena na programa Neovisni prije mnogo godine. Aplikacija je u načinu za održavanje tako da na Neovisni i traži promjene aplikacija je previsok za tvrtke Contoso. Ove aplikacije sadrži koji se temelji na web sučelju koji prikuplja korisničke vjerodajnice pomoću web-obrasca, a zatim potvrđuje korisnici izvođenjem programa LDAP Poveži sa tvrtke Active Directory. Contoso željeli migrirati ovu aplikaciju servisa Azure infrastrukture. To je poželjno aplikacija funkcionira kao što je bez promjene. Uz to, korisnici moraju biti moguće provjeriti autentičnost pomoću vjerodajnica za postojeće tvrtke i bez potrebe za retrain korisnicima postupiti drukčije. Drugim riječima, krajnjim korisnicima mora biti oblivious gdje je pokrenuti aplikaciju, a migracije mora biti prozirni na njih.

**Uvođenje bilješke**

Imajte na umu sljedeće važne detalje za taj scenarij implementacije:

- Provjerite je li aplikacija nije potrebna za izmjenu/pisanja u direktorij. Pristup zapisivanju LDAP upravljanih domene nudi Azure servisa Active Directory Domain Services nije podržana.

- Ne možete promijeniti lozinke izravno u odnosu na upravljanih domene. Krajnji korisnici mogu promijeniti svoju lozinku ili pomoću Azure AD samostalno promjena mehanizam prema lokalnog imenika. Te promjene se automatski sinkronizirane i dostupni upravljani domene.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Dizalica i shift lokalnog aplikacije koja koristi LDAP čitati za pristup direktoriju Azure infrastrukture servisima
Contoso je aplikaciju lokalne retka specifični za poslovanje (LOB) koji je razvio gotovo u deset godina prije. Ove aplikacije je direktorija svjesni i dizajniran za rad sa sustavom Windows Server AD. Aplikacija koristi LDAP (Lightweight Directory Access Protocol) da biste pročitali/atribute informacija o korisnicima iz servisa Active Directory. Aplikacija Izmijenite atribute ili u suprotnom pisanja u direktorij. Contoso želite migrirati ovu aplikaciju servisa Azure Infrastruktura i povući hardver lokalnog dospijeće trenutno hostira ove aplikacije. Aplikacija ne može biti potrebno ponovno napisati da biste koristili Moderna direktorija API-ji kao što su Azure AD grafikonu API utemeljen na OSTALE. Stoga Dizalica i shift mogućnost je potrebno whereby moguće je premjestiti aplikacije da biste pokrenuli u oblaku, bez izmjena kod ili upisivanjem aplikacije.

**Uvođenje bilješke**

Imajte na umu sljedeće važne detalje za taj scenarij implementacije:

- Provjerite je li aplikacija nije potrebna za izmjenu/pisanja u direktorij. Pristup zapisivanju LDAP upravljanih domene nudi Azure servisa Active Directory Domain Services nije podržana.

- Provjerite je li aplikacija potrebno shema servisa Active Directory Prilagođeno/prošireni. Proširenja shema nisu podržane u Azure AD domenske servise.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Migriranje lokalnog servisa i daemon aplikaciju servisa Azure infrastrukture Services
Neke aplikacije se sastoje od više razine, gdje nešto na razine mora poduzeti čija je autentičnost provjerena pozive u sloju pozadinskog kao što su sloju baze podataka. Računi servisa Active Directory najčešće koriste te korištenje slučajeva. Možete Dizalica i shift takve aplikacije servisa Azure Infrastruktura i korištenje Azure servisa Active Directory Domain Services za potrebe identiteta te aplikacije. Možete odabrati da biste koristili račun servisa koji se sinkroniziraju iz lokalnog imenika za Azure AD. Umjesto toga možete najprije stvoriti prilagođene OU, a zatim stvoriti zaseban račun u tom Organizacijska Jedinica za implementaciju takve aplikacije.

![Račun servisa pomoću WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso je aplikaciju sigurnog custom-built softver koji obuhvaća web-sučelja, SQL server i pozadinskog FTP poslužitelja. Windows integrirana provjera autentičnosti računa servisa koristi se za provjeru autentičnosti web-sučelja na FTP poslužitelj. Web-sučelja postavljena tako da se pokreće kao račun servisa. Da biste autorizirali pristup s računa za servis za web-sučelja je konfiguriran pozadinskog poslužitelja. Contoso želi ne moraju implementirati domene kontroler virtualnog računala u oblaku da biste premjestili ovu aplikaciju servisa Azure infrastrukture. Contoso je IT administrator može uvesti poslužiteljima na kojima se nalaze web-sučelja, SQL server i FTP poslužitelj za Azure virtualnim strojevima. Ove strojeva pa se pridružite programa Azure servisa Active Directory Domain Services upravljanih prilagođenu domenu. Nakon toga mogu koristiti isti račun servisa u svoje lokalnog imenika svrhu provjere autentičnosti za aplikaciju. Ovaj račun servisa sinkronizira s domenom upravljanih Azure servisa Active Directory Domain Services i dostupna je za korištenje.

**Uvođenje bilješke**

Imajte na umu sljedeće važne detalje za taj scenarij implementacije:

- Provjerite je li aplikacija koristi korisničkog imena i lozinke za provjeru autentičnosti. Potvrda/temelji provjeru autentičnosti pametne kartice ne podržava Azure servisa Active Directory Domain Services.

- Ne možete promijeniti lozinke izravno u odnosu na upravljanih domene. Krajnji korisnici mogu promijeniti svoju lozinku ili pomoću Azure AD samostalno promjena mehanizam prema lokalnog imenika. Te promjene se automatski sinkronizirane i dostupni upravljani domene.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp administratoru tvrtke Contoso-omogućuje stvaranje zbirke domene pridružili. Ova značajka omogućuje udaljene aplikacije posluživanje sustavom Azure RemoteApp za pokretanje na domeni pridruženo računala, a da biste pristupili ostalim resursima pomoću Windows integriran provjere autentičnosti. Contoso možete koristiti Azure servisa Active Directory Domain Services možete unijeti upravljanih domene koriste Azure RemoteApp domene pridruženo zbirke.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Dodatne informacije o ovom scenariju implementaciju potražite u članku udaljene radne površine usluge bloga pod naslovom [Dizalica i shift vaše radnih opterećenja s Azure RemoteApp i Azure servisa Active Directory Domain Services](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
