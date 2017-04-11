<properties 
    pageTitle="Vraćanje aplikacije u Azure" 
    description="Saznajte kako vratiti aplikacije iz sigurnosne kopije." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Vraćanje aplikacije u Azure

U ovom se članku objašnjava da biste vratili aplikaciju u [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md) prethodno sigurnosno (pogledajte [sigurnosne kopije vaše aplikacije u Azure](web-sites-backup.md)). Vraćanje aplikacije s njegova povezane baze podataka (SQL baze podataka ili MySQL) na zahtjev u prethodno stanje ili stvorite novu aplikaciju koja se temelji na jednom od sigurnosnu kopiju izvorne aplikacije. Stvaranje nove aplikacije koja se pokreće paralelno na najnoviju verziju mogu poslužiti za A / B testiranja.

Vraćanje iz sigurnosne kopije dostupna je za aplikacije koji se izvodi u **standardnom** i **Premium** sloju. Informacije o skaliranje gore aplikacije, potražite u članku [proširenja aplikacije u Azure](web-sites-scale.md). **Premium** sloju omogućuje veći broj dnevnih sigurnosne kopije koje je potrebno izvršiti od **standardne** sloju.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Vraćanje aplikacije iz postojeće sigurnosne kopije

1. Na plohu **postavki** aplikacije na portalu za Azure, kliknite **sigurnosno kopiranje** da biste prikazali plohu **sigurnosne kopije** . Zatim kliknite **Vrati sada** na naredbenoj traci. 
    
    ![Odaberite Vrati sada][ChooseRestoreNow]

3. U plohu **Vraćanje** odaberite sigurnosne kopije izvor. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    Mogućnosti **sigurnosnog kopiranja aplikacija** prikazuje sve postojeće sigurnosne kopije trenutnu aplikaciju, a jednostavno možete odabrati jednu. 
    Mogućnost **pohrane** omogućuje odabir bilo koju datoteku sigurnosne kopije ZIP iz svih postojeći račun za Azure prostora za pohranu i spremnik u svoju pretplatu. 
    Ako želite vratiti sigurnosnu kopiju nekoj drugoj aplikaciji, upotrijebite mogućnost **za pohranu** .

4. Nakon toga određivanje odredišta za vraćanje aplikacije u **Vraćanje odredište**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Ako odaberete **Prebriši**, bit će izbrisane sve postojeće podatke u trenutnom aplikacije. Prije nego što kliknete **u redu**, provjerite je li točno ono što želite učiniti.
    
    Možete odabrati **Postojeću aplikacije** da biste vratili sigurnosnu kopiju aplikacije u nekoj drugoj aplikaciji u istoj grupi nasljeđivanje vrste. Prije koristili tu mogućnost, trebali biste ste već stvorili nekoj drugoj aplikaciji u grupu resursa s zrcaljenje konfiguraciju baze podataka u jednu definirano u sigurnosnu kopiju aplikacije. 
    
5. Kliknite **u redu**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Preuzimanje ili brisanje sigurnosne kopije s računa za pohranu
    
1. Iz glavnog plohu **Pregled** portala za Azure odaberite **Računi za pohranu**.
    
    Prikazat će se popis svoje postojeće račune za pohranu. 
    
2. Odaberite račun za pohranu koji sadrži sigurnosne kopije koju želite preuzeti ili izbrisati.
    
    Prikazat će se plohu za račun za pohranu.

3. Odaberite željeni spremnik accountn plohu prostora za pohranu
    
    ![Prikaz spremnika][ViewContainers]

4. Odaberite datoteku sigurnosne kopije koje želite preuzeti ili izbrisati.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Kliknite **Preuzimanje** ili **Brisanje** ovisno o tome što želite učiniti.  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Praćenje postupak vraćanja
    
1. Da biste vidjeli detalje o uspjelo ili nije operacije vraćanja aplikacije, pomaknite se do plohu **Zapisnik nadzora** na portalu za Azure. 
    
    Plohu **zapisnike nadzora** prikazuje sve operacije, zajedno s razine, stanje, resursa i detalje o vremenu.
    
2. Pomaknite se prema dolje da biste pronašli na željeni postupak vraćanja i odaberite ga klikom.

Detalji o plohu prikazat će dostupne informacije vezane uz postupak vraćanja.
    
## <a name="next-steps"></a>Daljnji koraci

Sigurnosno kopiranje i vraćanje aplikacije servisa za aplikacije pomoću REST API-JA (pogledajte [Korištenje OSTALE sigurnosne kopije i vraćanje aplikacije servisa za aplikacije](websites-csm-backup.md)).

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
