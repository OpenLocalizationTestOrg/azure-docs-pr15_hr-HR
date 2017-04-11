
1. U plohu **hibridnog veze** kliknite vezu hibridnog koji ste upravo stvorili, a zatim kliknite **Postavljanje ga Slušatelj**.
    
    ![Kliknite Postavljanje ga Slušatelj](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. Otvorit će se plohu **hibridnog svojstva veze** . U odjeljku **Upravitelj veze s lokalnog hibridnog**, odabir **Preuzimanje i ručno konfiguriranje**, spremiti preuzete HybridConnectionManager.msi paketa i kopirajte niz za povezivanje pristupnika.
    
    ![Kliknite ovdje da biste instalirali](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. Iz administratorski naredbeni redak, upišite sljedeću naredbu da biste pokrenuli instalacijski program:

        start HybridConnectionManager.msi
 
7. Nakon izvođenja instalacijski program, kliknite **ne sada**, a zatim dođite do mape %ProgramFiles%\Microsoft\HybridConnectionManager, pokrenite HCMConfigWizard.exe i kliknite **da** u dijaloškom okviru **Kontrola korisničkih računa** .
        
7. Zalijepite niz za povezivanje hibridnog koju ste ranije kopirali pa kliknite **u redu**. 
    
    ![Instaliranje](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Kada se instalacija završi, kliknite **Zatvori**.
    
    ![Kliknite Zatvori](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    Stupac **Stanje** na plohu **hibridnog veze** sada prikazuje **povezan**. 
    
    ![Povezani statusa](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)