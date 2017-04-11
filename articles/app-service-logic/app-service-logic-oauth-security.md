<properties
    pageTitle="Sigurnost OAUTH u SaaS poveznika i aplikacije API | Azure"
    description="Saznajte više o sigurnosti OAUTH u poveznika i API aplikacije u aplikacije servisa za Azure; Arhitektura microservices; saas"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Saznajte više o sigurnosti OAUTH u SaaS poveznika

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju.

Mnoge softvera kao poveznici za servis (SaaS) kao što su Facebook, Twitter, DropBox i tako dalje od korisnika za provjeru autentičnosti protokolom OAUTH.  Prilikom korištenja ove SaaS poveznika iz aplikacija logike dajemo pojednostavnjeno korisničko sučelje na gdje kliknuti "Ovlasti" u dizajneru logike aplikacije. Kada vas **ovlasti**, što trebate prijavite se u (Ako nije već) i dati dozvole za povezivanje sa servisom SaaS u vaše ime. Nakon što omogućuje pristanak i autorizirali, aplikacija logike može pristupiti tih servisa SaaS.

## <a name="create-your-own-saas-app"></a>Stvaranje vlastite SaaS aplikacije
Sučelje za pojednostavljeni je moguće jer smo prethodno stvorene i registrirana naš aplikacije u tih servisa SaaS.  U određenim slučajevima, preporučujemo vam da biste registrirali i koristite vlastitu aplikacije.  To je potrebno, na primjer, kada želite koristiti te SaaS poveznika u prilagođene aplikacije. U ovom se primjeru koristi poveznik za DropBox, ali je postupak isti za sve poveznike koje ovise o OAUTH.

Čak i u kontekstu logike aplikacije, umjesto pomoću zadane aplikacije koje nudimo možete koristiti vlastitu aplikacija. Ako gumb "Ovlasti" ne uspije povezati, pokušajte stvaranju vlastite lokacije. Slijedi popis korake za poveznik Twitter:

1. Otvorite poveznik za Twitter na portalu za Azure pretpregled. Idite na **Pregled** > **API aplikacija**. Odaberite poveznik za Twitter:  
    ![][1]

2. Odaberite **Postavke** > **provjere autentičnosti**:  
    ![][2]

3. Kopirajte vrijednost **URI preusmjeravanje** :  
    ![][3]

4. Idite [na Twitteru](http://apps.twitter.com) i **stvorite novu aplikaciju**. U svojstvu **Povratnog URL** zalijepite vrijednost **Preusmjeravanje URI** kopirana poveznik za Twitter: ![][4]  
5. Prilikom stvaranja aplikacije Twitter, odaberite **ključ i tokeni programa Access**. Kopirajte ove vrijednosti.
6. U postavkama Twitter poveznik za provjeru autentičnosti, zalijepite te vrijednosti u svojstvima **ID klijenta** i **Tajna klijent** :   
    ![][5]  
7. Spremanje postavki poveznik.  

Sada mora biti moći koristiti svoje connector iz aplikacije logike. Kada koristite ovaj poveznik iz aplikacija logike, koristi aplikacije umjesto zadane aplikacije.  

> [AZURE.NOTE] Ako ste prethodno ovlašteni aplikaciju, možda ćete morati reauthorize aplikaciju.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
