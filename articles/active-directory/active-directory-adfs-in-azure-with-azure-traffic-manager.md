<properties
    pageTitle="Visoke dostupnosti unakrsno geografske AD FS implementacije u Azure s Azure promet Upravitelj | Microsoft Azure"
    description="U ovom dokumentu će Saznajte kako implementirati AD FS u Azure za visoke availablity."
    keywords="Ad fs s Upravitelj Azure promet, adfs s Azure promet za upravljanje geografske, višestruki podatkovnog centra, zemljopisni podatkovnim centrima, zemljopisni podatkovnim centrima s više, implementaciju AD FS u azure, implementaciju azure adfs, azure adfs, azure ad fs, implementaciju adfs, uvođenje servisa ad fs adfs u azure, implementaciju adfs u azure, uvođenje AD FS u azure, adfs azure, Uvod u AD FS, Azure AD FS u Azure, iaas , ADFS, premjestite adfs azure"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Visoke dostupnosti unakrsno geografske AD FS implementacije u Azure s Azure promet Manager

[AD FS implementacije u Azure](active-directory-aadconnect-azure-adfs.md) sadrži detaljne većinom kao uvođenje jednostavne AD FS infrastrukture za tvrtku ili ustanovu u Azure. Ovaj članak sadrži sljedeće korake da biste stvorili unakrsno geografske implementacije AD FS u Azure pomoću [Upravitelja promet Azure](../traffic-manager/traffic-manager-overview.md). Azure promet Upravitelj pomaže pri stvaranju geografski Podjela visoke dostupnosti i visokih performansi AD FS infrastrukture za tvrtku ili ustanovu tako da koristi raspon usmjeravanje metode dostupne za različite potrebe iz Infrastruktura.

Iznimno dostupno više geografske AD FS infrastruktura omogućuje:

* **Uklanjanja jednu točku pogreške:** S mogućnostima prebacivanje Azure promet upravitelja, možete postići visoko dostupna infrastrukturu za AD FS čak i kad je jedan od središta podataka u dijelu globusa funkcionira
* **Bolje performanse:** Predloženi implementacije možete koristiti u ovom članku omogućuju infrastrukture za AD FS visokih performansi koje možete pomoći korisnicima autentičnost brže. 

##<a name="design-principles"></a>Način dizajna

![Ukupni dizajn](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Osnovni dizajnerski način će biti isti kao što je navedeno u način dizajna u članku AD FS implementacije u Azure. Gornji dijagram prikazuje nastavkom jednostavne osnovni implementaciju na drugi zemljopisnom području. U nastavku su neke točke voditi računa prilikom proširivanje implementacije novog zemljopisnom području

* **Virtualne mreže:** Stvorite novi virtualne mreže u zemljopisnom području koje želite implementirati dodatna infrastruktura za AD FS. U dijagramu iznad Geo1 VNET i Geo2 VNET kao vidite dvije virtualne mreže na svakom zemljopisnom području.

* **Kontrolera domena i poslužitelja za AD FS u novi zemljopisnim VNET:** Preporučuje se za implementaciju domene kontrolera novi zemljopisnom području tako da poslužitelja za AD FS u području novi moraju se obratiti kontroler domene u drugom daleko mreže da biste dovršili za provjeru autentičnosti i time unaprjeđenje.

* **Račune za pohranu:** Računi za pohranu povezani su s područja. Budući da će implementacija strojeva u novi regiji, imat ćete za stvaranje novog računa za pohranu koja će se koristiti u regiji.  

* **Mreže sigurnosne grupe:** Kao što je mreža sigurnosne grupe stvorene u područje za pohranu računi ne može koristiti u drugom zemljopisnom području. Dakle, morat ćete stvoriti novi mrežu sigurnosne grupe sličan je u prvom zemljopisnom području INT i DMZ podmreže u novi zemljopisnom području.

* **Naljepnica DNS-a za javno IP adrese:** Azure promet upravitelj može se odnositi na krajnje točke samo putem DNS naljepnice. Zbog toga su potrebne za stvaranje naljepnica DNS-a za IP adrese web-mjesta s vanjskim učitavanja Balancers' javno.

* **Azure Upravitelj promet:** Upravitelj promet za Microsoft Azure omogućuje kontrolu raspodjele korisnika promet na vaše krajnje točke servisa izvodi u različitim podatkovnim centrima diljem svijeta. Azure promet Upravitelj funkcionira na razini DNS-a. Da biste usmjerili promet krajnjeg korisnika globalno distributed krajnje točke koristi DNS odgovore. Klijenti zatim povezati s tim krajnje točke izravno. S drugim mogućnostima usmjeravanje performanse, Weighted i prioriteta, jednostavno možete odabrati mogućnost usmjeravanja vam najviše odgovara potrebama vaše tvrtke ili ustanove. 

* **V neto V-neto veze između dviju regija:** Ne morate imati veze između virtualne mreže sam. Budući da svaki virtualne mreže ima pristup kontrolera domena i sam po sebi je poslužitelj za ADFS i WAP, možete raditi bez sve veze između virtualne mreže u različitim područjima. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Koraci za grupa Azure promet animacije

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>Uvođenje servisa AD FS u novi zemljopisnom području
Slijedite korake i smjernice za [AD FS implementacije u Azure](active-directory-aadconnect-azure-adfs.md) za implementaciju iste topologije u novi zemljopisnom području.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Oznaka DNS-a za javno IP adrese učitavanja Balancers Internet nasuprotne (javni)
Kao što je rečeno iznad Manager promet Azure može upućivati samo na DNS naljepnica kao krajnje točke i stoga je važno da biste stvorili DNS naljepnice za IP adrese web-mjesta s vanjskim učitavanja Balancers' javno. Ispod snimka zaslona prikazuje kako možete konfigurirati DNS naljepnicu za javnu IP adresu. 

![Natpis DNS-a](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Implementacija Azure Upravitelj promet

Slijedite korake u nastavku da biste stvorili profil za Upravitelj promet. Dodatne informacije, možete referirati na [Upravljanje profil programa Upravitelj promet Azure](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Stvaranje profila Upravitelja promet:** Dajte upravitelj profila promet jedinstven naziv. Taj naziv profila je dio naziva DNS-a i ponaša se kao prefiks natpis naziv promet Upravitelj domena. Naziv / dodaje se prefiks. trafficmanager.net da biste stvorili naljepnicu DNS-a za prometa manager. Snimka zaslona koja se nalazi ispod prikazuje promet Upravitelj DNS prefiks postavljanje kao mysts i će se rezultat oznaka DNS mysts.trafficmanager.net. 

    ![Stvaranje profila Upravitelja promet](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Promet metodu usmjeravanja:** U upravitelju promet dostupne su tri mogućnosti usmjeravanje:

    * Prioritet 
    * Performanse
    * Težinski faktor
    
    **Performanse** je preporučena mogućnost da biste postigli iznimno odredište infrastruktura za AD FS. Međutim, možete odabrati bilo koju metodu usmjeravanja najprikladnije su za vaše potrebe za implementaciju. Usmjeravanje odabrana mogućnost ne utječe na funkcionalnost servisa AD FS. Dodatne informacije potražite u članku [Upravitelj promet promet usmjeravanje metode](../traffic-manager/traffic-manager-routing-methods.md) . U uzorku snimka iznad koje možete vidjeti metodu **performanse** odabran.
   
3.  **Konfiguriranje krajnje točke:** Na stranici upravitelja promet na krajnje točke kliknite, a zatim odaberite Dodaj. Otvorit će se slično kao snimku zaslona koja se nalazi ispod Dodaj stranicu krajnje točke
 
    ![Konfiguriranje krajnje točke](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    Za različite unosa slijedite dolje smjernica:

    **Vrsta:** Odaberite Azure krajnju točku kao što smo će pokazivanjem Azure javnu IP adresu.

    **Naziv:** Stvaranje naziva koji želite pridružiti krajnju točku. To nije naziv DNS-a i ima bez sa slikom na DNS zapisa.

    **Ciljani vrsta resursa:** Kao vrijednost za to svojstvo, odaberite javnu IP adresa. 

    **Ciljani resursa:** Imat ćete mogućnost da biste odabrali s različitim naljepnicama DNS koje imate na raspolaganju u odjeljku pretplate. Odaberite DNS natpis za.

    Dodajte krajnja točka za svaki zemljopisnom području li upravitelj promet Azure usmjeravanje prometa.
    Dodatne informacije i detaljne upute za dodavanje / konfiguriranje krajnje točke u upravitelju promet odnose da biste [dodali, onemogućite, omogućivanje ili brisanje krajnje točke](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Konfiguriraj probni:** Na stranici upravitelja promet kliknite o konfiguraciji. Na stranici Konfiguracija ćete morati promijeniti postavke monitora isprobati HTTP priključak 80 i relativni put /adfs/probe

    ![Konfiguriranje probni](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Provjerite je li status krajnjih točaka ONLINE nakon dovršetka konfiguracije**. Ako su sve krajnje točke u stanju 'su smanjene', Upravitelj promet Azure neće imati najbolje pokušaj usmjeravanje pretpostavkom promet Dijagnostika nije valjana, a krajnje točke za sve su dostupna.

5. **Izmjena DNS zapise za Azure promet Manager:** Servis za vanjski pristup mora biti CNAME naziv DNS Manager promet Azure. Stvorite CNAME u odjeljku javne DNS zapisi tako da se obavi pokušava dosegne servis za vanjski pristup zaista dostigne promet Upravitelj Azure.

    Ako, na primjer, želite pokazivati fs.fabidentity.com servis za vanjski pristup upravitelju promet, trebali biste ažuriranje DNS zapisa resursa se sljedeće:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Testiranje usmjeravanje i AD FS prijavu   

###<a name="routing-test"></a>Usmjeravanje test

Vrlo osnovni test za postupka biti ping naziv DNS servis za vanjski pristup s računala na svakom zemljopisnom području. Ovisno o metodu usmjeravanja odabrali, krajnje je zapravo Pingovi odrazit će se u prikaz ping. Ako, na primjer, ako ste odabrali usmjeravanje performanse, krajnjoj točki nearest to regije klijentskog programa Pristigla će biti. U nastavku je snimka dva Pingovi s dvije različite regije klijentskih računala, u području EastAsia i u Zapad SAD-a. 

![Usmjeravanje test](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS prijavu test

Da biste testirali AD FS najjednostavnije pomoću IdpInitiatedSignon.aspx stranice. Da biste mogli učiniti da je potrebno da biste omogućili IdpInitiatedSignOn svojstava za AD FS. Slijedite korake u nastavku da biste provjerili postavu AD FS
 
1. Pokretanje u nastavku cmdlet poslužitelju za AD FS, pomoću komponente PowerShell postavite ga na omogućeno. Postavljanje AdfsProperties - EnableIdPInitiatedSignonPage $true
2. Iz bilo kojeg https:// pristup vanjskim strojno<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Trebali biste vidjeti AD FS stranice kao što su ispod:

    ![Test ADFS - test za provjeru autentičnosti](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    i na uspješno prijavu, ona omogućit će vam poruka o uspjehu kao što je prikazano u nastavku:

    ![Test ADFS - uspjeh provjere autentičnosti](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Srodne veze
* [Osnovni AD FS implementacije u Azure](active-directory-aadconnect-azure-adfs.md)
* [Upravitelj promet Microsoft Azure](../traffic-manager/traffic-manager-overview.md)
* [Upravitelj promet promet usmjeravanje metode](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Daljnji koraci
* [Upravljanje profil programa Upravitelj promet Azure](../traffic-manager/traffic-manager-manage-profiles.md)
* [Dodavanje, onemogućivanje, omogućivanje ili brisanje krajnje točke](../traffic-manager/traffic-manager-endpoints.md) 

