<properties
    pageTitle="Postavljanje VPN veze u odjeljku Upravljanje Azure API-JA"
    description="Saznajte kako postaviti VPN veza u Azure API upravljanje i pristup web-servisi kroz nju."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>Postavljanje VPN veze u odjeljku Upravljanje Azure API-JA

Podrška za VPN API upravljanja omogućuje povezivanje pristupnika za upravljanje API-JA s mrežom virtualne Azure (klasični). Time se omogućuje API za upravljanje klijentima za sigurno povezivanje na njihove pozadinskog web-servise koji su na lokalni ili u suprotnom se ne može pristupiti javnom Internet.

>[AZURE.NOTE] Azure upravljanja API funkcionira s klasični VNETs. Informacije o stvaranju klasični VNET potražite u članku [Stvaranje virtualne mreže (klasični) pomoću portala za Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Informacije o povezivanje classic VNETs ARM VNETS, potražite u članku [Povezivanje classic VNets nove VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Omogućiti VPN veze

>GRUPA podaci dostupna je samo u razine **Premium** i **za razvojne inženjere** . Da biste se prebacili, otvorite servis za upravljanje API [Klasični Portal Azure][] , a zatim otvorite karticu **Promjena veličine** . U odjeljku **Općenito** odaberite sloj Premium, a zatim kliknite Spremi.

Da biste omogućili grupa podaci, otvorite servis za upravljanje API [Azure klasični Portal][] i prijeđite na karticu **Konfiguracija** . 

U odjeljku VPN prijeđite **VPN vezu** **na**.

![Konfiguriranje kartica instance upravljanje API-JA][api-management-setup-vpn-configure]

Prikazat će se sada popis sva područja gdje će se servis za upravljanje API dodjeli.

Odaberite VPN-a i podmreže za svaku regiju. Na popisu VPN-ovi se popunjava na temelju virtualne mreže dostupne u pretplatu za Azure koje su postavljanje u regiji konfiguriranja.

![Odaberite VPN-a][api-management-setup-vpn-select]

Kliknite **Spremi** pri dnu zaslona. Nećete moći druge radnje servis za upravljanje API-JA na portalu klasični Azure dok se ažurira. Pristupnik za servis ostat će dostupna i runtime pozive treba može utjecati.

Imajte na umu da VIP adresu pristupnika promijeniti svaki put omogućuje ili onemogućuje VPN-a.

## <a name="connect-vpn"> </a>Za povezivanje s web-servisa iza VPN-a

Kada je servis za upravljanje API VPN-om, pristup web-servisa unutar virtualne mreže je ne razlikuje se od pristup javnim servisima. Samo upišite adresu lokalne ili naziv glavnog računala (Ako je DNS poslužitelj nije konfiguriran za Azure virtualne mreže) web-usluge u polje **URL web-servisa** prilikom stvaranja novog API-JA ili uređivanje postojećeg.

![Dodavanje API-JA s VPN-a][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Tražene priključke za podršku za VPN upravljanje API-JA

Kada je instanca servisa API upravljanje je smještena u VNET, koriste se priključke u tablici u nastavku. Ako su blokirane priključke, servis možda neće pravilno funkcionirati. Imate jedan ili više priključke blokirana je najčešće konfiguraciji problem prilikom korištenja upravljanja API-JA s na VNET.

| Priključke                      | Smjer        | Protokol za prijenos | Svrha                                                          | Izvor / odredište              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Dolazni          | TCP                | Klijent komunikacije Management API-JA                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Izlazni         | TCP                | Ovisnost upravljanje API-JA na Azure prostora za pohranu i Azure Service Bus | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Izlazni         | TCP                | Upravljanje API međuzavisnosti SQL                               | VIRTUAL_NETWORK / INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Izlazni         | TCP                | Upravljanje API međuzavisnosti Bus servisa                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Izlazni         | AMQP               | Ovisnost upravljanje API-JA za zapisnik događaja koncentrator pravila            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Dolazni/odlazni | UDP                | Upravljanje API međuzavisnosti Redis predmemorije                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Izlazni         | TCP                | Ovisnost upravljanje API-JA na zajedničkom mjestu Azure datoteka za BROJKA            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Instalacija prilagođeni DNS poslužitelja

Upravljanje API ovisi o broj Azure services. Kada je instanca servisa API upravljanje smješten u VNET na kojoj se koristi prilagođeni DNS poslužitelj, potrebne da biste mogli riješiti hostnames od tih servisa Azure. Na Prilagođena instalacija DNS-a, slijedite [ovaj](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) vodič.  

## <a name="related-content"> </a>Povezani sadržaj


* [Stvaranje virtualne mreže s vezom za VPN-a web-mjesto pomoću portala za klasični Azure][]
* [Korištenje značajke provjere API pratiti poziva u upravljanju API Azure][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure klasični Portal]: https://manage.windowsazure.com/

[Stvaranje virtualne mreže s vezom za VPN-a web-mjesto pomoću portala za klasični Azure]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Korištenje značajke provjere API pratiti poziva u upravljanju API Azure]: api-management-howto-api-inspector.md
