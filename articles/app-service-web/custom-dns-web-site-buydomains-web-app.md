<properties
    pageTitle="Kako kupiti naziv prilagođene domene u Azure aplikacije servisa web-aplikacijama"
    description="Saznajte kako kupiti prilagođenog naziva domene s web-aplikacijama u servisu Azure aplikacije."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Kupite i konfiguriranje prilagođenog naziva domene u aplikacije servisa za Azure

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Kada stvorite web-aplikacijama, Azure dodjeljuje ga poddomenu azurewebsites.net. Ako, na primjer, ako je web-aplikaciju programa pod nazivom **contoso**, URL je **contoso.azurewebsites.net**. Azure dodjeljuje i virtualna IP adresa.

Za web-aplikacije za proizvodnju vjerojatno će korisnici vidjeti prilagođenog naziva domene. U ovom se članku objašnjava kako kupiti i konfiguriranje prilagođene domene u [Aplikaciju servisa Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Pregled

Ako nemate naziv domene za web-aplikacije, možete ga jednostavno kupiti na [Portal za Azure](https://portal.azure.com/). Tijekom postupka za kupnju možete odabrati da bi "www" i korijensku DNS zapisima domene mapirani web App automatski. Također možete upravljati svojih domena unutar Azure Portal.


Kupnja naziva domena i dodjeljivati na web-aplikaciju, poduzmite sljedeće korake.

1. U pregledniku otvorite [Azure Portal](https://portal.azure.com/).

2. Na kartici **Web-aplikacije** kliknite naziv web-aplikacije, odaberite **Postavke**, a zatim odaberite **prilagođenih domena**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. U plohu **prilagođenih domena** kliknite **Kupnja domene**.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. U plohu **Kupnja domene** pomoću tekstni okvir upišite naziv domene koju želite kupiti, a zatim pritisnite Enter. Predloženi raspoloživih domena prikazat će se neposredno ispod tekstnog okvira. Odaberite što domenu koju želite kupiti. Možete kupiti više domena odjednom. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Kliknite **Kontakt informacije** i ispunite obrazac podaci za kontakt za domenu.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] Vrlo je važno Popunite potrebna polja s suvišni točnost moguće, osobito adresu e-pošte. U slučaju kupite domene bez "Zaštita privatnosti", možda će se zatražiti da provjerite je li e-pošte prije nego što domenu postaje aktivna. U nekim slučajevima netočnih podataka podatke za kontakt rezultirat će kupiti domena nije uspjelo. 

6. Sada možete odabrati,

    a) "automatsko je obnavljanje" domene svake godine
    
    b) izborno u "Zaštita privatnosti" koja se nalazi u kupovna cijena besplatno (osim TLD tko je registra ne podržava izjave o zaštiti privatnosti. For example:. co.in,. co.uk itd.)  
    
    c) "dodijelite zadane hostnames" za "www" i korijenski domenu na trenutno web-aplikaciji. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Mogućnost C konfigurira povezivanja DNS-a i naziv glavnog računala povezivanja automatski umjesto vas.  Taj se način web-aplikaciju programa može se pristupiti pomoću prilagođene domene čim kupnje dovršetka (baring DNS prijenos kašnjenja u nekoliko slučajeva). U slučaju web-aplikaciju programa je iza Azure promet upravitelj, nećete vidjeti mogućnost da biste dodijelili korijensku domenu, kao što je A zapisa ne funkcioniraju s programom promet Manager. Uvijek dodjeljujete s domenama/sub-domains kupili putem jednog Web App u drugom Web App i obratno. Korak 8 dodatne pojedinosti potražite u članku. 
    
7. Kliknite na **Odaberite** plohu **Kupnja domene** , a zatim će vidjeti podatke o za kupnju na plohu za **potvrdu** . Ako prihvatili uvjete za pravne i kliknite **kupite**, će se poslati svoje narudžbe, a možete nadzirati kupnje **obavijest**o. Kupnja domene može potrajati nekoliko minuta. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Ako je uspješno naručili domene, možete upravljati domene i dodjeljivanje na web-aplikaciju. Kliknite na **"..."** na desnoj strani domene. Zatim možete **otkazati kupnju** ili **Upravljanje domenom**. Kliknite **Upravljanje domena**, zatim ćemo možete povezati **poddomene** naša aplikacija za web na **domenu za upravljanje** plohu. Ako se želite povezati **poddomene** u različitim Web App izvođenje ovog koraka u kontekstu odgovarajući web-aplikaciji. Na tom mjestu ne za odabir da biste dodijelili domenu na krajnjoj točki Upravitelj promet (Ako je web-aplikacije iza TM) jednostavno odabir Upravitelj promet imenovati na padajućem izborniku. Time domenu i poddomenu automatski dodjeljuju na svim web-aplikacije iza te promet Upravitelj krajnjoj točki. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] "Otkažite kupnju" 5 dana cijelog povrat novca. Nakon 5 dana će vam neće moći "Odustani kupnju", umjesto toga prikazat će se mogućnost "Brisanje" domenu. Brisanje domene rezultirat će otpustite iz pretplate bez povrat novca web-mjesta i neće biti dostupne domene. 

Kada konfiguracije završi, naziv prilagođene domene će biti navedene u odjeljku **povezivanja naziv glavnog računala** web-aplikacije.

Sada, trebali biste moći unesite naziv prilagođene domene u pregledniku, a zatim potražite u članku da je uspješno vodi vas na web-aplikaciju.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Što se događa s prilagođenu domenu ste kupili

Prilagođene domene koje ste kupili u plohu **prilagođenih domena i SSL** povezana Azure pretplatu. Kad Azure resursa, te prilagođene domene je zasebne i neovisno iz aplikacije servisa za aplikacije koji su kupili domenu za. To znači da:

- Unutar portala za Azure možete koristiti prilagođenu domenu ste kupili za više od jedne aplikacije servisa za aplikaciju, a ne samo za kupili prilagođene domene za aplikaciju. 
- Možete upravljati prilagođene domene ste kupili u Azure pretplatu tako da plohu **prilagođenih domena i SSL** od *sve* aplikacije servisa za aplikacije u tu pretplatu.
- Možete dodijeliti bilo koju aplikaciju za aplikacije servisa iz iste pretplate na Azure poddomene unutar te prilagođene domene.
- Ako odlučite da biste izbrisali aplikaciju aplikacije servisa, možete odabrati da ne izbrišete prilagođenu domenu je povezana s ako želite nastaviti koristiti za druge aplikacije.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Ako ne vidite prilagođenu domenu ste kupili

Ako ste kupili prilagođenu domenu s unutar plohu **prilagođenih domena i SSL** , ali ne možete vidjeti prilagođene domene u odjeljku **upravljani domene**, provjerite sljedeće:

- Stvaranje prilagođene domene možda ste završili. Provjerite zvono obavijesti pri vrhu Azure portal za tijek.
- Stvaranje prilagođene domene možda imaju nije uspjela zbog nekog razloga. Provjerite zvono obavijesti pri vrhu Azure portal za tijek.
- Prilagođene domene možda ste je uspjela, ali u plohu možda neće se osvježiti još. Pokušajte ponovno otvoriti plohu **prilagođenih domena i SSL** .
- Možda ste izbrisali prilagođene domene u nekom trenutku. U zapisnicima nadzora tako da kliknete **Postavke** > **Zapisnike nadzora** iz glavnog plohu pokrenite aplikaciju. 
- Tražite plohu **prilagođenih domena i SSL** možda pripada aplikaciju koja je stvorena u neku drugu pretplatu na Azure. Prijeđite u nekoj drugoj aplikaciji u neku drugu pretplatu i provjerite njezinu plohu **prilagođenih domena i SSL** .  
  Unutar portala, nećete se moći vidjeti ili upravljali prilagođene domene stvorene u neku drugu pretplatu Azure od aplikaciju. Međutim, ako kliknete **Dodatno upravljanje** u plohu za **Upravljanje domenom** domenu, bit ćete preusmjereni davatelja domene web-mjesta, gdje ćete moći   [ručno konfiguriranje prilagođenu domenu kao što su vanjski prilagođenu domenu](web-sites-custom-domain-name.md)  
   za aplikacije stvorene u neku drugu pretplatu na Azure. 


