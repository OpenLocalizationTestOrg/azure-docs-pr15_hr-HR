Dok je moguće da biste zalijepili MongoLab URI u kodu, preporučujemo da konfiguriranje u okruženju radi lakšeg upravljanja. Na taj način, ako URI promijeni, možete ažurirati je putem portala za Azure bez potrebe za kod.


1. Na portalu za Azure odaberite **Web-aplikacije**.
1. Kliknite naziv web-aplikacije na popisu web-aplikacije.  
![WebAppEntry][entry-website]  
Prikazat će se na nadzornoj ploči Web App.

1. Na traci izbornika kliknite **Konfiguriraj** .  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Pomaknite se prema dolje do odjeljka nizove veze.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. U odjeljku **naziv**unesite MONGOLAB_URI.
1. Kao **vrijednost**zalijepite niz za povezivanje ćemo dobiti u prethodnom odjeljku.
1. Na padajućem popisu **Vrsta** (umjesto zadano **SQLAzure**) odaberite **Prilagođeno** .
1. Kliknite **Spremi** na alatnoj traci.  
![SaveWebApp][button-website-save]

**Bilješke:** Azure dodaje u **CUSTOMCONNSTR\_ ** prefiks koji će se ova varijabla koja je Zašto kod iznad reference **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
