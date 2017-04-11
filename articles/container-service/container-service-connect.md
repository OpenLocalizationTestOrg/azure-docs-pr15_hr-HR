<properties
   pageTitle="Povezivanje programa servisa Azure spremnik klaster | Microsoft Azure"
   description="Povezivanje programa klaster servisa Azure spremnik pomoću programa tunelom SSH."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, spremnika, Micro-servisima, Kontroler-OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Povezivanje programa servisa Azure spremnik klaster

Kontroler/OS i Docker Swarm klastere koje su uvedene Azure spremnik servis izložiti OSTALE krajnje točke. Međutim, te krajnje točke nisu otvoreni izvan svijeta. Da biste upravljali te krajnje točke, morate stvoriti sigurne ljuske (SSH) tunelom. Nakon što je SSH tunelom je uspostavljena, možete pokrećete naredbi za krajnje točke klaster i pogledati klaster korisničkog Sučelja putem preglednika na vlastiti sustav. Ovaj dokument vodit će vas kroz stvaranje programa tunelom SSH iz Linux, OS X i Windows.

>[AZURE.NOTE] Možete stvoriti sesiju SSH upravljanja sustavom klaster. Međutim, ne preporučujemo to. Raditi izravno na sustav upravljanja izlaže odgovornost za konfiguraciju slučajne promjene.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Stvaranje programa tunelom SSH na Linux ili OS X

Najprije kada stvarate programa tunelom SSH na Linux ili OS X je da biste pronašli javno DNS naziva uravnoteženja matrice. Da biste to učinili, proširite grupe resursa tako da se prikazuje se svaki resurs. Pronađite i odaberite javnu IP adresu glavnog. Otvorit će se plohu koji sadrži informacije o javnu IP adresu, što obuhvaća naziv DNS-a. Spremite ovaj naziv za kasnije korištenje. <br />


![Javni naziv DNS-a](media/pubdns.png)

Sada otvorite u ljusci i pokrenite sljedeću naredbu gdje:

**PRIKLJUČAK** je priključak za krajnju točku koju želite izložiti. Za Swarm, to je 2375. Da biste postigli Kontroler/OS, koristite priključak 80.  
Korisničko ime koje ste dobili prilikom implementiran klaster je **korisničko ime** .  
**DNSPREFIX** je prefiks DNS koju ste naveli prilikom implementiran klaster.  
**PODRUČJE** je regiju u kojoj se nalazi grupu resursa.  
**PATH_TO_PRIVATE_KEY** [NEOBAVEZNI] je put do privatni ključ koji odgovara javni ključ koje ste naveli prilikom stvaranja klaster spremnik servisa. Tu mogućnost koristite s -i označite zastavicom.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Veza priključak SSH je 2200 – ne standardne priključak 22.

## <a name="dcos-tunnel"></a>Kontroler/OS tunelom

Da biste otvorili tunelom za krajnje točke Kontroler/OS-odnose, izvršiti naredbu koja je slična sljedećoj:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Sada možete pristupiti Kontroler/OS-povezane krajnjih točaka po:

- KONTROLER/OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Isto tako, možete dobiti rest API-ji za svaku aplikaciju po ovaj tunelom.

## <a name="swarm-tunnel"></a>Swarm tunelom

Da biste otvorili tunelom krajnjoj Swarm, izvršiti naredbu koja izgleda otprilike ovako:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Sada varijablu okruženja DOCKER_HOST možete postaviti na sljedeći način. Možete nastaviti koristiti sučelje naredbenog retka Docker (EŽA) normalno.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Stvaranje programa tunelom SSH u sustavu Windows

Nema više mogućnosti za stvaranje SSH tunnels u sustavu Windows. Ovaj dokument će opisuju kako koristiti PuTTY da biste to učinili.

Preuzmite PuTTY sa sustavom Windows i pokrenite aplikaciju.

Unesite naziv glavnog računala koja se sastoji se od klaster administratorskog korisničkog imena i javne DNS naziva prvu matricu u klasteru. **Naziv glavnog računala** izgledat će ovako: `adminuser@PublicDNS`. Unesite 2200 **priključak**.

![PuTTY konfiguracije 1](media/putty1.png)

Odaberite **SSH** i **provjeru autentičnosti**. Dodavanje datoteka za privatnog ključa za provjeru autentičnosti.

![PuTTY konfiguracije 2](media/putty2.png)

Odaberite **Tunnels** i konfigurirati sljedeće prosljeđuju priključke:
- **Izvorni Priključak:** Postavkama – namijenjen je 80 Kontroler/OS ili 2375 za Swarm.
- **Odredište:** Korištenje localhost:80 Kontroler/OS ili localhost:2375 za Swarm.

U sljedećem primjeru je konfiguriran za Kontroler-OS, ali izgledat će slično kao za Docker Swarm.

>[AZURE.NOTE] Priključak 80 ne smije biti koristi prilikom stvaranja ovog tunelom.

![PuTTY konfiguracije 3](media/putty3.png)

Kada završite, spremite konfiguraciju veze te je povežite PuTTY sesiju. Kada se povežete, vidjet ćete konfiguracija priključak u PuTTY zapisnika događaja.

![PuTTY zapisnik događaja](media/putty4.png)

Kada ste konfigurirali u tunelom za Kontroler-OS, možete pristupiti povezane krajnje točke na:

- KONTROLER/OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Kada ste konfigurirali u tunelom za Docker Swarm, klaster Swarm možete pristupiti putem EŽA Docker. Najprije morat ćete konfigurirati varijablu okruženja sustava Windows, pod nazivom `DOCKER_HOST` s vrijednošću ` :2375`.

## <a name="next-steps"></a>Daljnji koraci

Upravljanje i spremnika Kontroler/OS ili Swarm:

- [Rad sa servisa Azure spremnik i Kontroler/OS](container-service-mesos-marathon-rest.md)
- [Rad sa servisa Azure spremnik i Docker Swarm](container-service-docker-swarm.md)
