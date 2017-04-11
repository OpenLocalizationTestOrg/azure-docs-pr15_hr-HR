<properties
    pageTitle="Otklanjanje poteškoća u Oblaku dodjeli | Microsoft Azure"
    description="Otklanjanje poteškoća s dodjeli ako pokrenete servise u Oblaku u Azure"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Otklanjanje poteškoća s dodjeli ako pokrenete servise u Oblaku u Azure

## <a name="summary"></a>Sažetak
Kada implementacija instance na servis u Oblaku ili Dodaj novo web-mjesto ili instance ulogu suradnika, Microsoft Azure dodjeljuje računalnim resursi. Povremeno primate pogreške prilikom izvršavanja te operacije, čak i prije dođete do ograničenja Azure pretplate. U ovom se članku objašnjava uzroci neke od uobičajenih pogrešaka pri dodjeli i predlaže moguće olakšava. Informacije može biti korisna kada planirate implementaciju servisa.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Pozadina – funkcioniranje dodijeljeni
Poslužitelji u Azure podatkovnim centrima imaju u klastere. Novi oblaka dodijeljeni zahtjeva za uslugu pokušava u više klastere. Kada se prvu instancu implementiran na neki servis u oblaku (u pripremna ili proizvodnje), koji servis u oblaku dobiva prikvačena na klaster. Dogodit će se sve daljnje Implementacija servisa u oblaku u istom klaster. U ovom se članku smo ćete odnose se na kao "prikvačiti klaster". 1 donji dijagram ilustrira slučaj normalni dodijeljeni koja pokušava više klastere; Dijagram 2 ilustrira kutije dodjele koji sadrži prikvačene klaster 2 jer je ondje gdje se hostira postojeće CS_1 servisa oblaka.

![Dodjela dijagrama](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Zašto se događa dodjeli
Kada je zahtjev za dodjelu prikvačena na klaster, postoji veći izgledi neuspješnih da biste pronašli besplatnim resursima Budući da je ograničeno na klaster dostupne resurse. Osim toga, ako je zahtjev za dodjelu prikvačena na klaster, ali vrsti resursa koje ste tražili ne podržava tu klaster, vaš zahtjev neće uspjeti čak i ako klaster ima besplatno resursa. Dijagram 3 ispod pokazuje kutije gdje prikvačene dodijeljeni neće uspjeti jer klaster samo kandidata nema slobodnih resursa. Dijagram 4 pokazuje kutije gdje prikvačene dodijeljeni nije uspjela zbog klaster samo kandidata ne podržava traženu VM veličinu, čak i ako klaster ima besplatno resursa.

![Prikvačeni dodjeli](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Otklanjanje poteškoća dodjeli za servise u oblaku
### <a name="error-message"></a>Poruka o pogrešci
Može se pojaviti sljedeća poruka o pogrešci:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Uobičajeni problemi
Slijede uobičajeni scenariji dodijeljeni koji uzrokuju zahtjev za dodjelu za biti prikvačena na jednom klaster.

- Implementacija vremensko razdoblje pripremna – ako je neki servis u oblaku implementacije u svakom vremensko razdoblje, zatim cijeli oblaku je prikvačena određene klaster.  To znači da ako implementacije već postoji u vremensko razdoblje proizvodnje, zatim novu pripremna implementaciju možete samo se dodijeliti u istom klaster kao jedno područje radnog. Ako klaster je nearing kapacitet, zahtjev možda neće uspjeti.

- U istoj klaster dodijeliti mora skaliranje – dodavanje novih instanci postojeće oblaku.  Mali skaliranje zahtjeve obično se može dodijeliti, ali ne uvijek. Ako klaster je nearing kapacitet, zahtjev možda neće uspjeti.

- Grupa afinitet - novu implementaciju sa servisom cloud prazan se može dodijeliti po tkanina u bilo kojem klaster u tom području osim ako servis u oblaku je fiksiran afinitet grupi. Implementacija istoj grupi afinitet će se pokušati izvesti na istom klaster. Ako klaster je nearing kapacitet, zahtjev možda neće uspjeti.

- Grupa afinitet vNet - starije virtualne mreže su uz afinitet grupe umjesto područja, a servise u oblaku u njima virtualne će biti prikvačena na klaster afinitet grupe. Implementacija toj vrsti virtualne mreže će se pokušati izvesti na prikvačene klaster. Ako klaster je nearing kapacitet, zahtjev možda neće uspjeti.

## <a name="solutions"></a>Rješenja

1. Ponovno implementirate na novi servis u oblaku – to je rješenje vjerojatno će biti najviše uspješno kao omogućuje platformu na raspolaganju sve klastere u tom području.

    - Implementacija povećavaju novi servis u oblaku  

    - Ažuriranje na CNAME ili zapis promet na novi servis u oblaku

    - Kad nula promet će starog web-mjesta, možete izbrisati stari servis u oblaku. Ovo je rješenje trebali biste plaćati nula nedostupnost.

2. Brisanje radnog i pripremna slobodnih - rješenje će zadržati postojeći naziv DNS-a, ali će uzrokovati nedostupnost u aplikaciji.

    - Brisanje proizvodnje i pripremna slobodnih postojeće oblaku tako da je servis u oblaku prazna, a zatim

    - Stvorite novu implementaciju u postojeći servis u oblaku. To će ponovno pokušati na sve klastere u regiji. Provjerite je li servis u oblaku je vezan uz afinitet grupi.

3. Rezervirane IP - rješenje automatski će zadržati postojeći IP adrese, ali će uzrokovati nedostupnost u aplikaciji.  

    - Stvaranje ReservedIP za implementaciju sustava postojeće pomoću komponente Powershell

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Slijedite #2 from above, pazeći da biste odredili novi ReservedIP u CSCFG servisa.

4. Uklanjanje grupe afinitet za nove implementacije - više ne preporučuje se afinitet grupe. Slijedite korake za #1 iznad za implementaciju novi servis u oblaku. Provjerite je li servis u oblaku nije u grupi afinitet.

5. Pretvorite regionalne virtualne mreže – potražite u članku [migriranje iz grupe afinitet regionalne virtualne mreže (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
