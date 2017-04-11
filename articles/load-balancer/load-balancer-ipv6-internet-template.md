<properties
    pageTitle="Uvođenje na mjesto na Internetu uravnoteženja rješenja s IPv6 pomoću predloška | Microsoft Azure"
    description="Kako implementirati podrška za IPv6 za Azure raspoređivača opterećenja i uravnoteženja VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure opterećenja, dvostruki snop, javnu ip, izvorni ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Uvođenje na mjesto na Internetu raspoređivača opterećenja rješenja s IPv6 pomoću predloška

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure EŽA](./load-balancer-ipv6-internet-cli.md)
- [Predložak](./load-balancer-ipv6-internet-template.md)

Azure opterećenja je opterećenja za sloj-4 (TCP, UDP). Raspoređivača opterećenja nudi visoke dostupnosti raspodijeliti dolazne promet među instanci dobar servisa u oblaku servise ili virtualnim strojevima u skupu raspoređivača opterećenja. Azure opterećenja također možete prikazati tih servisa na više priključaka, više IP adresa ili i jedno i drugo.

## <a name="example-deployment-scenario"></a>Primjer implementacije scenarija

Na sljedećem su dijagramu ilustrira rješenja za ujednačavanje opterećenja uvodi pomoću predloška za primjer opisane u ovom članku.

![Scenarij raspoređivača učitavanja](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

U ovom scenariju ćete stvoriti u sljedećim resursima Azure:

- virtualne mreže sučelje za svaki VM IPv4 i IPv6 adrese dodijeljeno
- mjesto na Internetu opterećenja na IPv4 i IPv6 javnu IP adresa
- dva učitavanje ujednačavanje pravila za mapiranje javno VIPs privatne krajnje točke
- Dostupnost postavljanje koji sadrži dvije VMs
- dva virtualnim strojevima (VMs)

## <a name="deploying-the-template-using-the-azure-portal"></a>Uvođenje predloška pomoću portala za Azure

U ovom se članku upućuje predložak koji se objavljuje u galeriji [Predložaka za brzi početak rada Azure](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) . Možete preuzeti predloška iz galerije ili pokretanje implementacije u Azure izravno iz galerije. U ovom se članku pretpostavlja da ste preuzeli predložak s vašim lokalnim računalom.

1. Otvorite portal za Azure i prijavite se pomoću računa koji ima dozvole za stvaranje VMs i mrežni resursi Azure pretplate. Osim toga, osim ako ne koristite postojećih resursa, račun mora dozvolu za stvaranje grupa resursa i račun za pohranu.

2. Kliknite "+ Novo" iz izbornika zatim upišite "predložak" u okvir za pretraživanje. Odaberite "Predložak implementacije" u rezultatima pretraživanja.

    ![LB-ipv6-portal-step2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. U na sve plohu, kliknite "Implementacija predložaka".

    ![LB-ipv6-portal-3.](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Kliknite "Stvaranje".

    ![LB-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Kliknite "Uređivanje predloška". Izbrišite postojeći sadržaj i kopirajte i zalijepite u cijeli sadržaj datoteku predloška (da biste je uključiti početka i završetka {}), a zatim kliknite "Spremanja."

    > [AZURE.NOTE] Ako koristite Microsoft Internet Explorer kada lijepite ste primanje dijaloški okvir s upitom želite li dopustiti pristup u međuspremnik sustava Windows. Kliknite "Dopustiti pristup".

    ![LB-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Kliknite "Uređivanje parametara". U plohu parametara navesti vrijednosti po navedene upute u odjeljku parametara predložak, a zatim kliknite "Spremi" da biste zatvorili plohu parametara. U plohu implementacije Prilagođeno odaberite pretplatu na postojeću grupu resursa ili stvorili. Ako stvarate grupu resursa, odaberite mjesto za grupu resursa. Nakon toga kliknite **pravne uvjete**, a zatim kliknite **kupite** za pravne uvjete. Azure započinje implementacija resurse. U svega nekoliko minuta za implementaciju svih resursa.

    ![LB-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Dodatne informacije o ovim parametrima potražite u odjeljku [Parametri predložaka i varijabli](#template-parameters-and-variables) u nastavku ovog članka.

7. Da biste vidjeli resurse stvorili predložak, kliknite Pregledaj, pomaknite se dolje na popisu dok ste potražite u članku "Grupa resursa", a zatim ga.

    ![LB-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Na grupe plohu resursa kliknite naziv grupe resursa koje ste naveli u koraku 6. Pogledajte popis svih resursa koja je postavila. Ako se svi nije dobro, "je uspjelo" trebalo bi pisati u odjeljku "Posljednje implementacija." Ako ne koristite račun provjerite ima li dozvole za stvaranje potrebne resurse.

    ![LB-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Ako pregledavate grupe resursa odmah nakon dovršetka koraku 6, "Posljednje implementacije" prikazat će status "Deploying" dok uvode resurse.

9. Kliknite "myIPv6PublicIP" na popisu resursa. Vidjet ima IPv6 adrese IP adresa i ima li naziv DNS vrijednost navedena za parametar dnsNameforIPv6LbIP u koraku 6. Ovaj resurs je javno IPv6 adrese i glavno računalo naziv koji je dostupan klijenti za Internet.

    ![LB-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Provjerite valjanost povezivanje

Kada se predložak uspješno je postavila, Provjeri valjanost povezivanje s povećanim sljedeće zadatke:

1. Prijavite se na portal za Azure i povezati se svaki VMs stvorio uvođenje predloška. Ako implementiran VM Windows Server, pokrenite ipconfig/sve iz naredbenog retka. Vidjet ćete da se VMs imaju IPv4 i IPv6 adrese. Ako implementiran Linux VMs, morate konfigurirati OS Linux prima dinamički IPv6 adrese pomoću upute za Linux-distribuciju.
2. Putem klijentskog programa IPv6 internetskom vezom započeti vezu na javnu IPv6 adresu raspoređivača opterećenja. Da biste potvrdili da su raspoređivača opterećenja opterećenja između dva VMs, nije moguće instalirati web-poslužitelj kao što je Microsoft Internet Information Services (IIS) na svakom na VMs. Zadano web-stranice na svakom poslužitelju može sadržavati tekst "Server0" ili "Poslužitelj1" identificirati samo ga. Zatim otvorite web-pregledniku na klijentu za IPv6 internetskom vezom i Pregledaj da biste na naziv glavnog računala naveden za parametar dnsNameforIPv6LbIP opterećenja da biste potvrdili povezivanje putem protokola IPv6 u kraj do kraja, za svaku VM. Ako vidite samo web-stranice iz samo jednog poslužitelja, možda ćete morati očistili predmemoriju preglednika. Otvorite više privatne sesije. Trebali biste vidjeti odgovor svaki poslužitelj.
3. Putem klijentskog programa IPv4 internetskom vezom započeti vezu na javnu IPv4 adresu raspoređivača opterećenja. Da biste potvrdili da je raspoređivača opterećenja dva VMs za ujednačavanje opterećenja, nije moguće testirati pomoću IIS, kao što je detaljne u koraku 2.
4. Iz svake VM pokretanje izlazne veze na uređaju sa sustavom IPv6 ili Internet IPv4 povezani. U oba slučaja izvorni IP vidjeti odredišni uređaj je s javnim IPv4 ili IPv6 adrese raspoređivača opterećenja.

>[AZURE.NOTE]
ICMP za IPv4 i IPv6 blokirana je u Azure mreže. Zbog toga ICMP alate kao što su uvijek pomoću naredbe ping neće uspjeti. Da biste testirali povezivanje, koristite zamjenski TCP kao što su TCPing ili cmdlet PowerShell Test NetConnection. Imajte na umu da IP adresa u dijagramu prikazani Primjeri vrijednosti koje se možda će se prikazati. Budući da IPv6 adrese dodjeljuju dinamički, adrese primite razlikovat će se i može se razlikovati po regijama. Osim toga, uobičajeno je javno IPv6 address na opterećenja da biste započeli s različitim prefiks od privatne IPv6 adrese u pozadinskoj.

## <a name="template-parameters-and-variables"></a>Parametri predložaka i varijabli

Predložak programa Azure Voditelj resursa sadrži više varijabli i parametre koje možete prilagoditi svojim potrebama. Varijable se koriste za fiksnih vrijednosti koje ne želite da korisnik može promijeniti. Parametri se koriste za vrijednosti koje želite da korisnik prilikom uvođenje predloška. Primjer predloška je konfiguriran za scenarij opisane u ovom članku. Ne možete prilagoditi potrebama vaše okruženje.

Primjer predložak koji se koristi u ovom članku obuhvaća sljedeće varijable i parametara:

| Parametar / varijable | Bilješke |
|-----------|-------|
| adminUsername | Navedite naziv administrator račun koji se koristi za prijavu na virtualnim strojevima sa. |
| adminPassword | Unesite lozinku za administratorski račun koji se koristi za prijavu na virtualnim strojevima sa. |
| dnsNameforIPv4LbIP | Navedite naziv glavnog računala DNS-a želite joj dodijeliti kao javno ime raspoređivača opterećenja. Ovaj naziv razrješava raspoređivača opterećenja javno IPv4 adresa. Naziv mora biti mala slova i odgovarati na regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Navedite naziv glavnog računala DNS-a želite joj dodijeliti kao javno ime raspoređivača opterećenja. Ovaj naziv razrješava raspoređivača opterećenja javno IPv6 adrese. Naziv mora biti mala slova i odgovarati na regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. To može biti isti naziv kao i IPv4 adresa. Kada klijent pošalje upit DNS-a za ovaj naziv vratit će Azure A i AAAA zapisi kada se zajednički koristi naziv. |
| vmNamePrefix | Navedite naziv prefiks VM. Predložak dodaje broj (0, 1, itd.) na naziv stvaranja na VMs. |
| nicNamePrefix | Navedite naziv prefiks sučelja mreže. Predložak dodaje broj (0, 1, itd.) na naziv stvaranja sučelje mreže. |
| storageAccountName | Unesite naziv postojećeg računa za pohranu ili navedite naziv novog će biti stvoren u predlošku. |
| availabilitySetName | Zatim unesite naziv dostupnost postavljena za uporabu s na VMs |
| addressPrefix | Prefiks adresa koji se koristi da biste definirali raspon adresa virtualne mreže |
| subnetName | Naziv podmreže u stvorene za na VNet |
| subnetPrefix | Prefiks adresa koji se koristi da biste definirali raspon adresa podmreži |
| vnetName | Navedite naziv VNet koji se koriste u VMs. |
| ipv4PrivateIPAddressType | Dodjela načina koji se koristi za privatne IP adrese (statički ili dinamički) |
| ipv6PrivateIPAddressType | Dodjela načina koji se koristi za privatne IP adrese (dinamički). IPv6 podržava samo dinamički dodijeljeni. |
| numberOfInstances | Koliko je instanci rasporediti opterećenje implementiran u predlošku |
| ipv4PublicIPAddressName | Navedite naziv DNS-a koji želite koristiti za komunikaciju s javnim IPv4 adresa raspoređivača opterećenja. |
| ipv4PublicIPAddressType | Dodjela načina koji se koristi za javne IP adrese (statički ili dinamički) |
| Ipv6PublicIPAddressName | Navedite naziv DNS-a koji želite koristiti za komunikaciju s javnim IPv6 address raspoređivača opterećenja. |
| ipv6PublicIPAddressType | Dodjela načina koji se koristi za javne IP adrese (dinamički). IPv6 podržava samo dinamički dodijeljeni. |
| lbName | Navedite naziv raspoređivača opterećenja. Taj naziv je prikazan na portalu ili koristiti kada koje upućuju na to naredbom EŽA ili PowerShell. |

Preostali varijable u predlošku sadrže izvedenih vrijednosti koje se dodjeljuju kada Azure stvara resurse. Promijenite te varijable.
