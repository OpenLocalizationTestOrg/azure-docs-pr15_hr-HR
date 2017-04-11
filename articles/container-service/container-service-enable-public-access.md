<properties
   pageTitle="Omogućivanje pristupa javno u aplikaciju programa ACS | Microsoft Azure"
   description="Upute za omogućivanje pristupa javno sa servisom Azure kontejner."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, spremnika, Micro-servisima, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Omogućivanje pristupa javnog servisa Azure spremnika u aplikaciju

Bilo koji Kontroler/OS spremnik u ACS [javno agent grupe](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) automatski izložen putem Interneta. Po zadanom priključke **80**, **443**, **8080** se otvaraju pa sve (javni) spremnik slušanje na te priključke se može pristupiti. U ovom se članku objašnjava da biste otvorili dodatne priključke za aplikacije u servisu Azure kontejner.

## <a name="open-a-port-portal"></a>Otvaranje priključka (portal) 

Najprije ćemo morati otvoriti priključak želimo.

1. Prijavite se na portal.
2. Traženje grupa resursa koji je implementiran spremnik servisa Azure da biste.
3. Odaberite agent opterećenja (koji se naziva slično **XXXX-agent-lb-XXXX**).

    ![Azure spremnik usluge raspoređivača opterećenja](media/container-service-dcos-agents/agent-load-balancer.png)

4. Kliknite **Probes** , a zatim **Dodaj**.

    ![Azure spremnik usluge raspoređivača opterećenja probes](media/container-service-dcos-agents/add-probe.png)

5. Ispunjavanje obrasca probni, a zatim kliknite **u redu**.

  	| Polje | Opis |
  	| ----- | ----------- |
  	| Ime  | Opisni naziv za probni. |
  	| Priključak  | Priključak spremnik za testiranje. |
  	| Put  | (Kad su u načinu rada HTTP-a) Relativna web-mjesta put do isprobati. HTTPS nisu podržani. |
  	| Interval | Vrijeme između probni pokušava u sekundama. |
  	| Dobro praga. | Broj uzastopnih probni pokušava prije odlučuje spremnik dobro. | 
    

6. Natrag na svojstva opterećenja agent, kliknite **Učitavanje ujednačavanje pravila** , a zatim **Dodaj**.

    ![Pravila raspoređivača opterećenja usluge za Azure spremnik](media/container-service-dcos-agents/add-balancer-rule.png)

7. Ispunjavanje obrasca raspoređivača opterećenja, a zatim kliknite **u redu**.

  	| Polje | Opis |
  	| ----- | ----------- |
  	| Ime  | Opisni naziv raspoređivača opterećenja. |
  	| Priključak  | Javni ulazni priključak. |
  	| Pozadinski priključak | Interna javno priključak spremnika da biste usmjerili promet na. |
  	| Grupa pozadinska aplikacija | Spremnika u ovom će biti cilj za ovaj opterećenja. |
  	| Isprobati | Probni koji se koristi da bi se utvrdilo dobar cilj u **grupe pozadinska aplikacija** . |
  	| Sesije postojanost | Određuje kako se treba obraditi promet od klijenta za trajanje sesiju.<br><br>**Ništa**: uzastopni zahtjeva putem klijentskog programa isti se može riješiti tako da sve kontejner.<br>**Klijent IP**: uzastopni zahtjeve iz iste IP klijent rješava isti kontejner.<br>**IP klijenta i protokol**: uzastopni zahtjeve iz iste klijent IP i protokol kombinacija rješava isti kontejner. |
  	| Neaktivne vremenskog ograničenja | TCP (samo) U minutama vrijeme da biste zadržali klijent TCP/HTTP otvorite bez potrebe za oslanjanjem na *produžite* poruke. |

## <a name="add-a-security-rule-portal"></a>Dodavanje pravila sigurnosti (portal)

Sljedeći je korak da biste dodali pravilo sigurnosti koji usmjerava promet iz naših otvorena priključka vatrozidu.

1. Prijavite se na portal.
2. Traženje grupa resursa koji je implementiran spremnik servisa Azure da biste.
3. Odaberite **javno** agent mrežu sigurnosne grupe (koji se naziva slično **XXXX-agent – javno-nsg-XXXX**).

    ![Azure spremnik servisa mreže sigurnosne grupe](media/container-service-dcos-agents/agent-nsg.png)

4. Odaberite **Ulazna pravila za sigurnost** , a zatim **Dodaj**.

    ![Sigurnosne grupe pravila za Azure spremnik usluge mreže](media/container-service-dcos-agents/add-firewall-rule.png)

5. Popunite pravila vatrozida Dopusti javno port (priključak), a zatim kliknite **u redu**.

  	| Polje | Opis |
  	| ----- | ----------- |
  	| Ime  | Opisni naziv pravila vatrozida. |
  	| Prioritet | Položaj prioriteta pravila. Na donjoj broj veći prioritet. |
  	| Izvor | Ograničiti dolazne rasponu IP adresa dopušteno ili odbio ovo pravilo. Pomoću **bilo koje** ne navedete ograničenja. |
  	| Servis | Odaberite skup unaprijed definiranih usluga namijenjen ovo pravilo za sigurnost. U suprotnom pomoću **Prilagođeno** da biste stvorili vlastiti. |
  	| Protokol | Ograničiti promet na temelju **TCP** i **UDP**. Pomoću **bilo koje** ne navedete ograničenja. |
  	| Raspon priključka | Kada je **servis** **Prilagođeno**, određuje raspon priključaka koje utječu na ovo pravilo. Možete koristiti jedan priključak, kao što su **80**ili raspon kao što je **1024 1500**. |
  	| Akcija | Dopustiti ili zabraniti prometa koji odgovaraju kriterijima. |

## <a name="next-steps"></a>Daljnji koraci

Saznajte više o razlika između [javnim i privatnim agenata Kontroler/OS](container-service-dcos-agents.md).

Pročitajte dodatne informacije o [upravljanju vaše spremnika Kontroler/OS](container-service-mesos-marathon-ui.md).