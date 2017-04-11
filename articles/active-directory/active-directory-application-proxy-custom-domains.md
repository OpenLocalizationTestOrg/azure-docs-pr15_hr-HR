<properties
    pageTitle="Rad s prilagođenih domena u Proxy aplikacije Azure AD | Microsoft Azure"
    description="Naslovnice kako raditi s prilagođenih domena u Proxy aplikacije za Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Rad s prilagođenih domena u Proxy aplikacije za Azure AD

Korištenje zadanu domenu omogućuje vam da biste postavili URL-a isti kao interne i vanjske URL-a za pristup aplikacija tako da korisnici imati samo jedan URL-a Imajte na umu da biste pristupili aplikacije, bez obzira na to gdje se pristup s. To omogućuje vam stvaranje jedne prečaca na ploči pristup za aplikaciju. Ako koristite zadanu domenu nudi Proxy aplikacije za Azure AD, postoji bez dodatnih konfiguracije morate omogućiti vaše domene. U slučaju da koristite prilagođenu domenu, postoji nekoliko stvari koje biste trebali učiniti da biste bili sigurni da Proxy aplikacije prepoznaje domene i Provjeri valjanost njegov certifikata.

## <a name="selecting-your-custom-domain"></a>Odabir prilagođene domene

1. Objavite svoju aplikaciju prema uputama u [aplikacijama za objavljivanje s Proxy aplikacije](active-directory-application-proxy-publish.md).
2. Kada se aplikacija prikaže na popisu aplikacija, odaberite ga i kliknite **Konfiguriraj**.
3. U odjeljku **Vanjski URL**, unesite svoju prilagođenu domenu.
4. Ako je vanjski URL https, zatražit će se prenijeti potvrdu da taj Azure može provjeriti valjanost URL-a aplikacije. Možete prenijeti i zamjenskih certifikat koji odgovara vanjski URL aplikacije. Tu domenu, morate biti na popisu vaša [Azure potvrde domene](https://msdn.microsoft.com/library/azure/jj151788.aspx). Azure mora imati certifikat za URL domene aplikacije ili zamjenskih certifikat koji odgovara vanjski URL za aplikaciju.
5. Provjerite je li da biste dodali DNS zapis koji usmjerava interne URL ADRESE za aplikaciju koja vam omogućuje da imaju isti URL za pristup podacima interne i vanjske i jedan prečac za aplikaciju na popisu aplikacija za korisnika.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Najčešća pitanja o radu s prilagođene domene

P: mogu li odabrati potvrdu već prenijeli bez ponovno prenijeti?  
A: prethodno prenesene certifikati su automatski vezane uz aplikacije i nema točno jedan certifikat koji se podudaraju aplikacije naziv glavnog računala.  

P: kako dodati certifikat i u kojem obliku treba izvezene potvrde je moguće prenijeti u?  
A: certifikat mora moguće prenijeti na stranici za konfiguraciju aplikacije. Certifikat mora biti PFX datoteke.  

P: mogu li se certifikati o ECC koristiti?  
A: postoji nema eksplicitnih ograničenja metode potpis.  

P: mogu li se certifikati o SAN koristiti?  
O: da.  

P: mogu li se certifikati o zamjenskih koristiti?  
O: da.  

P: mogu li se drugi certifikat koristiti na svaku aplikaciju?  
A: da, osim ako se dvije aplikacije zajednički koristiti iste vanjskog glavnog računala.  

P: Ako registrirati novu domenu, možete koristiti tu domenu?  
A: da, na popisu domena se ulaže s popisa provjerene domene na klijentu.  

P: što se događa kada je certifikata istekne?  
A: dobit ćete upozorenje u odjeljku certifikata na stranici za konfiguraciju aplikacije. Kada se korisnik pokuša pristup aplikaciji, sigurnosno upozorenje pojavit će se.  

P: što ako želim da biste zamijenili certifikata za dani aplikaciju?  
A: prenesite novi certifikat na stranici za konfiguraciju aplikacije.  

P: mogu li se izbrisati s certifikata i ga zamijeniti?  
A: kada prenesete novi certifikat, ako se stare certifikat ne koristi neku drugu aplikaciju, automatski će se izbrisati.  

P: što se događa kada je povučen u certifikata?  
A: Provjera opoziva neće se izvršiti za potvrde. Kada se korisnik pokuša pristup aplikaciji, ovisno o pregledniku, sigurnosno upozorenje može se prikazati.  

P: mogu li pomoću samopotpisanog certifikata?  
A: da, su dopuštene samopotpisane potvrde Imajte na umu da ako koristite privatne ustanove za izdavanje certifikata, CDP (certifikat opoziva točke raspodjele točke) za certifikat mora biti javno.  

P: postoji mjesto da biste vidjeli sve certifikate za klijentu?  
A: to nije podržano u trenutnoj verziji.  


## <a name="see-also"></a>Vidi također

- [Objavljivanje aplikacija pomoću Proxy aplikacije](active-directory-application-proxy-publish.md)
- [Omogućivanje jedinstvenu prijavu](active-directory-application-proxy-sso-using-kcd.md)
- [Omogućivanje pristupa uvjetno](active-directory-application-proxy-conditional-access.md)
- [Dodajte prilagođeni naziv domene za Azure AD](active-directory-add-domain.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)
