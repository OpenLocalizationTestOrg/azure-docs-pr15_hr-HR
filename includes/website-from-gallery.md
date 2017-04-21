Trgovina Azure postaje dostupan brojne popularne web-aplikacije razvio Microsoft, tvrtke trećih strana, a zatim Otvori izvor softver inicijative. Web-aplikacije koje su stvorene iz trgovine Azure zahtijevaju instalaciju softvera za sve osim preglednika koji se koristi za povezivanje s [Portala za pretpregled Azure](http://go.microsoft.com/fwlink/?LinkId=529715). 

U ovom ćete praktičnom vodiču ćete saznati:

- Kako stvoriti novu web-aplikaciju putem servisa Azure Marketplace.

- Kako implementirati web-aplikaciji putem portala za pretpregled Azure.
 
Ćete sastavljanje WordPress blog na kojem se koristi zadani predložak. Sljedeća ilustracija prikazuje dovršene aplikacije:


![Wordpress blog][13]

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="create-a-web-app-in-the-portal"></a>Stvaranje web-aplikacijama na portalu

1. Prijavite se na Portal Azure pretpregled.

2. Otvorite trgovine Windows Azure tako da kliknete ikonu **Marketplace** :

    ![Ikona Marketplace][marketplace]

    Ili klikom na ikonu **Nova** na gornjem desnom kutu na nadzornoj ploči i odabirom **trgovine** pri bottow na popisu.
    
    ![Stvori novo][5]
    
3. Odaberite **Web + Mobile**. Potražite **WordPress** i kliknite ikonu **WordPress** .

    ![WordPress s popisa][7]
    
5. Kad pročitate opis WordPress aplikacije, odaberite **Stvori**.

6. Kliknite **web-aplikacije**i unesite potrebne vrijednosti za konfiguriranje web-aplikaciju programa.
    
    ![Konfiguriranje aplikacije][8]

7. Kliknite **bazu podataka**, a zatim unesite potrebne vrijednosti za konfiguriranje baze podataka MySQL. 

    ![Konfiguriranje baze podataka][database]

8. Navedite naziv za novu grupu resursa.

    ![Postavljanje grupa resursa][groupname]

8. Ako je potrebno, kliknite **PRETPLATU**, a odredite pretplatu za korištenje. 

7. Nakon što dovršite definiranje web-aplikaciju, kliknite **Stvori**i pričekajte da se stvara novu web-aplikaciju.

   Prilikom stvaranja aplikacije vidjet ćete grupa resursa koja sadrži web-aplikacije i baze podataka.

   ![mogućnost Pokaži grupu][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Pokretanje i upravljanje web-aplikaciju programa WordPress
    
1. Kliknite novu web-aplikaciju da biste vidjeli detalje o aplikacije.

    ![pokretanje nadzorne ploče][10]

2. Na stranici **Essentials** kliknite **Pregledaj** ili vezu u odjeljku **URL-a** da biste otvorili stranicu dobrodošlice web-aplikaciji.

    ![URL web-mjesta][browse]

3. Ako niste instalirali WordPress, unesite podatke o odgovarajući konfiguraciji potrebnih WordPress i kliknite **WordPress instalacija** dovrši konfiguraciju i otvorite stranicu za prijavu u web-aplikaciji.

4. Kliknite **Prijava** , a zatim unesite vjerodajnice.  

5. Morat ćete novog web-aplikacijama WordPress koja izgleda slično web App.    

    ![web-mjesto WordPress][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
