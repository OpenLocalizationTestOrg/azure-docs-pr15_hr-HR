<properties
   pageTitle="Stvaranje servisa glavni portalu | Microsoft Azure"
   description="U članku se opisuje kako stvoriti novu aplikaciju servisa Active Directory i glavnicu usluge koje je moguće koristiti s kontrolu pristupa na temelju uloga u Azure Voditelj resursa za upravljanje pristupom resursima."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Stvaranje aplikacije servisa Active Directory i glavnicu servis koji mogu pristupiti resurse pomoću portala

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure EŽA](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Kada su aplikacije koje su potrebne za pristup i izmjena resursi, morate postaviti aplikaciju servisa Active Directory (AD) i dodijeliti potrebne dozvole. U ovoj se temi objašnjava te korake putem portala sustava. Trenutno, morate koristiti klasične portal za stvaranje nove aplikacije servisa Active Directory, a zatim promijenite Azure portal za dodjelu uloga aplikacije. 

> [AZURE.NOTE] Koraci u ovoj temi primjenjuju se samo kada se koristi **klasične portal** za stvaranje aplikacije za AD. **Ako koristite Azure portal za stvaranje aplikacije servisa Active Directory, ove korake neće uspjeti.** 
>
> Koje možda jednostavnije da biste postavili AD aplikacija i servisa glavni putem [komponente PowerShell](resource-group-authenticate-service-principal.md) ili [EŽA Azure](resource-group-authenticate-service-principal-cli.md), osobito ako želite koristiti certifikat za provjeru autentičnosti. U ovoj se temi pokazati kako pomoću certifikata.

Objašnjenje koncepata servisa Active Directory potražite u članku [aplikacija i servisa glavnicu objekte](./active-directory/active-directory-application-objects.md). Dodatne informacije o provjeri autentičnosti servisa Active Directory potražite u članku [Provjera autentičnosti scenariji za Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Detaljne upute Integracija aplikacije u Azure upravljanja resursima potražite u članku [Vodič za programere za autorizaciju s resursima API Azure](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Stvaranje aplikacije komponente Active Directory

1. Prijavite se na račun za Azure putem [klasične portal](https://manage.windowsazure.com/). 

2. Provjerite je li znate zadani Active Directory za vašu pretplatu. Samo možete dopustiti pristup aplikacijama u direktoriju isti kao svoju pretplatu. Odaberite **Postavke** , a zatim potražite naziv direktorija koji je povezan s pretplatom.  Dodatne informacije potražite u članku [kako Azure pretplate pridružuju Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![Pronalaženje zadani imenik](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. U lijevom oknu odaberite **Servisa Active Directory** .

     ![Odabir servisa Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Odaberite servisa Active Directory koji želite koristiti za stvaranje aplikacija. Ako imate više od jednog servisa Active Directory, stvaranje aplikacije u zadanom imeniku za vašu pretplatu.   

     ![Odaberite imenik](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Da biste pregledali aplikacije u direktoriju, odaberite **aplikacije**.

     ![Prikaz aplikacija](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Ako niste stvorili aplikacije u tom direktoriju prije, trebali biste vidjeti nešto slično kao na sljedećoj slici. Odaberite **Dodavanje APLIKACIJE**

     ![Dodavanje aplikacije](./media/resource-group-create-service-principal-portal/create-application.png)

     Ili kliknite **Dodaj** u dnu okna.

     ![Dodavanje](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Odaberite vrstu aplikacije koju želite stvoriti. U ovom ćete praktičnom vodiču odaberite **Dodavanje aplikacije je moje tvrtke ili ustanove razvoju**. 

     ![nove aplikacije](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Navedite naziv aplikacije, a zatim odaberite vrstu aplikacije koju želite stvoriti. U ovom ćete praktičnom vodiču stvaranje **API WEB AND/OR APLIKACIJU za WEB** i kliknite gumb Dalje. Ako ste odabrali **NATIVNI KLIJENTSKA APLIKACIJA**, preostale korake ovog članka ne odgovaraju iskustva.

     ![Naziv aplikacije](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Unesite svojstva za aplikacije. Za **Prijavu na URL**pružaju URI za web-mjesta koji opisuje aplikacije. Ne provjerava postojanje web-mjesta. Za **Aplikacije ID URI**pružaju URI koja služi za identifikaciju aplikacije.

     ![Svojstva aplikacije](./media/resource-group-create-service-principal-portal/app-properties.png)

Stvorite aplikaciju.

## <a name="get-client-id-and-authentication-key"></a>Ključ klijent ID-a i provjeru autentičnosti

Kada programatski prijaviti, morate id za svoju aplikaciju. Ako se aplikacija pokreće se u odjeljku vlastitu vjerodajnice, morate ključa za provjeru autentičnosti.

1. Odaberite karticu **Konfiguriraj** da biste konfigurirali vaše aplikacije lozinku.

     ![Konfiguriranje aplikacije](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopirajte **ID KLIJENTA**.
  
     ![id klijenta](./media/resource-group-create-service-principal-portal/client-id.png)

3. Ako aplikacija će se pokrenuti u odjeljku vlastitu vjerodajnice, pomaknite se prema dolje do odjeljka **tipke** i odaberite koliko želite lozinke moraju biti valjane.

     ![tipke](./media/resource-group-create-service-principal-portal/create-key.png)

4. Odaberite **Spremi** da biste stvorili ključa.

     ![Spremi](./media/resource-group-create-service-principal-portal/save-icon.png)

     Prikazuje se tipku spremljena, a možete je kopirati. Nećete kasnije dohvatiti tipku pa ga sada kopirali.

     ![sprema ključ](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Preuzmite id klijenta

Kada programatski prijaviti, morate proći id klijenta sa zahtjevom za provjeru autentičnosti. Web-aplikacije i web-aplikacije API-JA, možete dohvatiti id klijenta tako da odaberete **Prikaz krajnje točke** pri dnu zaslona i dohvaćanje id kao što je prikazano na sljedećoj slici.  

   ![id klijenta](./media/resource-group-create-service-principal-portal/save-tenant.png)

Također možete dohvatiti id klijenta putem komponente PowerShell:

    Get-AzureRmSubscription

Ili Azure EŽA:

    azure account show --json

## <a name="set-delegated-permissions"></a>Postavljanje delegiranim dozvola

Ako aplikacija pristupa resursima ime korisnika za prijavljeni u, morate je dodijeliti aplikacije ovlaštenog dozvolu za pristup drugim aplikacijama. Dopustiti pristup u odjeljku **dozvole na druge aplikacije** na kartici **Konfiguriraj** . Prema zadanim postavkama ovlaštenog dozvola već je omogućena za Azure Active Directory. Ostavite tu ovlaštenog dozvolu ne mijenja.

1. Odaberite **Dodaj aplikaciju**.

2. Na popisu odaberite **API za upravljanje servisa Windows Azure**. Zatim odaberite ikonu dovršeno.

      ![Odaberite aplikaciju](./media/resource-group-create-service-principal-portal/select-app.png)

3. Na padajućem popisu ovlaštenih dozvola odaberite **Upravljanje servisom Azure Access kao tvrtke ili ustanove**.

      ![Odaberite dozvole](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Spremiti promjene.

## <a name="assign-application-to-role"></a>Dodjeljivanje aplikacije uloga

Ako program izvodi u odjeljku vlastitu vjerodajnice, morate dodijeliti aplikacije u ulogu. Odlučiti koju ulogu predstavlja odgovarajuće dozvole za aplikaciju. Da biste saznali više o ulogama dostupna, u odjeljku [RBAC: ugrađena uloge](./active-directory/role-based-access-built-in-roles.md). 

Za dodjeljivanje uloge aplikaciju, morate imati odgovarajuću dozvolu. Konkretno, morate imati `Microsoft.Authorization/*/Write` access koja je dodijeljena uloga [vlasnika](./active-directory/role-based-access-built-in-roles.md#owner) ili [Korisnički pristup administratorsku](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) ulogu. Uloga suradnika nemate odgovarajuće pristup.

Opseg možete postaviti na razini pretplate, grupa resursa ili resurs. Dozvole se naslijeđuju niže razine opsega. Na primjer, dodavanje aplikacije ulozi čitač za grupu resursa znači da mogu čitati grupu resursa i nikakve resurse sadrži.

1. Da biste dodijelili aplikacije u ulogu, prijeđite na portalu klasični [Azure portal](https://portal.azure.com).

1. Provjerite dozvole da biste provjerili je li servis glavnicu možete dodijeliti uloge. Odaberite **Moje dozvole** za vaš račun.

    ![Odabir dozvola](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Prikaz dodijeljene dozvole za vaš račun. Kao što je naznačeno prethodno, morate pripadate uloge vlasnik ili Administrator za pristup korisnika ili prilagođeni uloga koja omogućuje pristup pisanje za Microsoft.Authorization. Sljedeća slika prikazuje računa koji je dodijeljena uloga suradnika za pretplatu koja nije odgovarajuće dozvole za dodjeljivanje aplikacije u ulogu.

    ![Prikaži Moje dozvole](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Ako nisu ispravne dozvole da biste dodijelili pristup aplikaciji, morate ili zahtjev za koje administrator pretplate dodaje na ulogu administratora korisnički pristup ili zatražiti da administrator daje pristup aplikaciji.

1. Dođite do razine opsega želite dodijeliti aplikacije. Dodjeljivanje uloge u dosegu pretplate, odaberite **pretplate**.

     ![Odaberite pretplatu](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Odaberite određenu pretplatu za dodjelu aplikacije.

     ![Odaberite pretplatu za dodjelu](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     U gornjem desnom kutu odaberite ikonu **programa Access** .

     ![Odaberite pristup](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Ili za dodjelu uloga u dosegu grupu resursa, idite u grupu resursa. Grupa plohu resursa odaberite **kontrolu pristupa**.

     ![Odabir korisnika](./media/resource-group-create-service-principal-portal/select-users.png)

     Sljedeći koraci nisu isto za sve opseg.

2. Odaberite **Dodaj**.

     ![Odaberite Dodaj](./media/resource-group-create-service-principal-portal/select-add.png)

3. Odaberite ulogu za **čitanje** (ili bilo kakve ulogu želite dodijeliti aplikacije).

     ![Odaberite ulogu](./media/resource-group-create-service-principal-portal/select-role.png)

4. Kada vam se najprije prikazuje popis korisnika možete dodati u ulogu, nećete vidjeti aplikacije. Vidjet ćete samo grupe i korisnicima.

     ![Prikaži korisnike](./media/resource-group-create-service-principal-portal/show-users.png)

5. Da biste pronašli svoju aplikaciju, ne morate tražiti ga. Počnite unositi naziv aplikacije, a promijenit će se popis dostupnih mogućnosti. Kada vam se prikaže na popisu, odaberite aplikacije.

     ![Dodjeljivanje uloge](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Odaberite **redu** da biste završili s dodjeljivanjem ulogu. Prikazat će se sada vašeg računala na popisu koristi dodijeljene ulogama za grupu resursa.


Dodatne informacije o dodjeli korisnicima i aplikacijama uloge putem portala sustava potražite u članku [Korištenje dodjele uloga upravljanje pristup resursi za Azure pretplate](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Probne aplikacije

Sljedeće ogledne aplikacije pokazalo kako se prijaviti kao Upravitelj servisa.

**.NET**

- [Implementacija programa SSH omogućeno VM s predloškom s .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Upravljanje Azure resursa i grupa resursa s .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Početak rada s resursima - implementacija pomoću predloška Azure resursima - u Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Početak rada s resursima - upravljanje grupa resursa – u Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Implementacija programa SSH omogućeno VM s predloškom u Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Upravljanje Azure resurse i grupe resursa s Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Implementacija programa SSH omogućeno VM s predloškom u Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Upravljanje Azure resursa i grupa resursa s Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Implementacija programa SSH omogućeno VM s predloškom u Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Upravljanje Azure resurse i grupe resursa s Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Daljnji koraci

- Saznajte više o određivanju sigurnosna pravila, potražite u članku [Utemeljeno na ulogama Azure kontrola pristupa](./active-directory/role-based-access-control-configure.md).  
- Pokazni videozapis od ovih koraka potražite u članku [Omogućivanje programskog upravljanja programa Azure resursa s Azure Active Directory](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

