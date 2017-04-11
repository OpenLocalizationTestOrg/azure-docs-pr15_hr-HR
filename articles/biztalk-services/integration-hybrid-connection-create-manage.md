<properties 
    pageTitle="Stvaranje i upravljanje vezama hibridnog | Microsoft Azure" 
    description="Saznajte kako povezivanje hibridnog, upravljanje vezu i instalirajte Upravitelja veze hibridnog. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Stvaranje i upravljanje vezama hibridnog


## <a name="overview-of-the-steps"></a>Pregled koraka
1. Stvorite vezu hibridnog tako da unesete **naziv glavnog računala** ili **FQDN** lokalnog resursa u privatne mreže.
2. Veza Azure web-aplikacijama ili Azure mobilne aplikacije hibridnog vezu.
3. Instalirajte Upravitelja veze hibridnog na vaše lokalne resursa i povezati određenu hibridnog vezu. Portal za Azure sadrži samo jedan pritisak sučelje za instalaciju i povezivanje.
4. Upravljanje vezama hibridnog i ključ veze.

Ova tema sadrži popis ove korake. 

> [AZURE.IMPORTANT] Nije moguće postaviti krajnjoj točki hibridnog vezu s IP adresom. Ako koristite IP adresu, možete ili može pristupiti lokalnog resursa, ovisno o klijentu. Veza za hibridno ovisi o klijenta način DNS-a pretraživanja. U većini slučajeva, __klijent__ je kodu aplikacije. Ako klijent izvođenje DNS vrijednosti, (ga neće pokušati riješiti IP adresu kao da je naziv domene (x.x.x.x)), a zatim promet se šalju putem veze za hibridno.
>
> Na primjer (pseudocode), koje ste definirali **10.4.5.6** kao vaše lokalne glavnog računala:
> 
> **Sljedeći scenarij funkcionira:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Sljedeći scenarij ne funkcionira:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Stvaranje veze za hibridno

Na portalu Azure pomoću web-aplikacije **ili** pomoću servisa BizTalk moguće je stvoriti hibridnog veze. 

**Da biste stvorili hibridnog veze pomoću web-aplikacije**, potražite u članku [Povezivanje Azure web-aplikacije programa resursa na lokalno](../app-service-web/web-sites-hybrid-connection-get-started.md). Upravitelj veze hibridnog (HCM) možete instalirati i s web-aplikaciju programa, što je željeni način. 

**Da biste stvorili veze hibridnog BizTalk Services**:

1. Prijavite se na [portal za Azure klasični](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. U lijevom navigacijskom oknu odaberite **BizTalk servise** , a zatim odaberite servis za BizTalk. 

    Ako nemate postojeći servis za BizTalk, možete ga [Stvori BizTalk servisa](biztalk-provision-services.md).
3. Odaberite karticu **Hibridnog veze** :  
![Kartica hibridnog veze][HybridConnectionTab]

4. Odaberite **Stvori hibridnog veze** ili odaberite gumb **DODAJ** na programsku traku. Unesite sljedeće:

    Svojstvo | Opis
--- | ---
Ime | Naziv veze hibridnog mora biti jedinstvena, a može biti isti naziv kao BizTalk servis. Možete unijeti bilo koje ime, ali moguće određenim s njezinu svrhu. Primjeri:<br/><br/>Lijepljenje*SQLServer*<br/>SupplyList*SharepointServer*<br/>Korisnici*OracleServer*
Naziv glavnog računala | Unesite potpuno kvalificirani glavno samo naziv glavnog računala ili IPv4 adresa lokalnog resursa. Primjeri:<br/><br/>mySQLServer<br/>*mySQLServer*. *Domena*. korporacija*yourCompany*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *yourCompany*.com<br/>10.100.10.10<br/><br/>Ako koristite IPv4 adresa, imajte na umu kod klijenta ili aplikacija može razriješiti IP adresa. U odjeljku važno Imajte na umu pri vrhu ove teme.
Priključak | Unesite broj priključka lokalnog resursa. Na primjer, ako koristite web-aplikacije, unesite priključak 80 ili priključak 443. Ako koristite SQL Server, unesite priključak 1433.

5. Odaberite potvrdite okvir da biste dovršili postavljanje. 

#### <a name="additional"></a>Dodatni

- Moguće je stvoriti više hibridnog veza. Potražite u članku na [BizTalk usluge: izdanja grafikona](biztalk-editions-feature-chart.md) za broj veza koje su dopuštene. 
- Svaku vezu s hibridnog stvara se pomoću par nizove veze: aplikacija tipki te SLANJE i lokalne prečaci koji se PRESLUŠALI. Svaki par ima primarne i sekundarne ključ. 


## <a name="LinkWebSite"></a>Povezivanje web-aplikacije aplikacije servisa za Azure ili mobilnoj aplikaciji

Da biste se povezali Web App ili mobilnu aplikaciju u aplikacije servisa za Azure postojeće hibridnog veze, odaberite **Koristi postojeću vezu hibridnog** u plohu hibridnog veze. U odjeljku [pristup lokalnog resursa pomoću hibridnog veze u aplikacije servisa za Azure](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Instalacija na Upravitelja veze hibridnog lokalni

Nakon stvaranja veze hibridnog instalirati upravitelja veze hibridnog na lokalnog resursa. Mogu se preuzeti iz aplikacije Azure web ili na servisu BizTalk. Koraci za BizTalk servisi: 

1. Prijavite se na [portal za Azure klasični](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. U lijevom navigacijskom oknu odaberite **BizTalk servise** , a zatim odaberite servis za BizTalk. 
3. Odaberite karticu **Hibridnog veze** :  
![Kartica hibridnog veze][HybridConnectionTab]
4. Na traci sa zadacima odaberite **Lokalne instalacije**:  
![Lokalne instalacije][HCOnPremSetup]
5. Odaberite **Instalacija i podešavanje** pokretanje ili preuzimanje hibridnog Upravitelja veze na lokalni sustav. 
6. Odaberite potvrdite okvir da biste pokrenuli instalaciju. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Dodatni
- Upravitelj veze hibridnog moguće je instalirati na operacijski sustavi u nastavku:

    - Windows Server 2008 R2 (.NET Framework 4,5 + i Windows Management Framework 4.0 + obavezno)
    - Windows Server 2012 (Windows Management Framework 4.0 + obavezno)
    - Windows Server 2012 R2


- Nakon što instalirate Upravitelja veze hibridnog, događa se sljedeće: 

    - Veza hibridnog hostirane na Azure automatski konfigurirana za korištenje primarni aplikacije niz za povezivanje. 
    - Resursa na lokalno automatski konfigurirana za korištenje primarni na lokalni niz za povezivanje.

- Upravitelj veze hibridnog morate koristiti niza za povezivanje valjani lokalni za autorizaciju. Azure web-aplikacije i mobilna aplikacija morate koristiti niza za povezivanje valjana aplikacija za autorizaciju.
- Ako instalirate neku drugu instancu u hibridnog Upravitelja veze na drugi poslužitelj mogu mijenjati veličinu hibridnog veze. Konfiguriranje korištenje iste adrese kao prvi lokalni ga slušatelj ga slušatelj lokalni. U tom slučaju promet je slučajno raspodijeljeno (kružnog) između aktivnog lokalnog slušače. 


## <a name="ManageHybridConnection"></a>Upravljanje vezama hibridnog
Da biste upravljali hibridnog veze, možete učiniti sljedeće:

- Pomoću portala za Azure i idite na servis za BizTalk. 
- Pomoću [REST API-ji](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopiranje/Obnovi hibridnog nizu za povezivanje

1. Prijavite se na [portal za Azure klasični](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. U lijevom navigacijskom oknu odaberite **BizTalk servise** , a zatim odaberite servis za BizTalk. 
3. Odaberite karticu **Hibridnog veze** :  
![Kartica hibridnog veze][HybridConnectionTab]
4. Odaberite vezu za hibridno. Na traci zadataka odaberite **Upravljanje veze**:  
![Upravljanje mogućnostima][HCManageConnection]

    **Upravljanje veze** popis aplikacija i lokalne nizove veze. Možete kopirati nizove veze ili Obnovi tipkovni prečac koji se koriste u nizu za povezivanje. 

    **Ako odaberete Obnovi**, tipkovni prečac zajednički koriste unutar niza za povezivanje se mijenja. Učinite sljedeće:
    - Azure klasični portalu odaberite **Sinkroniziraj tipke** u aplikaciji za Azure.
    - Ponovno pokrenite **Instalacijski program na lokalno**. Kada ponovno pokrenite instalacijski program na lokalno, lokalnog resursa automatski je konfiguriran za korištenje ažurirane primarni veza_niz.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Upotreba pravila grupe da biste odredili lokalnog resursa koji se koriste hibridnog veza

1. Preuzimanje [Upravitelja veze hibridnog Administrativni predlošci](http://www.microsoft.com/download/details.aspx?id=42963).
2. Izdvajanje datoteka.
3. Na računalu na kojem se mijenja pravilnika grupe, učinite sljedeće:  

    - Kopiraj u. ADMX datoteke u mapu *%WINROOT%\PolicyDefinitions* .
    - Kopiraj u. ADML datoteke u mapu *%WINROOT%\PolicyDefinitions\en-us* .

Kada se kopiraju, uređivaču pravila grupe možete koristiti da biste promijenili pravila.




## <a name="next"></a>Sljedeći

[Povezivanje Azure web-aplikacijama programa lokalnog resursa](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Povezivanje s lokalnog sustava SQL Server Azure web-aplikacije](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Pregled hibridnog veza](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Vidi također

[REST API-JA za upravljanje uslugama BizTalk na Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk servisa: Grafikon izdanja](biztalk-editions-feature-chart.md)  
[Stvaranje BizTalk servisa pomoću Azure klasični portala](biztalk-provision-services.md)  
[BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
