<properties
   pageTitle="Usporedba Lake spremišta podataka za Azure s Azure spremišta blobova | Microsoft Azure"
   description="Usporedba Lake spremišta podataka za Azure s blobova platforme Azure prostora za pohranu"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Usporedba Lake spremišta podataka za Azure i spremište blobova platforme Azure

Tablice u ovom se članku nalaze se razlike između Lake spremišta podataka za Azure i spremište blobova platforme Azure duž neke ključne aspekte veliki obradu podataka. Azure blobova je opće namjene, spremištu skalabilni objekata koji je namijenjen razna scenariji za pohranu. Azure podataka Lake spremište je spremište hyper skaliranje koja je optimizirana za radnih opterećenja analize velikih skupova podataka.

|    | Spremište Lake podataka za Azure  | Spremište blobova platforme Azure  |
|----|-----------------------|--------------------|
| Svrha  | Optimizirana za pohranu za radnih opterećenja analize velikih skupova podataka                                                                          | Opće namjene objekt za pohranu za razna scenariji za pohranu                                                                                |
| Korištenje slučajeva                                   | Obrada, interaktivne, strujanje analize strojnog učenja podataka kao što su datoteke, a zatim IoT podataka zapisnika, kliknite strujanja, velikih skupova podataka | Bilo koju vrstu podataka tekst ili binarni, kao što je aplikacija sigurnosno završi, sigurnosne kopije podataka, a zatim medijskih sadržaja za pohranu za strujanje i glavnu svrhu podataka |
| Koncepti                                | Računa spremišta podataka Lake sadrži mape koja sadrži u nizu podataka koji su pohranjeni kao datoteke | Račun za pohranu sadrži spremnika, kojemu u nizu podataka u obliku blob-ova |
| Struktura | Hijerarhijski datotečnom sustavu                                                                                                    | Spremište objekt s nehijerarhijskom prostor naziva  |
| API-JA   | REST API-JA putem HTTP | REST API-JA putem HTTP/HTTPS                                                                                                                            |
| Poslužiteljsko API-JA                             | [WebHDFS kompatibilnog REST API-JA](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Spremište blobova platforme Azure REST API-JA](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Klijentski sustav Hadoop datoteku                   | Da                                                                                                                         | Da                                                                                                                                                 |
| Operacija podataka – provjera autentičnosti            | Ovisno o [identiteta Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md) | Na temelju zajedničke tajne - [Tipke za pristup računu](../storage/storage-create-storage-account.md#manage-your-storage-account) i [Zajedničke pristupnih tipki za potpis](../storage/storage-dotnet-shared-access-signature-part-1.md).                                                                       |
| Operacija podataka - protokol provjere autentičnosti     | OAuth 2.0. Pozivi mora sadržavati je valjan JWT (JSON Web tokena) izdala Azure Active Directory                     | Kod provjere autentičnosti utemeljene na raspršivanje poruke (HMAC). Pozivi mora sadržavati Base64 kodirani SHA-256 raspršivanje iznad dijela HTTP zahtjev. |
| Operacija podataka – autorizacija               | POSIX popise kontrole pristupa (ACL).  ACL-a na temelju Azure Active Directory identiteta možete postaviti razinu datoteka i mapa. | Za račun razinom autorizacije – koristi [Račun pristupnih tipki](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Računa, spremnik ili autorizacije blob - koristi [Zajedničko korištenje pristupnih tipki potpisa](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Operacija podataka – nadzor                    | Dostupno. U odjeljku [ovdje](data-lake-store-diagnostic-logs.md) informacije.                                                                                                                   | Dostupna                                                                                                                                           |
| Šifriranje podataka na ostale                     | Prozirni, strani poslužitelja (uskoro dostupno)<ul><li>S tipkama servisa upravlja</li><li>S tipkama kupca upravlja u Azure KeyVault</li></ul>| <ul><li>Prozirni, strani poslužitelja</li> <ul><li>S tipkama servisa upravlja</li><li>S tipkama kupca upravlja u Azure KeyVault (uskoro dostupno)</li></ul><li>Klijentsko šifriranje</li></ul> |
| Upravljanje operacije (npr. račun stvoriti) | [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-what-is.md) (RBAC) nudi Azure za upravljanje računima                                                                       | [Kontrola pristupa na temelju uloga](../active-directory/role-based-access-control-what-is.md) (RBAC) nudi Azure za upravljanje računima                                                                                               |
| SDK-ovi za razvojne inženjere                              | .NET, Java, Node.js                                                                                                         | Python .net, Java, Node.js, C++, Ruby                                                                                                              |
| Performanse radno opterećenje Analytics              | Optimizirana izvedbe radnih opterećenja paralelno analize. Visoke propusnost i IOPS.                                           | Nisu optimizirane za radnih opterećenja analytics                                                                                                               |
| Ograničenja veličine                                 | Nema ograničenja veličine račun, veličina datoteka ili broj datoteka                                                                   | Ograničenja za određene dokumentirane [ovdje](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Zalihosti za zemlj.                              | Lokalno-suvišnih (većeg broja primjeraka podataka u jednom području Azure)                                                             | Lokalno suvišnih (LRS), globalno suvišnih (GRS), pristup za čitanje globalno suvišnih (RA-GRS). U odjeljku [ovdje](../storage/storage-redundancy.md) dodatne informacije |
| Stanje servisa                               | Javni pretpregled                                                                                                              | Općenito dostupan                                                                                                                                 |
| Regionalnih dostupnosti  | U odjeljku [ovdje](https://azure.microsoft.com/regions/#services)| U odjeljku [ovdje](https://azure.microsoft.com/regions/#services) |
| Cijena                                       |     U odjeljku [cijena](https://azure.microsoft.com/pricing/details/data-lake-store/)| U odjeljku [cijena](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Daljnji koraci

- [Pregled Lake spremišta podataka za Azure](data-lake-store-overview.md)
- [Početak rada s spremišta Lake podataka](data-lake-store-get-started-portal.md)



