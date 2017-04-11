## <a name="prerequisites"></a>Preduvjeti

Prije nego što smo možete napisati CDN upravljanje kod, moramo učinite neke Priprema da biste omogućili naš kod za interakciju s Voditelj resursa Azure.  Da biste to učinili, morat ćete:

* Stvorite grupu resursa koji sadrže profila CDN ćemo stvoriti pomoću ovog praktičnog vodiča
* Konfiguriranje Azure Active Directory za davanje provjere autentičnosti za naše aplikaciju
* Primijenite dozvole za grupu resursa tako da samo ovlašteni korisnici iz naših Azure AD klijenta možete raditi s naše CDN profila

### <a name="creating-the-resource-group"></a>Stvaranje grupa resursa

1. Prijava na [Portal za Azure](https://portal.azure.com).

2. Kliknite gumb **Novo** u gornjem lijevom kutu, a zatim **Upravljanje**i **Grupu resursa**.
    
    ![Stvaranje nove grupe resursa](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Nazovite grupa resursa *CdnConsoleTutorial*.  Odaberite pretplatu, a zatim odaberite mjesto vašoj blizini.  Ako želite, možda kliknite **Prikvači na nadzornoj ploči** potvrdni okvir da biste prikvačili grupa resursa nadzorne ploče na portalu.  To će vam tako biti kasnije lakše pronaći.  Kad unesete odabira, kliknite **Stvori**.

    ![Dodjela naziva grupa resursa](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Kad grupa resursa, ako niste Prikvači na nadzornu ploču, možete je pronaći tako da kliknete **Pregled**, a zatim **Grupe resursa**.  Kliknite grupu resursa da biste ga otvorili.  Zabilježite **ID pretplate**.  Ne možemo će vam kasnije trebati.

    ![Dodjela naziva grupa resursa](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Stvaranje aplikacije za Azure AD i dozvola

Postoje dva načina prikaza za provjeru autentičnosti aplikacije pomoću servisa Azure Active Directory: pojedinačnih korisnika ili glavni servisa. Glavni servis je slična račun servisa u sustavu Windows.  Umjesto određenom korisniku dodjelu dozvola za interakciju s profilima CDN, umjesto toga ne možemo Dodjela dozvola za servis glavni.  Upravitelji servisa se općenito koriste za automatiziranog, koji nije interaktivan procesa.  Iako ovog praktičnog vodiča je pisanje aplikaciju interaktivne konzole smo ćete fokus na glavni pristupu servisa.

Stvaranje glavni servis sastoji se od nekoliko koraka, uključujući stvaranja aplikacije za Azure Active Directory.  Da biste to učinili, ne možemo ćete [slijedite ovaj Praktični vodič](../articles/resource-group-create-service-principal-portal.md).

> [AZURE.IMPORTANT] Pripazite da slijedite korake u [povezane vodič](../articles/resource-group-create-service-principal-portal.md).  Je *iznimno važno* da dovršavanja točno onako kao što je opisano.  Provjerite jeste li Imajte na umu svoj **ID klijenta**, **naziv domene za klijenta** (najčešće na *. onmicrosoft.com* domene osim ako ste naveli prilagođenu domenu), **ID klijenta**i **ključ za provjeru autentičnosti klijenta**, kao što smo ćete te kasnije.  Pripazite da vrlo štititi **ID klijenta** i **ključ za provjeru autentičnosti klijenta**, kao što je koristi te vjerodajnice može biti svatko dopušta izvršavanje operacija kao Upravitelj servisa. 
>   
> Kada dođete do koraka pod nazivom [Konfiguriraj više klijentske aplikacije](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application), odaberite " **ne**".
> 
> Kada dođete na korak [dodijelili aplikacije ulogu](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role), koristiti grupu resursa koju smo stvorili u prethodnom, *CdnConsoleTutorial*, ali umjesto ulozi **čitač** dodijeliti ulogu **Suradnika CDN profila** .  Kada aplikacija dodijeliti ulogu **Suradnika CDN profila** u grupu resursa, vratite se u ovom ćete praktičnom vodiču. 

Kada ste stvorili na servisu glavni i dodijeljena uloga **Suradnika profila CDN** plohu **korisnika** za grupu resursa trebao bi izgledati ovako.

![Korisnici plohu](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Provjera autentičnosti interaktivnog korisnika

Ako umjesto glavni servisa, bi korisnika radije imala provjere autentičnosti interaktivne pojedinačnih korisnika, postupak vrlo je sličan onome za glavni servisa.  Zapravo, morat ćete slijedite isti postupak, ali mijenjati nekoliko manji.

> [AZURE.IMPORTANT] Samo slijedite ove daljnje korake ako odabirete za značajku provjere autentičnosti pojedinačnih korisnika servisa glavni.

1. Prilikom stvaranja aplikacije, a ne **Web-aplikacije**, odaberite **matičnoj aplikaciji**. 
    
    ![Matičnoj aplikaciji](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Na sljedećoj stranici, zatražit će se za na **preusmjeravanje URI**.  URI nastavlja se neće, ali ne zaboravite što ste unijeli.  Morat ćete ga kasnije. 

3. Nema potrebe da biste stvorili **ključ za provjeru autentičnosti klijenta**.

4. Umjesto dodjeljujete glavni servis ulogu **Suradnika CDN profila** , ne možemo ćete dodijeliti pojedinačnim korisnicima ili grupama.  U ovom primjeru, vidjet ćete I ste ulogu **Suradnika profila CDN** dodijeljen *CDN pokazni videozapis korisnika* .  
    
    ![Pojedinačne korisničkog pristupa](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

