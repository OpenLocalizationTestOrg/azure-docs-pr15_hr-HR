<properties
   pageTitle="Otkrivanje mogućnosti u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pomaže vam da biste razumjeli kako funkcioniraju mogućnosti otkrivanja Azure centar za sigurnost."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Centar za sigurnost Azure otkrivanje mogućnosti
Ovaj dokument govori o mogućnostima napredne otkrivanje Azure Sigurnosni centar, koji olakšava prepoznavanje aktivni prijetnji ciljanja resursi za Microsoft Azure i daje uvid potrebno odgovoriti brzo.

> [AZURE.NOTE] Napredni dlp-a dostupne su u u standardnom sloju od Azure centar za sigurnost. Dostupna je besplatnu probnu verziju 90 dana. Možete nadograditi iz odabira cijene sloju [Sigurnosna pravila](security-center-policies.md). Posjetite [Centar za sigurnost stranice](https://azure.microsoft.com/pricing/details/security-center/) da biste saznali više o cijene. 


## <a name="responding-to-todays-threats"></a>Odgovaranje na današnji prijetnji
Postoji značajne promjene u vodoravnom prijetnju tijekom posljednjeg 20 godina. U prošlosti, tvrtke obično samo morali razmišljati o web-mjestu defacement pojedinačne Napadači koji su uglavnom zanimati prikazuju "što nije služe". Današnji attackers su znatno jednostavnije i organizirati. Često imaju određene ciljeve financije i strateški. Također imaju dodatne resurse koji su dostupni na njih, kao što je možda biti funded značaja država ili organized crime.

Taj se način je koji vode neviđen stupanj professionalism u sortira napadaču. Više su oni zainteresirani web defacement. Sada su zainteresirani onemogućuje krađu informacije, financijskim računima i privatni podaci – koje mogu koristiti za generiranje novca na otvorenom tržištu ili odražava određenog poslovnog, političkih ili vojnu položaj. Još više vezane uz nego što te attackers s financijske cilj attackers koji krši mrežama ugroziti za infrastrukturu i osobe.

Odgovor, tvrtkama ili ustanovama često implementirati različite točke rješenja koja usredotočite se na defending opseg enterprise ili krajnje točke traženjem poznati napada potpisa. Da biste generirali veliku količinu Niska kvaliteta upozorenja koje zahtijevaju sigurnost analitičar razvrstavati i istražiti obično se ova rješenja. Većina tvrtki ili ustanova nemaju vrijeme i stručna znanja potrebno odgovoriti te upozorenja – unaddressed toliko Idi.  U međuvremenu, attackers su razvile njihove metode subvert mnoge utemeljen na potpis obranu i [prilagoditi oblaka okruženja](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Novi pristupa su potrebne za brzo prepoznavanje novih prijetnji i ubrzali otkrivanje i odgovor. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Kako centar za sigurnost Azure otkriva i odgovori prijetnji

Istraživači Microsoft sigurnost su stalno su obratite pozornost na prijetnji. Imaju pristup rastuća skup telemetrijskih koje ste dobili iz tvrtke Microsoft globalni prisutnosti u oblak i lokalnog. Zbirku wide Dostizanje i raznih skupova podataka Microsoftu Otkrijte nove napada obrazaca i trendova preko njegov lokalnim potrošača i enterprise proizvodima, kao i njegov internetske servise. Kao rezultat centar za sigurnost hitro možete ažurirati njegov algoritama otkrivanje kao attackers pustite novo, a sve sofisticirane opasnosti. Taj se način pomaže vam ih uskladili brzo premještanje prijetnju okruženje. 

Centar za sigurnost prijetnju otkrivanje radi tako da automatski prikupljanje sigurnosnih informacija iz Azure resursa, mreže i povezani partnerskih rješenja. Ga analizira te informacije, često correlating podatke iz više izvora, da biste odredili prijetnji. Sigurnosna upozorenja prioritet se u Centar za sigurnost te preporuke za remediate prijetnju.

![Centar za sigurnost prikupljanje podataka i prezentaciju](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Centar za sigurnost uključuje dodatnom sigurnošću analize koja je daleko okružje utemeljen na potpis pristupa. Otkrića u velikih skupova podataka i [strojnog učenja](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tehnologije leveraged se treba vrednovati događaje preko cijele oblaka tkanina – otkrivanje prijetnji koje bi nemoguće prepoznavanje pomoću ručno pristupa i procjenjivanje proizvod napada. Ove sigurnost analytics uključuje: 

- **Integrirano prijetnju obavještavanje**: Traži poznati bad Glumci po korištenje globalni prijetnju obavještavanje iz Microsoftove proizvode i usluge, Microsoft jedinica digitalni Crimes (DCU), Microsoft centar za sigurnost odgovor (MSRC) i vanjske sažeci sadržaja.
- **Behavioral analize**: primjenjuje poznati uzoraka da biste otkrili zlonamjerni ponašanje. 
- **Otkrivanje značajkom**: pomoću statističke Profiliranje sastaviti povijesne referentne vrijednosti. Upozorava na devijacija od utvrđene referente vrijednosti koje odgovaraju potencijalne vektor napada.


### <a name="threat-intelligence"></a>Obavještavanje prijetnju
Microsoft je immense količinu globalni prijetnju obavještavanje. Telemetriju u teče iz više izvora, kao što su Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft digitalni Crimes jedinica (DCU) i Microsoft centar za sigurnost odgovor (MSRC). Istraživači su primati i prijetnju obavještavanje informacije koje se zajednički koristi glavne oblaka davateljima usluga i pretplaćuje se na prijetnju obavještavanje RSS sažetke sadržaja s trećim stranama. Centar za sigurnost Azure pomoću ove informacije upozorenje da biste prijetnji s poznatim bad Glumci. Primjeri obuhvaćaju sljedeće:

- **Izlazni komunikacije zlonamjerni IP adresu**: odlazni promet poznati botnet ili darknet vjerojatno znači da vaše resursa ugrožena, a zatim je napadaču je Pokušaj izvršavanja naredbe na sustav ili exfiltrate podatke. Centar za sigurnost Azure uspoređuje mrežnog prometa s bazom podataka tvrtke Microsoft globalni prijetnju i upozorava vas ako otkrije komunikacije zlonamjerni IP adresu.

## <a name="behavioral-analytics"></a>Behavioral analytics

Tehnika analizira i uspoređuje podatke u skup poznati uzoraka je behavioral analize. Međutim, te uzoraka nisu jednostavni potpisi. Su određene do složenih strojnog učenja algoritmima pomoću kojih se primjenjuju na pretraživanje velikog skupova podataka. Oni također ovise o kroz oprezni analizu zlonamjerni ponašanja expert analitičar analitičar podataka. Centar za sigurnost Azure možete koristiti behavioral analytics za prepoznavanje ugrožena resursi koji se temelji na analizu zapisnika virtualnog računala, virtualne mreže uređaj zapisnika, tkanina zapisnike, rušenje ispisi i drugih izvora. 

Uz to, postoji korelacije s drugim signale da biste provjerili dokaz Primamo kampanje za podršku. U ovom korelacije olakšava prepoznavanje događaje koje su u skladu s utvrđene pokazatelja narušena pouzdanost. Primjeri obuhvaćaju sljedeće:

- **Suspicious obradi izvođenja**: Attackers uključivanja nekoliko postupaka za izvršavanje zlonamjernog softvera bez otkrivanja. Na primjer, napadaču može dati zlonamjernog softvera istog naziva kao valjane sistemske datoteke, ali potvrdite te datoteke na drugo mjesto, upotrijebite naziv koji je vrlo je sličan benign datoteke ili maski od datotečnog nastavka true. Centar za sigurnost modelima procesa ponašanja i monitora obrada izvršavanja da biste otkrili outliers kao što su.  
- **Skrivene od zlonamjernog softvera i prikaze djece pokušava**: sofisticirane zlonamjerni softver je moći evade proizvodi tradicionalni antimalware tako da nikada ne pisanje na disk ili šifriranje komponente softver pohranjene na disku.  Međutim, takve zlonamjernog softvera moguće je otkriti analizom memorije zlonamjernog softvera morate napuste kašnjenja u memoriji funkcioniranje. Kada se ruši se softver, pad snima dio memorije u trenutku u pad sustava.  Analizom memorije u na pad centar za sigurnost Azure može prepoznati tehnike koji se koriste za izrabljuje slabe točke u softver, pristup povjerljivim podacima i tajno zadržava s prijavom u compromised računala bez koje utječu na performanse računala.
- **Lateral premještanja i interna reconnaissance**: održati u compromised mreža i pronađite/harvest koristan podataka, attackers često pokušaj premještanja lateralno compromised računalu drugima unutar iste mreže. Centar za sigurnost nadzire postupak i prijava aktivnosti da biste otkrili pokušaja da biste proširili do napadač foothold unutar mreže, primjerice udaljene Naredba izvršavanja mreže probing i numeriranje računa.
- **Zlonamjerni skripte komponente PowerShell**: PowerShell koristi se Napadači izvršiti kodu na ciljnom virtualnim strojevima za različite potrebe. Centar za sigurnost provjerava ima li aktivnost ljuske PowerShell za dokaz sumnjivoj aktivnosti. 
- **Izlazni napada**: Attackers često ciljani oblaka resursi s ciljem korištenja tih resursa postavljanja dodatne napada. Ugrožena virtualnih računala, na primjer, možda će se pokretanje napada brute prisilno protiv drugih virtualnim strojevima, slanje neželjene e-pošte ili skeniranje otvorene priključke i na drugim uređajima na Internetu. Primjenom strojnog učenja mrežni promet centar za sigurnost može prepoznati kada izlaznog mrežnu komunikaciju biti dulji od pravila. Slučaju neželjene e-pošte, centar za sigurnost i korelaciji promet neobično e-pošte s obavještavanje iz sustava Office 365 da biste utvrdili je li e-pošte vjerojatno nefarious ili rezultat s kampanjom valjane e-pošte.  

### <a name="anomaly-detection"></a>Otkrivanje značajkom

Centar za sigurnost Azure koristi i značajkom prepoznavanja za označavanje prijetnji. Za razliku behavioral analize (a to ovisi o poznatim uzoraka izvedene iz velikih skupova podataka), značajkom prepoznavanja je više "prilagođenih" i usredotočuje se na referente vrijednosti koje su specifične za vaše implementacije. Strojnog učenja primjenjuje da biste odredili normalni aktivnosti za vaše implementacije, a zatim se generiraju pravila za definiranje uvjeta ekstremne koja bi mogla predstavljati sigurnosni događaj. Evo jednog primjera:

- **Dolazni RDP/SSH brute prisilno napada**: vaš implementacijama možda zauzet virtualnim strojevima s mnogo prijave svaki dan i drugim virtualnim strojevima koji imaju vrlo mali ili bilo koju prijave. Centar za sigurnost Azure možete odrediti osnovne aktivnosti prijave za ove virtualnim strojevima i koristiti strojnog učenja da biste odredili što je izvan normalni prijava aktivnosti. Ako se broj prijave ili doba dana prijave ili mjesta iz kojih su zatražili prijave ili druge značajke vezane uz prijava znatno razlikuje od osnovne crte, zatim upozorenja može biti generira. Ponovno strojnog učenja određuje što je važan.

## <a name="continuous-threat-intelligence-monitoring"></a>Neprekinuti prijetnju obavještavanje nadzora

Centar za sigurnost Azure pristajete sigurnost Istraživanje i podataka znanstvenog ekipa koje se neprestano praćenje promjena u vodoravnom prijetnju. To obuhvaća sljedeće inicijative:

- **Nadzor obavještavanje prijetnju**: prijetnju obavještavanje obuhvaća mehanizme, pokazatelja, posljedice i s akcijama savjete o postojeći ili novih prijetnji. Ti podaci se zajednički koristi u zajednici za sigurnost i Microsoft neprestano nadzire sažetke sadržaja obavještavanje prijetnju internim i vanjskim izvorima.
- **Zajedničko korištenje signal**: uvida s timovima sigurnost preko općenite portfelja tvrtke Microsoft cloud lokalnog servisa, poslužiteljima i klijenta krajnjoj točki uređaji i zajednički koristiti i analizirati. 
- **Microsoft sigurnost specijalistima**: tijeku radnje sa timovima preko Microsoft koje funkcioniraju u specijalizirano sigurnost polja, kao što su forensics i web-otkrivanje napada.
- **Otkrivanje usklađivanje**: algoritmi će se izvoditi na temelju skupa podataka stvarnih klijenata i sigurnost istraživači su rad s klijentima da biste provjerili rezultate. TRUE i false positives koriste se za preciznije određivanje algoritama strojnog učenja.

Ove kombinirane naporima culminate u novi i poboljšani dlp-a, koji je koje su prednosti trenutačno – nema akcije koje možete poduzeti.

## <a name="see-also"></a>Vidi također
U ovom dokumentu naučili za otkrivanje centar za sigurnost Azure mogućnosti funkcioniranje. Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Centar za sigurnost Azure planiranja i vodič](security-center-planning-and-operations-guide.md)
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md)
- [Sigurnosna upozorenja po vrsti u centru za sigurnost Azure](security-center-alerts-type.md)
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa.
- [Sigurnost Azure blog](http://blogs.msdn.com/b/azuresecurity/) – pronaći bloga o Azure sigurnost i usklađenost.
