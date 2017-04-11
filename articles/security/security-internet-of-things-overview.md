<properties
   pageTitle="Pregled sigurnosti Internet stvari | Microsoft Azure"
   description=" Azure internetske servise stvari (IoT) nudi širok raspon mogućnosti. Ovaj članak sadrži znati radi zaštite vaše IoT rješenja Azure. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="internet-of-things-security-overview"></a>Pregled Internet stvari sigurnosti

Azure internetske servise stvari (IoT) nudi širok raspon mogućnosti. Ove usluge ocjene enterprise omogućuju vam:

- Prikupljanje podataka s uređaja
- Analiza podataka strujanja u-kretanja
- Pohrana i velikih skupova podataka za upite
- Vizualizacija podataka u stvarnom vremenu i povijesne
- Integracija s pozadinske sustave

Izlaganje ove mogućnosti, Azure IoT programski paketi zajedno većem broju servisa Azure s prilagođene datotečnim nastavcima kao konfiguriranog rješenja. Ova konfiguriranog rješenja su osnovni implementacije uobičajene uzorke rješenje IoT koji vam pomoći da biste smanjili vrijeme poduzeti prilikom izlaganja vaš IoT rješenja. Korištenje IoT softver razvojni paketi, možete prilagoditi i proširivanja tih rješenja da bi odgovarao s vlastitim potrebama. Ova rješenja možete koristiti i kao primjeri ili predložaka kada su razvoja novih IoT rješenja.

Paket Azure IoT je Napredna rješenje za vaše potrebe IoT. Međutim, nije upmost važnosti vašeg rješenja IoT dizajnirani s sigurnošću na umu od početka. Zbog sheer broj IoT uređaja, sve incident vezan uz sigurnost brzo postaju Primamo događaj s značajan rezultate.

Pomoću kojih se objašnjava kako radi zaštite vaše IoT rješenja, imamo sljedeće informacije.

## <a name="security-architecture"></a>Sigurnosna arhitektura

Prilikom dizajniranja sustav, važno je da biste shvatili potencijalnih prijetnji taj sustav i sukladno tome, dodajte odgovarajuće obranu dizajniran i architected sustav. Je osobito važno za dizajniranje proizvoda od početka s sigurnošću na umu jer objašnjenje kako napadaču mogu ugroziti sustava olakšava sigurni odgovarajuće mitigations će se prikazati od početka.

O IoT sigurnosna arhitektura možete saznati tako da pročitate [Internet od stvari sigurnosna arhitektura](../iot-suite/iot-security-architecture.md).

U ovom se članku opisuje u sljedećim temama:

- [Sigurnost započinje s modelom prijetnju](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [Sigurnost u IoT](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Prijetnju Modeliranje arhitektura Azure IoT referenca](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Sigurnost od dna prema gore

Na IoT predstavlja jedinstveni izazove sigurnost, privatnost i usklađenost u tvrtkama diljem svijeta. Za razliku od tehnologije tradicionalni cyber gdje te probleme vezane uz softver i kako je implementirana, IoT odnosi što se događa kada se cyber i fizičke svijetu Spoji. Zaštita IoT rješenja zahtijeva osiguravanje sigurne dodjeljivanje uređaja, sigurne veze između tih uređaja i oblaka i zaštitu sigurne podataka u oblaku tijekom obrade i prostora za pohranu. Radi protiv takve funkcionalnost, međutim, su resursa ograničeno uređaja, geografske raspodjele implementacijama i velik broj uređaja unutar rješenja u.

Možete saznati kako rukovati sigurnost u ta područja tako da pročitate [Internet stvari sigurnost od dna prema gore](../iot-suite/securing-iot-ground-up.md).

U članku se opisuje u sljedećim temama:

- [Sigurna infrastrukture od dna prema gore](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure – sigurne IoT infrastrukture za svoju tvrtku](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Najbolje prakse

Zaštita infrastruktura za IoT zahtijeva stroge sigurnost u dubine strategije. Počevši od zaštiti podataka u oblaku, zaštita integritet podataka na putu s Interneta javno i pruža mogućnost sigurnog Dodjela uređaja, svaki sloj sastavlja veća sigurnost jamstvo u cjelokupan infrastrukture.

Možete saznati o sigurnosti Internet stvari najbolje prakse tako da pročitate [najbolje prakse za sigurnost internetske mogućnosti](../iot-suite/iot-security-best-practices.md).

U članku se opisuje u sljedećim temama:

- [Proizvođač hardvera IoT/integrator](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [IoT rješenja za razvojne inženjere](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [Deployer IoT rješenja](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [Operator IoT rješenja](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
