 (sigurnosne kopije sefovi<properties
   pageTitle = "Azure Backup limits table"
   Opis = "u članku se opisuje ograničenje Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Sljedeća ograničenja primjenjuju se na Azure sigurnosnu kopiju.

| Ograničenje identifikator | Zadano ograničenje |
|---|---|
|Broj poslužitelja/strojevima koji mogu biti registrirani protiv svake zbirke ključeva|50 za Windows Server/klijent/SCDPM <br/> 200 za IaaS VMs|
|Veličina izvora podataka za podatke pohranjene u Azure sigurnog spremišta|54400 GB Maks<sup>1</sup>|
|Broj sigurnosne kopije sefovi koje je moguće stvoriti u Azure pretplate|25 (sigurnosne kopije sefovi) <br/> 25 oporavak servisa sigurnog po regijama|
|Koliko je puta sigurnosne kopije može se zakazati prema danu|3 dnevno za Windows Server/klijenta <br/> 2 dnevno SCDPM <br/> Jednom dnevno za IaaS VMs|
|Podaci diskova priložiti Azure virtualnog računala za stvaranje sigurnosne kopije|16|

- <sup>1</sup> Ograničenje 54400 GB ne primjenjuju se na IaaS VM sigurnosnu kopiju.

