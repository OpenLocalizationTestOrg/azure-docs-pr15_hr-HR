<properties 
    pageTitle="Najčešća pitanja vezana uz preusmjeravanje | Microsoft Azure"
    description="Odgovori na neka najčešća pitanja o preusmjeravanja Azure."
    services="service-bus"
    documentationCenter="na"
    authors="jtaubensee"
    manager=""
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub" />

# <a name="relay-faq"></a>Prijenos najčešća Pitanja

U ovom se članku navedeni odgovori na neka najčešća pitanja o programu Microsoft Azure preusmjeravanja. Možete posjetiti i [Azure podržava najčešća pitanja vezana uz](http://go.microsoft.com/fwlink/?LinkID=185083) općenite Azure informacije cijene i podrška. Uključene su u sljedećim temama:

- [Općenita pitanja o preusmjeravanja Azure](#general-questions)
- [Cijene](#pricing)
- [Kvota](#quotas)
- [Upravljanje pretplate i polja naziva](#subscription-and-namespace-management)
- [Otklanjanje poteškoća](#troubleshooting)

## <a name="general-questions"></a>Općenita pitanja

### <a name="what-is-azure-relay"></a>Što je preusmjeravanja Azure?

[Usluge preusmjeravanja](relay-what-is-it.md) pruža mogućnost proziran hostira i pristup servisima WCF s bilo kojeg mjesta. Drugim riječima, omogućuje hibridnog programa koji se pokreću u Azure podatkovnog centra i enterprise okruženju na lokaciji.

### <a name="what-is-a-relay-namespace"></a>Što je prostor naziva preusmjeravanja?

[Prostor naziva](Relay-create-namespace-portal.md) daje scoping spremnik adresiranja preusmjeravanja resursa u aplikaciji. Stvaranje jednog je potrebno da biste koristili preusmjeravanja web-mjesta i bit će prvog koraka u početak rada.

## <a name="pricing"></a>Cijene

U ovom se odjeljku navedeni odgovori na neka najčešća pitanja o preusmjeravanja cijene strukturu. Možete posjetiti i [Najčešća pitanja vezana uz Azure podršku](http://go.microsoft.com/fwlink/?LinkID=185083) opće informacije cijene Microsoft Azure. Potpune informacije o preusmjeravanja cijene potražite u članku [servis Bus cijene pojedinosti](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-relay"></a>Kako vam naplatiti za prijenos?

Potpune informacije o preusmjeravanja cijene, pročitajte članak [servisa Bus cijene pojedinosti][Pricing overview]. Osim cijene zabilježili, vam se naplatiti za prijenosi pridruženih podataka za izlazne izvan podatkovnog centra u kojima je dodijeljena aplikacije.

### <a name="what-usage-of-relay-is-subject-to-data-transfer"></a>Što Upotreba preusmjeravanja podložni prijenos podataka?

Prijenos obuhvaća 5 GB podataka ingress mjesečno, po pretplati. Postoji bez dodatnih Azure ingress/izlazne naknade za podatke koji se koriste preusmjeravanja.

Iznos podataka za prijenos za ingress pošiljatelja samo, kao preusmjeravanja slušače plaćati naknadu podataka. Na primjer, ako pošaljete 1 GB, vam samo naplaćivati za 1 GB čak i ako na ga slušatelj primili 1 GB i možda izvan podatkovnim centrima Azure korisnika.

### <a name="how-are-relay-hours-calculated"></a>Način izračuna preusmjeravanja sati?

Prijenos sati se naplatiti za kumulativnu količinu vremena tijekom kojeg svaki preusmjeravanja otvoren "" tijekom dano razdoblje naplate. Na preusmjeravanja je implicitno instancirati i otvoriti na zadani prostor za naziv kada ga slušatelj preusmjeravanja najprije poveže s te adrese. Prijenos je zatvoren samo kad je zadnji ga slušatelj prekida vezu sa njegovu adresu. Zbog toga u svrhu naplate u preusmjeravanja smatra "Otvori" od vremena prvi ga slušatelj preusmjeravanja povezuje, vrijeme zadnjeg preusmjeravanja ga slušatelj prekida vezu sa naziva. Drugim riječima, na preusmjeravanja smatra se otvori kad god je jedan ili više slušače preusmjeravanja povezani s njegove adrese Bus servisa.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-given-relay"></a>Što ako imam više ga slušatelj povezan s određenom preusmjeravanja?

U nekim slučajevima jedan prijenosnik može imati više povezanih slušače. Na preusmjeravanja smatra "otvoriti" kada je s njom povezano barem jedan ga slušatelj preusmjeravanja. Dodavanje dodatnih slušače Otvori preusmjeravanja rezultirat će dodatnih preusmjeravanja sati. Broj preusmjeravanja pošiljatelja (klijenti koji poziva ili slanje poruka preusmjeravanje) povezan s na prijenos i ne utječe na izračuna preusmjeravanja sati.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Kako se izračunava metar poruke za preusmjeravanje WCF?

**Ovo je dodijeljen preusmjeravanje WCF i nije trošak hibridnog veze**

Općenito govoreći, naplatu poruke izračunavaju se za preusmjeravanje na isti način opisan za brokered entiteti (redova, teme i pretplate). Međutim, postoji nekoliko najvažnije razlike:

Slanje poruke preusmjeravanja za Bus servis je tretira kao na "puno kroz" da biste poslali ga slušatelj preusmjeravanja koja prima poruke, umjesto slanja Bus preusmjeravanja servisa slijedi isporuke da biste ga slušatelj preusmjeravanja. Stoga na zahtjev za odgovor stil servisa poziva (od najviše 64 KB) na temelju ga slušatelj preusmjeravanja rezultirat će dvije poruke za naplatu: jednu poruku naplatu zahtjev i jednu naplatu poruku odgovora (odgovor uz pretpostavku da je \<= 64 KB). Razlikuje se od pomoću reda mediate između klijenta i usluge. U potonjem slučaju isti uzorak zahtjev za odgovor je potrebna zahtjev za slanje u redu čekanja, nakon čega slijedi dequeue/isporuci iz reda čekanja servis, nakon čega slijedi odgovor Pošalji u drugom redu, a dequeue/isporuci iz tog reda čekanja klijentu. Koriste isti (\<= 64 KB) veličina pretpostavke cijeloj, uzorak mediated red stoga posljedicu četiri naplatu poruke, dvaput broj naplatiti za implementaciju isti uzorak pomoću preusmjeravanja. Naravno, postoje prednosti korištenja reda čekanja da biste postigli ovaj uzorak, primjerice rok trajanja i učitavanje ujednačavanje. Sljedeće pogodnosti možda poravnanje dodatnih troškova.

Preusmjeravanje koje se otvaraju pomoću WCF povezivanja netTCPRelay tretirati poruke ne kao pojedinačne poruke, ali kao strujanja podataka slijedi kroz sustav. Drugim riječima, samo pošiljatelj i ga slušatelj imaju uvid u kadar pojedinačne poruke šalju i primaju putem ovog povezivanja. Dakle, za preusmjeravanje pomoću netTCPRelay uvez, svi podaci se smatra strujanje radi izračuna naplatu poruke. U ovom slučaju Bus servisa će izračunati ukupnu količinu podataka šalje ili primljeni putem svake pojedine preusmjeravanja na temelju 5 minuta i podijelite taj zbroj 64 KB da bi se odredili broj naplatu poruka za prijenos u pitanju tijekom tog razdoblja.

## <a name="quotas"></a>Kvota

|Naziv kvote|Opseg|Vrsta|Ponašanje prilikom skokova|Vrijednost|
|---|---|---|---|---|
|Istodobni slušače na na prijenos|Entitet|Statički|Daljnji zahtjevi za dodatne veze će biti odbijene i iznimku će primio pozivanja kod.|25|
|Istodobni prijenos slušače|Sistemskih|Statički|Daljnji zahtjevi za dodatne veze će biti odbijene i iznimku će primio pozivanja kod.|2000|
|Istodobni prijenos veze po sve preusmjeravanja krajnje točke u polje naziva servisa|Sistemskih|Statički|-|5000|
|Prijenos krajnje točke po polje naziva servisa|Sistemskih|Statički|-|10 000|
|Dopuštene veličine poruka za preusmjeravanje [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) i [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx)|Sistemskih|Statički|Dolazne poruke koje premašuju ove kvote će biti odbijene i iznimku će primio pozivanja kod.|64KB
|Dopuštene veličine poruka za preusmjeravanje [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) i [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx)|Sistemskih|Statički|-|Neograničeno|

### <a name="does-relay-have-any-usage-quotas"></a>Mora li preusmjeravanja sve kvota za korištenje?

Prema zadanim postavkama za sve spremanja servisa Microsoft postavlja se zbrajanja mjesečni kvota korištenja koji se izračunava preko svih pretplata klijenta. Jer smo shvatili da možda trebate više ta ograničenja, obratite se službi u bilo kojem trenutku tako da bismo shvatili vašim potrebama i odgovarajuće prilagodite ta ograničenja. Bus servisa kvota zbrajanja korištenje su na sljedeći način:

- 5 milijarde poruke
- 2 milijuna preusmjeravanja sata

Dok ne možemo rezervirati desno da biste onemogućili klijentova računa koji ima prekoračiti njegova kvota za korištenje u određenom mjesecu, ne možemo će navedite obavijesti e-pošte i provjerite više pokušava obratiti klijentu prije poduzimanja akcija. Klijenti premašuju ove kvote i dalje biti odgovoran za troškove koji prelaze na kvote.

#### <a name="naming-restrictions"></a>Imenovanje ograničenja

Prijenos polja naziva može biti samo između 6 50 znakova.

## <a name="subscription-and-namespace-management"></a>Upravljanje pretplate i polja naziva

### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Kako migrirati prostor naziva na drugu pretplatu na Azure?

Možete koristiti naredbe ljuske PowerShell (pronaći u članku [ovdje](../service-bus-messaging/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription)) da biste premjestili prostor naziva s jednog Azure pretplate na drugi. Da bi se izvršavanje operacija naziva mora već biti aktivna. Korisnik izvršavanja naredbe i mora biti administrator na izvorišno i odredišno pretplate.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-their-suggested-actions"></a>Što su iznimke generira API-ji za prijenos Azure i njihovih predložene korake?

[Prijenos iznimke] [ Relay exceptions] se članku opisuju neke iznimke s predložene korake.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Pristup potpis zajednički koristiti i jezike koji podržavaju generiranje potpis?

Zajednički pristup potpisi su mehanizam provjere autentičnosti na temelju SHA – 256 sigurne raspršivanja ili ji. Upute za stvaranje vlastitog potpisa u čvor, PHP, Java i C\#, potražite u članku [Zajedničko korištenje programa Access potpisa][] .

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Zajednički pristup potpisa]: service-bus-sas-overview.md

## <a name="next-steps"></a>Sljedeće korake:

- [Stvaranje prostor naziva](relay-create-namespace-portal.md)
- [Početak rada s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Početak rada s čvor](relay-hybrid-connections-node-get-started.md)