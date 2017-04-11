
1. Posjetite [Azure Portal]. Kliknite **Pregledaj sve** > **Mobilne aplikacije** > pozadinskog koju ste upravo stvorili. U odjeljku postavke mobilne aplikacije kliknite **brzi početak rada** > **Cordova**. U odjeljku **Konfiguriranje klijentska aplikacija**odaberite **Stvori novu aplikaciju**, a zatim kliknite **Preuzmi**. Time će se preuzeti dovršili projekt Cordova unaprijed konfiguriran za povezivanje s vašeg pozadinski aplikacije.

2. Otpakiravanje preuzetu datoteku ZIP na direktorij na tvrdom disku, dođite do datoteke rješenja (.sln) i otvoriti pomoću programa Visual Studio.

5. U Visual Studio, odaberite platformu rješenje (Android, iOS ili Windows) s padajućeg izbornika pokraj strelicu start, a zatim odaberite određene implementacije uređaja ili emulator tako da kliknete padajući izbornik zelenu strelicu. Imajte na umu da možete koristiti zadane platformu za Android i emulator val. Vodiči za naprednije ćete morati odaberite podržani uređaj ili emulator. 

6. Pritisnite F5 ili kliknite zelenu strelicu da biste sastavili i i pokretanje aplikacije Cordova. Ako vam se prikaže dijaloški okvir sigurnosne u emulator traženja pristupa s mrežom, prihvatite je.   

7. Nakon što se pokreće aplikaciju na uređaj ili emulator, upišite smislen tekst u **novi tekst Enter**, kao što je _Dovršeno vodič_ , a zatim kliknite gumb **Dodaj** .  
To Azure pozadinskog ranije uveden šalje zahtjev za objavu. Umeće podatke pozadinskog iz zahtjeva za u tablicu TodoItem u SQL baze podataka i vraća informacije o upravo pohranjene stavke natrag u mobilnoj aplikaciji. Mobilne aplikacije na popisu prikazuje podatke.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Ponovite prethodna tri koraka za svaku platforme uređaja koje namjeravate podržava.

[Portal za Azure]: https://portal.azure.com/
