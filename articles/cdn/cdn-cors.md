<properties
    pageTitle="Azure CDN pomoću CORS | Microsoft Azure"
    description="Saznajte kako koristiti u Azure sadržaja isporuke mreže (CDN) da biste s više Origin resursa zajedničko korištenje (CORS)."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Azure CDN pomoću CORS     

## <a name="what-is-cors"></a>Što je CORS?

CORS (presjeći polazište zajedničko korištenje resursa) je HTTP značajka koja omogućuje web-aplikacije pokretanje u odjeljku jednu domenu da biste pristupili resursa u drugu domenu. Da biste smanjili mogućnost skriptiranja napada web-mjesta, svi Moderna web-preglednici implementirati sigurnosnog ograničenja naziva [isti polazište pravila](http://www.w3.org/Security/wiki/Same_Origin_Policy).  To sprječava web-stranice iz pozivanja API-ji u drugu domenu.  CORS omogućuje siguran način da biste omogućili jednu domenu (domenu izvor) da biste pozvali API-ji u drugu domenu.
 
## <a name="how-it-works"></a>Kako funkcionira
1.  Web-pregledniku šalje zahtjev za mogućnosti s **polazište** HTTP zaglavlje. Vrijednost tog zaglavlja je poslužena na stranici domena. Kada stranice s https://www.contoso.com pokuša pristupiti podacima o korisniku u domenu fabrikam.com, sljedeće zaglavlje zahtjev će biti poslane fabrikam.com: 
    
    `Origin: https://www.contoso.com`
 
2.  Poslužitelj možda odgovor na poruke uz sljedeće:
    - **Pristup-kontrola-Dopusti-izvor** zaglavlja u njegov odgovor koji označava dopušteno koje polazište web-mjesta. Ako, na primjer:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Stranica pogreške ako poslužitelj ne dopušta zahtjev za više izvora
    - **Pristup-kontrola-Dopusti-izvor** zaglavlja s zamjenski znak koji omogućuje sve domene:
        
        `Access-Control-Allow-Origin: *`
 
Složena HTTP zahtjeva postoji "prije isporuke" zahtjev gotovo prvog određuju ima li dozvolu prije slanja zahtjeva za cijelu.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Zamjenski znak ili jedan izvor scenariji

CORS na Azure CDN funkcionirat će automatski bez dodatna konfiguracija kada **Access-kontrola-Dopusti-izvor** zaglavlja postavljen na zamjenski znak (*) ili jedan izvor.  Na CDN će predmemoriju prvi odgovor i daljnji zahtjevi će koristiti isti zaglavlja.
 
Ako zahtjeva već uvedena CDN prije CORS nije moguće postaviti na na izvor, morat ćete čišćenje sadržaja na krajnjoj točki sadržaja da biste ponovno učitali sadržaja pomoću **Pristup-kontrola-Dopusti-izvor** zaglavlja.
 
## <a name="multiple-origin-scenarios"></a>Scenariji više izvora

Ako morate dopustiti specifičan popis drugačijeg izvora da biste imati dopuštenje za CORS, što će se nešto složenije. Problem se pojavljuje kada u CDN predmemorira zaglavlje **Pristup-kontrola-Dopusti-polazište** za prvi CORS polazište.  Kada drugi CORS origin izvrši zahtjeva za kasnije, na CDN će poslužena predmemorirani **Pristup-kontrola-Dopusti-Origin** zaglavlje, koji ne odgovaraju.  Da biste to ispravili na nekoliko načina.
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium s Verizon

Da biste omogućili tu, najbolje je da biste koristili **Premium CDN Azure s Verizon**, koji se izlaže neke napredne funkcije. 
 
Morat ćete [stvoriti pravilo](cdn-rules-engine.md) da biste provjerili **Origin** zaglavlje na zahtjev.  Ako je valjan origin, pravilo postavit zaglavlje **Pristup-kontrola-Dopusti-Origin** s origin naveden u zahtjevu za.  Ako origin naveden u zaglavlju **polazište** je dopušteno, pravilo treba izostavite zaglavlje **Pristup-kontrola-Dopusti-Origin** čega će preglednik da biste odbili zahtjev. 
 
Da biste to učinili modul pravila na dva načina.  U oba slučaja zaglavlje **Pristup-kontrola-Dopusti-polazište** iz datoteke poslužitelj potpuno zanemarena, modul pravila u CDN potpuno upravlja dopuštene drugačijeg izvora u CORS.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Uobičajeni izraz sa svim valjani drugačijeg izvora
 
U tom slučaju možete stvoriti uobičajenog izraza koji uključuje sva drugačijeg izvora želite dopustiti: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Azure CDN iz Verizon** koristi [Perl kompatibilne Regularni izrazi](http://pcre.org/) kao njegov modula za regularne izraze.  Da biste provjerili valjanost Uobičajeni izraz možete koristiti alat kao što su [uobičajeni 101 izraza](https://regex101.com/) .  Imajte na umu da znak "/" vrijedi u regularne izraze i ne morate unijeti prespojni znak, no escaping taj znak preporučeni način rada i očekuje neke regex alata za provjeru valjanosti.

Uobičajeni izraz udovoljava, pravilo će zamijeniti **Pristup-kontrola-Dopusti-izvor** zaglavlja (ako ih ima) iz polazište s polazište koju je poslao zahtjev.  Možete dodati i dodatne CORS zaglavlja, kao što su **Kontrola-Dopusti – načini pristupa**.

![Primjer pravila s uobičajenog izraza](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Pravilo zaglavlja zahtjev za svaki izvor.

Umjesto regularne izraze, umjesto toga možete stvoriti zaseban pravilo za svaki izvor kojeg želite dopustiti pomoću **Zatražiti zamjenski zaglavlja** [zadovoljavaju uvjet](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1). Kao i kod metodu Uobičajeni izraz modul pravila koja se sama postavlja CORS zaglavlja. 
  
![Primjer pravila bez uobičajenog izraza](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] U primjeru iznad, koristite zamjenski znak * govori modul pravila tako da odgovara HTTP i HTTPS.
 
### <a name="azure-cdn-standard"></a>Standardna Azure CDN

Na Azure CDN standardni profili samo mehanizam da biste dopustili više drugačijeg izvora bez korištenja zamjenskih polazište je koristiti [predmemoriju niza upita](cdn-query-string.md).  Morate omogućiti postavku niza upita za krajnju točku CDN, a zatim pomoću niza upita jedinstveni zahtjeva iz svakog dopuštene domene za. Time će rezultirati CDN predmemoriranje zasebni objekt za svaki niz jedinstveni upita. Taj se način, međutim, nije idealna, kao što će rezultirati većeg broja primjeraka predmemorirani na na CDN iste datoteke.  

