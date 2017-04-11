<properties
    pageTitle="Što učiniti u slučaju programa Azure servisa prekidu koje utječu na Azure ključ sigurnog | Microsoft Azure"
    description="Saznajte što učiniti u slučaju programa Azure service prekidu koje utječu na sigurnog ključ Azure."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure dostupnosti sigurnog ključ i zalihosti

Azure ključ sigurnog značajke više Slojevi zalihosti da biste bili sigurni da ključeva i tajne ostaju dostupna u aplikaciji čak i ako se neće uspjeti pojedinačnih komponenti za taj servis.

Sadržaj sustava ključa sigurnog su replicirati unutar područja i sekundarne područje barem 150 milja Odsutan, ali unutar iste Zemljopis. To se održavaju visoke rok trajanja tajne te tipki.

Ako pojedinačnih komponenti unutar servisa sigurnog ključa uspjeti, zamjenski komponente unutar područja se korak u posluživanje zahtjev da biste bili sigurni da ne postoji bez smanjene performanse funkcionalnosti. Nije potrebno ništa poduzimati pokretanje to. Automatski će se dogoditi i će biti prozirni vama.

U događaj rijetko cijelo Azure područje nedostupan, zahtjevi za koje ste napravili od Azure ključ sigurnog u tom području su automatski usmjerena (*nije uspjela tijekom*) na sekundarnom područje. Kada primarni regija ponovno postane dostupan, zahtjevi za usmjeriti natrag (*nije uspjela natrag*) primarni regiju. Ponovno nije potrebno ništa poduzimati jer je to se događa automatski.

Postoji nekoliko upozorenja Imajte na umu:

* Slučaju prebacivanje u regiji može potrajati nekoliko minuta za servis nije uspjela tijekom. Dok se ne dovrši u prebacivanje, zahtjeve za to vrijeme možda neće uspjeti.
* Po dovršetku je prebacivanje na ključa sigurnog je u načinu samo za čitanje. Zahtjevi za koje su podržane u tom načinu su:
 * Ključni sefovi popisa
 * Dobiti svojstva ključa sefovi
 * Popis tajne
 * Početak tajne
 * Popis tipki
 * Početak tipke (svojstava)
 * Šifriranje
 * Dešifriranje
 * Prelamanje
 * Ukini prelamanje
 * Provjerite je li
 * Prijava
 * Sigurnosno kopiranje
* Kada je prebacivanje nije uspio natrag, dostupni su sve zahtjev vrste (uključujući čitanja *i* pisanja zahtjeva).
