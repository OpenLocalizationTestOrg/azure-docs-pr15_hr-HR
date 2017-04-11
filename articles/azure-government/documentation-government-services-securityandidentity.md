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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Identitet i Azure državne sigurnosti

##  <a name="key-vault"></a>Ključni zbirke ključeva
Detalje na taj servis i kako je koristiti potražite u članku na <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure ključ sigurnog javno dokumentacije.</a>

### <a name="data-considerations"></a>Razmatranja podataka
Sljedeće informacije služi za identifikaciju Azure državne granicu za Azure ključ sigurnog:

| Regulated/kontrolirati podataka dopušteno | Regulated/kontrolirati podataka nije dopuštena |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Svi podaci šifriran pomoću ključa Azure ključ sigurnog može sadržavati Regulated/kontrolirati podataka. | Azure ključ sigurnog metapodataka nije dopušteno sadrže izvoz upravlja. Ovaj metapodataka uključuje sve podatke o konfiguraciji koji unijeli prilikom stvaranja i održavanja zbirke ključeva vaš ključ.  Unesite Regulated/kontrolirati podatke u sljedeća polja: nazive grupa resursa, ključ sigurnog imena, a zatim naziv pretplate |

Ključ sigurnog načelu dostupan je u državne Azure. Kao javno, postoji bez nastavka tako sigurnog ključ je dostupan putem komponente PowerShell i EŽA samo.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije i ažuriranja, pretplatite se na <a href="https://blogs.msdn.microsoft.com/azuregov/">državne Blog o programu Microsoft Azure.</a>
