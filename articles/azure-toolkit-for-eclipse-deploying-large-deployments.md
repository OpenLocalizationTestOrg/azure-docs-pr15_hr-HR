<properties
    pageTitle="Implementacija velike implementacije"
    description="Saznajte kako implementirati velike implementacije korištenje kompleta alata za Azure za Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Implementacija velike implementacije #

Ako je prevelik da bi se nalaziti u zadanu mapu approot implementaciju sustava, lokalno spremište resursa možete koristiti kao implementacije korijenske mape za vaše JDK i poslužitelj aplikacije.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Da biste upotrijebili korijenske mape za implementaciju lokalno spremište resursa za velike implementacije ##

1. Stvorite novi resurs lokalno spremište. Nije važno naziv resursa. Resursi za pohranu se definiraju na razini uloge. Najbrži način da biste pristupili lokalno spremište konfiguracije dijaloškom okviru s kojeg ste mogli stvoriti novi lokalno spremište resurs je na sljedeći način: desnom tipkom miša kliknite uloga u prikazu **Programa Project Explorer** (čvor Azure projekta ako ne vidite željenu ulogu), kliknite **Azure**, a zatim kliknite **Lokalno spremište**. U dijaloškom okviru **Lokalno spremište** , kliknite **Dodaj** da biste stvorili novi resurs lokalno spremište.
1. Postavite željene veličine do najmanje 2048 MB (ništa manje uzrokovati istu datoteku veličina probleme kao što je koji će se pojaviti u na approot).
1. Provjerite da **Čišćenje sadržaja kada je instanca uloga brisanja** je li potvrđen okvir; To će spriječiti logike za implementaciju pri pokretanju pokrenut u sukobu s postojećih datoteka u resursa kada je instanca uloga brisanja.
1. Provjerite je li vrijednost **varijable okruženja pohrani put direktorija resursa nakon implementacije** postavljen niz **DEPLOYROOT**. Dijalog resursa lokalno spremište izgledat će otprilike ovako.
    ![][ic667943]

Osim toga, ako koristite **DEPLOYROOT** kao *naziv* lokalnog resursa, a ne promijenite naziv varijable okruženje za automatski generirani (koji će biti postavljena na **DEPLOYROOT_PATH** u tom slučaju), koji daju za svoju aplikaciju.

Dodatne informacije o stvaranju lokalno spremište resursa pronaći ćete na [lokalno spremište svojstva][].

## <a name="see-also"></a>Vidi također ##

[Azure komplet alata za Eclipse][]

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Lokalno spremište svojstava]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
