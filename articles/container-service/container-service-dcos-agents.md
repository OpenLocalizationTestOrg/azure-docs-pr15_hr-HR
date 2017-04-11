<properties
   pageTitle="Javne i privatne Kontroler/OS Agent grupe ACS | Microsoft Azure"
   description="Agent za javne i privatne grupe funkcioniranje s do servisa Azure spremnik klaster."
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
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Kontroler/OS Agent grupe servisa Azure spremnik

Servis za spremnik Azure Kontroler/OS dijeli agenata u javne i privatne grupe. Implementacija možete stvoriti ili resurse pristupačnost između računala na servisu kontejner. Strojeva može biti izložen putem Interneta (javnih) ili drži Interna (privatna). U ovom se članku daje Kratak pregled Zašto su javne i privatne grupe aplikacija.

### <a name="private-agents"></a>Privatni agenata

Agent za privatne čvorove pokrenuti putem koje nisu usmjeravati mreže. Ovu mrežu je dostupan samo administrator zone ili preko ruba usmjerivač javne zone. Prema zadanim postavkama Kontroler/OS pokreće aplikacija na privatne agent čvorove. Potražite u [dokumentaciji Kontroler/OS](https://dcos.io/docs/1.7/administration/securing-your-cluster/) dodatne informacije o sigurnosti mreže.

### <a name="public-agents"></a>Javni agenata

Agent za javne čvorove pokrenuti Kontroler/OS aplikacija i servisa putem mreže javno dostupno. Potražite u [dokumentaciji Kontroler/OS](https://dcos.io/docs/1.7/administration/securing-your-cluster/) dodatne informacije o sigurnosti mreže.

## <a name="using-agent-pools"></a>Korištenje agent grupe

Prema zadanim postavkama **Marathon** uvodi sve novu aplikaciju za *privatne* čvorove agent. Morate izričito implementacija aplikacije čvor *javno* prilikom stvaranja aplikacije. Odaberite karticu **Dodatno** i unesite **slave_public** vrijednost **Prihvatili uloge resursa** . Ovaj postupak je opisan [u nastavku](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) i u dokumentaciji [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) .

## <a name="next-steps"></a>Daljnji koraci

Pročitajte dodatne informacije o [upravljanju vaše spremnika Kontroler/OS](container-service-mesos-marathon-ui.md).

Saznajte kako [otvoriti vatrozida](container-service-enable-public-access.md) nudi Azure dopustili pristup javnim vaše spremnik Kontroler/OS.