<properties
    pageTitle="Azure državne dokumentaciju | Microsoft Azure"
    description="Ovo omogućuje comparision značajki i upute na razvoj aplikacija za državne ustanove Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Azure državne računalnim

##  <a name="virtual-machines"></a>Virtualnim strojevima

Detalje na taj servis i kako je koristiti potražite u članku [Azure virtualnim strojevima veličine](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Varijacije

Sljedeće VM SKU-ove su obično dostupnih (GA) u Azure državne:

VM SKU|Nova Gov SAD-a|IA Gov SAD-a|Bilješke
---|---|---|---
ODGOVORA|GA|GA|Ništa
Dv1|GA|-|Ništa
DSv1|GA|-|Ništa
Dv2|GA|GA|15 uskoro dostupno
F|GA|GA|Ništa
G|Planirani|-|Ništa

###  <a name="data-considerations"></a>Razmatranja podataka

Sljedeće informacije služi za identifikaciju granicu Azure državne virtualnim računalima sustava za Azure:

| Regulated/kontrolirati podataka dopušteno | Regulated/kontrolirati podataka nije dopuštena |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unijeli podatke, spremljene i obrađuju unutar na VM može sadržavati izvoz upravlja. Binarne datoteke radi virtualnim računalima sustava unutar Azure. Statički authenticators, kao što su lozinke i PIN-ove pametne kartice za pristup komponente platforme Azure. Privatni ključevi certifikata koji se koriste za upravljanje komponente platforme Azure. SQL nizova veze.  Ostale sigurnosne informacije/tajne, kao što su certifikati, ključeva za šifriranje, Matrica tipke i pohranu ključeva pohranjenih u Azure services.  | Metapodaci nije dopušteno sadrže izvoz upravlja. Ti metapodaci uključuje sve podatke o konfiguraciji koji unijeli prilikom stvaranja i održavanja sustava Azure virtualnog računala.  Unesite Regulated/kontrolirati podatke u sljedeća polja: smjernice za nazive uloga, grupa resursa, nazivi implementaciju, nazive resursa, oznake resursa  

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije i ažuriranja, pretplatite se na <a href="https://blogs.msdn.microsoft.com/azuregov/">državne Blog o programu Microsoft Azure.</a>
