<properties
pageTitle="Instalacija i postavljanje alata za izradu u GitHub"
description="Alati i korake za postavljanje za izradu Azure sadržaja u GitHub."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Instalacija i postavljanje alata za izradu u GitHub

Slijedite korake u ovom članku da biste postavili alate za pojačava Azure tehničku dokumentaciju. Povremeni i Povremeni suradnici vjerojatno možete koristiti GitHub korisničkog Sučelja opisani u koraku 2.

Ako ne znate brojka, možda ćete morati pregled nekih brojka terminologija: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Osim toga, ovaj niti StackOverflow sadrži na Pojmovnik brojka susretati u tom skupu koraka: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Sadržaj

- [Stvaranje GitHub računa i postavljanje profila](#create-a-github-account-and-set-up-your-profile)
- [Registracija za Disqus](#sign-up-for-disqus)
- [Određivanje hoće li zaista morate slijediti ostatak ovih koraka](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Dozvole u GitHub](#permissions-in-github)
- [Instalacija brojka za Windows](#install-git-for-windows)
- [Omogući provjeru autentičnosti dvofaktorska analiza varijance](#enable-two-factor-authentication)
- [Instaliranje programa za uređivanje markdown](#install-a-markdown-editor)
- [Konfiguriranje Atom](#configure-atom)
- [Fork spremište i kopirajte ga na računalo](#fork-the-repository-and-copy-it-to-your-computer)
- [Konfiguriranje korisničkog imena i lokalno e-pošte](#configure-your-user-name-and-email-locally)
- [Daljnji koraci](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Stvaranje GitHub računa i postavljanje profila

Da biste slali Azure Tehnički sadržaj, morat ćete [GitHub](http://www.github.com) računa.

Ako ste Microsoft suradnika, morate postaviti račun GitHub tako jasno prepoznate kao zaposlenik u programu Microsoft. Postavljanje profila na sljedeći način:

- **Slike profila**: slika koje (obavezno)
- **Naziv**: naziv vaše ime i Prezime (obavezno)
- **E-pošte**: adresu e-pošte Microsoft (nije obavezno)
- **Tvrtke**: Microsoft Corporation (obavezno)
- **Lokacija**: popis lokacije (nije obavezno)

Vaš profil mora oblik sličan ovaj profil:

<p align="center">
 ![Primjer GitHub profila](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Registracija za Disqus

Objavljenih Azure tehničkih članaka sadrži komentar strujanje nudi Disqus servisa.

 ![Logotip discus](./media/tools-and-setup/discus.png)

Ako ste Microsoft zaposlenika, a ako ste autor ili suradnika na članak, morate se registrirati za Disqus da sudjelujete u strujanje komentar članka.

1. Registracija za račun na [http://www.disqus.com/](http://www.disqus.com/)
2. Popunite svoj profil na sljedeći način:

 - **Ime i prezime**: svoje ime i prezime kao što je prikazano u svoj unos za adrese adresara Microsoft plus zagradama informacije koje je vaša pseudonim plus @MSFT. Oblik: *prvi i zadnji [alias@MSFT] *
 - **Lokacija**: lokacije
 - **Kratki bio-hranom**: naslov

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Određivanje hoće li zaista morate slijediti ostatak ovih koraka

Možda neće morati slijedite korake u ovom članku. Ovisi o sortiranje sadržaja doprinos želite ili morate donijeti.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Slanje samo za tekst promijenite postojeći članak

Ako samo trebate ili želite izvršiti tekstnih ažuriranja postojeći članak, vjerojatno ne morate slijedite preostale korake. Uređivač utemeljen na webu markdown GitHub, možete koristiti za slanje promjena. Samo kliknite vezu GitHub članka koji želite izmijeniti:

 ![Primjer GitHub profila](./media/tools-and-setup/contributetogit.png)

 Zatim kliknite ikonu za uređivanje u verziji GitHub članka

 ![Primjer GitHub profila](./media/tools-and-setup/editicon.PNG)

 Koji se otvara uređivač jednostavno koristi web koji olakšava poslati promjene. Ne morate slijediti korake u ovom članku.

###<a name="all-other-changes"></a>Sve ostale promjene
Korisničko Sučelje GitHub ne podržava stvaranje nove datoteke te povlačenje i ispuštanje slika. No kada radite u korisničkom Sučelju, upravljanje grana može biti zbunjujuće tako da se obično preporučujemo da instalirate alate i dodatne naredbe za stvaranje i Upravljanje člancima. Ako želite koristiti korisničko Sučelje, pogledajte:

- [Stvaranje datoteka na Github](https://github.com/blog/1327-creating-files-on-github)
- [Prijenos datoteka na vašem spremišta](https://github.com/blog/2105-upload-files-to-your-repositories)

Za sljedeće vrste rada, preporučujemo da instalirate i Saznajte kako pomoću alata:

 - Promjena glavne članka
 - Stvarati i objavljivati novi članak
 - Dodavanje nove slike ili ažuriranje slike
 - Ažuriranje članak tijekom razdoblja dana bez objavljivanje promjene svaki od tih dana
 - Stvaranje sadržaja za izdanje koja ima Go na određeni dan u određeno vrijeme

##<a name="permissions-in-github"></a>Dozvole u GitHub

Bilo koja osoba s računom GitHub mogu pridonijeti Azure Tehnički sadržaj putem naš javno spremište na [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content). Potrebni su bez posebne dozvole.

Ako ste Microsoft PM ili writer koji radi na Azure sadržaja, morate raditi u našem privatno spremišta sadržaja, azure sadržaj cijena. Posjetite [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) da bi se zatražila čitanja dozvole koje će vam provjerite prilozima do privatne repo – prijavite se u GitHub pomoću gumba > kliknite Azure > kliknite **Uključi tima** ili **pridružiti drugom timu**, zatim potražite i pridruživanje grupi **azure sadržaj čitati** .

## <a name="install-git-for-windows"></a>Instalacija brojka za Windows

Instalirajte brojka za Windows s [http://git-scm.com/download/win](http://git-scm.com/download/win). Preuzimanje instalira sustav kontrola brojka verziju, a instalira brojka tulumu, naredbenog retka aplikaciju koju ćete koristiti za interakciju s vaše lokalne brojka spremište.

Možete prihvatiti zadane postavke; Ako želite da se naredbe da bi bio dostupan unutar naredbenog retka sustava Windows, odaberite mogućnost koja omogućuje ga.

<p align="center">
 ![Primjer GitHub profila](./media/tools-and-setup/gitbashinstall.png)

(Napomena: to nije isti kao "Github za Windows". "Github za Windows" različite utemeljenu na GUI alat je koji će funkcionirati i želite li Pročitajte više o sami. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Omogući provjeru autentičnosti dvofaktorska analiza varijance

Morate omogućiti dva provjere autentičnosti (2FA) na vašem računu GitHub ako radite u privatnu spremišta sadržaja. Potreban je u privatnu spremištu.

Da biste to omogućili, slijedite upute u oba sljedeća GitHub temama pomoći:

- [Informacije o programu dvostruka provjera autentičnosti](https://help.github.com/articles/about-two-factor-authentication/)
- [Stvaranje token za pristup za korištenje naredbenog retka](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Kada stvorite token, odaberite sve dostupne u korisničkom Sučelju ([pojedinosti o svaki doseg](https://developer.github.com/v3/oauth/#scopes)) token stvaranje dosega

Kada omogućite 2FA, morate unijeti token za pristup umjesto GitHub lozinku u naredbenom retku pri pokušaju pristupiti GitHub spremište iz naredbenog retka. Pristupni token nije provjere autentičnosti kod koji se isporučuju u tekstnu poruku kada postavljate 2FA. To je dugo niz koji izgleda otprilike ovako: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Nekoliko napomene vezane uz sljedeće:

- Kada stvorite token za pristup, spremite ga u tekstnu datoteku da biste uvijek su dostupne kada vam je potrebna.

- Kasnije, kada je potrebno da biste zalijepili token, znate da biste zalijepili u naredbenom retku dva načina:

 - Kliknite ikonu u gornjem lijevom kutu prozora naredbenog retka > Uređivanje > Zalijepi.
 - Desnom tipkom miša kliknite ikonu u gornjem lijevom kutu prozora, a zatim kliknite Svojstva > Mogućnosti > Brzo uređivanje. To konfigurira naredbeni redak tako da možete zalijepiti klikom desnom tipkom miša u prozoru naredbenog retka.

## <a name="install-a-markdown-editor"></a>Instaliranje programa za uređivanje markdown

Ne možemo autor sadržaja pomoću jednostavne "markdown" notaciju na popisu datoteke radije od kompleksnog "Marža" (HTML, XML itd.). Tako, morate instalirati markdown editor.

- **Atom**: Većina nam koristite GitHub, Atom markdown uređivač: [http://atom.io](http://atom.io). Potrebna licenca za tvrtke. Sadrži provjeru pravopisa.

- **Blok za pisanje**: Blok za pisanje možete koristiti za vrlo laganih mogućnost.

- **Prose**: to je uređivač markdown laganih, elegantni, mrežno i otvaranje izvora koji nudi pregled. Posjetite [http://prose.io](http://prose.io) i autorizirali Prose u vašem spremištu.

- **[Visual Studio kod](https://www.visualstudio.com/products/code-vs.aspx)** - Microsoftovim unosa na to mjesto.

## <a name="configure-atom"></a>Konfiguriranje Atom

Ako koristite Atom, morat ćete postaviti nekoliko stvari.

- Atom zadane postavke za korištenje 2 razmake za kartice, ali Markdown očekuje 4 razmake. Ako ga ostavite na zadani dvaju, svoj članak izgledaju sjajno u lokalnom pretpregled, ali ne prilikom uvoza u Azure. Tako, konfiguriranje Atom koristiti razmake 4 - tu postavku možete pronaći u odjeljku datoteka > Postavke > postavke uređivača > kartica duljine.
- Vjerojatno će i želite uključiti meki prelamanje u ovom odjeljku, koji ne isto kao "prelamanje riječi" u Bloku za pisanje.
- Da biste uključili markdown pretpregleda, kliknite paketa > Pretpregled Markdown > Uključivanje i isključivanje pretpregleda. Da biste uključili ili isključili pretpregled HTML prikaz, poslužite se Ctrl-Shift-M.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Fork spremište i kopirajte ga na računalo

1. Stvaranje o ogranku u spremištu u GitHub - Idi na gornjem desnom kutu stranice i kliknite gumb o ogranku. Ako se to od vas zatraži, odaberite svoj račun kao mjesto gdje se treba stvorene u o ogranku. Time ste stvorili kopiju spremište unutar vašeg računa brojka koncentratora. Općenito govoreći, tehničke autorima i program upravitelji potrebno o ogranku azure-sadržaj-cijena, privatni repo. Zajednica suradnici morati o ogranku azure sadržaja, javno repo. Trebate samo o ogranku jednom; Nakon postavljanja prvog ako želite kopirati na o ogranku na neko drugo računalo, imate samo za izvođenje naredbe koje pratite u ovom odjeljku da biste kopirali u repo s računalom.  Ako odaberete da biste stvorili forks oba spremišta, morat ćete stvoriti o ogranku za svaki spremište.

2. Kopirajte na osobne pristup tokena koju ste dobili od [https://github.com/settings/tokens](https://github.com/settings/tokens). Možete prihvatiti zadane dozvole za token.  Spremite na osobne pristup tokena u tekstnoj datoteci za kasnije ponovno korištenje.

3. Zatim kopirajte spremište na računalo s vjerodajnice ugrađen u niz naredbe.  Da biste to učinili, otvorite tulumu brojka i Pokreni kao administrator. U naredbeni redak upišite slijedeću naredbu.  Ta se naredba stvara direktorij azure-content(-pr) na vašem računalu.  Ako koristite zadano mjesto, bit će pri c:\users<your Windows user name>\azure-content(-pr).

Javni repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Privatni repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Ako, na primjer, ta se naredba Kloniraj može izgledati otprilike ovako:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Postavljanje veze udaljene spremište i konfiguriranje vjerodajnice

Stvaranje reference na spremište korijenski unosom te naredbe. To postavlja povezivanje sa spremištem u GitHub tako da možete dobiti najnovije promjene na lokalno računalo, a zatim ponovno automatske promjene GitHub. Ta se naredba konfigurira vaš token lokalno tako da ne morate unesite svoje ime i lozinku svaki put kada pokušate da biste pristupili upstream repo i vaše o ogranku na GitHub.

Javni repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Privatni repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

To je obično traje neko vrijeme. Kada to učinite, nećete morati ponovno o ogranku ili ponovno unesite vjerodajnice. Samo promijenile kopirati na forks na lokalno računalo ponovno postavite alate na drugom računalu.


## <a name="configure-your-user-name-and-email-locally"></a>Konfiguriranje korisničkog imena i lokalno e-pošte

Da biste bili sigurni su navedene pravilno kao suradnika, morate konfigurirati svoje korisničko ime i e-pošte na lokalno u brojka.

1. Pokretanje tulumu brojka te Prijelaz iz jednog u azure sadržaja ili azure sadržaj cijena:

   ````
   cd azure-content
   ````

 ili

   ````
   cd azure-content-pr
   ````

2. Konfiguriranje korisničko ime tako da odgovara naziva tijekom postavljanja čini u svom profilu GitHub:

    ````
    git config --global user.name "John Doe"
    ````
3. Konfiguriranje e-pošte tako da odgovara primarni e-pošte na određen u svom profilu GitHub; Ako ste MSFT zaposlenika, mora biti adresu e-pošte MSFT:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Vrsta `git config -l` i pregledajte postavke lokalne da biste bili sigurni korisničko ime i e-pošte u konfiguraciji točni.

##<a name="next-steps"></a>Daljnji koraci

- Razumijevanje vrstu sadržaja koji pripada u tehničkoj repo sadržaja, a znali što se ne nalaze. Potražite u članku [smjernice sadržaja kanala](./content-channel-guidance.md)!
- Slijedite [ove korake da biste stvorili ili izmijenili članak i pošaljite ga za objavljivanje](./git-commands-for-master.md).
- Kopirajte [predložak markdown](../markdown templates/markdown-template-for-new-articles.md) kao temelj za novi članak.
- Koristite [navedeni popis za provjeru da biste provjerili zahtjev za istaknuti će zadovoljavaju kriterije kvalitete](./contributor-guide-pr-criteria.md) za spajanje.


###<a name="contributors-guide-navigation"></a>Suradnici vodič za navigaciju

- [Članak](./../README.md)
- [Indeks članaka upute](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
