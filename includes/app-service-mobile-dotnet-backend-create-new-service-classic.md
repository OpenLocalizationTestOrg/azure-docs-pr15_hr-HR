1. Prijavite se na [Portal za Azure].

2. Kliknite **+ NOVO** > **Web + Mobile** > **Mobilnu aplikaciju**, navedite naziv vaše pozadinskog mobilnu aplikaciju.

3. **Grupa resursa**, odaberite postojeću grupu resursa ili stvorite novi (pomoću isti naziv kao aplikacijom.) 
 
    Možete odabrati neku drugu tarifu za aplikacije servisa ili stvorite novi. Dodatne informacije o aplikaciji servisa tarife i kako stvoriti novi plan u različitim cijene sloju i u željenom mjestu potražite u članku [Pregled za detaljnije tarife aplikacije servisa za Azure](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Aplikacije servisa za planiranje**, odabire se zadani plan (u [standardni sloju](https://azure.microsoft.com/pricing/details/app-service/)). Možete odabrati i neku drugu tarifu, ili [stvorite novi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Aplikacije servisa za planiranje postavki određuju [mjesto, značajke, cijena i resursi za izračun](https://azure.microsoft.com/pricing/details/app-service/) povezan s vašom aplikacijom. 

    Kada odlučite u planu, kliknite **Stvori**. Time se stvara pozadinskog mobilnu aplikaciju. 
    
6. U plohu **Postavke** za nove pozadinske mobilnu aplikaciju, kliknite **Brzi start** > svoju platformu klijentskih aplikacija > **Povezivanje baze podataka**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. U plohu **Dodavanje podatkovne veze baze podataka** kliknite **Bazu podataka SQL** > **Stvori novu bazu podataka**, upišite **naziv**baze podataka, odaberite sloj cijene, a zatim kliknite **poslužitelja**.  Možete ponovno iskoristiti ovu novu bazu podataka. Ako već imate baze podataka na istom mjestu, umjesto toga možete odabrati **Koristi postojeću bazu podataka**. Korištenje baze podataka na nekom drugom mjestu ne preporučujemo da zbog troškove propusnost i Latencija veće.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. U Elektronička ploča **Novi poslužitelja** upišite naziv poslužitelja jedinstveni u polje **naziv poslužitelja** , omogućuju prijavu i lozinku, potvrdite **Dopusti azure servisa za pristup poslužitelju**, a zatim kliknite **u redu**. Ovako možete stvoriti novu bazu podataka.

9. Vratite se u plohu **Dodaj podatkovnu vezu** kliknite **niz za povezivanje**, unesite vrijednosti za prijavu i lozinku za bazu podataka pa kliknite **u redu**. Pričekajte nekoliko minuta za bazu podataka da biste se uvesti uspješno prije nastavka.

<!-- URLs. -->
[Portal za Azure]: https://portal.azure.com/
