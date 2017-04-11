<properties
   pageTitle="Kako koristiti Power BI ugrađene s OSTATKOM | Microsoft Azure"
   description="Saznajte kako koristiti Power BI ugrađene s OSTATKOM "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Kako koristiti Power BI ugrađene s OSTATKOM


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI ugrađene: Što je to i što je
Pregled dodatka Power BI ugrađeni je opisan u službeni [Power BI ugrađene web-mjesta](https://azure.microsoft.com/services/power-bi-embedded/), ali Pogledajmo Kratak pregled prije ćemo dobiti u detalje o korištenju s OSTATKOM.

To je vrlo jednostavne zaista. Programa Neovisni često želi da biste koristili vizualizacije dinamički podataka dodatka [Power BI](https://powerbi.microsoft.com) u vlastita računala kao korisničkog Sučelja sastavnih blokova.

No znate, ugrađivanje pločica ili izvješćima servisa Power BI u web-stranicu već moguće je bez servisa Power BI ugrađene Azure pomoću **Dodatka Power BI API -JA**. Kada želite zajedničko korištenje izvješća iz iste tvrtke ili ustanove, možete je ugraditi izvješća s provjerom autentičnosti Azure AD. Morate korisnika koji je prikaza izvješća prijavite se pomoću vlastitih Azure AD računa. Kada želite omogućiti zajedničko korištenje izvješća za sve korisnike (uključujući vanjskih korisnika), možete jednostavno ugraditi uz anonimni pristup.

No vidite, ovaj jednostavni ugrađivanje rješenja prilično ne zadovoljava potrebe Neovisni aplikacije.
Većina aplikacija Neovisni potrebne za pružanje podataka za svoje kupce ne nužno korisnike iz svoje tvrtke ili ustanove. Na primjer, ako održavate servis za tvrtka A i B tvrtke, korisnici u tvrtki odgovora trebali biste vidjeti samo podatke za svoje tvrtke A. To je više tenancy potreban je za isporuku.

Aplikacija Neovisni može biti nudi vlastitu metoda provjere autentičnosti kao što je provjera autentičnosti obrazaca, osnovne provjere autentičnosti i Dr.. Nakon toga ugrađivanje rješenja moraju surađivati s Ova metoda provjere autentičnosti koje postojeće sigurno. Također je potrebno za korisnike koji će moći koristiti te aplikacije Neovisni bez dodatnih kupnju ili licenciranja pretplate Power BI.

 **Power BI ugrađene** namijenjen je točno ove vrste Neovisni scenarijima. Kada imamo te kratko Upoznavanje van, recimo počnite u Detalji

Možete koristiti .NET \(C#) ili Node.js SDK jednostavno sastavljanje aplikacije s Power BI ugrađeni. No, u ovom se članku smo ćete u članku se objašnjava o tijeku HTTP \(s AuthN) dodatka Power BI bez SDK-ovi. Objašnjenje ovaj tijek možete izraditi aplikacije **s bilo kojeg programski jezik**, a koji mogu razumjeti Duboko bit Power BI ugrađeni.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Prikupljanje radnog prostora za stvaranje Power BI i get tipkovni prečac \(Provisioning)
Power BI ugrađene jedan je od servisa Azure. Neovisni koji koriste Azure Portal se naplatiti naknade za korištenje \(po zaračunava sesiji korisnika), i korisnika koji pregledava izvješće ne naplaćuje ili čak i potrebna pretplata na Azure.
Prije početka naš razvoj aplikacija, ne možemo morate stvoriti **zbirke radnog prostora za Power BI** pomoću portala za Azure.

Pojedinom radnom prostoru programa Power BI ugrađeni je radni prostor za svakog korisnika (klijentsko), a ne možemo možete dodati više radnih prostora u svakoj zbirci radnog prostora. Isti pristupni ključ se koristi za svaku zbirku radnog prostora. Efekt, radni prostor je sigurnost granicu za Power BI ugrađeni.

![](media\power-bi-embedded-iframe\create-workspace.png)

Kada smo završili sa stvaranjem zbirke radnog prostora, kopirajte tipka za pristup s portala Azure.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Ne možemo Dodjela resursa za zbirke radnog prostora i ključ pristup putem REST API-JA. Dodatne informacije potražite u članku [Power BI resursa davatelja API -ji](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Stvaranje datoteke .pbix s Power BI Desktop
Zatim ćemo morate stvoriti podatkovne veze baze podataka i izvješća ugrađivanje.
Za taj zadatak postoji bez programiranja ili kod. Samo koristimo Power BI Desktop.
U ovom se članku smo neće proći kroz pojedinosti o tome kako koristiti Power BI Desktop. Ako vam je potrebna pomoć za ovdje, potražite u članku [Uvod u Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Za našeg primjera ćemo samo koristiti [Maloprodaja analizu uzorka](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Stvaranje radnog prostora za Power BI

Sad kad za dodjelu resursa sve završi, Započnimo stvaranje radnog prostora za klijenta u zbirci radnog prostora putem REST API-ji. U sljedećim HTTP objavu zahtjev (REST) je stvaranje novog radnog prostora u našem postojeću zbirku radnog prostora. U našem primjeru zbirke naziv radnog prostora je **mypbiapp**.
Ne možemo postavite tipkovni prečac koji smo prethodno kopirali kao **AppKey**. To je vrlo jednostavne provjere autentičnosti!

**HTTP zahtjev**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP odgovor**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Vraćeni **workspaceId** koristi se za sljedeće naknadni pozivi API-JA. Naš aplikacije morate zadržati tu vrijednost.

## <a name="import-pbix-file-into-the-workspace"></a>Uvoz datoteke .pbix u radnom prostoru
Pojedinom radnom prostoru možete hostirati u jednu datoteku Power BI Desktop s dataset \(uključujući postavke izvora podataka) i izvješća. Ne možemo naše .pbix datoteku možete uvesti u radni prostor, kao što je prikazano u nastavku kod. Kao što vidite, ne možemo možete prenijeti binarni .pbix datoteku pomoću MIME multipart u HTTP-a.

Fragment uri **32960a09-6366-4208-a8bb-9e0678cdbb9d** u workspaceId, a parametar upita **datasetDisplayName** naziv skupa podataka da biste stvorili. Stvoreno dataset nalaze svi podaci povezani artefakte u .pbix datoteke kao što su kao uvezene podatke, pokazivač s izvorom podataka i sl..

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Ovaj zadatak uvoz naići na neko vrijeme. Po dovršetku naš aplikacije mogu postavljati status zadatka pomoću ID-a za uvoz. U našem primjeru uvoza id je **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Sljedeće se s pitanjem status pomoću ID-a u ovom uvoza:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Ako ne zadatak dovršen, HTTP odgovor može biti ovako:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Ako je zadatak dovršen, HTTP odgovor može biti više ovako:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Povezivanje izvora podataka \(i više tenancy podataka)
Dok je gotovo sve artefakte u datoteci .pbix uvezeni u našem radnog prostora, vjerodajnica za izvore podataka nisu. Zbog toga prilikom korištenja **načinu rada DirectQuery**, ugrađeni izvješća nije ih moguće prikazati ispravno. No kada koristite **način uvoza**, ne možemo prikaz izvješća pomoću postojeće uvezenih podataka. U tom slučaju smo morate postaviti vjerodajnica na sljedeći način putem OSTALE pozive.

Najprije ćemo mora se pristupnik za izvor podataka. Ne možemo znati dataset **id** prethodno vraćeni ID-a.

**HTTP zahtjev**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP odgovor**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Pomoću vraćeni pristupnika id i id izvora podataka \(potražite u članku prethodne **gatewayId** i **ID-a** u rezultatu vraćena), ne možemo vjerodajnicu za izvor podataka možete promijeniti na sljedeći način:

**HTTP zahtjev**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP odgovor**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

U proizvodnje, ne možemo možete postaviti niz drugu vezu za svaki radni prostor putem REST API-JA. \(i.e, možete odvojiti baze podataka za svakog korisnika.)

Sljedeće promjene izvora podataka putem OSTALE u nizu za povezivanje.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Ili koristimo sigurnosti na razini retka u Power BI ugrađene i odvojiti podatke za svakog korisnika u jedno izvješće. As a Result, možete Dodjela svako izvješće klijenta s istom .pbix \(korisničko Sučelje itd.) i izvora podataka.

> [AZURE.NOTE] Ako koristite **način uvoza** umjesto **načinu rada DirectQuery**, ne postoji način da biste osvježili modelima putem API-JA. Možete i lokalne datasources putem servisa Power BI pristupnika još nije podržan u Power BI ugrađeni. Međutim, zaista ćete htjeti nadzirali [blog za Power BI](https://powerbi.microsoft.com/blog/) za što je novo, a što dolazi u budućnosti izdanja.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Provjera autentičnosti i hosting (ugrađivanje) izvješća u našem web-stranicu

Na prethodni REST API-JA, koristimo tipka za pristup **AppKey** sam kao zaglavlje autorizacije. Budući da te pozive se može riješiti na strani poslužitelja pozadinskog, je sigurno.

No, kada smo ugrađivanje izvješća naš web-stranicu, tu vrstu sigurnosnih informacija želite rukovati pomoću JavaScript \(sučelju). Zatim vrijednost zaglavlja autorizacije morate zaštićen. Naš tipkovnog otkrivanja je zlonamjeran korisnik ili kodu, možete nazvati sve operacije pomoću sljedećeg ključa.

Kada smo ugrađivanje izvješća naš web-stranicu, koristimo morate izračunati token umjesto tipka za pristup **AppKey**. Naš aplikacije potrebno je stvoriti Web tokena Json za OAuth \(JWT) koji se sastoji od na zahtjevima i izračunati digitalni potpis. Kao što je prikazano ispod ovaj OAuth JWT je točka-zarezom kodiranih niz tokena.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Najprije morate Priprema ulazne vrijednosti koji je potpisan kasnije. Ta vrijednost je niz kodiran url (rfc4648) base64 sljedeće json, a to su zarezima, točke \(.) znak. Kasnije, ne možemo objašnjavaju kako doći do id izvješća.

> [AZURE.NOTE] Ako želimo za uporabu retka razinu sigurnosti (RLS) Power BI ugrađene, ne možemo morate navesti **korisničko ime** i **ulogama** u na zahtjevima.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Nakon toga morate stvoriti niz base64 kodirani HMAC \(potpis) s SHA256 algoritam. Traje ulaznu vrijednost je prethodni niz.

Zadnje, morate kombinirati vrijednost i potpis niz pomoću razdoblje \(.) znak. Dovršeni niz je tokena aplikacije za ugrađivanje izvješća. Čak i ako token za aplikaciju je otkriti zlonamjernom korisniku, nije moguće dohvatiti izvorne tipka za pristup. Ovaj token aplikacije brzo će isteći.

Evo primjera i za sljedeće korake:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Na kraju izvješća ugraditi u web-stranicu

Ugrađivanja naš izvješće ne možemo morate dohvaćanje URL-a Ugradi i izvješća **ID-a** pomoću sljedećih REST API-JA.

**HTTP zahtjev**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP odgovor**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Ne možemo možete ugraditi izvješća u našem web-aplikaciji pomoću prethodne token aplikacije.
Ako ne možemo pogledajte sljedeći ogledni kod bivši dio je isti kao u prethodnom primjeru. U dijelu potonjem ovom je primjeru prikazano **embedUrl** \(potražite u članku prethodni rezultat) u iframe, te je objavljivanje token aplikacije u iframe.

> [AZURE.NOTE] Morat ćete promijenite vrijednost id izvješća u jednu od svojih. Pogreška u našem sustavu za upravljanje sadržajem, oznake iframe u uzorku kod je pročitajte i doslovno. Uklanjanje capped tekst oznake ako kopirate i zalijepite kod uzorka.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

I Evo naš rezultata:

![](media\power-bi-embedded-iframe\view-report.png)

Trenutačno Power BI ugrađene samo prikazuje izvješće u iframe. No nadzirali [Blog za Power BI](). Poboljšanja za buduće može koristiti novi strani klijenta API-ji koji će Javite nam poslali informacije u iframe kao i pristup informacijama. Zanimljiv sadržaji!


## <a name="see-also"></a>Vidi također
- [Provjera autentičnosti i dopuštanja u Power BI ugrađeni](power-bi-embedded-app-token-flow.md)
