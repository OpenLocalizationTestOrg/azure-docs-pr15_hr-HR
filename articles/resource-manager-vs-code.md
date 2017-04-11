<properties
   pageTitle="Pomoću koda za dodavanje veze za VANJSKIH s resursima predlošci | Microsoft Azure"
   description="U članku se objašnjava postavljanje Visual Studio kod za stvaranje predložaka za Azure Voditelj resursa."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Rad s predlošcima za Azure Voditelj resursa u kodu Visual Studio

Azure resursima Predlošci su JSON datoteke koje opisuju resursa i povezane ovisnosti. Te se datoteke katkad može biti velike i komplicirano pa tooling podršku je važno. Visual Studio kod je novi, laganih, Otvori izvor, više platformi kod. Podržava stvaranje i uređivanje predložaka resursima putem [novi nastavak](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Dodavanje veze za VANJSKIH kod pokreće svugdje gdje, a ne zahtijevaju pristup Internetu osim ako ne želite implementirati predloške Voditelj resursa.

Ako već nemate Dodavanje veze za VANJSKIH kod, možete ga instalirati na [https://code.visualstudio.com/](https://code.visualstudio.com/).

## <a name="install-the-resource-manager-extension"></a>Instalirajte proširenje Voditelj resursa

Da biste radili s predlošcima JSON u kodu za dodavanje veze za VANJSKIH, morate instalirati datotečni nastavak. Sljedeći koraci preuzmite i instalirajte jezičnu podršku za predloške JSON Voditelj resursa:

1. Dodavanje veze za VANJSKIH kod za pokretanje 
2. Otvorite brzi Otvori (Ctrl + P) 
3. Pokrenite sljedeću naredbu: 

        ext install azurerm-vscode-tools

4. Ponovno pokrenite kod za dodavanje veze za VANJSKIH kada se to od vas zatraži da biste omogućili datotečni nastavak. 

 Posao gotovo!

## <a name="set-up-resource-manager-snippets"></a>Postavljanje isječci Voditelj resursa

Prethodne korake instaliran podršku tooling, ali sada ćemo morate konfigurirati Dodavanje veze za VANJSKIH kod tako da koristi isječci JSON predložak.

1. Kopirajte sadržaj datoteke iz spremišta [azure-xplat-arm-tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) u međuspremnik.
2. Dodavanje veze za VANJSKIH kod za pokretanje 
3. Dodavanje veze za VANJSKIH kod, može otvoriti datoteku isječci JSON navigacijom ili **datoteku** -> **Preference** -> **Korisnik isječci** -> **JSON**ili tako da odaberete **F1** i pisati **Preference** dok se ne možete odabrati **preference: isječci**.

    ![Preferirani isječci](./media/resource-manager-vs-code/preferences-snippets.png)

    Mogućnosti odaberite **JSON**.

    ![Odaberite json](./media/resource-manager-vs-code/select-json.png)

4. Lijepi sadržaj datoteke na korak 1 u datoteci isječci korisnika prije konačnih "}" 
5. Provjerite je li u JSON Traži u redu i postoje bilo kojeg mjesta bez squiggles. 
6. Spremite i zatvorite datoteku isječci korisnika.

To je sve što je potrebno da biste počeli koristiti isječci Voditelj resursa. Nakon toga ne možemo ćete staviti ovaj će instalacijski program za testiranje.

## <a name="work-with-template-in-vs-code"></a>Rad s predloškom Dodavanje veze za VANJSKIH kod

Da biste započeli rad s predloškom najjednostavnije privucite jedan brzi Start predložaka dostupnih na [Github](https://github.com/Azure/azure-quickstart-templates) ili koristite neku od svojih. Možete jednostavno [Izvoz predloška](resource-manager-export-template.md) iz nekog od svojih grupa resursa putem portala sustava. 

1. Ako ste izvezli predloška iz grupe resursa, otvorite izdvojene datoteke u kodu za dodavanje veze za VANJSKIH.

    ![Prikaz datoteka](./media/resource-manager-vs-code/show-files.png)

2. Otvorite datoteku u template.json tako da mogu uređivati i dodavanje nekoliko dodatnih resursa. Nakon što u **"resursi": [** pritisnite enter da biste započeli novi redak. Ako upišete **arm**, prikazat će se upit o popis mogućnosti. Te su mogućnosti isječci predložak koji ste instalirali. Trebao bi izgledati ovako: 

    ![Prikaži isječci](./media/resource-manager-vs-code/type-snippets.png)

3. Odaberite isječak koji želite. Za ovaj članak li se odabir **okvira ip** da biste stvorili novu javnu IP adresu. Postavili zareza iza desne uglate zagrade "}" novostvorenu resursa da biste bili sigurni predložak vrijedi sintakse.

     ![Dodavanje zarez](./media/resource-manager-vs-code/add-comma.png)

4. Dodavanje veze za VANJSKIH kod sadrži ugrađeni IntelliSense. Dok uređujete predložaka, dodavanje veze za VANJSKIH kod predlaže dostupne vrijednosti. Ako, na primjer, odjeljak varijable predloška, dodati **""** (dva dvostruke navodnike), a zatim odaberite **Ctrl + razmaknica** između tih ponude. Primit ćete s mogućnostima uključujući **varijabli**.

    ![dodati varijable](./media/resource-manager-vs-code/add-variables.png)

5. IntelliSense može predložiti i dostupne vrijednosti ili funkcije. Da biste postavili svojstvo na vrijednost parametra, stvaranje izraza s **"[]"** i **Ctrl + razmaknica**. Možete početi upisivati ime funkcije. Kada pronađete željenu funkciju, odaberite **karticu** .

    ![Dodavanje parametra](./media/resource-manager-vs-code/select-parameters.png)

6. Ponovno odaberite **Ctrl + razmaknica** unutar funkcije da biste vidjeli popis parametara dostupnih u predlošku.

    ![Dodavanje parametra](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Ako imate problema sheme provjere valjanosti u predlošku, vidjet ćete upoznati squiggles u uređivaču. Popis pogrešaka i upozorenja možete pogledati tako da upišete **Ctrl + Shift + M** ili odabira u posebni znakovi u donjem lijevom statusnoj traci.

    ![pogreške](./media/resource-manager-vs-code/errors.png)

    Provjera valjanosti predloška može pomoći pri otkrivanju problema sintaksa; Međutim, može se prikazati pogreške koje možete zanemariti. U nekim slučajevima uređivaču uspoređuje predloška sa shemom koja nije ažuran i stoga prijavljuje pogrešku čak i ako znate da je ispravan. Na primjer, pretpostavimo da funkcije nedavno dodana upravitelju resursa, ali ne ažuriraju u shemi. Uređivač prijavljuje pogrešku unatoč fact funkcija radi pravilno tijekom implementacije.

    ![poruka o pogrešci](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Implementacija nove resurse

Kada je spreman predložak možete implementirati nove resurse slijedeći upute u nastavku: 

### <a name="windows"></a>Windows

1. Otvorite naredbeni redak PowerShell 
2. Da biste vrsta prijave: 

        Login-AzureRmAccount 

3. Ako imate više pretplata, dobit ćete popis pretplate s:

        Get-AzureRmSubscription

    I odaberite pretplatu za korištenje.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Ažuriranje parametara u datoteci parameters.json
5. Pokrenite Deploy.ps1 uvođenje predloška na Azure

### <a name="osxlinux"></a>OSX/Linux

1. Otvorite prozor terminal 
2. Da biste vrsta prijave:

        azure login 

3. Ako imate više pretplata, odaberite desno pretplatu za:

        azure account set <subscriptionNameOrId> 

4. Ažurirajte parametara u datoteci parameters.json.
5. Da biste implementirali predložak, pokrenite:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o predlošcima potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).
- Da biste saznali više o funkcijama predložak, potražite u odjeljku [funkcije predloška Azure Voditelj resursa](resource-group-template-functions.md).
- Više primjera o radu s Visual Studio kod potražite u članku [Sastavljanje oblaka aplikacije Visual Studio koda](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) iz [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 povezivanje [pokazni videozapis](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Dodatne početak rada s HealthClinic.biz videozapis, potražite u članku [Početak rada za alate za razvojne inženjere za Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
