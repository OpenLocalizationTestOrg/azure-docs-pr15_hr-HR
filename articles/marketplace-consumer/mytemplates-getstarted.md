<properties
   pageTitle="Početak rada s predlošcima privatne | Microsoft Azure"
   description="Dodavanje, upravljanje i zajedničko korištenje privatne predloške pomoću portala za Azure, Azure EŽA ili PowerShell."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Početak rada s predlošcima privatne portala za Azure

Predložak programa [Azure Voditelj resursa](../resource-group-authoring-templates.md) je deklarativno predložak koji se koristi za određivanje implementaciju sustava. Možete definirati resurse za implementaciju rješenja i navedite parametre i varijabli koje omogućuju unos vrijednosti za različite okruženja. Predložak se sastoji od JSON i izraza koji možete koristiti za sastavljanje vrijednosti za implementaciju sustava.

Nova mogućnost **predložaka** [Azure Portal](https://portal.azure.com) uz davatelja resursa **Microsoft.Gallery** možete koristiti kao datotečni nastavak [Azure Marketplace](https://azure.microsoft.com/marketplace/) da biste omogućili korisnicima omogućuje stvaranje, upravljanje i implementacija privatne predložaka iz osobnoj biblioteci.

Ovaj dokument vodit će vas kroz dodavanje, upravljanje i zajedničko korištenje privatne **predložak** pomoću portala za Azure.

## <a name="guidance"></a>Upute

Sljedeće prijedloge za omogućit će vam iskoristiti **predloške** prilikom rada s vašeg rješenja:

- **Predložak** je encapsulating resursa koja sadrži predložak Voditelj resursa i dodatnih metapodataka. To se ponaša vrlo slično stavke na tržištu. Ključne razlike je privatne stavke umjesto javno trgovine stavke.
- **Predlošci** biblioteka funkcionira i za korisnike koji morate li prilagoditi postupak implementacije.
- **Predlošci** dobro funkcioniraju za korisnike kojima je potrebna jednostavne spremište unutar Azure.
- Započnite s postojećeg predloška Voditelj resursa. Traženje predložaka u [github](https://github.com/Azure/azure-quickstart-templates) i [Izvoz predloška](../resource-manager-export-template.md) iz postojeće grupe resursa.
- **Predlošci** je uz korisnika koji se objavljuje ih. Naziv izdavača vidljiv svakome tko ima pristup za čitanje na njega.
- **Predlošci** resursima resursa i nije moguće preimenovati jednom objavljena.

## <a name="add-a-template-resource"></a>Dodavanje resursa predloška

Da biste stvorili **predložak** resursa na portalu za Azure na dva načina.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Način 1: Stvaranje novog predloška resursa iz izvodi grupa resursa

1. Idite u postojeću grupu resursa na portalu Azure. U odjeljku **Postavke**odaberite **Izvezi predložak** .
2. Nakon izvoza predložak Voditelj resursa, pomoću gumba **Spremi predložak** da biste spremili u spremište **predložaka** . Dovršavanje detalje o izvoz predloška potražite [u nastavku](../resource-manager-export-template.md).
<br /><br />
![Izvoz grupa resursa](media/rg-export-portal1.PNG)  <br />

3. Odaberite gumb naredbe **spremiti predložak** .
<br /><br />

4. Unesite sljedeće podatke:

    - Naziv – naziv objekta predložak (Napomena: to je naziv Voditelj resursa za Azure temelje. Sve imenovanja ograničenja i ga nije moguće promijeniti nakon stvaranja).
    - Opis – kratak sažetak o predlošku.

    ![Spremanje predloška](media/save-template-portal1.PNG)  <br />

5. Kliknite **Spremi**.

    > [AZURE.NOTE] Predložak plohu izvoz prikazuje obavijesti kada izvezene resursima predložak sadrži pogreške, ali i dalje moći spremiti ovaj predložak resursima predložaka. Provjerite je li Provjera i otklanjanje problema vezanih uz predložak sve Voditelj resursa prije redeploying izvezene resursima predložak.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B. Način 2: Dodajte novi resurs predloška iz Pregledaj

Novi **predložak** možete dodati i pomoću privremeno na + Dodaj naredbeni gumb u **Pregled > Predlošci**. Morat ćete unijeti naziv, opis i resursima predložak JSON.

![Dodavanje predloška](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery je klijentom koji se temelje davatelja Azure resursa. Predložak resurs je uz korisnika za kojeg je stvorena. Ne uz neki određeni pretplatu. Potrebno je odabrati samo kada implementacije predloška pretplatu.

## <a name="view-template-resources"></a>Resursi za predložak prikaza

Svi **Predlošci** dostupne mogu vidjeti na **Pregled > Predlošci**. To obuhvaća **Predlošci** koje ste stvorili kao i one kojima je omogućeno zajedničko korištenje s vama s različitim razinama dozvola. Da biste vidjeli više pojedinosti u odjeljku [Kontrola pristupa](#access-control-for-a-tenant-resource-provider) .

![Predloška prikaza](media/view-template-portal1.PNG)  <br />

Detalje o **predlošku** možete pogledati tako da kliknete u stavku na popisu.

![Predloška prikaza](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Uređivanje predloška resursa

Možete pokrenuti tijek uređivanje **predloška** tako da desnom tipkom miša kliknete stavku na popis za pregled ili tako da odaberete naredbeni gumb Uredi.

![Uređivanje predloška](media/edit-template-portal1a.PNG)  <br />

Opis ili tekst predloška Voditelj resursa možete urediti. Budući da je to je naziv resursa Voditelj resursa, naziv ne možete uređivati. Kada uređujete predložak resursima JSON smo će provjeriti da biste bili sigurni koji je valjan JSON. Odaberite **u redu** , a zatim **Spremi** da biste spremili predložak ažurirani.

![Uređivanje predloška](media/edit-template-portal2a.PNG)  <br />

Nakon spremanja **predloška** prikazat će obavijest o potvrdi.

![Uređivanje predloška](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Uvođenje predloška resursa

Možete implementirati **predložak** koje imate dozvole za **čitanje** . Tijek implementacije pokreće standardni plohu uvođenje predloška Azure. Ispunjavanje vrijednosti za parametre resursima predložak da biste nastavili s uvođenje.

![Uvođenje predloška](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Zajedničko korištenje nekog resursa predloška

**Predložak** resursa možete zajednički koristiti s svoja pitanja. Zajedničko korištenje ponaša slično [Dodjela uloge za neki resurs na Azure](../active-directory/role-based-access-control-configure.md). Vlasnik **predložak** nudi dozvole da biste drugim korisnicima koji omogućuje interakciju s resursa za predložak. Osoba ili grupa osoba na zajedničko korištenje **predloška** s će moći vidjeti predložak Voditelj resursa i njezina svojstva galerije.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Kontrola pristupa za resurse Microsoft.Gallery

Uloga | Dozvole
---|----
Vlasnik | Omogućuje potpunu kontrolu na predlošku resursa obuhvaća zajedničko korištenje
Čitač | Omogućuje čitanje i Execute(Deploy) predložak resurs
Suradnik | Omogućuje dozvolu za uređivanje i brisanje predloška resurs. Korisnik ne možete zajednički koristiti predložak s drugim korisnicima

Odaberite **zajedničko korištenje** na stavci za pregled tako da desnom tipkom miša kliknete ili plohu prikaz određene stavke. Time se pokreće sučelje za zajedničko korištenje.

![Zajedničko korištenje predloška](media/share-template-portal1a.png)  <br />

 Sada možete odabrati ulogu i korisnika ili grupu za pristup određeni **predložak**. Dostupne uloge su vlasnik, čitač i suradnika. Da biste vidjeli više pojedinosti u gornjem dijelu [Kontrola pristupa](#access-control-for-a-tenant-resource-provider) .

![Zajedničko korištenje predloška](media/share-template-portal2b.png)  <br />

![Zajedničko korištenje predloška](media/share-template-portal3b.png)  <br />

Kliknite **Odabir** i **u redu**. Sada možete vidjeti korisnike ili grupe koji ste dodali resursa.

![Zajedničko korištenje predloška](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Predložak možete zajednički koristiti samo s korisnicima i grupama u istom klijentu Azure Active Directory. Ako zajednički koristite predloška s adresom e-pošte koja nije u klijentu pozivnicu poslat će se pitanje korisniku pridruživanje klijentu kao gost.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o stvaranju predložaka Voditelj resursa, u odjeljku [Predlošci na dokumentima](../resource-group-authoring-templates.md)
- Da biste shvatili funkcije možete koristiti u predlošku Voditelj resursa, potražite u odjeljku [funkcije predloška](../resource-group-template-functions.md)
- Upute o dizajniranju predložaka potražite u članku [najbolje prakse za dizajniranje predložaka Voditelj resursa za Azure](../best-practices-resource-manager-design-templates.md)
