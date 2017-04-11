<properties 
    pageTitle="Korištenje Azure portal za implementaciju Azure resursi | Microsoft Azure" 
    description="Korištenje portala za Azure i upravljanje za Azure resursa za implementaciju resurse." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Implementacija resursa s resursima predloške i portal za Azure

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure EŽA](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API-JA](resource-group-template-deploy-rest.md)

U ovoj se temi objašnjava pomoću [portala za Azure](https://portal.azure.com) [Voditelj resursa Azure](azure-resource-manager/resource-group-overview.md) implementacija Azure resurse. Da biste saznali više o upravljanju resursa, potražite u članku [Upravljanje Azure resursi putem portala](./azure-portal/resource-group-portal.md).

Trenutno ne svaku uslugu podržava portala i resursima. Za te servise, morate koristiti [klasične portal](https://manage.windowsazure.com). Status svaki servis, potražite u članku [Azure portala dostupnost grafikona](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Stvaranje grupa resursa

1. Da biste stvorili grupu prazan resursa, odaberite **Novo** > **upravljanja** > **Grupu resursa**.

    ![Stvaranje grupe prazan resursa](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Dajte mu naziv i lokaciju, a ako je potrebno, odaberite pretplatu. Morate upišete mjesto za grupu resursa jer grupa resursa pohranjuje metapodatke o resurse. Usklađenost razloga preporučujemo vam da biste odredili gdje je pohranjena te metapodataka. Općenito govoreći, preporučujemo da navedete na mjesto na kojem će se nalaziti Većina resurse. Korištenje na istom mjestu može pojednostavniti svoj predložak.

    ![Postavljanje grupe vrijednosti](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Implementacija resursa iz trgovine

Kada stvorite grupu resursa, možete implementirati resurse da biste ga iz trgovine. Na tržištu nudi unaprijed definirana rješenja za uobičajeni scenariji.

1. Da biste započeli implementacije, odaberite **Novo** i vrstu resursa koju želite uvesti. Nakon toga potražite određenu verziju resursa koje želite uvesti.

    ![Implementacija resursa](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Ako ne vidite određenog rješenja koju želite uvesti, možete potražiti na tržištu ga.

    ![pretraživanje marketplace](./media/resource-group-template-deploy-portal/search-resource.png)

3. Ovisno o vrsti odabranog resursa, imate skup relevantnih svojstva da biste postavili prije implementacije. Te mogućnosti ne prikazuju se ovdje, kao što su razlikuju se ovisno o vrsti resursa. Za sve vrste, morate odabrati grupu resursa odredište. Sljedeća slika prikazuje kako stvoriti web-aplikacijama i njegove implementacije kod koji ste stvorili grupu resursa.

    ![Stvaranje grupa resursa](./media/resource-group-template-deploy-portal/select-existing-group.png)

    Osim toga, možete odlučiti da biste stvorili grupu resursa pri implementaciji resurse. Odaberite **Stvaranje nove** i grupu resursa dodijelite naziv.

    ![Stvori novu grupu resursa](./media/resource-group-template-deploy-portal/select-new-group.png)

4. Uvođenja započinje. Uvođenje može potrajati nekoliko minuta. Po završetku implementaciju, vidjet ćete obavijest.

    ![Prikaz obavijesti](./media/resource-group-template-deploy-portal/view-notification.png)

5. Nakon što implementirate resursa, grupa resursa možete dodati dodatni resursi pomoću naredbe **Dodaj** na plohu grupu resursa.

    ![Dodavanje resursa](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Implementacija resursa iz prilagođenog predloška

Ako želite izvršiti implementacije, ali ne koristite neku od predložaka na tržištu, možete stvoriti prilagođeni predložak koji definira infrastrukture za rješenje. Dodatne informacije o stvaranju predložaka, potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).

1. Implementirati prilagođeni predložak putem portala sustava, odaberite **Novo**, a rasponom implementacije **Predloška** dok ne odaberete od mogućnosti.

    ![uvođenje predloška za pretraživanje](./media/resource-group-template-deploy-portal/search-template.png)

2. Odaberite **Predložak implementacije** dostupnih resursa.

    ![Odaberite predložak implementacije](./media/resource-group-template-deploy-portal/select-template.png)

3. Nakon pokretanja uvođenje predloška, otvorite prazni predložak koji je dostupan za prilagodbu.

    ![Stvaranje predloška](./media/resource-group-template-deploy-portal/show-custom-template.png)

    U uređivaču dodajte sintaksa JSON koji definira resursa koje želite uvesti. Odaberite **Spremi** kada to učinite. Smjernice o pisanju sintaksa JSON, potražite u članku [Vodič za upravljanje resursima predložak](resource-manager-template-walkthrough.md).

    ![Uređivanje predloška](./media/resource-group-template-deploy-portal/edit-template.png)

4. Ili, možete odabrati unaprijed postojećeg predloška iz [predložaka Azure brzi početak rada](https://azure.microsoft.com/documentation/templates/). Ti predlošci su pridonio zajednica. Oni pokrivaju mnogo uobičajeni scenariji i netko dodao predloška koja je slična što želite uvesti. Pretražite predloške za pronalaženje sadržaja koji odgovara vašem scenariju.

    ![Odaberite predložak za brzi početak rada](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    Odabrani predložak možete pogledati u uređivaču.

5. Nakon unošenja sve druge vrijednosti, odaberite **Stvori** da biste uvođenje predloška. 

    ![uvođenje predloška](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Implementacija resursa iz predloška spremljeni na račun

Na portalu omogućuje vam da biste spremili predložak na račun za Azure i ponovno implementirate kasnije. Dodatne informacije o radu s ih spremiti predložaka, [Početak rada s predlošcima privatne portala za Azure](./marketplace-consumer/mytemplates-getstarted.md).

1. Da biste pronašli predloške spremljene, odaberite **Pregledaj** > **predložaka**.

    ![Pregled predložaka](./media/resource-group-template-deploy-portal/browse-templates.png)

2. Na popisu Predlošci spremljeni na račun servisa odaberite onu koju želite raditi.

    ![spremljeno predložaka](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Odaberite **uvođenja** implementirati ovog spremljenog predloška.

    ![Implementacija spremljenog predloška](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali zapisnike nadzora, potražite u članku [nadzora operacije s Voditelj resursa](resource-group-audit.md).
- Da biste otklonili pogreške implementaciju, potražite u članku [Otklanjanje poteškoća resursa grupe implementacijama pomoću portala za Azure](resource-manager-troubleshoot-deployments-portal.md).
- Učitavanje predloška iz implementacije ili grupu resursa, potražite u članku [Izvoz Voditelj resursa Azure predloška iz postojećih resursa](resource-manager-export-template.md).





