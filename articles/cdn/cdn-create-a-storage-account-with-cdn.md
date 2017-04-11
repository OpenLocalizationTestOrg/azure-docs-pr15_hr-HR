<properties
    pageTitle="Integracija s računom za pohranu s CDN | Microsoft Azure"
    description="Saznajte kako koristiti Azure sadržaj isporuke mreže (CDN) za isporuku sadržaja velikom propusnošću po predmemoriranje blob-ova iz spremišta Azure."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>Integracija s računom za pohranu s CDN

CDN mogu omogućiti predmemorije sadržaj iz Azure prostora za pohranu. Globalni rješenje za sadržaj velikom propusnošću po predmemoriranje blob-ova i statički sadržaj računalnim instanci pri fizičke čvorove u Sjedinjenim Američkim Državama, Europa, Azija, Australija i Južna Amerika nudi razvojnim inženjerima.


## <a name="step-1-create-a-storage-account"></a>Korak 1: Stvaranje računa za pohranu

Da biste stvorili novi račun za pohranu za pretplatu na Azure pomoću sljedećeg postupka. Račun za pohranu omogućuje pristup servisa Azure prostora za pohranu. Račun za pohranu predstavlja najvišu razinu naziva za pristup svim komponente servisa Azure pohrane: bloba, reda čekanja services, i tablice services. Da biste saznali više, pogledajte [Uvod u spremište na platformi Microsoft Azure](../storage/storage-introduction.md).

Stvaranje računa za pohranu, mora biti administrator servisa ili zajednički administratora za povezane pretplatu.

> [AZURE.NOTE] Možete koristiti za stvaranje računa za pohranu, uključujući Azure Portal i Powershell na nekoliko načina.  Za ovaj vodič smo hoćete li koristiti Portal za Azure.  

**Stvaranje računa za pohranu za pretplatu na Azure**

1.  Prijava na [Portal za Azure](https://portal.azure.com).
2.  U gornjem lijevom kutu odaberite **Novo**. U dijaloškom okviru **Novo** odaberite **podataka + prostor za pohranu**, a zatim kliknite **račun za pohranu**.

    **Stvaranje računa za pohranu** plohu pojavit će se.

    ![Stvaranje računa za pohranu][create-new-storage-account]

4. U polje **naziv** upišite naziv poddomene. Stavka može sadržavati 3 24 mala slova i brojeva.

    Ta vrijednost postaje naziv glavnog računala unutar URI koji se koristi za adresa Blob, reda čekanja ili tablicu resursi za pretplatu. Da biste riješili spremnik resursa u servisu Blob, koristite URI u sljedećem obliku, gdje * &lt;StorageAccountLabel&gt; * se odnosi na vrijednost koju ste upisali u **Unesite URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Važne:** Natpis URL poddomene račun za pohranu URI za obrasce i mora biti jedinstven među svim servisima smještene u Azure.

    Ta vrijednost služi i kao naziv tog računa za pohranu na portalu ili kada programatski pristup ovog računa.

5. Ostavite zadane vrijednosti za **implementaciju model**, **vrstu računa**, **performanse**i **replikacije**. 

6. Odaberite **pretplatu** s koristit će se račun za pohranu.

7. Odaberite ili stvorite **Grupu resursa**.  Dodatne informacije o grupama resursa potražite u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Odaberite mjesto za vaš račun za pohranu.

8. Kliknite **Stvori**. Postupak stvaranja računa za pohranu može potrajati nekoliko minuta.


## <a name="step-2-create-a-new-cdn-profile"></a>Korak 2: Stvaranje novog profila CDN

Zbirka krajnje točke CDN je CDN profila.  Svaki profil sadrži jednu ili više CDN krajnje točke.  Možda želite koristiti više profila da biste organizirali CDN krajnje točke domene internet, web-aplikacije ili nekog drugog kriterija.

> [AZURE.TIP] Ako već imate CDN profil koji želite koristiti za ovaj vodič, prijeđite na [korak 3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Korak 3: Stvaranje krajnju točku CDN

**Da biste stvorili krajnju CDN za vaš račun za pohranu**

1. [Portal za upravljanje Azure](https://portal.azure.com)dođite do CDN profila.  Koje možda imaju prikvačena ga na nadzornu ploču u prethodnom koraku.  Ako vam ne možete pronaći ga tako da kliknete **Pregled**, a zatim **CDN profila**, a na profilu planirate dodati na krajnjoj točki za.

    Pojavit će se profil plohu CDN-a.

    ![CDN profila][cdn-profile-settings]

2. Kliknite gumb **Dodaj krajnjoj točki** .

    ![Dodavanje gumba za krajnje točke][cdn-new-endpoint-button]

    Pojavit će se plohu **Dodavanje krajnje točke** .

    ![Dodavanje plohu krajnje točke][cdn-add-endpoint]

3. Unesite **naziv** za ovaj CDN krajnjoj točki.  Ovaj naziv će se koristiti za pristup predmemorirani resursa na domeni `<endpointname>.azureedge.net`.

4. Na padajućem popisu **Vrsta izvora** odaberite *prostora za pohranu*.  

5. Na padajućem popisu **polazište naziv glavnog računala** odaberite svoj račun za pohranu.

6. Ostavite zadane vrijednosti za **polazište put**, **izvor zaglavlja glavnog računala**i **protokol/polazište priključak**.  Morate navesti barem jedan protokol (HTTP ili HTTPS).

    > [AZURE.NOTE] Tu konfiguraciju omogućuje sve svoje javno vidljive spremnika u vašem računu za pohranu za predmemoriranje u na CDN.  Ako želite ograničiti opseg u jedan spremnik pomoću **polazište put**.  Imajte na umu spremnik mora imati vidljivost postavljena na javno.

7. Kliknite gumb **Dodaj** da biste stvorili novi krajnjoj točki.

8. Nakon stvaranja krajnju točku, prikazuje se popis krajnje točke profila. Prikaz popisa prikazuje se URL koristi za pristup predmemoriranog sadržaja, kao i domenu izvor.

    ![CDN krajnje točke][cdn-endpoint-success]

    > [AZURE.NOTE] Krajnja točka odmah neće moći koristiti.  To može potrajati i do 90 minuta za registraciju za prijenos putem mreže CDN. Korisnici koji pokušaju odmah koristiti naziv domene CDN primiti Šifra stanja 404 dok ne bude dostupno putem na CDN sadržaj.


## <a name="step-4-access-cdn-content"></a>Korak 4: Access CDN sadržaja

Da biste pristupili predmemoriranog sadržaja na na CDN, koristite navedeni na portalu CDN URL. Adresa za predmemorirani blob bit će otprilike ovako:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Kada omogućite CDN pristup računu za pohranu ili hostira servisa, svi objekti javno dostupna su uvjete za predmemoriranje rub CDN. Ako mijenjate objekt koji se trenutno predmemorirani u na CDN, novi sadržaj neće biti dostupne putem na CDN dok se CDN osvježava njezin sadržaj kada istekne predmemorirani sadržaja vrijeme važenja razdoblja.

## <a name="step-5-remove-content-from-the-cdn"></a>Korak 5: Uklanjanje sadržaja u CDN

Ako više ne želite predmemorije objekta u na Azure sadržaja isporuke mreže (CDN), možete učiniti nešto od sljedećeg postupka:

-   Možete unijeti spremnik privatno umjesto Tenis. Dodatne informacije potražite u članku [Upravljanje anonimni pristup za čitanje spremnika i blob-ova](../storage/storage-manage-access-to-resources.md) .
-   Možete onemogućiti ili brisanje krajnje točke CDN pomoću portala za upravljanje.
-   Možete izmijeniti na glavnom računalu servisu više odgovora na zahtjeve za objekt.

Objekt već spremljen u na CDN će ostati predmemorirani dok isteka razdoblja vrijeme važenja objekta ili dok je očišćene krajnju točku. Kada istekne razdoblje vrijeme važenja na CDN će provjeriti je li krajnju točku CDN još uvijek valjan i objekta i dalje anonimno pristupiti. Ako nije, zatim objekt više spremit će se.


## <a name="additional-resources"></a>Dodatni resursi

-   [Upute za mapiranje CDN sadržaj prilagođene domene](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
