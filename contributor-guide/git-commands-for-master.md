<properties pageTitle="Brojka naredbe za novi članak stvaranje ili ažuriranje postojeći članak" description="Upute za rad s Azure tehničke sadržaja GitHub spremišta." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Brojka naredbe za novi članak stvaranje ili ažuriranje postojeći članak


## <a name="standard-process-working-from-master"></a>Standardni proces (rad s matrice)
Slijedite korake u ovom se članku na svojem računalu stvorite lokalnu podružnice radu tako da možete stvoriti novi članak sekcije tehničku dokumentaciju azure.microsoft.com ili ažurirati postojeće članak.

1. Pokrenite tulumu brojka (ili alat naredbenog retka koji koristite za brojka).

 **Bilješke:** Ako radite u spremištu javno, promijenite azure sadržaj cijena azure sadržaj u sve naredbe.

2. Promijenite azure sadržaj cijena:

        cd azure-content-pr
3. Pogledajte osnovne granu:

        git checkout master

4. Stvaranje Osvježi lokalni rad grani izvedene iz glavnog granu:

        git pull upstream master:<working branch>


5. Premjestite se u novi radni granu:

        git checkout <working branch>

6. Neka vaše o ogranku znate koji ste stvorili lokalne granu rad:

        git push origin <working branch>

7. Stvorite novi članak ili mijenjati postojeći članak. Da biste otvorili i stvoriti nove datoteke markdown i koristite Atom (http://atom.io) kao uređivač markdown pomoću programa Windows Explorer. Nakon što ste stvorili ili izmijenili članak i slike, prijeđite na sljedeći korak.

8. Dodavanje i Zapiši promjene:

        git add .
        git commit –m "<comment>"
        
   Ili, da biste dodali samo određenim datoteka izmjene:

        git add <file path>
        git commit –m "<comment>"

   Ako ste izbrisali datoteke, morate koristiti:
   
        git add --all
        git commit -m "<comment>"

9. Ažurirajte vaše lokalne podružnice radu s promjenama upstream:

        git pull upstream master

10. Automatske promjene na vašem o ogranku na GitHub:

        git push origin <working branch>

12. Kada ste spremni poslati sadržaj da biste upstream osnovne podružnice za pripremna provjere valjanosti, i/ili objavljivanje, u korisničkom Sučelju GitHub Stvaranje zahtjeva za istaknuti od vašeg o ogranku osnovne grani.

13. Ako ste zaposlenik u spremištu privatne pošaljete promjene se automatski kopirana bez postavljanja i pripremna vezu zapisuje na zahtjev za istaknuti. Pregledajte postupnu sadržaj i odjaviti u zahtjev za komentare istaknuti dodavanjem komentar **#sign isključiti** .  To označava promjene su spremni pomiču se uživo.  Ako ne želite istaknuti zahtjev da biste prihvatili – ako su samo pripremna promjene - dodati bilješke **#hold isključivanje** istaknuti zahtjev.

14. Zahtjev za acceptor istaknuti pregledava zahtjev za istaknuti, nudi povratne informacije i/ili prihvati zahtjev za istaknuti. 

15. Ako želite provjeriti objavljeni članak ili promjene na

 http://Azure.microsoft.com/Documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Za objavljivanje

- Članci su objavljene na približno 10:00 Prijepodne i 3:00 Prikazano po pacifičkom vremenu, od ponedjeljka do petka. Može potrajati do 30 minuta članke da se pojavi online nakon objavljivanja. Imajte na umu istaknuti zahtjev je spojiti pregledavatelja zahtjev istaknuti prije promjene mogu se uključiti u sljedeće zakazano objavljivanje pokrenuti. Morate raditi s vašeg istaknuti pregledavatelja zahtjev na vrijeme, da biste bili sigurni spajanja istaknuti zahtjev za objavljivanje određenih pokrenuti. U suprotnom PRs pregledavaju u redoslijedu su i poslane.

- Ako ste zaposlenik rad u privatnu spremište, sve zahtjeve za istaknuti podliježe pravila za provjeru valjanosti koje morate adresirana prije zahtjev za istaknuti mogu spojiti. 

## <a name="working-with-release-branches"></a>Rad s grana izdanje

Kada se radi o izdanju granu, najbolji način za stvaranje lokalne granu rad iz podružnice izdanje je Sintaksa naredbe:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Time ste stvorili lokalnu granu izravno iz upstream grani izbjegavanje lokalne spajanja.

