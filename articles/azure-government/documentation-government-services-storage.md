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
    ms.date="10/13/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-storage"></a>Azure državne prostora za pohranu

##  <a name="azure-storage"></a>Azure prostora za pohranu

Detalje na taj servis i kako je koristiti potražite u članku [javno dokumentaciju Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Varijacije

URL-ovi za račune za pohranu u Azure državne razlikuju:

Vrsta servisa|Azure javnosti|Azure državne ustanove
---|---|---
Spremište blobova platforme|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Red čekanja za pohranu|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Spremište tablica|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Sve skripte i kod potrebno je račun za odgovarajući krajnje točke.  Potražite u članku [Konfiguriranje nizove veze Azure prostora za pohranu](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Dodatne informacije o API-ji potražite u članku <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Oblaka za pohranu računa Graditelj</a>.

Sufiks krajnja točka za korištenje u te overloads je core.usgovcloudapi.net 

### <a name="considerations"></a>Razmatranja

Sljedeće informacije služi za identifikaciju Azure državne granicu za pohranu Azure:

| Regulated/kontrolirati podataka dopušteno | Regulated/kontrolirati podataka nije dopuštena |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Podaci uneseni, sprema i obrađuju unutar programa Azure prostora za pohranu proizvoda može sadržavati izvoz upravlja. Statički authenticators, kao što su lozinke i PIN-ove pametne kartice za pristup komponente platforme Azure. Privatni ključevi certifikata koji se koriste za upravljanje komponente platforme Azure. Ostale sigurnosne informacije/tajne, kao što su certifikati, ključeva za šifriranje, Matrica tipke i pohranu ključeva pohranjenih u Azure services. | Azure spremišta metapodataka nije dopušteno sadrže izvoz upravlja. Ovaj metapodataka uključuje sve podatke o konfiguraciji koji unijeli prilikom stvaranja i održavanja proizvoda za pohranu.  Unesite Regulated/kontrolirati podatke u sljedeća polja: grupa resursa, nazivi implementaciju, nazive resursa, oznake resursa  

##  <a name="premium-storage"></a>Prostor za pohranu Premium

Detalje na taj servis i kako je koristiti potražite u članku [prostora za pohranu Premium: visokih performansi prostor za pohranu radnih opterećenja virtualnog računala Azure](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Varijacije

Prostor za pohranu Premium Općenito dostupan je u USGov Virginia. To obuhvaća DS niz virtualnih računala. 

### <a name="considerations"></a>Razmatranja

Naveden iste informacije za pohranu podataka vrijediti za račune za pohranu premium. 

##  <a name="next-steps"></a>Daljnji koraci

Za dodatne podatke i ažuriranja pretplatite se na <a href="https://blogs.msdn.microsoft.com/azuregov/">vlade Blog o programu Microsoft Azure.</a>
