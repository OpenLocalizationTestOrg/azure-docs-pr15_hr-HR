<properties
    pageTitle="Stvaranje web-aplikacijama Java u aplikacije servisa za Azure | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča objašnjava implementacija web-aplikacijama Java aplikacije servisa za Azure."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Stvaranje web-aplikacijama Java u aplikacije servisa za Azure

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Pomoću ovog praktičnog vodiča pokazuje kako stvoriti Java [web app u aplikacije servisa za Azure] pomoću [Portala za Azure]. Portal za Azure je web-sučelja koje možete koristiti da biste upravljali Azure resurse.

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate račun sustava Microsoft Azure. Ako nemate račun, možete ga [aktivirati svoje prednosti pretplatnika Visual Studio] ili [prijavite se za besplatnu probnu verziju].
>
> Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa]. Postoji, možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa za; nužan bez kreditne kartice, a nema p.

## <a name="java-application-options"></a>Mogućnosti aplikacije Java

Možete postaviti Java aplikaciju u web-aplikaciji programa aplikacije servisa na nekoliko načina. 

1. Stvaranje aplikacije programa, a zatim konfiguriranje **postavki aplikacije**.

    Aplikacije servisa za nudi nekoliko Tomcat i Jetty verzija, za zadanu konfiguraciju. Ako aplikaciju koja će biti smješten će raditi s jednim od ugrađenih verzijama, taj način postavljanja spremniku web najlakši, a savršen kada želite učiniti je prijenos war datoteke u spremniku za web. Za ovu metodu stvorite aplikaciju na portalu za Azure, a zatim idite na **Postavke aplikacije** plohu aplikacije da biste odabrali vaša verzija jezika Java uz željeni spremnik web Java. Kada koristite ovu metodu Java i vaše spremnik web su pokrenuti s programske datoteke. Drugi načini smjestite spremnik za web, te potencijalno na JVM prostor na disku. Kada koristite ovaj model, nemate pristup za uređivanje datoteke u taj dio datotečnom sustavu. To znači ne može obavljati zadatke kao što konfigurirati *server.xml* datoteku ili postavite datoteka u biblioteci u mapi */lib* . Dodatne informacije potražite u [Stvaranje i konfiguriranje web-aplikacijama Java](#appsettings) odjeljak u nastavku ovog praktičnog vodiča.
    
2. Pomoću predloška iz trgovine Azure Marketplace.

    Trgovine Windows Azure sadrži predloške koje automatsko stvaranje i konfiguriranje web-aplikacije Java s Tomcat ili Jetty spremnika web. Spremnika web stvorite Predlošci su konfigurirati. Dodatne informacije potražite u odjeljku [Korištenje predloška Java iz trgovine Azure](#marketplace) ovog praktičnog vodiča.
  
3. Stvaranje aplikacije za ručno kopirali i uređivanje datoteka konfiguracije 

    Možda ćete morati hostira prilagođenu aplikaciju Java koja implementacija u bilo kojem od spremnika web nudi aplikacije servisa. Ako, na primjer:
    
    * Aplikacija Java potrebna je verzija Tomcat ili Jetty koja nije izravno podržava aplikacije servisa ili iz galerije.
    * Aplikacija Java uzima HTTP zahtjeva i implementacija kao u WAR u spremniku postojećih web.
    * Želite konfigurirati spremnik web ispočetka. 
    * Želite koristiti verziju jezika Java koji nije podržan u aplikacije servisa i želite prenijeti sami.

    Za predmete kao što su ovi, možete stvoriti aplikaciju pomoću portala za Azure i ručno unijeti odgovarajuće runtime datoteke. U ovom slučaju datoteke će Brojanje na temelju vašeg kvota za pohranu prostora za plan aplikacije servisa. Dodatne informacije potražite u članku [Prijenos prilagođenu web-aplikaciju Java za Azure].

## <a name="portal"></a>Stvaranje i konfiguriranje web-aplikacijama Java

U ovom se odjeljku objašnjava stvorite web-aplikaciju i konfigurirajte je za Java pomoću **postavki aplikacije** plohu portala.

1. Prijava na [Portal za Azure].

2. Kliknite **Novo > Web + Mobile > web-aplikacije**.

    ![Novu web-aplikaciju][newwebapp]

4. Unesite naziv za web-aplikacije u okvir **Web app** .

    Taj naziv mora biti jedinstvena u domeni azurewebsites.net jer URL web-aplikaciji bit će {name}. azurewebsites.net. Ako ne naziv unesite jedinstveni, pojavljuje se crveni uskličnik u tekstni okvir.

5. Odaberite **Grupu resursa** ili stvorite novi.

    Dodatne informacije o grupama resursa potražite u članku [pomoću portala za Azure upravljanja Azure resurse].

6. Odaberite **Plan mjesto aplikacije servisa** ili stvorite novi.

    Dodatne informacije o tarifama za aplikacije servisa za potražite u članku [Pregled tarife aplikacije servisa za Azure].

7. Kliknite **Stvori**.

    ![Stvaranje web-aplikacije][newwebapp2]
 
8. Prilikom stvaranja web-aplikaciji kliknite **web-aplikacije > {web-aplikaciju programa}**.
 
    ![Odaberite Web App][selectwebapp]

9. U plohu **Web app** kliknite **Postavke**.

10. Kliknite **Postavke aplikacije**.

11. Odaberite željenu **verziju jezika Java**. 

12. Odaberite željeni **Java pomoćnu verziju**. Ako ste odabrali **najnovijeg**, aplikacije će koristiti najnovije radne verzije koja je dostupna u aplikacije servisa za taj Java glavnu verziju. Stavka **najnovijeg** je jedinstveni Java aplikacije stvorene iz **Postavke aplikacije**. Ako stvorite Java aplikacije iz galerije, imate upravljanje vlastitim spremnik i JVM promjene. 

12. Odaberite željeni **spremnik Web**. Ako ste odabrali spremnik čiji naziv počinje s **najnovijeg**, aplikacija će biti zadržane na najnoviju verziju te web spremnik glavna verzija koja je dostupna u aplikacije servisa. 

    ![Verzija spremnik za web][versions]

13. Kliknite **Spremi**.

    Unutar nekoliko sekundi dok web-aplikaciju programa će postati utemeljenih i konfiguriran za korištenje spremnik web koji ste odabrali.

14. Kliknite **web-aplikacije > {novog web-aplikaciju programa}**.

15. Kliknite **URL-a** da biste pregledali na novo mjesto.

    Web-stranicu potvrditi da ste stvorili utemeljena na web-aplikacijama.

## <a name="marketplace"></a>Korištenje predloška Java iz trgovine Azure Marketplace

U ovom se odjeljku opisano kako pomoću servisa Azure Marketplace da biste stvorili web-aplikacijama Java. Isti Općenito tijek se može koristiti i za stvaranje utemeljena na mobilni ili aplikaciju za API-JA. 

1. Prijava na [Portal za Azure]

2. Kliknite **Novo > trgovine**.

    ![Nove trgovine][newmarketplace]

3. Kliknite **Web + Mobile**.

    Možda ćete se morati pomaknuti ulijevo da biste vidjeli plohu **Marketplace** gdje možete odabrati **Web + Mobile**.

4. U tekstni okvir za pretraživanje unesite naziv poslužitelja za aplikaciju Java, kao što su **Apache Tomcat** ili **Jetty**, a zatim pritisnite Enter.

5. U rezultatima pretraživanja kliknite poslužitelj aplikacije Java.

    ![Mobilni Jetty web][webmobilejetty]

6. U na prvi **Apache Tomcat** ili **Jetty** plohu, kliknite **Stvori**.

    ![Jetty plohu portala][jettyblade]

7. U na sljedeći **Apache Tomcat** ili **Jetty** plohu, unesite naziv za web-aplikacije u okvir **Web app** .

    Taj naziv mora biti jedinstvena u domeni azurewebsites.net jer URL web-aplikaciji bit će {name}. azurewebsites.net. Ako ne naziv unesite jedinstveni, pojavljuje se crveni uskličnik u tekstni okvir.

8. Odaberite **Grupu resursa** ili stvorite novi.

    Dodatne informacije o grupama resursa potražite u članku [pomoću portala za Azure upravljanja Azure resurse].

9. Odaberite **Plan mjesto aplikacije servisa** ili stvorite novi.

    Dodatne informacije o tarifama za aplikacije servisa za potražite u članku [Pregled tarife aplikacije servisa za Azure].

10. Kliknite **Stvori**.

    ![Stvaranje jetty Portal][jettyportalcreate2]

    U kratko vrijeme, obično manje u minutu Azure Završi stvaranje novog web-aplikaciji.

11. Kliknite **web-aplikacije > {novog web-aplikaciju programa}**.

12. Kliknite **URL-a** da biste pregledali na novo mjesto.

    ![Jetty URL-a][jettyurl]

    Tomcat isporučuje se s zadani skup stranice. Ako ste odabrali Tomcat, prikazat će stranice koji je slično kao u sljedećem primjeru.

    ![Web-aplikacije pomoću Apache Tomcat][tomcat]

    Ako ste odabrali Jetty, prikazat će stranice koji je slično kao u sljedećem primjeru. Jetty ne sadrži zadani skup stranice, pa ovdje ponovno koristiti isti JSP koji se koristi za prazno web-mjesto Java.

    ![Web-aplikacije pomoću Jetty][jetty]

Sad kad ste stvorili web-aplikaciji pomoću programa spremnik aplikaciju, u odjeljku [sljedeće korake](#next-steps) informacije o kako prenijeti aplikacija na web-aplikaciji.

## <a name="next-steps"></a>Daljnji koraci

Sada imate koja Java aplikacije izvodi u web-aplikaciji u servisu Azure aplikacije. Da biste implementirali vlastitog koda web App, pročitajte članak [Dodavanje aplikacije ili web-stranice na web-aplikaciju Java].

Dodatne informacije o razvoju aplikacija za Java u Azure potražite u članku [Razvojni centar za Java].

<!-- URL List -->

[Dodavanje aplikacije ili web-stranicu na web-aplikaciju Java]: ./web-sites-java-add-app.md
[Aplikacije servisa za Azure tarife za pregled]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Portal za Azure]: https://portal.azure.com/
[Aktivacija sustava pogodnosti pretplatnički Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Prijavite se za besplatnu probnu verziju]: http://go.microsoft.com/fwlink/?LinkId=623901
[Pokušajte aplikacije servisa]: http://go.microsoft.com/fwlink/?LinkId=523751
[Web app u aplikacije servisa za Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Razvojni centar za Java]: /develop/java/
[Da biste upravljali resursi za Azure pomoću portala za Azure]: ../azure-portal/resource-group-portal.md
[Prijenos prilagođenu web-aplikaciju Java Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
