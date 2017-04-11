<properties
    pageTitle="Ispravljanje pogrešaka Azure aplikacije u Eclipse"
    description="Informirajte se o aplikacijama Azure ispravljanje pogrešaka koji se korištenje kompleta alata za Azure za Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Ispravljanje pogrešaka Azure aplikacije u Eclipse #

Korištenje kompleta alata za Azure za Eclipse, možete ispraviti pogreške aplikacije li kojima rade u Azure ili emulator računalnim ako koristite Windows kao operacijski sustav. Sljedeća slika prikazuje dijaloški okvir koji se koristi da biste omogućili daljinsko uklanjanje programskih pogrešaka svojstava **značajka ispravljanja pogrešaka** :

![][ic719504]

Pomoću ovog praktičnog vodiča podrazumijeva da već imate aplikaciju koja je uspješno stvorena pa se upoznati s računalnim emulator i implementacija Azure.

Ćemo kao početnu točku koristiti aplikaciju praktičnom vodiču [pomoću biblioteke izvođenje servisa Azure u JSP][] ove teme. Prije nastavka, ako već niste učinili stvoriti te aplikacije.

## <a name="to-debug-your-application-while-running-in-azure"></a>Za ispravljanje pogrešaka aplikacije prilikom pokretanja servisu Azure ##

>[AZURE.WARNING] Kompleta alata za trenutni podrška za daljinsko Java uklanjanje programskih pogrešaka prvenstveno je namijenjen implementacijama izvodi u emulator Azure računalnim. Budući da je veza za ispravljanje pogrešaka sigurna, ne trebate omogućiti daljinsko uklanjanje programskih pogrešaka u implementacijama radnog. Ako vam je potrebna za ispravljanje pogrešaka aplikacije koje se izvode u oblaku Azure, koristiti pripremna implementaciju, ali shvatite da neovlaštenog pristupa za ispravljanje pogrešaka sesiju moguće je ako netko zna IP adresa uvođenja oblaka, čak i ako je pripremna implementacije.

1. Stvaranje projekta za testiranje u na emulator: U Eclipse na Project Explorer desnom tipkom miša kliknite **MyAzureProject**, kliknite **Svojstva**, kliknite **Azure**i postavite **izgraditi za** **implementaciju na cloud**.
1. Ponovno stvaranje projekta: na izborniku Eclipse kliknite **projekt**, a zatim kliknite **Sastavljanje sve**.
1. Implementacija aplikacije da biste *pripremna* u Azure.
    >[AZURE.IMPORTANT] Kao što je rečeno iznad, preporučljivo je ispraviti pogreške u emulator računalnim u većini slučajeva, zatim ispravljanje pogrešaka u okruženju pripremna samo ako je potrebna dodatna ispravljanje pogrešaka. Preporučuje se ne ispravljanje pogrešaka u radnom okruženju.
1. Kada uvođenja spreman u Azure, dobiti naziv DNS-a za implementaciju s [Portal za upravljanje Azure][]. Pripremna implementacije ima naziv DNS-a u obliku http://*&lt;guid&gt;*. cloudapp.net, gdje * &lt;guid&gt; * je dodijelio Azure GUID vrijednost.
1. U Eclipse na Project Explorer desnom tipkom miša kliknite **WorkerRole1**, kliknite **Azure**, a zatim **značajka ispravljanja pogrešaka**.
1. U dijaloškom okviru **Svojstva za ispravljanje pogrešaka WorkerRole1** :
    1. Provjera **omogućiti daljinsko uklanjanje programskih pogrešaka za ta uloga.**
    1. Da biste postigli **krajnja točka unosa za korištenje**, koristite **značajka ispravljanja pogrešaka (javno: 8090, Privatno: 8090)**.
    1. Provjerite je li poništen potvrdni okvir **Pokretanje JVM u obustavljenom načinu, Čekanje na vezu za ispravljanje pogrešaka** .
        >[AZURE.IMPORTANT] Mogućnost **Pokretanje JVM u obustavljenom načinu, Čekanje na vezu za ispravljanje pogrešaka** namijenjen za ispravljanje pogrešaka scenariji u emulator računalnim na samo za napredno (ne i za implementacije u oblaku). Ako se koristi mogućnost **Pokretanje JVM u obustavljenom načinu, Čekanje na vezu za ispravljanje pogrešaka** odgoditi će na poslužitelj prilikom pokretanja postupka dok ispravljanje Eclipse povezano s njegova JVM. Dok za ispravljanje pogrešaka sesiju pomoću emulator računalnim nije koristili tu mogućnost, nije ga koristiti za ispravljanje pogrešaka sesiju u oblak implementacije. Inicijalizacija na poslužitelju u zadatku Azure pokretanje odvija, a Azure oblaka ne dostupnim javno krajnje točke dok ne Završi zadatak pokretanja. Dakle, pokretanje procesa će uspješno dovršena ako je omogućena je u oblak uvođenja, jer je nećete moći primati veze s vanjskim klijenta za Eclipse.
1. Kliknite **Stvori konfiguracije za ispravljanje pogrešaka**.
1. U dijaloškom okviru **Konfiguriranje ispravljanje pogrešaka Azure** :
    1. **Java projekta za ispravljanje pogrešaka**, odaberite mogućnost projekt **MyHelloWorld** .
    1. **Konfiguriranje ispravljanje pogrešaka za**, provjerite **Azure oblaka (pripremna)**.
    1. Provjerite je li **Azure izračunati emulator** nije potvrđen.
    1. Za **glavno računalo**, unesite naziv DNS postupnu uvođenja, ali bez prethodnog **http://**. Na primjer (koristi vaše GUID umjesto GUID ovdje prikazani): **4e616d65-6f6e-6 d 65-6973-526f62657274.cloudapp.net**
1. Kliknite **u redu** da biste zatvorili dijaloški okvir za **Konfiguraciju za ispravljanje pogrešaka za Azure** .
1. Kliknite **u redu** da biste zatvorili dijaloški okvir **Svojstva za ispravljanje pogrešaka WorkerRole1** .
1. Ako još nemate točku prekida je već postavio u index.jsp, postavite ga:
    1. Unutar Eclipse na Project Explorer proširite **MyHelloWorld**, proširite **WebContent**i dvokliknite **index.jsp**.
    1. Unutar index.jsp, desnom tipkom miša kliknite plava traka s lijeve strane kodu programskog jezika Java, a zatim kliknite **Uključivanje i isključivanje prekidne točke**, kao što je prikazano na sljedećim mjestima: ![][ic551537]
1. Unutar Eclipse na izborniku kliknite **Pokreni** , a zatim kliknite **Konfiguracije za ispravljanje pogrešaka**.
1. U dijaloškom okviru **Za ispravljanje pogrešaka konfiguracije** proširite **Udaljenog računala Java** u lijevom oknu, odaberite **Azure oblaka (WorkerRole1)**pa kliknite **ispravljanje pogrešaka**.
1. U pregledniku pokrenuti postupnu aplikaciju, **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, zamjenjujući GUID iz naziva DNS-a za * &lt;guid&gt;*. Ako se zatraži da **Potvrdite promjenu perspektive** dijaloški okvir, kliknite **da**. Ispravljanje pogrešaka sesiju sada moraju izvršiti redak kod ondje gdje je postavljeno na točku prekida.

>[AZURE.NOTE] Ako se pokuša pokrenuti u alat za analizu daljinske ispravljanje pogrešaka veza implementacije koja je pokrenut više instanci uloge, ne možete trenutno kontrolirati koje instancu program za ispravljanje pogrešaka će se prethodno povezati, kao što je Azure opterećenja će nasumično odaberite instancu. Kada ste povezani s tu instancu, no nastavit ispravljanje pogrešaka u istoj instanci. Napomena i, ako je razdoblje neaktivnosti više od 4 minuta (na primjer, kada ste na točku prekida predugo), Azure zatvoriti u vezu.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Ispravljanje pogrešaka posebnu ulogu instancu više instanci implementacije ##

Kada implementaciju sustava radi u oblaku, najvjerojatnije pokrećete ga u više računalnim, ili uloga instance. Omogućuje vam da iskoristite prednost Azure 99.95% dostupnost jamstva i promjena veličine izvan vaše aplikacije.

U takvim slučajevima, morate daljinski ispravljanje pogrešaka aplikacije Java u određenim ulogama instanci. Međutim, Ako omogućite samo običan unos krajnje za ispravljanje pogrešaka Azure opterećenja će vam tako biti gotovo nemoguće povezivanje program za ispravljanje pogrešaka na instancu komponente posebnu ulogu. Umjesto toga ga će vas spojiti na instancu komponente uloga izdvajanja nasumično.

Ovo je vrsta scenarij gdje iskoristi instancu unos krajnje točke će olakšati za ispravljanje pogrešaka posebnu ulogu instance.

Recimo da namjeravate pokrenuti do 5 uloga pojavljivanja za implementaciju sustava. Korištenje stranica svojstava **krajnje točke** u dijaloškom okviru svojstva uloga, stvaranje krajnje točke unosa instance web-mjesta i dodijeliti određeni raspon priključaka javno umjesto broj priključka jedan. Ako, na primjer, u okviru ulazni **javno priključak** navedite **81 85**.

Nakon implementacije aplikacije s ovu instancu krajnjoj točki Azure će dodijeliti jedinstveni broj priključka iz ovog raspona svakoj instanci uloge. Zatim, da biste saznali koji instancu imate dodijeljeno koje broj priključka, možete koristiti varijablu okruženja *InstanceEndpointName***_PUBLICPORT** (pri čemu *InstanceEndpointName* je dodijeljena kada ste stvorili krajnju točku instancu) automatski konfigurirana tako da na komplet alata za implementaciju sustava (Ako, na primjer, vraćanjem njegovom vrijednošću u podnožje web-stranice tako da nije moguće je pročitati kada pregledavate Internet da biste ga).

Kada znate koje broj priključka za javno tu instancu dodijeljena, ga možete referencirati u konfiguraciji ispravljanje pogrešaka u Eclipse, tako da affixing na naziv glavnog računala servisa. To će omogućiti Eclipse program za ispravljanje pogrešaka za povezivanje s tu instancu i neke druge instance.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Samo u sustavu Windows: za ispravljanje pogrešaka aplikacije pokrenutim emulator računalnim ##

>[AZURE.NOTE] Azure emulator dostupna je samo u sustavu Windows. Ako koristite operacijski sustav koji nije Windows, preskočite ovaj odjeljak. 

1. Stvaranje projekta za testiranje u na emulator: U Eclipse na Project Explorer desnom tipkom miša kliknite **MyAzureProject**, kliknite **Svojstva**, kliknite **Azure**i postavite **Sastavljanje za** **testiranje u emulator**.
1. Ponovno stvaranje projekta: na izborniku Eclipse kliknite **projekt**, a zatim kliknite **Sastavljanje sve**.
1. U Eclipse na Project Explorer desnom tipkom miša kliknite **WorkerRole1**, kliknite **Azure**, a zatim **značajka ispravljanja pogrešaka**.
1. U dijaloškom okviru **Svojstva za ispravljanje pogrešaka WorkerRole1** :
    1. Provjera **omogućiti daljinsko uklanjanje programskih pogrešaka za ta uloga.**
    1. Da biste postigli **krajnja točka unosa za korištenje**koristite zadane krajnje točke automatski generira alata navedena kao **značajka ispravljanja pogrešaka (javno: 8090, Privatno: 8090)**.
    1. Provjerite je li isključena mogućnost **Pokretanje JVM u obustavljenom načinu, Čekanje na vezu za ispravljanje pogrešaka** .
        >[AZURE.IMPORTANT] Mogućnost **Pokretanje JVM u obustavljenom načinu, Čekanje na vezu za ispravljanje pogrešaka** namijenjen za ispravljanje pogrešaka scenariji u emulator računalnim na samo za napredno (ne i za implementacije u oblaku). Ako se koristi mogućnost **Pokretanje JVM u obustavljenom načinu, Čekanje na vezu za ispravljanje pogrešaka** odgoditi će na poslužitelj prilikom pokretanja postupka dok ispravljanje Eclipse povezano s njegova JVM. Dok za ispravljanje pogrešaka sesiju pomoću emulator računalnim nije koristili tu mogućnost, nije ga koristiti za ispravljanje pogrešaka sesiju u oblak implementacije. Inicijalizacija na poslužitelju u zadatku Azure pokretanje odvija, a Azure oblaka ne dostupnim javno krajnje točke dok ne Završi zadatak pokretanja. Dakle, pokretanje procesa će uspješno dovršena ako je omogućena je u oblak uvođenja, jer je nećete moći primati veze s vanjskim klijenta za Eclipse.
1. Kliknite **Stvori konfiguracije za ispravljanje pogrešaka**.
1. U dijaloškom okviru **Konfiguriranje ispravljanje pogrešaka Azure** :
    1. **Java projekta za ispravljanje pogrešaka**, odaberite mogućnost projekt **MyHelloWorld** .
    1. **Konfiguriranje ispravljanje pogrešaka za**, provjerite **Azure izračunati emulator**.
1. Kliknite **u redu** da biste zatvorili dijaloški okvir za **Konfiguraciju za ispravljanje pogrešaka za Azure** .
1. Kliknite **u redu** da biste zatvorili dijaloški okvir **Svojstva za ispravljanje pogrešaka WorkerRole1** .
1. Postavite na točku prekida u index.jsp:
    1. Unutar Eclipse na Project Explorer proširite **MyHelloWorld**, proširite **WebContent**i dvokliknite **index.jsp**.
    1. Unutar index.jsp, desnom tipkom miša kliknite plava traka s lijeve strane kodu programskog jezika Java, a zatim kliknite **Uključivanje i isključivanje prekidne točke**, kao što je prikazano na sljedećim mjestima: ![][ic551537]

       Ako vidite ikonu točku prekida unutar plava traka s lijeve strane kodu programskog jezika Java postavljen je na točku prekida.
1. Pokretanje aplikacije u emulator računalnim klikom na gumb **pokreću se u Azure Emulator** na alatnoj traci Azure.
1. Unutar Eclipse na izborniku kliknite **Pokreni** , a zatim kliknite **Konfiguracije za ispravljanje pogrešaka**.
1. U dijaloškom okviru **Za ispravljanje pogrešaka konfiguracije** proširite **Udaljenog računala Java** u lijevom oknu, odaberite **Azure Emulator (WorkerRole1)**pa kliknite **ispravljanje pogrešaka**.
1. Nakon emulator računalnim označava da program izvodi, preglednika i pokrenite **http://localhost:8080/MyHelloWorld**.
    Ako se zatraži da **Potvrdite promjenu perspektive** dijaloški okvir, kliknite **da**.
    Ispravljanje pogrešaka sesiju sada moraju izvršiti redak kod ondje gdje je postavljeno na točku prekida.

To prikazivao upute za ispravljanje pogrešaka u emulator računalnim; u sljedećem odjeljku objašnjava implementiran na Azure aplikaciju za ispravljanje pogrešaka.

## <a name="debugging-notes"></a>Ispravljanje pogrešaka bilješke ##

* Nakon ispravljanje pogrešaka, možete se prebacivati perspektive iz **ispravljanje pogrešaka** **Java** putem kliknete Eclipse na izborniku klikom **prozor** **Otvori perspektive**i odabirom perspektive koji želite koristiti.
* Da biste omogućili daljinsko uklanjanje programskih pogrešaka u GlassFish, nemojte koristiti udaljene pogrešaka konfiguracije značajka komplet alata za Azure za Eclipse. Umjesto da ručno konfiguriranje GlassFish. Zbog načina GlassFish tretira Java mogućnosti unaprijed definirane u varijable okruženja, kompleta alata za udaljene pogrešaka konfiguracije značajka neće ispravno funkcionirati s GlassFish. Ako je omogućen kompleta alata za udaljene pogrešaka konfiguracije značajke, to može onemogućiti GlassFish.

## <a name="see-also"></a>Vidi također ##

[Azure komplet alata za Eclipse][]

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Portal za upravljanje Azure]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Pomoću biblioteke izvođenje servisa Azure u JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
