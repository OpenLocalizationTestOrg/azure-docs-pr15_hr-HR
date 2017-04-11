<properties
    pageTitle="Azure ključ sigurnog rješenja u prijava analitiku | Microsoft Azure"
    description="Da biste pregledali Azure ključ sigurnog zapisnika možete koristiti rješenja Azure ključ sigurnog u zapisnik analize."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure rješenja sigurnog ključ (pretpregled) u zapisnik Analytics

>[AZURE.NOTE] To je [rješenje za pretpregled](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Da biste pregledali Azure ključ sigurnog AuditEvent zapisnika možete koristiti rješenja Azure ključ sigurnog u zapisnik analize.

Možete omogućiti zapisivanje događaja u nadzora za Azure ključ sigurnog. Ti zapisnici zapisuju u spremište blobova platforme Azure gdje se zatim može indeksirati zapisnika Analytics za pretraživanje i analiza.

## <a name="install-and-configure-the-solution"></a>Instaliranje i konfiguriranje rješenja

Slijedite ove upute za instalaciju i konfiguriranje rješenja Azure ključ sigurnog:

1.  Omogućivanje [dijagnostičkog zapisnika sigurnog ključ](../key-vault/key-vault-logging.md) resursa koje želite nadzirati
2.  Konfiguriranje zapisnika Analytics za čitanje zapisnike iz spremišta blobova pomoću postupak opisan u [JSON datoteke u spremište blobova platforme](../log-analytics/log-analytics-azure-storage-json.md).
3.  Omogućivanje rješenja Azure ključ sigurnog pomoću postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  

## <a name="review-azure-key-vault-data-collection-details"></a>Pregledajte pojedinosti zbirke podataka za Azure ključ sigurnog

Rješenja Azure ključ sigurnog prikuplja dijagnostički zapisnici iz spremišta blobova platforme Azure sigurnog ključ Azure.
Nema agent za prikupljanje podataka potreban je.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za Azure ključ sigurnog.

| Platforme | Izravni agent | Agent sustavi centar operacije Manager (SCOM) | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Azure|![ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Da](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![ne](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 minuta|

## <a name="use-azure-key-vault"></a>Korištenje Azure sigurnog ključa

Nakon što instalirate rješenje, možete pogledati sažetak zahtjev statusi tijekom vremena za nadziranim ključ sefovi pomoću **Sigurnog ključ Azure** pločica na stranici **Pregled** zapisnika analize.

![Slika Azure ključ sigurnog pločica](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Kada kliknete pločicu **Pregled** , prikaz sažetaka vaše zapisnika, a zatim prikažete u detalje o sljedećih kategorija:

- Volumen sve operacije ključa sigurnog tijekom vremena
- Nije uspjelo količine postupak tijekom vremena
- Prosječne latencije radu postupak
- Kvaliteta servisa za operacije s broj operacije koje potrajati više od 1000 ms i popis operacije koje potrajati više od 1000 ms

![Slika nadzorne ploče sigurnog ključ Azure](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Slika nadzorne ploče sigurnog ključ Azure](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Da biste pogledali detalje o sve operacije

1. Na stranici **Pregled** kliknite pločicu **Sigurnog ključ Azure** .
2. Na nadzornoj ploči **Azure ključ sigurnog** pregledali sažetak informacija u jednom u blades pa kliknite jednu da biste vidjeli detaljne informacije o tome na stranici zapisnika pretraživanja.

    Na bilo kojoj od stranica za pretraživanje zapisnika, možete pogledati rezultate prema vremenu, detaljne rezultate i povijesti zapisnika pretraživanja. Možete filtrirati i pozornici da biste suzili rezultate.

## <a name="log-analytics-records"></a>Prijava analitiku zapisa

Rješenja Azure ključ sigurnog analizira zapise koji se vrsta **KeyVaults** koji su prikupljeni s [AuditEvent zapisnike](../key-vault/key-vault-logging.md) u Dijagnostika Azure.  Svojstva za te zapise su u sljedećoj tablici.  

| Svojstvo | Opis |
|:--|:--|
| Vrsta | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | IP adresa klijenta koji je izvršio zahtjev |
| Kategorija      | Zapisnici sigurnog ključ AuditEvent je vrijednost jedne, koja je dostupna.|
| CorrelationId | Neobavezni GUID koji klijent prenositi za povezivanje klijentski se zapisnici sa zapisnicima davatelj usluge (ključ sigurnog). |
| DurationMs | Vrijeme trajanja za servisni zahtjev za REST API-JA u milisekundama. Da bi se vrijeme koje ste izmjerili na klijentskoj strani ne odgovaraju ovaj put to ne uključuje latenciju mreže. |
| HttpStatusCode_d | Šifra stanja HTTP vratio zahtjev |
| Id_s       | Jedinstveni ID zahtjeva |
| Identity_o | Identitet iz token koji je unesen pri stvaranju zahtjev za REST API-JA. To je obično "korisnik", "glavni servis" ili kombinaciju "korisniku + ID programa" kao predmet zahtjeva dobivenima u cmdlet Azure PowerShell. |
| OperationName      | Naziv operacije, kao što je zabilježeni u [Azure ključ sigurnog bilježenje u zapisnik](../key-vault/key-vault-logging.md)|
| OperationVersion      | Verzija REST API-JA zatražio klijent|
| RemoteIPLatitude | Zemljopisnu širinu klijenta koji je obavio zahtjev |
| RemoteIPLongitude | Dužina klijent koji ste napravili zahtjev |
| RemoteIPCountry | Država klijent koji ste napravili zahtjev  |
| RequestUri_s | URI zahtjeva |
| Resurs   | Naziv ključa zbirke ključeva |
| ResourceGroup | Grupa resursa od ključne zbirke ključeva |
| ResourceId | ID Azure resursima resursa. Za zapisnike sigurnog ključ, to je uvijek ID sigurnog ključ resursa. |
| ResourceProvider | *MICROSOFT. KEYVAULT* |
| ResultSignature  | HTTP stanje|
| ResultType      | Rezultat zahtjeva za REST API-JA|
| SubscriptionId | ID pretplate za Azure pretplate koja sadrži sigurnog ključ |


## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne podatke sigurnog ključ Azure pomoću [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) .
