
<properties 
    pageTitle="Procjenu korištenja propusnosti mreže Azure RemoteApp | Microsoft Azure"
    description="Saznajte više o propusnosti mreže za Azure RemoteApp zbirke i aplikacije."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Procjenu korištenja propusnosti mreže Azure RemoteApp 

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp koristi Remote Desktop Protocol (RDP) za komunikaciju između aplikacija u oblak Azure i vaši korisnici. Ovaj članak sadrži nekoliko osnovnih smjernica možete koristiti za procjenu te korištenje mreže i potencijalno procjenu korištenja propusnosti mreže po korisniku Azure RemoteApp.

Procjena korištenja propusnosti po korisniku je vrlo složenim web-mjesta i zahtijeva više aplikacija istovremeno pokrenuto u višestrukim zadacima scenarijima gdje može utjecati aplikacije tuđe performanse koji se temelji na njihov zahtjev za propusnost mreže. Čak i vrstu klijenta udaljene radne površine (kao što su Mac klijent nasuprot HTML5 klijenta) može dovesti do različitih propusnosti rezultata. Da biste lakše rješavati te komplikacije, ne možemo ćete podijeliti scenariji za korištenje u nekoliko uobičajenih kategorija za replikaciju scenariji stvarnog života. (Pri čemu je scenarij stvarnog života se, Naravno, kombinacije kategorije i razlikuje se korisnik)

Prije nego što smo dodatno - otvorite mora se Imajte na umu da pretpostavimo RDP pruža pravi izvrstan okruženje za većinu scenariji za korištenje na mrežama Latencija ispod 120 ms i propusnosti veće od 5 MB prostora – to se temelji na mogućnost RDP-dinamički prilagodite pomoću na dostupna propusnost mreže i propusnosti Procijenjena aplikacije. U ovom se članku prelazi one "Većina njihove upotrebe" možete pogledati na rub, gdje scenariji početi vraćanja i korisnički doživljaj počne slabije.

Sada potražite u sljedećim člancima detalje, uključujući čimbenici koje treba uzeti u obzir, osnovne preporuke i što smo niste uključili u našem procjenjuje.

- [Kako učiniti propusnost mreže i kvalitetu iskustvo rada zajedno?](remoteapp-bandwidthexperience.md)
- [Upotreba propusnost mreže s nekim uobičajeni scenariji za testiranje](remoteapp-bandwidthtests.md)
- [Ako nemate vremena ili mogućnost da biste testirali brzi pogrešaka](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Što su smo ne uključujući?

Prilikom pregleda predloženih testira i preporuke naš cjelokupan (i admittedly generički), imajte na umu da je nekoliko čimbenika koji smo jeste li razmotriti. Na primjer, komplikacije zadovoljstva korisnika koje ste dobili od asimetričnim prirode prijenos nasuprot preuzmite propusnosti. Asimetričnim prirode Većina Wi-Fi mreža će uz to utjecati na performanse i dojam korisničkog sučelja. Interaktivni scenarijima za do promet možda prioritet manji od na upstream, koji mogu povećati broj okvira izgubiti zvučnog ili videozapisa i stoga utjecati na korisnike korisničkog sučelja strujanje. Možete pokrenuti vlastite eksperimenata da biste vidjeli što je dobro za određene koristi slučaj i mreže.

Iako ćemo razmotriti preusmjeravanje, ne možemo jeste li nisu uzeti u obzir utjecaj na propusnost za mrežni promet uzrokovanih priložene uređaja, kao što je prostor za pohranu, pisača, skenera, web-kamera i drugi USB uređaji. Efekt te uređaje obično privremeno spikes potrebama propusnosti i nestaje nakon dovršetka zadatka. No ako dosad često propusnosti zahtjev može biti vrlo uočljivijih.

Ne možemo ne raspravljati i kako jednom korisniku može utjecati na drugim korisnicima unutar iste mreže. Ako, na primjer, jedan korisnik troše videozapis 4K 100 MB/s mreže znatno može utjecati drugim korisnicima na toj istoj mreži pokušaja isti zadatak. Nažalost dohvaća progresivno teže da biste odredili utjecaj Istodobni korištenja da bi se dobilo zajedničkim ili sve encompassing preporuke o kako sustav izvodi pri zbrajanja. Sve možemo izgovorite jest da pozadinsku tehnologiju protokol bi najbolje korištenja propusnosti dostupnih mrežnih, ali se u njoj nalaze ograničenja.