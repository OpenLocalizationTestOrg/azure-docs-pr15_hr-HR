<properties 
    pageTitle="Kako koristiti Blitline za sliku obrada - Azure značajka vodič" 
    description="Saznajte kako koristiti servis Blitline postupak slike unutar Azure aplikacije." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Kako koristiti Blitline s Azure i Azure prostora za pohranu

Ovaj vodič će objašnjavaju kako pristupati servisima Blitline i slanje zadataka Blitline.

## <a name="what-is-blitline"></a>Što je Blitline?

Blitline je obrade servis koji omogućuje enterprise razine slika obrada pri razlomak cijene koje bi trošak sami možete napraviti je slika u oblaku.

Činjenica je da obrada slika već obavljeno uvijek iznova, obično izgraditi od početka projektirane za svaki web-mjesta. Ne možemo shvatili to jer smo već sastavljali ih milijuna puta previše. Jedan dan smo odlučili možda je vrijeme smo samo to učiniti za sve. Ne možemo znati kako to učiniti, učiniti brzo i učinkovito i svi rade u aplikacijom Spremi.

Dodatne informacije potražite u članku [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Što Blitline nije...

Da biste pojašnjavaju što je Blitline korisne su za, često je lakše prepoznali Blitline ne radnje prije prelaska prema naprijed.

- Blitline ne sadrži HTML miniaplikacije za prijenos slika. Morate imati slike dostupne javnosti ili s ograničenim dozvolama koji su dostupni za Blitline dosegne.

- Blitline učinite slika uživo obrade, kao što je Aviary.com

- Blitline Prihvati prijenos slika, ne možete izravno automatske slike da biste Blitline. Morate ih automatske Azure prostora za pohranu ili drugim mjestima Blitline podržava i zatim recite Blitline gdje ih.

- Blitline je massively paralelno i učinite sve sinkrono obrada. Što znači da vam mora dati nam je postback_url i ne možemo može vam reći kada Gotovi smo obradi.

## <a name="create-a-blitline-account"></a>Stvaranje računa Blitline

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Kako stvoriti Blitline posla

Blitline koristi JSON da biste definirali akcije koje želite pisati na slici. U ovom JSON sastoji se od nekoliko jednostavnih polja.

U primjeru najjednostavnije je na sljedeći način:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Ovdje imamo JSON koji će se preuzeti sliku "src" "... boys.jpeg", a zatim promijenite veličinu tu sliku da biste 240 x 140.

ID aplikacije je nešto možete pronaći **Veze informacije** ili **Upravljanje** kartica na Azure. To je vaš tajnu identifikator koji omogućuje vam pokretanje zadataka na Blitline.

Parametar "Spremi" označava informacije o mjesto na koje želite smjestiti sliku nakon što smo ste obrađuju. U ovom slučaju trivial još niste definirali jednu. Ako nije definirana lokacija Blitline će spremiti ga lokalno (i privremeno) na lokaciji jedinstveni oblaka. Moći da biste to mjesto s JSON vratio Blitline se pri upućivanju na Blitline. Identifikator "image" potrebna je, a možete vratiti spremanja za prepoznavanje ovaj posebice slike.

Možete pronaći dodatne informacije o *funkcijama* podržavamo ovdje: <http://www.blitline.com/docs/functions>

Možete pronaći i dokumentaciju o mogućnostima posao ovdje: <http://www.blitline.com/docs/api>

Nakon što dodate na JSON sve što trebate napraviti je **OBJAVA** da biste`http://api.blitline.com/job`

Prikazat će se JSON natrag koja izgleda otprilike ovako:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


To obavijestit će vas da Blitline primio je zahtjev je prekinut je u redu čekanja za obradu te kada se dovrši slika bit će dostupni u: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_Aplikacije\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Kako spremiti sliku na račun servisa Azure prostora za pohranu

Ako imate račun za Azure prostora za pohranu, možete jednostavno imati Blitline automatske obrađeni slike u vašem Azure kontejner. Dodavanjem "azure_destination" definirati lokaciju i dozvole za Blitline da biste primijenili na.

Evo jednog primjera:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Ispunjavanjem CAPITALIZED vrijednosti s vlastitim pošaljete JSON-http://api.blitline.com/job a i slike "src" će se obrađuju s filtrom zamagljivanjem i zatim pomiču vam Azure odredište.

###<a name="please-note"></a>Napomena:

Na SAS mora sadržavati cijeli SAS url, uključujući naziv datoteke odredište.

Primjer:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Možete čitati i najnovije izdanje sustava Blitline na Azure prostora za pohranu dokumenata [ovdje](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Daljnji koraci

Posjetite blitline.com da biste saznali više o sve naše ostale značajke:

* Krajnja točka API Blitline dokumenti <http://www.blitline.com/docs/api>
* Funkcija Blitline API <http://www.blitline.com/docs/functions>
* Primjeri Blitline API <http://www.blitline.com/docs/examples>
* Treći dio biblioteke Nuget <http://nuget.org/packages/Blitline.Net>
