<properties
   pageTitle="Azure AD Connect sinkronizacije: direktorij proširenja | Microsoft Azure"
   description="U ovoj se temi opisuje značajku proširenja direktorija u Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect sinkronizacije: direktorij proširenja
Proširenja direktorija omogućuje vam da biste proširili shemu u Azure AD vlastite atributa iz lokalnog imeničkog servisa Active Directory. Ova značajka omogućuje sastavljanje aplikacije LOB troše atribute nastavka za upravljanje lokalnim. Ti atributi se može trošiti putem [proširenja direktorija Azure AD grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) ili [Microsoft Graph](https://graph.microsoft.io/). Vidjet ćete atribute dostupna pomoću [programa explorer Azure AD grafikonu](https://graphexplorer.cloudapp.net) i [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) .

Trenutno ne Office 365 radno opterećenje troši te atribute.

Konfiguriranje koje atribute koje želite sinkronizirati na putu prilagođene postavke u čarobnjaku za instalaciju.
![Čarobnjak za nastavak sheme](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) instalacije prikazuje sljedećih atributa koji nisu valjani ikone:

- Vrste objekata korisnika i grupa
- Atributi pojedinačnim: niza, Booleove vrijednosti, cijeli broj, binarni.
- S većim brojem vrijednosti atributa: niza, u binarni.

Popis atributa je za čitanje iz predmemorije stvara tijekom instalacije Azure AD Connect. Ako extended shema servisa Active Directory dodatnim atributima na [shemu morate osvježiti](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) prije te nove atribute su vidljive.

Objekt može sadržavati do 100 atributa direktorija proširenja. Maksimalni duljina iznosi 250 znakova. Ako je vrijednost atributa dulja je, odbacuju se decimale po modula za sinkroniziranje.

Tijekom instalacije sustava Azure AD Connect se aplikacija registrira koje su dostupne te atribute. Vidjet ćete ovu aplikaciju na portalu za Azure.  
![Aplikacija za nastavak sheme](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Ti atributi Zasad su dostupni putem grafikona:  
![Grafikon](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Atributi su mjestu s nastavkom\_{AppClientId}\_. Na AppClientId sadrži iste vrijednosti za sve atribute u direktoriju Azure AD.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
