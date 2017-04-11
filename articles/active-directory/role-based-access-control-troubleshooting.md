<properties
    pageTitle="Na temelju uloga otklanjanje poteškoća s pristupom kontrola | Microsoft Azure"
    description="Pronađite pomoć vezanu uz problemi i pitanja o resursima za kontrolu pristupa temelji uloge."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Otklanjanje kontrola pristupa na temelju uloga

## <a name="introduction"></a>Uvod

[Na temelju uloga kontrola pristupa](role-based-access-control-configure.md) je Napredna značajka koja omogućuje delegiranje preciznije pristupa resursima servisa Azure. To znači da smatrate sigurni dodjelu određene osobe pravo na korištenje točno koje trebaju i više. Međutim, ponekad može biti složene modela resursa za Azure resurse i može biti teško razumjeti točno što dopuštate dozvole.

Ovaj dokument poslat će vam znali što mogu očekivati kada koristite neki od uloga na portalu za Azure. Ove tri uloge pokriva sve vrste resursa:

- Vlasnik  
- Suradnik  
- Čitač  

Vlasnici i suradnici imaju puni pristup sučelje za upravljanje, ali suradnik ne može dati pristup drugim korisnicima ili grupama. Što se malo zanimljive s ulogom čitač tako da je gdje ćete provedu neko vrijeme. Potražite u [članku početak rada na temelju uloga kontrola pristupa](role-based-access-control-configure.md) detalje o tome kako dopustiti pristup.

## <a name="app-service-workloads"></a>Radnih opterećenja servisa aplikacija

### <a name="write-access-capabilities"></a>Mogućnosti pristupa za pisanje

Ako dopustite samo za čitanje pristupa jednog web-aplikacijama, neke značajke onemogućuju, koje možete očekivati. Sljedeće značajke upravljanja potreban **Pisanje** access web App (Suradnik ili vlasnik), a neće biti dostupan u bilo kojem scenariju samo za čitanje.

- Naredbe (npr. start tabulatora, itd.)
- Promjena postavki poput Općenito konfiguracije, postavke mjerila, sigurnosne postavke i postavke nadzora
- Pristup vjerodajnice za objavljivanje i druge tajne kao što su postavki aplikacije i nizove veze
- Strujanje zapisnika
- Konfiguriranje dijagnostičkog zapisnika
- Konzola (naredbeni redak)
- Aktivne i nedavne implementacije (za lokalni brojka neprekinuti implementacija)
- Procijenjena provedu
- Testovi za web
- Virtualne mreže (vidljivo samo za čitanje ako virtualne mreže prethodno konfigurirana tako da korisnici s pristupom za pisanje).

Ako ne možete pristupiti bilo koju od tih pločica, morat ćete zatražite od administratora za pristup suradničkom web-aplikaciji.

### <a name="dealing-with-related-resources"></a>Postupanje s Povezani resursi

Web-aplikacije su komplicirano tako da je prisutnost nekoliko različitih resursa koji interplay. Evo grupu uobičajeni resursa s nekoliko web-mjesta:

![Grupa resursa za web app](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Kao rezultat, ako nekom dodijelite pristup samo web-aplikaciju, većina funkcija na plohu web-mjesta na portalu za Azure će biti onemogućena.

Te stavke potreban **Pisanje** pristup **aplikacije servisa za plan** koji odgovara vašem web-mjestu:  

- Prikaz web-aplikaciji je cijene sloju (besplatno ili standardna)  
- Promjena veličine konfiguracije (broj instanci, veličina virtualnog računala, a zatim automatsko skaliranje postavke)  
- Kvota (prostora za pohranu, propusnost, CPU-a)  

Te stavke zahtijevaju **Pisanje** pristup cijelu **grupu resursa** koja sadrži web-mjesta:  

- SSL certifikata i povezivanja (to je zato što SSL certifikata mogu zajednički koristiti web-mjesta u istoj grupi resursa i mjesto na zemlj.)  
- Upozorenja pravila  
- Automatsko skaliranje postavke  
- Komponente uvida aplikacija  
- Testovi za web  

## <a name="virtual-machine-workloads"></a>Radnih opterećenja virtualnog računala

Slično kao što su s web-aplikacijama neke značajke na plohu virtualnog računala potreban pristup za pisanje virtualnog računala ili drugih resursa u grupu resursa.

Virtualnim strojevima vezani uz domene imena, virtualne mreže, račune za pohranu i upozorenja pravila.

Te stavke zahtijevaju **Pisanje** pristup **virtualnog računala**:

- Krajnje točke  
- IP adrese  
- Diskova  
- Proširenja  

Ove potreban **Pisanje** access i **virtualnog računala**i **grupa resursa** (uz naziv domene) koja se:  

- Postavljanje dostupnosti  
- Učitavanje Balansirani skup  
- Upozorenja pravila  

Ako ne možete pristupiti bilo koju od tih pločica, morat ćete zatražite od administratora za pristup suradničke grupe resursa.

## <a name="see-more"></a>Potražite dodatne informacije
- [Kontrola pristupa temelji uloga](role-based-access-control-configure.md): početak rada s RBAC na portalu za Azure.
- [Ugrađeni uloge](role-based-access-built-in-roles.md): dohvaćanje detalja o uloge od kojih svaka standard u RBAC.
- [Prilagođeno uloga Azure RBAC](role-based-access-control-custom-roles.md): Saznajte kako stvoriti prilagođene uloge da odgovara vašim potrebama programa access.
- [Izvješće o povijesti promjena za stvaranje programa access](role-based-access-control-access-change-history-report.md): vođenja evidencije promjena dodjele uloga u RBAC.
